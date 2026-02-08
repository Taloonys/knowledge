# Филосовия
> Создаётся некоторый пул, который содержит пачку инициализированных и готовых к использованю объектов, клиент просто периодически извлекает нужные объекты, использует и возвращает обратно в пул (вместо удаления).
* Нужно для повышения перфоманса, когда повторное создание или удаление объектов затратно. Мы просто вводим на такой случай концепт "переиспользования".
# Пример
```py
@dataclass
class DatabaseConnection:
    """Reusable объект - соединение с БД"""
    connection_id: int
    is_active: bool = True


class ConnectionPool:
    """Pool - управляет пулом соединений"""
    _instance: Optional['ConnectionPool'] = None
    _lock: Lock = Lock()
    
    def __new__(cls):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
                cls._instance._initialize()
            return cls._instance
    
    def _initialize(self):
        self._available: List[DatabaseConnection] = []
        self._in_use: List[DatabaseConnection] = []
        self._max_size = 5
        
        for i in range(self._max_size):
            self._available.append(DatabaseConnection(i))
    
    def acquire(self) -> Optional[DatabaseConnection]:
        """Получить соединение из пула"""
        if not self._available:
            return None  # или можно создать новое, если позволяет политика
        
        connection = self._available.pop()
        self._in_use.append(connection)
        return connection
    
    def release(self, connection: DatabaseConnection):
        """Вернуть соединение в пул"""
        if connection in self._in_use:
            self._in_use.remove(connection)
            connection.is_active = True  # сброс состояния
            self._available.append(connection)


#
# Client usage
#
pool = ConnectionPool()
conn = pool.acquire()

try:
    # работа с соединением
    print(f"Using connection {conn.connection_id}")
finally:
    # возвращаем в пул
    pool.release(conn)
```

# Вариации
* Пул с фиксированным максимальным кол-вом объектов
* Пул с динамическим кол-вом объектов
