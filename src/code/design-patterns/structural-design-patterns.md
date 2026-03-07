---
tags: [design-patterns, structural]
aliases: [Структурные паттерны]
---

> Совокупность решений, объединяющих в одну "структуру" несколько сущностей или сервисов, cпособной на расширение и открытой к модификациям.

# Adapter
> есть 2 типа, они не особо друг с другом совместимы -> спавним прослойку для их совместимости
  ```ts
  class OldApi {
    run (x:number) { return x + 1; }
  }
  class Adapter {
    constructor(private api = new OldApi()) { /* empty */}
    do (x:number) { return this.api.run(x); }
  }
  ```
  
# Bridge
> отделяем абстракцию от реализации.. т.е. отделяем некоторый "движок" и подсовываем его потом 

```ts
interface Renderer {  // это всё же рендер круга, но скипнем длинны названия
	drawCircle(radius: number): void  
}  
  
class VectorRenderer implements Renderer {  
	drawCircle(radius: number) {  
		console.log(`Vector circle radius=${radius}`)  
	}  
}  
  
class RasterRenderer implements Renderer {  
	drawCircle(radius: number) {  
		console.log(`Raster circle radius=${radius}`)  
	}  
}  
  
  
// абстракция  
abstract class Shape {  
	protected renderer: Renderer  
  
	constructor(renderer: Renderer) {  
		this.renderer = renderer  
	}  
  
	abstract draw(): void  
}  
  
  
// имплементация от абстракции
class Circle extends Shape {
	// кушаем рендер и сохраняем его.. можно синтаксис сохранения*
	constructor(renderer: Renderer, private radius: number) {
		super(renderer)  // вызов Shape конструктора
	}  
  
	draw() {
		this.renderer.drawCircle(this.radius)  // рисуем, пользуясь рендером
	}  
}

// типы рендеров
const vector = new VectorRenderer()  
const raster = new RasterRenderer()  

// круг с конкретными реализациями рендера
const circle1 = new Circle(vector, 5)  
circle1.draw()
  
const circle2 = new Circle(raster, 10)  
circle2.draw()
```
  
# Composite
> дерево объектов, лист и контейнер имеют один интерфейс

```js
  class Node {
    children = []; // тут будет пачка листьев
    
    // добавляем листочек себе
    add(c) { this.children.push(c); } 

	// мы не умеем сами рендерить, поэтому просто у все листьев требуем рендер
    render() { 
      this.children
        .forEach(c => c.render?.()); 
    }
  }
  
  class Leaf {
    // тот самый листочек с рендером
    render() { console.log("leaf"); }
  }
  
  new Node()
	  .add(new Leaf())
	  .add(new Leaf())
	  .add(new Leaf())
	  .render(); // тут просто вызовем у всех добавленных листьев render()
```
  
# Decorator
> динамически добавляем поведение через обёртки
- частный случай - передавать callback'и куда либо
```python
def do_with(x, fn):  
	print(f"x = {x}")  
	
	res = fn(x)  
	print(f"job with x return - {res}")
	return res  
  
  
def square(x):    
	return x * x  
  
  
def double(x):    
	return 2 * x 
  
  
do_with(4, square) # хотим, чтобы do_with ещё и возводил в квадрат
do_with(4, double) # хотим, чтобы do_with ещё и удваивал
```
  * да, в питоне есть свой механизм декораторов, но он нас мало волнует сейчас
  
# Facade
> простое API поверх сложной подсистемы...
- как бы просто удобное api которые программисты и так постоянно делают или хотят делать
```js
  const startApp = () => {
    connectDB();
    initCache();
    bootHttp();
  };
```
  
# Proxy
> по-настоящему создаём объект только в момент того, когда его кто-то тыкает
- обычный `lazy loading`

```python
  class LazyImg:
      def __init__(self, path):
          self.path = path  # какие-то нужные нам метаданные
          self._real = None # но объекта пока нет
		  
      def data(self):
	      """ 
	      Функцию доступа. 
	      Пока нас никто не трогает - нас нет.
	      Когда нас кто-то трогает - создаёмся по-настоящему.
	      """
          if not self._real:
              self._real = open(self.path,'rb').read()
		
          return self._real # если мы уже живы, то просто access
```
  
# Flyweight
> разделяем общее состояние между уникальными объектами
- один не очень однозначный пример - мапа, где у нас общая память, но уникальные значения
	- где при обращении к элементу - если его нет, то он создаётся; а если он есть - отдаётся

```python
class TreeType:  
	def __init__(self, name, color):  
		self.name = name  
		self.color = color  
  
  
tree_types = {}  
  
def get_tree_type(name, color):  
	if name not in tree_types:  
		tree_types[name] = TreeType(name, color)
		
	return tree_types[name]  

  
class Tree:  
	def __init__(self, x, y, tree_type):  
		self.x = x            # координата х
		self.y = y            # координата у
		self.type = tree_type # вот тут мы храним информацию о дереве
  
#
# Все 3 дерева ссылаются на 1 "архитип"
#
trees = []  
  
oak = get_tree_type("Oak", "green")  
  
trees.append(Tree(10, 20, oak))  
trees.append(Tree(30, 40, oak))  
trees.append(Tree(50, 60, oak))
```
  
# Dependency Injection
> зависимости передаются извне, объект сам их не создаёт
- в примере ниже можно было бы представить, как `Controller` мог внутри себя хранить поле именно типа `UserRepo`, тогда он бы был жёстко с ним связан, это проблему мы решаем
- технически пример ещё связан с `type erasure` из-за использования `any`... но не суть...

```ts
class Controller {  
	constructor(private repo: { all(): any[] }) 
	{ }  
  
	list() {  
		return this.repo.all()  
	}  
}  
  
class UserRepo {  
	all() {  
		return ["Alice", "Bob"]  
	}  
}

class FakeRepo {  
	all() {  
		return ["test"]  
	}  
}  
  
const controller = new Controller(new UserRepo())
const controller = new Controller(new FakeRepo()) // легко можем и подменить
```
  
# Fluent Interface
- методы возвращают `this`, позволяя цепочки вызово методов
	- ничего кроме красоты и мб читабельности...
```js
  const q = new Query()
	  .select("id")
	  .where("active=1")
	  .limit(10);
```
  
# Registry
> глобальная таблица сервисов/конфигураций
```python
  registry = { "logger": ConsoleLogger() }
  
  def get(name):
      return registry[name]
```
  
# Data Mapper
> слой, переводящий объекты домена в записи БД и обратно
```ts
  class UserMapper {
    toRow (u: User) { 
      return { 
        id:   u.id, 
        name: u.name 
      }; 
    }
	
    fromRow (r: any) { 
	    return new User(r.id, r.name); 
	}
  }
```
