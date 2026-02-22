---
tags: [code, languages, cpp]
aliases: [Some resources]
---

# Some resources
* [Многопоточность - C++ course notes](https://cpp-kt.github.io/cpp-notes/26_multithreading.html)
* [Многопоточность и Thread Pool в C++](https://habr.com/ru/articles/738250/)
* [Common multithreading](../../common/multithreading.md)
* [OS Threads](../../../common/threads.md)

# std::thread
> Реализация потоков в С++

```cpp
std::thread t1([]{ 
	std::cout << "Do sth" << std::endl;
});
// На потоке t1 будет выполнена лямбда
```

* если поток joinable (ассоциирован и создан в живом потоке), но при этом не `detach()`'ed и не `join`'ed -> в момент своего завершения вызовет `std::terminate()`
## join()
> Блокирует вызывающий поток, пока созданный поток не завершит свою работу

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
# std::mutex
[mutex locking](mutex.md)

# std::condition_variable
> Определённый примитив стандарта, позволяющий пробуждать потоки при выполнении “определённого условия”.

# memory order
[memory order](memory-order.md)
# Если вдруг показалось, что потоки - конец и что всё наконец-то понял, то кушай:

```cpp
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
