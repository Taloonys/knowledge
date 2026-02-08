---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Объект-издатель оповещает подписчиков о событиях.

## Где помогает
- UI/event bus/webhooks.
- Отделить источник данных от реакций на него.

## Мини-пример
```js
class EventBus{
  subs=[]; on(fn){this.subs.push(fn);}
  emit(x){this.subs.forEach(fn=>fn(x));}
}
const bus=new EventBus();
bus.on(msg=>console.log("got",msg));
bus.emit("hi");
```

## Замечания
- Не забывайте отписки, чтобы не копить утечки.
- В асинхронном коде используйте очереди/дебаунсы, если событий много.
