---
tags: [design-patterns, code-patterns]
aliases: [Полезные паттерны кода]
---

> Полезные приёмы вне классических GoF.

# Service Locator
> технически это просто registry, только для сервисов
- а значит, тут создана неявная зависимость по некоторму ключу/полю
```ts
class ServiceLocator {  
	private static services = new Map<string, any>()  
  
	static register(name: string, service: any) {  
		this.services.set(name, service)  
	}  
  
	static get(name: string) {  
		return this.services.get(name)  
	}  
}  
  
  
ServiceLocator.register("logger", console)  
  
  
class UserService {  
	logger = ServiceLocator.get("logger")  
  
	create() {  
		this.logger.log("user created")  
	}  
}  
  
  
new UserService().create()
```
- registry - условно, мапа созданных объектов, а тут предполагается возможная доп. логика под сервисы, их инициализацию и т.п.
  
# Repository
> слой, прячущий хранение/SQL за интерфейсом коллекции доменных объектов
```python
  class UserRepo:
      def __init__(self, db):
          self.db = db
		  
      def by_id(self, i):
          return self.db.query("select * from users where id=?", (i,))
```
- orm ~ *repository* + *interpreter patterns* + some logic
  
# Сущность-Атрибут-Значение (EAV)
  > грубо говоря, это набор атрибутов заранее неизвестен, поэтому данные вместо табличного хранения предполагают хранение по строкам
  
```ts
const eav = [
	// т.е. природа атрибута и значений во всём списке далеко не одинаковая
	{ entity: "shirt", attr: "size", value: "L" },  
	{ entity: "shirt", attr: "color", value: "red" },  
	  
	{ entity: "laptop", attr: "cpu", value: "i7" },  
	{ entity: "laptop", attr: "ram", value: "16GB" },  
];  
  
function attrsOf(entity) {  
	return eav  
		.filter(r => r.entity === entity)  
		.reduce((o, r) => ({ 
			...o,
			 [r.attr]: r.value }
		), {});  
}  
  
console.log(attrsOf("shirt"));  
console.log(attrsOf("laptop"));

// output
// { size: 'L', color: 'red' }  
// { cpu: 'i7', ram: '16GB' }
```
- в примере выше 