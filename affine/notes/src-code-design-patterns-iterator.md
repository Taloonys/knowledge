---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Единый способ обходить коллекцию, не зная её внутренностей.

## Где помогает
- Нужно пройтись по разным структурам (список, дерево, стрим) одинаково.
- Ленивая генерация элементов.

## Мини-пример
```js
const range = (a,b) => ({
  *[Symbol.iterator](){ for(let i=a;i<=b;i++) yield i; }
});
for (const x of range(1,3)) console.log(x);
```

## Замечания
- В JS/TS стандартный `Symbol.iterator`, в Python — `__iter__`/`__next__`.
- Итерируемые объекты удобно комбинировать с `map/filter`.
