---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Делегируем поведение объекту-состоянию; переключение состояния меняет логику.

## Где помогает
- Автоматы: двери, банкоматы, UI формы, игры.
- Избегать большого `switch` по статусу.

## Мини-пример
```ts
interface State{ next():State; act():void; }
class Closed implements State{
  next(){ return new Open(); }
  act(){ console.log("lock"); }
}
class Open implements State{
  next(){ return new Closed(); }
  act(){ console.log("open"); }
}
class Door{ constructor(public s:State){} click(){ this.s=this.s.next(); this.s.act(); } }
new Door(new Closed()).click();
```

## Замечания
- Состояния лучше делать неизменяемыми и мелкими.
- Иногда достаточно стратегий + поле `status`; выбирайте проще.
