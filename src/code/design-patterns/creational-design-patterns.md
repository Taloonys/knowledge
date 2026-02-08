> Паттерны, которые отвечают за создание объектов без жёсткой привязки к конкретным типам.

## Быстрый конспект
- **Factory Method** — базовый класс делегирует `make()` в потомки; клиент знает только интерфейс продукта.
  ```python
  class Report: ...
  class PdfReport(Report): ...
  class ReportFactory:
      def make(self, kind):  # small switch hidden here
          return PdfReport() if kind == "pdf" else Report()
  rpt = ReportFactory().make("pdf")
  ```
- **Abstract Factory** — набор фабричных методов, выдающих согласованные продукты.
  ```ts
  interface UI { btn(): Button; modal(): Modal; }
  class MacUI implements UI { btn(){return new MacBtn()} modal(){return new MacModal()} }
  function render(ui:UI){ ui.btn().click(); ui.modal().open(); }
  render(new MacUI());
  ```
- **Builder** — поэтапная сборка сложного объекта, шаги скрыты в билдерах.
  ```js
  class BurgerBuilder { cheese(){this.c='cheddar';return this;} patty(x){this.p=x;return this;} done(){return {c:this.c,p:this.p}} }
  const burger = new BurgerBuilder().patty('beef').cheese().done();
  ```
- **Prototype** — копируем уже настроенный объект.
  ```js
  const proto = { role:'user', perms:['read'] };
  const admin = { ...proto, role:'admin', perms:[...proto.perms,'write'] };
  ```
- **Singleton** — одно состояние на процесс.
  ```python
  class Config: _one=None
      def __new__(cls): cls._one=cls._one or super().__new__(cls); return cls._one
  ```
- **Object Pool** — берём/возвращаем готовые объекты вместо создания каждый раз.
  ```python
  pool=[Conn() for _ in range(5)]
  def acquire(): return pool.pop() if pool else Conn()
  def release(c): pool.append(c)
  ```

* [Fabric methods](fabric-family.md)
* [Builder](builder.md)
* [Prototype](prototype.md)
* [Singleton](singleton.md)
- [Object pool](object-pool.md)
- Fluent Builder
