---
tags: [design-patterns, code-patterns]
aliases: [Полезные паттерны кода]
---

> Полезные приёмы вне классических GoF.

- **Service Locator** — контейнер, выдающий зависимости по имени/типу (неявная инъекция).
  ```ts
  const locator = new Map([["logger", console]]);
  const logger = locator.get("logger");
  logger.log("hi");
  ```
- **Repository** — слой, прячущий хранение/SQL за интерфейсом коллекции доменных объектов.
  ```python
  class UserRepo:
      def __init__(self, db):
          self.db=db
      def by_id(self, i):
          return self.db.query("select * from users where id=?", (i,))
  ```
- **Сущность-Атрибут-Значение (EAV)** — гибкая схема key-value на сущность, когда столбцы заранее неизвестны.
  ```ts
  const attrs = { "42": { color: "red", size: "L" } };
  const size = attrs["42"]["size"]; // "L"
  ```

- [Service Locator](service-locator.md)
- [Repository](repository.md)
- [EAV](eav.md)
