---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Отделяет доменные объекты от слоя хранения, переводя их в записи БД и обратно.

## Где помогает
- Доменные модели не должны знать про SQL/ORM детали.
- Тестируемый слой: маппер можно подменить.

## Мини-пример
```ts
class User{ constructor(public id:number, public name:string){} }
class UserMapper{
  toRow(u:User){ return { id:u.id, name:u.name }; }
  fromRow(r:any){ return new User(r.id, r.name); }
}
```

## Замечания
- Отличается от Active Record: сущности не знают про БД.
- Мапперы хорошо сочетаются с репозиториями/спецификациями.
