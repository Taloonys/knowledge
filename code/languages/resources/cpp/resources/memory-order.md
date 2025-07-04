> Это концепция, связанная с управлением видимостью и последовательностью операций чтения и записи в многопоточных программах. 
> Почти во всех операциях и механизмах синхронизации используется memory order, чтобы контролировать, как происходят взаимодействия между потоками относительно доступа к памяти.

На уровне процессоров и компиляторов операции чтения и записи могут быть переупорядочены для повышения производительности. Однако это может привести к неожиданному поведению в многопоточных программах, когда один поток не видит изменений, сделанных другим, или видит их в неправильном порядке. Чтобы предотвратить такие проблемы, используются различные модели синхронизации и типы **memory order**.

# memory_order_relaxed (расслабленный порядок)

| Зачем есть? | Операция выполняется без синхронизации с другими потоками. |
| --- | --- |
| Когда используется? | Атомарная операция просто выполняется, но не гарантирует видимость изменений для других потоков немедленно. |
| Примерный случай | Когда важна только атомарность операции, но не требуется синхронизация между потоками. |

```cpp
std::atomic<int> x(0);
x.store(10, std::memory_order_relaxed); 
// сохраним и пофиг на синхронизацию между потоками
```

# memory_order_acquire (порядок получения)

| Зачем есть? | Операция чтения с данным порядком гарантирует, что все предыдущие операции других потоков завершены перед выполнением потоком этой операции. |
| --- | --- |
| Когда используется? | Используется для чтения данных, гарантируя, что все последующие операции выполняются **после** того, как операция чтения завершена. |
| Примерный случай | При необходимости синхронизации потоков при чтении данных, которые могут быть модифицированы другим потоком. |

```cpp
std::atomic<bool> ready(false);
int data = 0;

void consumer() 
{
    // Ожидание, пока другой поток не установит ready в true
    while (!ready.load(std::memory_order_acquire))
    {
        // активное ожидание
    }
    
    // После acquire гарантируется, что мы видим изменения, сделанные до ready.store
    std::cout << "Data is: " << data << std::endl;
}

void producer() 
{
    data = 42;  // Запись данных
    ready.store(true, std::memory_order_release);  // Установка флага с release
}

int main() 
{
    std::thread t1(consumer);
    std::thread t2(producer);

    t1.join();
    t2.join();
    return 0;
}
```

# memory_order_release (порядок освобождения)

| Зачем есть? | Операция записи с **release** гарантирует, что все операции записи будут выполнены до этой операции записи. |
| --- | --- |
| Когда используется? | перед записью атомарной переменной с `memory_order_release` все предыдущие записи данных должны завершиться. |
| Примерный случай | при записи данных, которые будут читаться другим потоком с `memory_order_acquire`. |

```cpp
std::string value;
std::atomic<bool> value_present;

void produce() 
{
    value = "hello";
    value_present.store(true, std::memory_order_release);
    // ... 
}

void try_consume() 
{
    if (value_present.load(std::memory_order_acquire)) 
    {
        std::string  tmp = value;
    }
}

```

# memory_order_acq_rel (приобретение и освобождение)

| Зачем есть?         | Объединяет оба режима: **acquire** и **release**                                                                                          |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
|                     | Чтение с таким порядком ведёт себя как **acquire**, а запись - как **release**                                                            |
| Когда используется? | При выполнении операций, которые требуют как чтения (получения), так и записи (освобождения) данных в рамках синхронизации между потоками |
| Примерный случай    | см. выше…                                                                                                                                 |


```cpp
std::atomic<int> counter(0);

void Incrementer() 
{
    for (int i = 0; i < 10000; ++i) 
    {
		    // Атомарное увеличение с acq_rel
        counter.fetch_add(1, std::memory_order_acq_rel);
        // Такая операция гарантирует, что
        // 1. Все операции записи выполнены до модификации counter
        // 2. Все операции чтения в текущем потоке уже завершены
    }
}

int main() 
{
    std::thread t1(Incrementer);
    std::thread t2(Incrementer);

    t1.join();
    t2.join();

		// Здесь release гарантирует, что все операции записи завершены
    std::cout << "Final counter value: " << counter.load(std::memory_order_relaxed) 
              << std::endl;
    return 0;
}

```

# memory_order_seq_cst (последовательный порядок)

<aside>
💡

seq - sequential (последовательный)

cst - consistent (~согласосванный)

</aside>

| Зачем есть? | Это самый строгий и стандартный порядок. Гарантирует, что все атомарные операции в программе выполняются в одном и том же порядке для всех потоков. |
| --- | --- |
| Когда используется? | Когда нужна полная упорядоченность и синхронизация между потоками. Это самый простой для понимания и самый медленный вариант. |
| Примерный случай | см. выше… |

```cpp
std::atomic<int> x(0), y(0);

void thread1() 
{
		// Данное правило говорит, что все атомарные операции внутри 1 потока
		// .. будут идти ровно в том порядке, в котором они вызываются в этом потоке
    x.store(1, std::memory_order_seq_cst);
    int r1 = y.load(std::memory_order_seq_cst);
    std::cout << "Thread 1: y = " << r1 << std::endl;
}

void thread2() 
{
    y.store(1, std::memory_order_seq_cst);
    int r2 = x.load(std::memory_order_seq_cst);
    std::cout << "Thread 2: x = " << r2 << std::endl;
}

int main() 
{
		// .. но то, какой из этих потоков будет выполняться первым - никем не гарантируется
    std::thread t1(thread1);
    std::thread t2(thread2);
   
	  // т.е. результат 

    t1.join();
    t2.join();

    return 0;
}

```