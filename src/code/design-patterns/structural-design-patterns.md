---
tags: [design-patterns, structural]
aliases: [Структурные паттерны]
---

> Совокупность решений, объединяющих в одну "структуру" несколько сущностей или сервисов, cпособной на расширение и открытой к модификациям.

## Быстрый конспект
- **Adapter** — оборачиваем несовместимый интерфейс в нужный.
  ```ts
  class OldApi {
    run (x:number) { return x + 1; }
  }
  class Adapter {
    constructor(private api = new OldApi()) { /* empty */}
    do (x:number) { return this.api.run(x); }
  }
  ```
  
- **Bridge** — отделяем абстракцию от реализации, комбинируем свободно.
  ```python
  class Renderer: ...

  class Vector(Renderer):
      def draw(self,shape):
          print("draw", shape)

  class Shape:
      def __init__(self, r):
          self.r = r

  class Circle(Shape):
      def draw(self):
          self.r.draw("circle")

  Circle(Vector()).draw()
  ```
  
- **Composite** — дерево объектов, лист и контейнер имеют один интерфейс.
  ```js
  class Node{
    children = [];
    add(c) { this.children.push(c); }
    render() { 
      this.children
        .forEach(c => c.render?.()); 
    }
  }
  
  class Leaf{
    render() { console.log("leaf"); }
  }
  
  new Node().add(new Leaf()).render();
  ```
  
- **Decorator** — динамически добавляем поведение через обёртки.
  ```ts
  const withCache = (fn: (x: any) => any) => {
    const m = new Map();
	
    return x => {
      if (m.has(x)) 
        return m.get(x);
      const res = fn(x);
      m.set(x, res);
	  
      return res;
    };
  };
  ```
  
- **Facade** — простое API поверх сложной подсистемы.
  ```js
  const startApp = () => {
    connectDB();
    initCache();
    bootHttp();
  };
  ```
  
- **Proxy** — контролируем доступ или ленивую загрузку вместо реального объекта.
  ```python
  class LazyImg:
      def __init__(self,path):
          self.path = path
          self._real = None
		  
      def data(self): 
          if not self._real:
              self._real = open(self.path,'rb').read()
			  
          return self._real
  ```
  
- **Flyweight** — разделяем неизменяемое общее состояние между множеством объектов.
  ```js
  const glyphPool = new Map();
  const glyph = ch => glyphPool.get(ch) || 
                      glyphPool.set(ch, { ch })
                        .get(ch);
  ```
  
- **Dependency Injection** — зависимости передаются извне, объект сам их не создаёт.
  ```ts
  class Controller{
    constructor (private repo: { all(): any[] }) { }
    list() { return this.repo.all(); }
  }
  
  new Controller(new UserRepo());
  ```
  
- **Fluent Interface** — методы возвращают `this`, позволяя цепочки.
  ```js
  const q = new Query()
	  .select("id")
	  .where("active=1")
	  .limit(10);
  ```
  
- **Registry** — глобальная таблица сервисов/конфигураций.
  ```python
  registry = { "logger": ConsoleLogger() }
  
  def get(name):
      return registry[name]
  ```
  
- **Data Mapper** — слой, переводящий объекты домена в записи БД и обратно.
  ```ts
  class UserMapper{
    toRow (u: User) { 
      return { 
        id: u.id, 
        name: u.name 
      }; 
    }
	
    fromRow (r: any) { return new User(r.id, r.name); }
  }
  ```

- [Adapter](adapter.md)
- [Bridge](bridge.md)
- [Composite](composite.md)
- [Decorator](decorator.md)
- [Facade](facade.md)
- [Proxy](proxy.md)
- [Flyweight](flyweight.md)
- [Dependency Injection](dependency-injection.md)
- [Fluent Interface](fluent-interface.md)
- [Registry](registry.md)
- [Data Mapper](data-mapper.md)
