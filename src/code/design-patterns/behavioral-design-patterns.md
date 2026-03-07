---
tags: [design-patterns, behavioral]
aliases: [Поведенческие паттерны]
---

> Как объекты общаются и передают работу друг другу.

# Command
> действие как объект: ставим в очередь, логируем, отменяем и т.п.
```ts
// Command  
interface Command {  
	exec(): void  
}  
  
// Receiver  
class Light {  
	on() { { console.log("Light ON") }  
}  
  
// ConcreteCommand  
class TurnOn implements Command {  
	constructor(private light: Light) 
	{ }  
  
	exec() { this.light.on() }  
}  
  

const light = new Light()  
const queue: Command[] = [  
	console.log("invoke begin") 
	new TurnOn(light),  
	new TurnOn(light)  
]  
  
// aka Invoker выполняет очередь  
queue.forEach(cmd => cmd.exec())
```
  
# Iterator
> единый интерфейс обхода без знания внутренностей (ну т.е. не надо знать, тип каждого элемента)
- далеко за примерами ходить не надо - база для контейнеров в любом ЯП
```python
class Iterator:  
	def __init__(self, items):  
		self.items = items  
		self.i = 0  
  
	def next(self):  
		if self.i >= len(self.items):  
			return None  
			
		v = self.items[self.i]  
		self.i += 1  
		return v  
  
  
it = Iterator([1, 2, 3])  
  
while True:  
	x = it.next()  
	if x is None:  
		break  
	print(x)
```
  
# Mediator
>общение компонентов через посредника, меньше связей между ними
```python
  class Chat: # сам медиатор/посредник
      def __init__(self):
          self.us = [] # все чаттеры
		  
      def send(self, msg, me):
          for u in self.us:
              # msg получают все чатеры, кроме самого себя
              if u != me:
                  u.rec(msg)
				  
  class User: # компоненты, которые общаются
      def __init__(self,chat):
          self.c = chat
          chat.us.append(self)
		  
      def send(self,m):
          self.c.send(m, self)
		  
      def rec(self,m):
          print(m)
```
  
# Memento
> сохраняем снимок состояния, чтобы откатывать
```python
  class Doc: 
      def __init__(self):
          self.text = ""
		  
      def save(self):
	      """ Возвращает снапшот документа """
          return self.text
		  
      def load(self, snap):
	      """ Документ загружается со снапшота """
          self.text = snap

  snap = doc.save()
  doc.text += "!"
  doc.load(snap)
```
  
# Observer
> подписчики реагируют на события объекта.
```js
// publisher
class Subject {  
	constructor() {  
		this.observers = []  // подписчики
	}  
  
	// подписаться = добавить себе саба
	subscribe(obs) {  
		this.observers.push(obs)  
	}  
  
    // уведомляем каждого подписчика данными
	notify(data) {  
		this.observers.forEach(o => o.update(data))  
	}  
}  
  
// подписчики / сабы / observer'ы
class Logger {
	update(msg) {  
		console.log("log:", msg)  
	}  
}  
  
class Metrics {  
	update(msg) {  
		console.log("metrics got:", msg)  
	}  
}  
  
// use
const subject = new Subject()  

// подписываются на все события
subject.subscribe(new Logger())  
subject.subscribe(new Metrics())  
  
// отправляем сабам строку
subject.notify("user created")
```
  
# State
> объект делегирует поведение текущему состоянию
```ts
// состояние как объект
interface State {  
	click(p: Player): void  
}  
  
// игрок знает, что у него будет состояние, но что это - не знает
class Player {  
	constructor(public state: State) 
	{ }  
  
	click() {  
		this.state.click(this)  // отдаём сами себя*
	}  
}

// имлементация состояний
class Stopped implements State {  
	click(p: Player) {  
		console.log("play")  
		p.state = new Playing()  // ради интереса трогаем другое состояние
	}  
}  
  
class Playing implements State {  
	click(p: Player) {  
		console.log("stop")  
		p.state = new Stopped()  // ради интереса трогаем другое состояние
	}  
}

// игрок получает состояние, их там вообще 2 внутри, но он про них ничего не знает
const player = new Player(new Stopped())  
  
player.click()  
player.click()
```
  
# Strategy
> подменяем алгоритм через общий интерфейс

```python
def pay(amount, strategy):  
	strategy.pay(amount)  
  
  
class Cash:  
	def pay(self, amount):  
		print("pay cash:", amount)  
  
  
class Card:  
	def pay(self, amount):  
		print("pay card:", amount)  
  
# выбор стратегии оплаты =)
pay(100, Cash())  
pay(200, Card())
```

- ранее был похожий паттер - декоратор, глобально они почти одинаковы, разница лишь в том, что в декораторе *идейно* мы добавляем поведение (декорируем), а у тут мы выбираем между 2 цельными поведениями/стратегиями
  
# Template Method
  > скелет алгоритма в базовом классе, шаги переопределяем

```ts
  abstract class Parser {
    parse() {
      this.read();
      this.transform();
    }
	
    abstract read(): void;
    abstract transform(): void;
  }
  
  class JsonParser extends Parser {
    read() { /* ... */ }
    transform() { /* ... */ }
  }
```
  - около дефолт, да? а эта группа паттернов почти вся такая...
  
# Visitor
> выносим операции из объектов, объекты принимают визитора
```ts
// объекты, которые только лишь принимают "посетителя"
  class Circle { 
    accept(v) { v.circle(this); } 
  }
  
  class Rect { 
    accept(v) { v.rect(this); } 
  }
  
  // посетитель
  const visitor = {
    circle: c => console.log(c),
    rect: r => console.log(r),
  };
  
  // job
  [new Circle(), new Rect()].forEach(x => x.accept(visitor));
```
  
# Null Object
> безопасная "пустая" реализация вместо `null`

```js
const NullLogger = { log() { /* no-op */ } };
const logger = maybeLogger || NullLogger;
  
logger.log("ok");
```
- нередко похоже на костыль, да? чаще всего оно таким и будет...
  
# Chain of Responsibility
> цепочка обработчиков до тех пор, пока кто-то не справится.
```ts
  const handlers = [h1, h2, h3];
  
  const handle = req => {
	// вообще это find_if где h(req) возвращает true/false при попытке хэндла
    const found_h = handlers.find(h => h(req));

	console.log(found_h 
		? "Handler found, job done" 
		: "failed to handle"
	)
  };
```
  
# Interpreter
> маленький язык с простым парсером/деревом вычислений
```python
class Number:  
	def __init__(self, value):  
		self.value = value  
  
	def interpret(self):  
		return self.value  
  
  
class Add:  
	def __init__(self, left, right):  
		self.left = left  
		self.right = right  
  
	def interpret(self):  
		return self.left.interpret() + self.right.interpret()  
  
  
expr = Add(Number(1), Number(2))  
  
print(expr.interpret())
```
- суть в том, что в `Number` можно было бы передать и какой-нибудь *hex* или *римскую цифру*, задача такого паттерна - интерпретировать операции/значения и прочее в соответствующие вещи из небольших языков в текущий язык программирования
	- это часто всякие sql (т.е. всякие ORM), regex или поприетарные микро языки
  
# Specification
> в теории это обёртки на предикаты/операции и прочее.. зачем? а затем, что в таких операциях может быть чуть больше логики, чем просто `and/or/else`
```js
class Spec {  
	and(other) {  
		return new AndSpec(this, other)  
	}  
}  
  
class Adult extends Spec {  
	check(u) {  
		return u.age >= 18  
	}  
}  
  
class HasDocs extends Spec {  
	check(u) {  
		return Boolean(u.passport)  
	}  
}  
  
class AndSpec extends Spec {  
	constructor(a, b) {  
		super()  
		this.a = a  
		this.b = b  
	}  
  
	check(u) {  
		return this.a.check(u) && this.b.check(u)  
	}  
}

const spec = new Adult().and(new HasDocs())  

spec.check({ age: 20, passport: true }) // true
```
