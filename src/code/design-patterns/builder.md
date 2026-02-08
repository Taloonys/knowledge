---
tags: [code, design-patterns]
aliases: [builder]
---

> Собираем сложный объект по шагам, храним промежуточное состояние внутри билдера.

Мини-пример (JS):
```js
class PizzaBuilder {
  cheese(x){ this._cheese=x; return this; }
  size(x){ this._size=x; return this; }
  bake(){ return { cheese:this._cheese||'mozz', size:this._size||'m' }; }
}
const pizza = new PizzaBuilder().size('l').cheese('cheddar').bake();
```

Идея:
- Шаги задаются методами билдера, можно вызывать в любом порядке/наборе.
- `build()`/`bake()` возвращает готовый объект и может сбрасывать внутреннее состояние (по необходимости).
- Director (если нужен) знает порядок шагов и дергает билдер; Client дергает только билдер.
