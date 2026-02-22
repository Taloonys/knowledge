---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Запрос проходит по цепочке обработчиков, пока кто-то не обработает.

## Где помогает
- Middleware/фильтры: HTTP, CLI, пайплайны.
- Правила/валидации, которые можно включать/выключать.

## Мини-пример
```ts
type Handler = (req:any)=>boolean;
const h1:Handler = r => (r.type==="a") && console.log("a")===undefined;
const h2:Handler = r => (r.type==="b") && console.log("b")===undefined;
const chain=[h1,h2];
const handle=req=>{ for(const h of chain) if(h(req)) return; };
handle({type:"b"});
```

## Замечания
- Возвращайте явный результат (успех/передать дальше).
- Следите за порядком обработчиков.
