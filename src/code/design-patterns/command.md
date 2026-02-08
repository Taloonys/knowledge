> Заворачиваем действие и параметры в объект, чтобы вызывать позже, ставить в очередь или отменять.

## Где помогает
- Логи/очереди действий, макросы.
- Undo/redo: храним историю команд.

## Мини-пример
```ts
interface Command{ exec():void }
class TurnOn implements Command{
  constructor(private dev:{on():void}){}
  exec(){ this.dev.on(); }
}
const history:Command[]=[new TurnOn(light)];
history.forEach(c=>c.exec());
```

## Замечания
- Команда может содержать `undo()`.
- Хранилище команд = точка расширения для ретраев, аудита, очередей.
