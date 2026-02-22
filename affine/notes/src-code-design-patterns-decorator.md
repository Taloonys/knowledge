---
tags: [code, design-patterns]
aliases: [Субъекты]
---

> Данный паттерн нужен для того, чтобы, не меняя уже существующие сущности, добавлять им новый функционал.
> В некоторых ЯП есть **подобное название,** например, **python**. Но это **не одно и то же** 

![Untitled](../../../images/code__design-patterns__Untitled%203.png)

![Untitled](../../../images/code__design-patterns__Untitled%201%201.png)

## Субъекты

### Component Interface

- Интерфейс для Concrete Component(-s) и Decorator(-s)

```cpp
class IceCream {
public:
	virtual std::string GetDescription() const = 0;
	virtual double Cost() const = 0;
};

```

### Concrete Component

- Некоторые объекты, к которым мы хотим добавить функционал

```cpp
class VanillaIceCream : public IceCream {
public:
	std::string GetDescription() const override {
		return "Vanilla Ice Cream";
	}

	double Cost() const override {
		return 160.0;
	}
};

```

### Decorator

- Интерфейс для декоратора, который будет ответственен за добавление нового функционала и будет "обёртывать" Component

```cpp
class IceCreamDecorator : public IceCream {
protected:
	IceCream* ice_cream;
public:
	IceCreamDecorator(IceCream* ic) : ice_cream(ic) {}
public:
	std::string GetDescription () const override {
		return ice_cream->GetDescription();
	}

	double cost() const override {
		returrn ice_cream->Cost();
	}
};

```

### Concrete Decorator

- Реализация Decorator

```cpp
class ChocolateDecorator : public IceCreamDecorator {
public:
	ChocolateDecorator(IceCream* ic) : IceCreamDecorator(ic) {}
public:
	std::string GetDescription() const override {
		return ice_cream->GetDescription() + " with Chocolate";
	}

	double cost() const override {
		return ice_cream->Cost() + 100.0;
	}
};

class CaramelDecorator : public IceCreamDecorator {
public:
	CaramelDecorator(IceCream* ic) : IceCreamDecorator(ic) {}
public:
	std::string GetDescription() const override {
	        return iceCream->GetDescription() + " with Caramel";
	}

	double cost() const override {
	        return iceCream-Cost() + 150.0;
	}
};

```

### Client

```cpp
int main()
{
	std::unique_ptr<IceCream> vanilla_ice_cream = stg::make_unique<VaniullaIceCream>();
	std::cout << "Order: " << vanilla_ice_cream->GetDescription() << ", "
		<< "Cost: Rs. " << vanilla_ice_cream->Cost()
		<< std::endl;

	std::unique_ptr<IceCream> chocolate_ice_cream = stg::make_unique<ChocolateDecorator>();
	std::cout << "Order: " << chocolate_ice_cream->GetDescription() << ", "
		<< "Cost: Rs. " << chocolate_ice_cream->Cost()
		<< std::endl;

	std::unique_ptr<IceCream> caramel_ice_cream = stg::make_unique<CaramelDecorator>();
	std::cout << "Order: " << caramel_ice_cream->GetDescription() << ", "
		<< "Cost: Rs. " << caramel_ice_cream->Cost()
		<< std::endl;

	return 0;
}

```

## Нюансы

- Возможны ситуации, когда порядок определения новых фич с помощью декораторов имеет место быть, в таких случаях неправильный порядок может сделать объект невалидным
- Помимо того, что есть ЯП, которые такое поддерживают по умолчанию.. есть ЯП, которые такое совсем не способны делать

## Use-cases

- Когда надо динамически добавлять к объекту новые функциональные возможности. При этом данные возможности могут быть сняты с объекта
- Когда применение наследования неприемлемо. Например, если нам надо определить множество различных функциональностей и для каждой функциональности наследовать отдельный класс, то структура классов может очень сильно разрастись. Еще больше она может разрастись, если нам необходимо создать классы, реализующие все возможные сочетания добавляемых функциональностей

---

> Не то, чтобы это прям этот паттерн, но просто описатель проектирования класса, который в некоторым смысле можно декорировать… (я его просто встретил и хотел бы запомнить)
> 

```cpp
//
// main.cpp
//

void ApplySocket()
{
	boost::asio::socket socket(/*path*/);
	ScopeExit socket_raii = [&socket]{
		socket.close();
	};
	
	/*..*/
}

int main() 
{
	Service serv
		.On(ApplySocket)
		.On([]{/* any other callback */})
		.Start()
}

//
// Service.cpp
//

Service& Service::On(std::function<void ()> handler)
{
	handlers_.insert(handler);
}

void Service:Start()
{
	for(auto handler : handlers)
	{
		handler();
	}

}

```