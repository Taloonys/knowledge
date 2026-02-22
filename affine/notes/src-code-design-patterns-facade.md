---
tags: [code, design-patterns]
aliases: [Субъекты]
---


> Фасад даёт простой вход в сложную подсистему, скрывая детали.

Коротко (JS):
```js
class VideoFacade {
  play(file) { this.decode(file); this.render(); }
  decode(f){ /* heavy */ }
  render(){ /* heavy */ }
}
new VideoFacade().play('movie.mp4'); // клиенту достаточно одного вызова
```

![Untitled](../../../images/code__design-patterns__Untitled%204.png)

![Untitled](../../../images/code__design-patterns__Untitled%201%202.png)

## Субъекты

### Facade

- Класс, на который смотрит Client, чтобы взаимодействовать с системой

```cpp
class Car {
	Engine engine_;
	Lights lights_;
public:
	void StartCar() {
		engine_.Start();
		lights_.TurnOn();
		print("Car is ready to drive");
	}

	void StopCar() {
		engine_.Stop();
		lights_.TurnOff();
		print("Car has stopped");
	}
};

```

### Subsystems

- Какие-то сложные системные элементы, обращение к которым нужно объеденить под что-то одно

```cpp
class Engine {
public:
	void Start() {
		print("Engine Started");
	}

	void Stop() {
		print("Engine Stopped");
	}
};

class Lights {
public:
	void TurnOn() {
		print("Lights on");
	}

	void TurnOff() {
		print("Lights off");
	}
};

```

### Client

```cpp
int main()
{
	Car car;
	car.StartCar();
	car.StopCar();
}

```

## Нюансы

- В некоторой степени ограничивает контроль над системой, в целом
- Возможны нарушения Open-Closed принципов
- Потенциально Фасад может разрастись до огромных размеров
- Возможно дублирование функциональности кода в некоторых случаях

## Use-cases

- Когда имеется сложная система, и необходимо упростить с ней работу. Фасад позволит определить одну точку взаимодействия между клиентом и системой.
- Когда надо уменьшить количество зависимостей между клиентом и сложной системой. Фасадные объекты позволяют отделить, изолировать компоненты системы от клиента и развивать и работать с ними независимо.
- Когда нужно определить подсистемы компонентов в сложной системе. Создание фасадов для компонентов каждой отдельной подсистемы позволит упростить взаимодействие между ними и повысить их независимость друг от друга.

### Common uses

- Ок для упрощения испоьзования библиотеки или API
- Для задач интегрирования
- GUI UX
- Доступ к БД
- Networking, аля сокрытие таких действий, как создание сеанса, подключение к сокету и т.п.
- Общее обращение ко многим семействам конфигураций
- Logging
- Authentication and authorization
- ...
