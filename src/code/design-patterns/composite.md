> Суть паттерна заключается в том, чтобы к одному объекту можно было обращаться точно так же, как к коллекции (или контейнеру.. или множеству.. или т.п.) этих объектов. На русском такой паттерн называют "**Компановщик**".

Данный паттерн ОЧЕНЬ часто применим

![Untitled](../../../images/code__design-patterns__Untitled%202.png)

## Субъекты

### Component

- Интерфейс для всех компонентов "семейства" объектов

```cpp
class IPage {
public:
	virtual ~IPage() noexcept = default;
public:
	virtual void Add(IPage po) = 0; // это добавление элемента на страницу, а не то, что описывается в Composite
	virtual void Remove() = 0;
	virtual void Delete(IPage po) = 0; // и тут я понял, почему гугл нейминг пишет функции с заглавной буквы
	// delete с маленькой буквы был бы ключевым словом...
};

```

## Composite

- Некоторый компонент, который при этом может содержать и множество других компонентов, а также реализует механизм для их добавления и удаления

```cpp
class Copy : public IPage {
	std::vector<IPage> copy_pages_;
public:
	void AddElement(IPage po) {
		copy_pages_.push_back(po);
	}

	void Add(IPage po) {
		print("sth is added to copy "); // я устал писать std::, поэтому перешёл для удобства на print
	}

	void Remove() {
		print("sth is removed from copy");
	}

	void Delete(IPage po) {
		print("sth was deleted from the copy");
	}
}

```

### Leaf

- Непосредственно сам конечный компонент

```cpp
class Page final : public IPage {
public:
	void Add(IPage po) override {
		std::cout << "sth added on page" << std::endl;
	}

	void Remove() override {
		std::cout << "sth removed from page" << std::endl;
	|}

	void Delete(IPage po) override {
		std::cout << "sth was deleted from page" << std::endl;
	}
};

```

### Client

```cpp
int main()
{
	// Создаём конечные объекты
	Page a;
	Page b;

	// Добавляем их в композитную коллекцию
	Copy all_copy;
	all_copy.AddElement(a);
	all_copy.AddElement(b);

	// Используем как коллекци, так и объект через тот же Add, например
	all_copy.Add(b);
	a.Add(b);

	// то же "типа" одинаковое использование
	all_copy.Remove();
	b.Remove();
}

```

## Нюансы

- Perfect

## Use-cases

- Когда объекты должны быть реализованы в виде иерархической древовидной структуры
- Когда клиенты единообразно должны управлять как целыми объектами, так и их составными частями. То есть целое и его части должны реализовать один и тот же интерфейс