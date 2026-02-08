> Интерфейс коллекции доменных объектов, скрывающий детали хранения/SQL.

## Где помогает
- Отделить бизнес-логику от конкретной БД/ORM.
- Тесты: легко подменить in-memory реализацией.

## Мини-пример
```python
class UserRepo:
    def __init__(self, db): self.db=db
    def by_id(self, uid): return self.db.query("select * from users where id=?", (uid,))
    def add(self, user): self.db.execute("insert into users values (?,?)", (user.id, user.name))
```

## Замечания
- В сочетании со спецификациями: `repo.find(spec)`.
- Не кладите внутрь бизнес-логику — только доступ к данным.
