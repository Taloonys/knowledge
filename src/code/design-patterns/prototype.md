
> Суть паттерна заключается в том, чтобы обеспечить клонирование объекта. Во многих ЯП для этого есть просто библиотечный метод, например std::clone() в С++.

![Untitled](../../../images/code__design-patterns__Untitled%208.png)

## Когда использовать Прототип?

- Когда конкретный тип создаваемого объекта должен определяться динамически во время выполнения
- Когда нежелательно создание отдельной иерархии классов фабрик для создания объектов-продуктов из параллельной иерархии классов (как это делается, например, при использовании паттерна Абстрактная фабрика)
- Когда клонирование объекта является более предпочтительным вариантом нежели его создание и инициализация с помощью конструктора. Особенно когда известно, что объект может принимать небольшое ограниченное число возможных состояний.

## Субъекты

### Prototype Interface

- Интерфейс всех объектов данного семейства, в котором представлен метод клонирования

```cpp
class IShape {
pulbic:
	virtual ~IShape() noexcept = default;
public:
	virtual std::unique_ptr<IShape> clone() const = 0;
	virtual void draw() const = 0;
}

```

### Concrete Prototypes

- Конкретные объекты некоторого семейства интерфейса Prototype Interface, которые должны реализовать все методы интерфейса, включая clone

```cpp
class Circle final : public IShape {
	double radius_ = 0.0;
public:
	explicit Circle(double r) : radius_(r) = default;
public:
	std::unique_ptr<IShape> clone() override {
        return std::make_unique<Circle>(*this);
	};

	void draw() override {
		std::cout << "Drawing a circle wirh r = " << radius_ << std::endl;
	};
}

class Rectangle final : public IShape {
    double width_ = 0.0;
    double height_ = 0.0;
public:
    explicit Rectangle(double w, double h) : width_(w), height_(h) {}
public:
    std::unique_ptr<IShape> clone() const override {
        return std::make_unique<Rectangle>(*this);
    };

    void draw() const override {
        std::cout << "Drawing a rectangle with width " << width_ << " and height " << height_ << std::endl;
    };
};

```

### Client

```cpp
int main()
{
	Circle circle_proto(5.0);
	Rectangle rect_proto(4.0, 6.0);

	auto shape_rect = rect_proto.clone();
	auto shape_circle = circle_proto.clone();

	shape_rect->draw();
	shape_circle->draw();

	return 0;
}

```

Важно обратить внимание на возвращаемый методом clone() объект (возвращается интерфейс родителя), конструктор копирования может скопировать свой же объект и вернуть свою копию, а clone() в текущем паттерне возвращает объект, содержимое которого определяется в run-time, но обращение к такому объекту происходит только через интерфейс родителя.

## Нюансы

- Очевидные вопросы с указателями при копировании объекта, что так же может вызвать утечки
- В некоторых сценариях возможны цикличные зависимости