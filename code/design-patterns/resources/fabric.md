> Фабричный метод - это паттерн, который определяет интерфейс для создания объектов некоторого класса, но непосредственное решение о том, объект какого класса создавать происходит в подклассах. То есть паттерн предполагает, что базовый класс делегирует создание объектов классам-наследникам

# Субъекты

## Abstract creator
> некоторый абстрактный класс (интерфейс) с методом, в теории должен уметь создавать и возвращать объект (или управление на него)

```cpp
class IShapeFactory
{
	virtual std::unique_ptr<IShape> CreateShape() = 0; // C++ mechanic for owning
	virtaul ~IShapeFactory() noexcept = default; // C++ philosophy
};
```

## **Concrete Creator**
> конкретный подкласс, содержащий реализацию метода (описанного выше) с созданием конкретных объектов

```cpp
class CircleFactory final : public IShapeFactory
{
	std::unique_ptr<IShape> Create() override 
	{
		return std::make_unique<Circle>() 
	};
};

class SquareFactory final : public IShapeFactory
{
	std::unique_ptr<IShape> create() override 
	{ 
		return std::make_unique<Square>() 
	};
};
```

## **Abstract Product**
> некоторый интерфейс уже создаваемого Creater'ом объекта

```cpp
 class IShape
 {
 public:
	 virtual void craw() = 0; // just method, that client can use on created obj
	 virtaul -IShape() noexcept = default; // just for interface phillosophy in C++
 }

```

## **Concrete Product**
> конкретный класс с реализацией методов объекта, создаваемого Creator'ом

```cpp
class Circle final : public IShape
{
public:
	void craw() override {
		std::cout << "drawing a circle" << std::endl;
	};
};

class Square final : public IShape
{
	void craw() override {
		std::cout << "drawing a square" << std::endl;
	};
};

```

## **Client**
> некоторый внешний юзер, который использует интерфейс Creator, чтобы создать какой-то объект (из набора разных) и с помощью интерфейса Abstract Product может с ним взаимодействовать

```cpp
int main()
{
	auto circle_factory = std::make_unique<CircleFactory>();
	auto square_factory = std::make_unique<SquareFactory>();

	auto circle = circle_factory->createShape();
	auto square = square_factory->createShape();

	circle->craw();
	square->craw();

	return 0;
}

```

# Details
Таким образом, можно представить случай, что человек просто выбирает, что нужно создать:

```cpp
//...
auto shape_factory = std::make_unique<IShapeFactory>();

std::getline(std::cin, input_shape);
if(input_shape == "circle") shape_factory.reset(new CircleFactory);
else if(input_shape == "square") shape_factory.reset(new SquareFactory);
else throw std::logic_error("Wrong Input");

auto shape = shape_factory->CreateShape();
shape.draw();
//...

```

Таким образом, клиенту не нужно знать как что-то создаётся, ему на вход (любым образом) приходит предложение что-то создать, он для этого использует фабрику, где в зависимости от выбранного на вход типа объекта создаётся фабрика под конкретный объект. По факту, независимо от того, что создастся, клиент использует этот создаваемый объект одинаково. (Просто использует draw())
В идеале, вместо switch-case можно делать контейнер (индекс вызываемого объекта - способ его создания).. или любой другой способ реализации, это не сильно важно.

```cpp
using namespace std; // для читаемости

enum EShapes {
	kCircle,
	kSquare,
};

unordered_map<EShapes, IShapeFactory> factory_map;

factory_map.insert(kCircle, make_unique<CircleFactory>());
factory_map.insert(kSquare, make_unique<SquareFactory>());

getline(std::cin, input_shape);
assert(factory_map.contains(input_shape));
auto shape = factory_map[input_shape].createShape();
shape.craw();

```

Таким образом, чтобы программа умела создавать разного рода объекты, достаточно добавить класс для создания объекта и сам объект, добавить его в перечень создаваемых объектов (if-else, switch-case, container), и использовать новой объект так же, как и другие.
## Нюансы
- Такие объекты потом сложно связывать между собой (передача данных между ними)
- Конкретно данная реализация фабрики подразумевает использование лишь одного "семейства" интефрейсов
- Некоторые ЯП имеет проблемы с run-time perfomance при использовании подобного паттерна

## Use-cases
- Когда заранее неизвестно, объекты каких типов необходимо создавать
- Когда система должна быть независимой от процесса создания новых объектов и расширяемой: в нее можно легко вводить новые классы, объекты которых система должна создавать.
- Когда создание новых объектов необходимо делегировать из базового класса классам наследникам
