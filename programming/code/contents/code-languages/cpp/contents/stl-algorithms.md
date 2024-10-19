# Algorithms with STL

---

# for_each

> Вызывает функцию с каждым элементом последовательности.


```cpp
std::vector<int> v { 3, 2, 3, 4 };
auto print = [](int x) { std::cout << x; };
std::for_each(v.begin(), v.end(), print);
```

# search, search_n

> search → Ищет вхождение одной последовательности в другую последовательность.
> search_n → Возвращает итератор на начало последовательности из n одинкаовых элементов или end.
> 

```cpp
auto it = search_n(data.begin(), data.end(), howMany, value);
```

# missmatch

> Возвращает пару итераторов на первое несовпадение элементов двух последовательностей.

```cpp
std::vector<int> x { 1, 2 };
std::vector<int> y { 1, 2, 3, 4 };
auto pair = std::mismatch(x.begin(), x.end(), y.begin());
// pair.first == x.end()
// pair.second = y.begin() + 2
```

# count, count_if

# transform

# find, find_if

# accumulate, reduce

# partial_sum

# adjacent_difference

# shuffle

# reverse

# all_of, any_of, none_of

# copy, copy_if

# iota

# generate, generate_n

# fill, fill_n

# inlcusive_scan, exlusive_scan