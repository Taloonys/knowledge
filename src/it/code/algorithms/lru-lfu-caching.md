---
tags: [code, algorithms]
aliases: [LRU]
---

* Весь код ниже -> исключительно мои попытки наспех что-то воспроизвести
# LRU
> Least recently used
> Удалются из кэша только самые старые по "метке времени" доступа записи.
> Кэш размерностью 2 -> записи {1, 2} -> добавляем запись {3} -> дропается {2} из кэша
## classic way
> Стадартный вариант, если на не волнует учёт меток времеини и прочего
> **bidirection-list** + **hashmap with list-iterators**
```cpp
template <typename Id, typename Data>
class LRU {
  //
  // In list : 
  // .. (front -> most used)
  // .. (tail -> least used)
  // That rule is based on list-re-insertions
  //
  using List = std::list<std::pair<Id, Data>>;
  using Map = std::unordered_map<Id, typename List::iterator>;
  
  List items_;
  Map index_;
  size_t limit_;

public:
  LRU(size_t limit) : limit_(limit) {}

  void Set(const Id& key, Data&& value) {
    if (index_.count(key)) {
      items_.erase(index_[key]);
    } else if (items_.size() == limit_) {
      index_.erase(items_.back().first);
      items_.pop_back();
    }
    
    items_.push_front({key, std::forward(value)});
    index_[key] = items_.begin();
  }

  const Data& Get(const Id& key) {
    if (!index_.count(key))
      throw std::runtime_error("not found");
      
    auto it = index_[key];
    items_.splice(items_.begin(), items_, it); // move to front
    
    return it->second;
  }
};
``` 
## hashmap2x+set of pairs
> Относительно учебный вариант
> Ещё подходит, если нам важно хранить какого угодно рода "метку времен"
* тут `pivot_time_` нужен лишь как опорное значение, от которого мы будем делать инкременты и получать около-уникальные значения, вот его как раз иногда заменяют на time_stamp прям
* тут есть немного учёт многопоточности, это мало что даёт для визуального понимания
```cpp
template <typename Id, typename Data> //
class TLruCache {
  //
  // 'recently used' value~ the more uses -> the higher value
  //
  using ru_val_t = std::size_t;
  using ru_metric_t = std::pair<ru_val_t, Id>;

private:
  mutable std::shared_mutex mtx_;
  std::size_t size_limit_ = 0;

  //
  // access counter, used for unique values generator in set
  // set has lexicograph comparison
  //
  int pivot_time_ = 0;

  std::unordered_map<Id, ru_val_t> last_used_;
  std::unordered_map<Id, Data> data_;
  std::set<ru_metric_t> queue_;

public:
  explicit TLruCache(std::size_t sizeLimit) : size_limit_(sizeLimit) {}

public:
  void Erase(const Id &key) {
    if (!Exist(key)) {
      throw std::runtime_error("key not found");
    }

    std::unique_lock lock(mtx_);
    queue_.erase({last_used_[key], key});
    last_used_.erase(key);
    data_.erase(key);
  }

  void Set(const Id &key, Data &&value) {
    std::unique_lock lock(mtx_);
    if (data_.count(key)) {
      //
      // If contains -> full drop (our decision)
      //
      queue_.erase({last_used_[key], key});
      data_.erase(key);
      last_used_.erase(key);
    }

    if (queue_.size() == size_limit_) {
      //
      // Close to size limit? -> drop oldest
      //
      auto [_, key_oldest] = *queue_.cbegin();
      queue_.erase({last_used_[key_oldest], key_oldest});
      last_used_.erase(key_oldest);
      data_.erase(key_oldest);
    }

	//
	// Insertion
	//
    last_used_[key] = pivot_time_++;
    data_[key] = std::forward<Data>(value);
    queue_.insert({last_used_[key], key});
  }

  const Data &Get(const Id &key) {
    std::unique_lock lock(mtx_);
    if (!data_.count(key)) {
      throw std::runtime_error("key not found");
    }

	//
    // update usage value by re-inserting 
    // * to trigger `set` balancing
    //
    queue_.erase({last_used_[key], key});
    last_used_[key] = pivot_time_++;
    queue_.insert({last_used_[key], key});

    return data_[key];
  }

  [[nodiscard]] bool Exist(const Id &key) const {
    std::shared_lock lock(mtx_);
    return data_.count(key);
  }
};
```
# LFU
> Least frequently used
> Удаляются только самые редкие по использованию записи
> Смотрим на пример в LRU, если на {2} заходили часто -> то оно не дропнется из кэша.
* мдя-реализация, но около похоже на правду, почти везде О(1) операции
* расставить мьютексы может каждый дурак, поэтому не трогал их тут
* потенциально непонятно, как правильно обновлять min frequency в erase операции
	* потенациально можно fallback пару завести
	* сделать явный поиск по группам, групп всё равно будет очень сильно меньше, чем N
```cpp
template <typename Id, typename Data> //
class TLfuCache {
  using freq_t = std::size_t;
  using freq_group_t = std::list<Id>;

private:
  std::size_t size_limit_ = 0;
  std::size_t min_freq_group_ = 0;

  struct Node {
    Data value;
    std::size_t freq;
  };

  std::unordered_map<Id, Node> data_;
  std::unordered_map<freq_t, freq_group_t> freq_groups_;
  std::unordered_map<Id, typename freq_group_t::iterator> groups_pos_;

public:
  explicit TLfuCache(std::size_t sizeLimit) : size_limit_(sizeLimit) {}

public:
  void Erase(const Id &key) {
    if (!Exist(key)) {
      throw std::runtime_error("key not found");
    }

    freq_groups_[data_[key].freq].erase(groups_pos_[key]);
    groups_pos_.erase(key);
    data_.erase(key);

	//
	// Drop freq_group if empty
	// !!! Potential issue, i don't update min_freq_group here
	//
    if (freq_groups_[min_freq_group_].empty()) {
      freq_groups_.erase(min_freq_group_);
    }
  }

  void Set(const Id &key, Data &&value) {
    if (Exist(key)) {
      //
      // If we have that key -> clear it everywhere
      //
      Erase(key);
    }

    if (groups_pos_.size() == size_limit_) {
      //
      // Evict least frequent (any if more than one)
      //
      auto &freq_group = freq_groups_[min_freq_group_];
      const auto victim_id = freq_group.front();

      freq_group.pop_front();
      groups_pos_.erase(victim_id);
      data_.erase(victim_id);
    }

    //
    // Insert
    //
    data_[key] = {std::forward<Data>(value), 1};
    freq_groups_[1].push_back(key);
    groups_pos_[key] = std::prev(freq_groups_[1].end());

    min_freq_group_ = 1;
  }

  const Data &Get(const Id &key) {
    if (!data_.count(key)) {
      throw std::runtime_error("key not found");
    }

    auto &target_freq = data_[key].freq;
    auto old_freq = target_freq;

    //
    // Erase from frequency groups
    //
    auto list_pos = groups_pos_[key];
    freq_groups_[target_freq].erase(list_pos);

    //
    // If frequency group is empty -> drop it
    //
    if (freq_groups_[min_freq_group_].empty()) {
      freq_groups_.erase(min_freq_group_);
      if (min_freq_group_ == old_freq)
        //
        // Increase known min_freq_group_
        //
        ++min_freq_group_;
    }

    //
    // Inrease frequency + update group + update iterator
    //
    ++target_freq;
    freq_groups_[target_freq].push_back(key);
    groups_pos_[key] = std::prev(freq_groups_[target_freq].end());

    return data_[key].value;
  }

  [[nodiscard]] bool Exist(const Id &key) const { return data_.count(key); }
};

```