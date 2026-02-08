> Сохраняем снимок состояния объекта, чтобы откатывать.

## Где помогает
- Undo/redo в редакторах.
- Чекпоинты в длинных процессах.

## Мини-пример
```python
class Doc:
    def __init__(self): self.text=""
    def save(self): return self.text
    def load(self, snap): self.text=snap

doc=Doc(); doc.text="a"
snap=doc.save()
doc.text+="b"
doc.load(snap)  # снова "a"
```

## Замечания
- Снимок стоит хранить неизменяемым.
- Следите за размером снимков (инкрементальные дельты/компрессия).
