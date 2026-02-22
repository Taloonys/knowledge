---
tags: [code, design-patterns]
aliases: [Субъекты]
---

> Данный паттерн ориентирован на задачу разделения интерфейса и его реализации друг от друга таким образом, чтобы можно было менять их независимо.


![Untitled](../../../images/code__design-patterns__Untitled%201.png)

## Субъекты

### Abstraction

- Интерфейс, которым будет пользоваться Client

```cpp
class IShape {
public:
	virtual void draw() = 0;
};

```

### Refined Abstraction

- Некоторые классы, которые расширяют поведение Abstraction и предлагают свои реализациии методов соответственно

```cpp
class IRenderer {
public:
	virtual void render() = 0;
};

```

### Implementation

- Реализация Refined Abstraction классов

```cpp
class VectorRenderer : public IRenderer{
public:
	void render() override {
		std::cout << "Rendering as vector"<< std::endl;
	};
};

class RasterRenderer : public IRenderer {
public:
	void render() override {
		std::cout << ""Rendering as a raser" << std::endl;
	};
 };

```

### Concrete Implementation

- Реализация Abstraction с использованием дополненных Refined Abstraction реализациями Implementation

```cpp
class Circle : public IShape {
	Renderer& renderer_;
public:
	Circle(Renderer& renderer) : renderer_(renderer) {};
	void draw() override {
		std::cout << "Drawing a circle" << std::endl;
	};
};

class Square : public IShape {
	Renderer& renderer_;
public:
	Square(Renderer& renderer) : renderer_(renderer) {};
	void draw() override {
		std::cout << "Drawing a square" << std::endl;
	};
};

```

### Client

```cpp
int main()
{
// средства расширенного фукнционала со своей реализацией
	VectorRenderer vector_renderer;
	RasterRenderer raster_renderer;

// объекты, что умеют использовать изменённые "реализации"
	Circle circle(vector_renderer);
	Square square(raster_renderer);

// интерфейс тот же, что и мог бы быть, но реализацию можем спокойно менять
	circle.draw();
	square.draw();

	return 0;
}

```

## Нюансы

- Чем больше пар объект-реализация будет, тем сложнее такой код будет поддерживать
- Сложно поддерживать поведение кода при добавлении кучи реализаций к, условно, одному интерфейсу

## Use-cases

- Когда надо избежать постоянной привязки абстракции к реализации
- Когда наряду с реализацией надо изменять и абстракцию независимо друг от друга. То есть изменения в абстракции не должно привести к изменениям в реализации