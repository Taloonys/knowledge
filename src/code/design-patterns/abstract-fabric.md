
---

> Abstract Factory выдаёт согласованный набор объектов (семейство), чтобы UI/движок выглядел целостно.

Мини-пример (TS):
```ts
interface Widgets { btn(): string; input(): string; }
class MacWidgets implements Widgets { btn(){return 'MacBtn'} input(){return 'MacInput'} }
class WinWidgets implements Widgets { btn(){return 'WinBtn'} input(){return 'WinInput'} }

function render(ui: Widgets) {
  console.log(ui.btn(), ui.input()); // всегда совместимые элементы
}
render(new MacWidgets());
```

# Subjects

## Abstart factory interface

> интерфейс методов, для создания Abstract Product Types
> 

```rust
class IPizzaFactory {
public:
	virtual ~IPizzaFactory() noexcept = default;
public:
	virtual std::shared_ptr<IPizza> createPepperoniPizza() = 0;
	virtual std::shared_ptr<IPizza> createCheesePizza() = 0;
}
```

## **Concrete Factories**

> реализация методов для создания семейств продуктов или конкретных продуктов
> 

```cpp
class ChicagoPizzaFactory : public IPizzaFactory {
public:
	std::shared_ptr<IPizza> createPepperoniPizza() override {
		return std::make_shared<ChicagoPepperoniPizza>();
	}

	std::shared_ptr<IPizza> createCheesePizza() override {
		return std::make_shared<ChicagoCheesePizza>();
	}
}

class NewYorkPizzaFactory : public IPizzaFactory {
public:
	std::shared_ptr<IPizza> createPepperoniPizza() override {
		return std::make_shared<NewYorkPepperoniPizza>();
	}

	std::shared_ptr<IPizza> createCheesePizza() override {
		return std::make_shared<NewYorkCheesePizza>();
	}
}

```

## **Abstract Product Interface**

> интерфейс методов семейства продуктов
> 

```cpp
class IPizza {
public:
	virtual ~IPizza() noexcept = default
public:
	virtual void bake() = 0;
	virtual void cut() = 0;
	virtual void box() = 0;
};

```

## **Concrete Products**

> сами продукты/объекты
> 

Chicago семейство пицц:

```cpp
class ChicagoCheesePizza : public IPizza {
public:
    void bake() override {
        std::cout << "Baking Chicago-style cheese pizza." << std::endl;
    }

    void cut() override {
        std::cout << "Cutting Chicago-style cheese pizza." << std::endl;
    }

    void box() override {
        std::cout << "Boxing Chicago-style cheese pizza." << std::endl;
    }
};

class ChicagoPepperoniPizza : public IPizza {
public:
    void bake() override {
        std::cout << "Baking Chicago-style pepperoni pizza." << std::endl;
    }

    void cut() override {
        std::cout << "Cutting Chicago-style pepperoni pizza." << std::endl;
    }

    void box() override {
        std::cout << "Boxing Chicago-style pepperoni pizza." << std::endl;
    }
};

```

NewYork семейство пицц:

```cpp
class NewYorkCheesePizza : public IPizza {
public:
	void bake() override {
		std::cout << "Bake New York-style Cheese Pizza" << std::endl;
	}

	void cut() override {
		std::cout << "Cut New York-style Cheese Pizza" << std::endl;
	}

	void box() override {
		std::cout << "Box New York-style Cheese Pizza" << std::endl;
	}
 };

class NewYorkPepperoniPizza : public IPizza {
public:
    void bake() override {
        std::cout << "Baking New York-style pepperoni pizza." << std::endl;
    }

    void cut() override {
        std::cout << "Cutting New York-style pepperoni pizza." << std::endl;
    }

    void box() override {
        std::cout << "Boxing New York-style pepperoni pizza." << std::endl;
    }
};

```

## **Client**

> какой-то юзер нашего кода извне
> 

```cpp
int main()
{
	auto new_york_factory = std::make_shared<NewYorkPizzaFactory>();
	auto new_york_cheese_pizza = new_york_factory->createCheesePizza();
	auto new_york_pepperoni_pizza = new_york_factory->createPepperoniPizza();

	auto chicage_factory = std::make_shared<ChicagoPizzaFactory>();
	auto chicago_cheese_pizza = chicage_factory->createCheesePizza();
	auto chicago_pepperoni_pizza = chicage_factory->createPepperoniPizza();

	new_york_cheese_pizza->bake();
	new_york_cheese_pizza->cut();
	new_york_cheese_pizza->box();

	new_york_pepperoni_pizza->bake();
	new_york_pepperoni_pizza->cut();
	new_york_pepperoni_pizza->box();

	chicago_cheese_pizza->bake();
	chicago_cheese_pizza->cut();
	chicago_cheese_pizza->box();

	chicago_pepperoni_pizza->bake();
	chicago_pepperoni_pizza->cut();
	chicago_pepperoni_pizza->box();

	return 0;
}

// ИЛИ ПО ФЕНШУЮ

{ // ...
	auto callable = [](std::shared_ptr<IPizza> pizza) {
		pizza->bake();
		pizza->cut();
		pizza->box();
	};

    callable(new_york_cheese_pizza);
    callable(new_york_cheese_pizza);
    callable(new_york_cheese_pizza);
    callable(new_york_cheese_pizza);

	return 0;
}
```

## Нюансы

- Если решиться менять "семейство", то потом возможны многочисленные изменения в объектах и в остальном коде
- Может усугубить run-time perfomance в некоторых ЯП

## Use-cases

- Когда система не должна зависеть от способа создания и компоновки новых объектов
- Когда создаваемые объекты должны использоваться вместе и являются взаимосвязанными
