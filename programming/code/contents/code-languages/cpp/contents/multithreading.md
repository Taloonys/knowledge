--- 

# Some resources

[Многопоточность - C++ course notes](https://cpp-kt.github.io/cpp-notes/26_multithreading.html)

[Многопоточность и Thread Pool в C++](https://habr.com/ru/articles/738250/)

# Немного определений

## Base terms

> Многопоточность была ещё в ПЭВМ (ПК), где был всего 1 процессор (и 1 поток). Как она реализовывалась? - был некоторый механизм, который в какие-то моменты времени переключался на другую задачу, быстренько просматривался статус или ещё что-то и происходило переключение обратно.
> 
- Подобного рода имитация параллельности можно именовать как **псевдопараллельное выполнение**.
- **Многозадачность** - способность среды выполнять параллельно или псевдопараллельно (например асинхронщина) несколько задач
- **Многопоточность** - способность окружения выполнять в одном контексте несколько задач параллельно за счёт использования нескольких потоков
- **Процесс** - некоторая программа, которая может содержать в себе:
    - потоки
    - открытые дескрипторы (сокеты, файлы и т.п.)
    - дочерние процессы
    - адресное пространство
    - и т.п.

## **Поток**

> В контекста программирования: поток - *независимая последовательность выполнения инструкций внутри одного процесса с общей памятью, но собственными регистрами и стеком*.
> 

<aside>
💡 Чуть больше про потоки:

[Threads](threads.md)

</aside>

<aside>
💡

Важно понимать, что без специальных механизмов синхронизации у нас нет никакой возможности узнать, какой поток какое состояние имеет в данный момент времени или же, что он выполняет.

</aside>

---

# std::thread

> Реализация потоков в С++
> 

```cpp
std::thread t1([]{ 
	std::cout << "Do sth" << std::endl;
});
// На потоке t1 будет выполнена лямбда
```

<aside>
💡

Деструктор std::thread вызывает terminate()

</aside>

## join()

> Блокирует вызывающий поток, пока созданный поток не завершит свою работу
> 

```cpp
std::thread t1([]{ 
	std::cout << "Do sth" << std::endl;
});
// мы, как вызывающий поток, не сможем вызвать принт ниже, 
// пока t1 не напечатает свой принт
t.join(); 

std::cout << "finished" << std::endl;
```

## detach()

> Позволяет отделить вызываемый поток от вызывающего, позволяя тому существовать самостоятельно
> 

```cpp
std::thread t1([]{
	for (int i = 0; i < 150; ++i)
	{
		std::cout << "Did " << i << " cycle\n";
	}
});

t1.detach();
std::cout << "Main thread finished" << std::endl;
// В итоге получим, что основной поток закончил свою работу где-то посреди спама чисел
// ... или ещё до него
```

---

# std::mutex

[mutex](mutex.md)

---

# std::atomic

## atomic is…

> Это специальный объект, воплощающий специальные инструкции атомарности для указанного передаваемого объекта.
Атомарная операция - некоторая неделимая операция, промежуточнное состояние которой другие потоки не способны увидеть.
> 

Т.е. разные потоки не могут видеть промежуточный результат выполнения какого-нибудь, условно, сложения, они могут видеть только “начальное состояние” и “конечный результат”.

<aside>
💡

Инструкции по обеспечения атомарности для процессора зависят от архитектуры. В разных архитектурах атомарность описана по-разному.

</aside>

```cpp
static int v1 = 0;
static std::atomic<int> v2 { 0 };

int add_v1() 
{
    return ++v1;
    /* Generated x86-64 assembly:
        mov     eax, DWORD PTR v1[rip]       -> read
        add     eax, 1                       -> modify
        mov     DWORD PTR v1[rip], eax       -> write
    */
    // Т.е.  в этом цилке операций read-modify-write, может вдруг выполниться
    // другой поток, что в целом приведёт к UB
}

int add_v2() 
{
    return v2.fetch_add(1);
    /* Generated x86-64 assembly:
        mov        eax, 1                               -> only modify
        lock xadd  DWORD PTR _ZL2v2[rip], eax
        ! Операция lock представляет собой гарантию на уровне процессоров
        ! что к текущей кэш линии, где лежит v2, может обратиться только 1 ядро
        ! * Кэш линия - просто система кэширования данных в процессоре
        ! * * линию здесь можно ассоциировать с конвеером закэшированных данных
    */
}
```

## spinlock

> Механизм синхронизации, который используется для защиты критических секций в многопоточке. 
> Работает по принципу “while(true) { спим? → да → крутимся дальше } … { спим? → Неа, вставай давай → выход из while(true) → делаем дело”
> 

<aside>
💡

Не забываем, что такое “активное ожидание” удерживает процессорное время в этом потоке, ну т.е. в этом потоке процессор только ждёт и всё.

</aside>

```cpp
#include <atomic>

class spinlock final
{
    std::atomic_flag locked = ATOMIC_FLAG_INIT;
    
public:
    void lock() 
    {
		    //
        // Пытаемся установить флаг, пока он уже установлен
        //
        
        while (locked.test_and_set(std::memory_order_acquire))
        {
            // активное ожидание, "крутимся", пока флаг не освободится
        }
    }

    void unlock() 
    {
        locked.clear(std::memory_order_release);  // Освобождаем флаг
    }
};
```

<aside>
💡

Использовать лишь когда:

- Ожидается, что блокировка будет удерживаться очень короткий промежуток времени (например, несколько инструкций).
- Затраты на переключение контекста (например, при использовании обычного мьютекса, который может переводить поток в состояние ожидания) выше, чем затраты на "кручение" в цикле.
    - (если ожидается, что блокировка будет освобождена почти мгновенно, то затраты на ожидание через спинлок могут оказаться ниже, чем затраты на переход потока в спящий режим и его последующее пробуждение)
    - * mutex усыпляет поток, а spinlock заставляет процессор “активно ждать”
</aside>

---

# std::condition_variable

> Определённый примитив стандарта, позволяющий пробуждать потоки при выполнении “определённого условия”.

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <deque>
#include <condition_variable>

using lock_guard_t = std::lock_guard<std::mutex>;
using uniq_guard_t = std::unique_guard<std::mutex>;

template <typename T>
struct concurrent_queue 
{
    void push(T value) 
	  {
        uniq_guard_t lg(m);
        q.push_back(std::move(value));
        lg.unlock();
        
        // Это уведомляет один из потоков, ожидающих в pop, о том, 
        // что элемент добавлен и его можно забрать.
        cv.notify_one();  
    }
    
    T pop() 
    {
        uniq_guard_t ul(m);
        
        // Заставляет поток ожидать, пока условие (очередь не пуста) не выполнится.
        // Когда условие выполняется, поток продолжает выполнение и извлекает 
        // .. элемент из очереди.
        cv.wait(ul, [this]{ 
	        return !q.empty(); 
	      });
	      
        T result = q.front();
        q.pop_front();
        return result;
    }

    bool empty() const 
    {
        lock_guard_t lg(m);
        return q.empty();
    }

private:
    mutable std::mutex m;
    std::deque<T> q;
    std::condition_variable cv;  // Условная переменная
};
```

<aside>
💡

У него есть несколько значимых функций:

- wait - поток начинает спать
- notify_one - пробуждает один поток
    - Пробуждает только тот 1 поток, который вызвал  wait(), какой именно из пачки потоков будет выбран - неизвестно, стандарт ничего в этом случае явно не гарантирует
- notify_all - пробуждает все потоки (все ожидающие потоки, у которых есть wait)
</aside>

<aside>
💡

`wait` принимает в качестве аргумента `unique_lock`. Это нужно, чтобы между `unlock` и вызовом `wait` другой поток ничего не испортил. 

</aside>

---

# memory order

[memory order](memory-order.md)

---

# Если вдруг показалось, что потоки - конец и что всё наконец-то понял, то кушай:

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <queue>
#include <functional>
#include <mutex>
#include <condition_variable>
#include <future>

class ThreadPool final
{
public:
    ThreadPool(size_t numThreads);
    ~ThreadPool();

    // Добавление новой задачи в очередь
    template<class F, class... Args>
    auto enqueue(F&& f, Args&&... args) 
		    -> std::future<typename std::result_of<F(Args...)>::type>;

private:
    // Вектор рабочих потоков
    std::vector<std::thread> workers;
    // Очередь задач
    std::queue<std::function<void()>> tasks;

    // Синхронизация
    std::mutex queueMutex;
    std::condition_variable condition;
    bool stop;

    // Рабочая функция для потоков
    void worker();
};

ThreadPool::ThreadPool(size_t numThreads) 
		: stop(false) 
{
    for (size_t i = 0; i < numThreads; ++i) 
    {
        workers.emplace_back([this]{ 
		        this->worker(); 
		    });
    }
}

ThreadPool::~ThreadPool() 
{
    {
        std::unique_lock<std::mutex> lock(queueMutex);
        stop = true;
    }
    
    condition.notify_all();
    for (std::thread &worker : workers) 
    {
        worker.join();
    }
}

void ThreadPool::worker() 
{
    while (true) 
    {
        std::function<void()> task;
        
        {
            std::unique_lock<std::mutex> lock(queueMutex);
            condition.wait(lock, [this]{ 
		            return stop || !tasks.empty();
		        });
		        
            if (stop && tasks.empty())
            {
                return;
            }
            
            task = std::move(tasks.front());
            tasks.pop();
        }
        
        task();
    }
}

template<class F, class... Args>
auto ThreadPool::enqueue(F&& f, Args&&... args) 
		-> std::future<typename std::result_of<F(Args...)>::type>
{
    using return_type = typename std::result_of<F(Args...)>::type;

    auto task = std::make_shared<std::packaged_task<return_type()>>(
        std::bind(std::forward<F>(f), std::forward<Args>(args)...)
    );

    std::future<return_type> result = task->get_future();
    
    {
        std::unique_lock<std::mutex> lock(queueMutex);
        tasks.emplace([task]() { (*task)(); });
    }
    
    condition.notify_one();
    return result;
}

int main() 
{
    ThreadPool pool(4);

    auto result1 = pool.enqueue([] { return 1; });
    auto result2 = pool.enqueue([] { return 2 + 2; });

    std::cout << result1.get() << std::endl;  // Выведет: 1
    std::cout << result2.get() << std::endl;  // Выведет: 4

    return 0;
}

```