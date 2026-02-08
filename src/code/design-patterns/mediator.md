---
tags: [code, design-patterns]
aliases: [Где помогает]
---

> Компоненты общаются через посредника, уменьшая прямые связи.

## Где помогает
- UI формы: поля уведомляют медиатор, он решает, что включать/выключать.
- Чат/шина: участники не знают друг о друге.

## Мини-пример
```python
class Chat:
    def __init__(self): self.users=[]
    def send(self,msg,me): 
        for u in self.users:
            if u!=me: u.receive(msg)
class User:
    def __init__(self, chat): self.chat=chat; chat.users.append(self)
    def send(self,msg): self.chat.send(msg,self)
    def receive(self,msg): print(msg)
```

## Замечания
- Упрощает тестирование и замену участников.
- Медиатор может стать "божественным объектом" — держите логику скромной.
