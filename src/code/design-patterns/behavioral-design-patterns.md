> Как объекты общаются и передают работу друг другу.

## Быстрый конспект
- **Command** — действие как объект: ставим в очередь, логируем, отменяем.
  ```ts
  class TurnOn {
    constructor(private d: { on(): void }) {}
    exec() { this.d.on(); }
  }
  const q = [new TurnOn(light)];
  q.forEach(cmd => cmd.exec());
  ```
- **Iterator** — единый интерфейс обхода без знания внутренностей.
  ```js
  const coll = {
    items: [1, 2, 3],
    *[Symbol.iterator]() {
      for (const i of this.items) yield i;
    },
  };
  for (const x of coll) {
    /* ... */
  }
  ```
- **Mediator** — общение компонентов через посредника, меньше связей между ними.
  ```python
  class Chat: 
      def __init__(self):
          self.us=[]
      def send(self,msg,me):
          for u in self.us:
              if u!=me:
                  u.rec(msg)
  class User:
      def __init__(self,chat):
          self.c=chat
          chat.us.append(self)
      def send(self,m):
          self.c.send(m,self)
      def rec(self,m):
          print(m)
  ```
- **Memento** — сохраняем снимок состояния, чтобы откатывать.
  ```python
  class Doc: 
      def __init__(self):
          self.text=""
      def save(self):
          return self.text
      def load(self, snap):
          self.text=snap

  snap = doc.save()
  doc.text += "!"
  doc.load(snap)
  ```
- **Observer** — подписчики реагируют на события объекта.
  ```js
  class Event {
    subs = [];
    emit(x) { this.subs.forEach(f => f(x)); }
    on(f) { this.subs.push(f); }
  }
  ev.on(console.log);
  ev.emit("hi");
  ```
- **State** — объект делегирует поведение текущему состоянию.
  ```ts
  class Door {
    constructor(public state: State) {}
    click() { this.state = this.state.next(); this.state.act(); }
  }
  class Closed implements State {
    act() { console.log('lock'); }
    next() { return new Open(); }
  }
  class Open implements State {
    act() { console.log('open'); }
    next() { return new Closed(); }
  }
  new Door(new Closed()).click();
  ```
- **Strategy** — подменяем алгоритм через общий интерфейс.
  ```python
  def pay(amount, strategy): strategy(amount)
  pay(100, lambda a: print(f"cash {a}"))
  ```
- **Template Method** — скелет алгоритма в базовом классе, шаги переопределяем.
  ```ts
  abstract class Parser {
    parse() {
      this.read();
      this.transform();
    }
    abstract read(): void;
    abstract transform(): void;
  }
  class JsonParser extends Parser {
    read() { /* ... */ }
    transform() { /* ... */ }
  }
  ```
- **Visitor** — выносим операции из объектов, объекты принимают визитора.
  ```ts
  class Circle { accept(v){ v.circle(this); } }
  class Rect { accept(v){ v.rect(this); } }
  const visitor = {
    circle: c => console.log(c),
    rect: r => console.log(r),
  };
  [new Circle(), new Rect()].forEach(x => x.accept(visitor));
  ```
- **Null Object** — безопасная "пустая" реализация вместо `null`.
  ```js
  const NullLogger = { log(){ /* no-op */ } };
  const logger = maybeLogger || NullLogger;
  logger.log("ok");
  ```
- **Chain of Responsibility** — цепочка обработчиков до тех пор, пока кто-то не справится.
  ```ts
  const handlers = [h1, h2, h3];
  const handle = req => {
    const handler = handlers.find(h => h(req));
    if (handler) handler(req);
  };
  ```
- **Interpreter** — маленький язык с простым парсером/деревом вычислений.
  ```python
  def eval_expr(tokens):
      left, op, right = tokens
      if op == "+":
          return left + right
      return left - right

  eval_expr([1, "+", 2])
  ```
- **Specification** — предикаты скомбинированные через `and/or/not`.
  ```js
  const adult = u => u.age >= 18;
  const hasDocs = u => Boolean(u.passport);
  const canEnter = u => adult(u) && hasDocs(u);
  ```

## Подробнее
- [Command](command.md)
- [Iterator](iterator.md)
- [Mediator](mediator.md)
- [Memento](memento.md)
- [Observer](observer.md)
- [State](state.md)
- [Strategy](strategy.md)
- [Template Method](template-method.md)
- [Visitor](visitor.md)
- [Null Object](null-object.md)
- [Chain of Responsibility](chain-of-responsibility.md)
- [Interpreter](interpreter.md)
- [Specification](specification.md)
