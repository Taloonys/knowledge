---
tags: [design-patterns, creational]
aliases: [Порождающие паттерны]
---

> Паттерны, которые отвечают за создание объектов без жёсткой привязки к конкретным типам.

# Factory
> По сути это единая точка создания объектов

> Любого рода фабрика используется практически везде, когда речь заходит о конструировании объектов, будь то GUI библиотеки, Gamedev с объектами на сцене, разные подключения к БД и т.п.

## Factory Method
> базовый класс делегирует `создание` в потомки; клиент знает только интерфейс продукта.
  ```python
  class Report: ...
  
  class PdfReport(Report): ...
  
  class ReportFactory:
      def make(self, kind): 
	      """Создаём через одно место разного рода объекты, тут это зависит от kind"""
          return PdfReport() if kind == "pdf" else Report()
		  
  rpt = ReportFactory().make("pdf") # просим у фабрики пдфный класс
  ```

## Abstract Factory
> та же фабрика, но весь "switch-case" для выбора, кого создаём, запрятан в систему типов.

 - Ниже пример через интерфейсы, если какой-то класс, не имплементит интерфейс, то мы его создать не сможем - т.е. умеем создавать только заданное семейство типов и объектов
```ts
  interface UI // указываем, каким семейством объектов хотим ограничить
  { 
    btn(): Button; 
    modal(): Modal; 
  }
  
  class MacUI implements UI
  { 
    btn() { return new MacBtn() }
    modal() { return new MacModal() } 
  }

  // принимаем только типы "семейства" UI 
  function render(ui:UI) { 
	  ui.btn().click();
	  ui.modal().open();  
  }
  
  render(new MacUI());
```
- ну и видим, что фабрика не только лишь для создания
  
# Builder
> для поэтапного создания объекта и/или когда мы хотим выбирать части объекта
	
  ```js
  class BurgerBuilder { 
  
	cheese() { 
      this.c = 'cheddar';
	  return this;
    } 
  
  patty(x) {
	this.p = x;
	return this;
  } 
  
  done() { 
    return {
      c: this.c,
      p: this.p
    }
  }
  
  }
  
  const burger = new BurgerBuilder()
	.patty('beef') // фактически после создания выбираем "начинку"
	.cheese()      // добавляем часть "начинки"
	.done();       // завершаем "конструирование"
  ```
  - есть такое понятие как `Fluent Build` когда методы объекта возвращают ссылку на себя же, чтобы можно красиво было писать конструктор через цепочку вызовов...
  
# Prototype
> создаём настроенный (обычно легковесный) объектик и просто копируем его для использования в нескольких местах мб

```js
  const proto = { 
    role: 'user', 
    perms: ['read'] 
  };
  
  const admin = { 
	...proto, 
	role: 'admin', 
	perms: [...proto.perms, 'write'] 
  };
```
  
# Singleton
 > создаём на всё время 1 единственный экземпляр класса, любое обращение к его `instance/GetInstance()` будет возвращать один и тот же объект, а не создавать новый
 
```cpp
class Singleton {
	static Singleton* instance_;    // static гарантирует, что объект всегда будет 1 на всю программу (ну почти) 
public:
	static Singleton* getInstance() // весь упор в подобный метод, который возвращает всегда один и тот же экземпляр
	{
		if (!instance) 
			instance_ = new Singleton();
		return *instance;
	};

	void doSth() const {
		std::cout << "Singleton do sth" << std::endl;
	};
}

auto& singleton = Singleton::getInstance();
singleton.someOperation();
```

- джуны взревут "антипаттерн", в реале - ну и ладно
- в теории да, если слишком на него уповать, то вот такие "очень важные" вещи могут просачивать по смыслу во многие классы, вытаскивать/разгребать/раскручивать смысловое спагетти зависимостей с таким классом потом будет очень неудобно
	- поэтому такие вещи должны как минимум: делать что-то одно и не очень большое
# Object Pool
> берём/возвращаем готовые объекты вместо создания каждый раз
 
```python
  pool = [Conn() for _ in range(5)] # вектор соединений
  
  def acquire():
    """ Клиенту отдаём одно из наших соединений"""
    return pool.pop() if pool else Conn()
	
  def release(c): 
    """ Возвращаем себе обратно в хранилище соединеней"""
    pool.append(c)
    
connection = pool.acquire()
# connection job...
pool.release(connection)
```

- очень частая вещь в геймдеве и вопросах оптимизации