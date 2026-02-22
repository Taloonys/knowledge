---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Новые операции выносятся в отдельные объекты, которые «посещают» элементы.

## Где помогает
- Нужно добавить поведение для набора разнотипных объектов, не меняя их классы.
- AST обход, экспорт в разные форматы, валидации.

## Мини-пример
```ts
class Circle{ accept(v){ v.circle(this); } }
class Rect{ accept(v){ v.rect(this); } }
const printer = { circle: c => console.log("circle"), rect:r=>console.log("rect") };
[new Circle(), new Rect()].forEach(x=>x.accept(printer));
```

## Замечания
- Добавлять новый тип элемента — дорого (правим всех визиторов).
- Добавлять новое действие — дёшево (пишем нового визитора).
