# Mutex is…

> **Mutual exclusion (взаимное исключение)** - механизм синхронизации потоков, позволяющий защитить данные, используемые одним потоком, от воздействия других потоков.
- lock()
    > Захватывает поток, т.е. поток, который вызывает этот метод, имеет эксклюзивные права на выполнение кода, другие потоки не имеют права вмешиваться.
- unlock()
    > Противоположно lock(), освобождает поток от захвата.

```cpp
std::mutex mtx;                         // Описатель взаимной блокировки
int number = 0; 
  
void Increment()
{ 
    mtx.lock();                         // сюда зайдут и t1 и t2, но мы не знаем точно кто и когда
    
    for (int i = 0; i < 1000000; i++)
    { 
        number++;                       // какая-то операция на изменение данных
    } 
      
    mtx.unlock();                       // поток, что успел выполнить код, позволяет другому сделать своё дело
} 
  
int main() 
{ 
    std::thread t1(Increment); 
    std::thread t2(Increment); 
      
    t1.join(); 
    t2.join(); 
      
    std::cout << "Number after execution of t1 and t2 is "<< number; 
      
    return 0; 
} 

```


mutex, как тип внутри, представляет собой совокупность атомарных операций, может использоваться для многих контекстов исполнения, одновременно. 
Он как бы является описателем блокировки или же можно представить его как std::atomic_bool, где потоки постоянно спрашивают, могут ли она щас выполняться или нет:

- какой-то из потоков зашёл в код, переключил мьютекс на “используется”
- другие потоки зашли в код, спросили “занят ли кем-то мьютекс
- ждут пока мьютекс “освободят
- когда мьютекс освобождается → какой-то следующий 1 поток успевает залететь
- ... всё по новой

# Race condition
> Основной задачей мьютекс является предотвращение такого undefined behaviour (UB) как **race condition (состояние гонки)**, когда данные одним потоком могут меняться, а другим в этот же момент считываться.

# mutex/lock granularity
> Это концепция, описывающая, объём кода, защищённых мьютексом…

## Coarse-grained locking
> При грубой гранулярности мьютекс защищает огромный кусок кода

```cpp
std::mutex mtx;
std::array<int, 1000> accounts;

void Transfer(size_t to, size_t from, int amount) 
{
    mtx.lock();
    
    if (accounts[from] < amount) 
    {
        m.unlock();
        throw std::runtime_error("insufficient funds");
    }
    
    accounts[from] -= amount;
    accounts[to]   += amount;
    
    mtx.unlock();
}
```

Может значительно уменьшить производительность в многопоточных приложениях, поскольку потоки вынуждены ждать друг друга, даже если они работают с разными частями данных. Однако код куда проще понять и контроллировать.

## Fine-grained locking
> При тонкой гранулярности используется несколько мьютексов, защищающих как можно меньшие куски кода…

```cpp
struct account {
    std::mutex  mtx;
    int32_t     balance;
};

using lock_guard_t = std::lock_guard<std::mutex>;

std::array<account, 1000> accounts;

void Transfer(size_t to, size_t from, int amount) 
{
    lock_guard_t lg_from(accounts[from].mtx);
    lock_guard_t lg_to(accounts[to].mtx);
    
    if (accounts[from].balance < amount) 
    {
        throw std::runtime_error("insufficient funds");
    }
    
    accounts[from].balance -= amount;
    accounts[to].balance   += amount;
}
```

Может значительно улучшить производительность за счёт лучшей параллелизации, но цена этого — усложнение кода и потенциальные проблемы с синхронизацией, такие как deadlock.

# deadlock
> Это ситуация, когда два потока пытаются захватить два мьютекса в разном порядке, из-за чего могут заблокировать друг друга. Лучше посмотреть пример:

```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx1;
std::mutex mtx2;

using namespace std::chrono_literals; 			// * для сокращённой записи времени

void thread1() 
{
    std::lock_guard<std::mutex> lock1(mtx1);
    std::this_thread::sleep_for(50ms);       		// Имитация работы

    //
    // Поток 1 пытается захватить mtx2, но он уже захвачен потоком 2, 
    // поэтому поток 1 блокируется, ожидая освобождения mtx2
    //
    std::lock_guard<std::mutex> lock2(mtx2);

    std::cout << "Thread 1 has locked both mutexes.\n";
}

void thread2() 
{
    std::lock_guard<std::mutex> lock1(mtx2);
    std::this_thread::sleep_for(50ms);       		// Имитация работы

    //
    // Поток 2 пытается захватить mtx1, но он уже захвачен потоком 1, 
    // поэтому поток 2 блокируется, ожидая освобождения mtx1
    //
    std::lock_guard<std::mutex> lock2(mtx1);

    std::cout << "Thread 2 has locked both mutexes.\n";
}

int main() 
{
    std::thread t1(thread1);
    std::thread t2(thread2);
    
    //
    // В итоге оба потока ждут освобождения друг друга
    // т.е. они друг друга заблокировали...
    //

    t1.join();
    t2.join();

    return 0;
}
```

Для решения подобного рода проблемы, есть, например, std::scoped_lock

# lock_guard
> Как и для любого объекта, у которого есть паттерн взаимодействия “открыть - закрыть”, у мьютекса есть обёртка RAII

[RAII](raii.md)

**Особенности:**
- Не может управлять состоянием мьютекса (например, блокировать или разблокировать его).
- Используется для простых случаев, когда мьютекс должен быть захвачен на время жизни объекта.
- Не поддерживает отложенную блокировку и передачу владения мьютексом.

```cpp
//
// Пример ранее можно переписать так:
//

std::mutex mut;

void Increment()
{ 
    std::lock_guard lock(mut);
    
    for (int i = 0; i < 1000000; i++)
    { 
        number++;
    } 
}
```
💡

В отличие от unique_lock, который будет дальше, этот способен только лочить 1 мьютекс при создании и разлочивать при уничтожении, и только эти действия в эти моменты, т.е. областью применения такой RAII обёртки является какая-нибудь область видимости одной функции

## unique_lock
> Это болеее прокаченная версия lock_guard, позволяющая когда угодно разблокировать и блокировать мьютекс, также владение мьютексом можно передавать.

```cpp
std::mutex mtx;
void Critical_section()
{
    std::unique_lock<std::mutex> lock(mtx);   	// Захват мьютекса

    //
    // Код в критической секции
    //

    lock.unlock(); 				// Явная разблокировка мьютекса

    //
    // Некритический код
    //

    lock.lock(); 				// Повторная блокировка мьютекса

    //
    // Снова критический код
    //
}
```

**Особенности:**

- Поддерживает отложенную блокировку (можно создать объект, не блокируя мьютекс сразу).
- Позволяет вручную блокировать и разблокировать мьютекс в течение времени жизни объекта.
- Поддерживает передачу владения мьютексом другому `unique_lock`.
- Требует немного больше накладных расходов по сравнению с `lock_guard` из-за дополнительной гибкости.

## scoped_lock (C++17)
> Может захватывать **несколько мьютексов сразу**, **предотвращая deadlock**. Он используется в ситуациях, когда необходимо захватить несколько мьютексов одновременно, и выполняет эту операцию **атомарно**.

**Особенности:**

- Захватывает несколько мьютексов атомарно, что предотвращает возникновение deadlock.
- Не поддерживает отложенную блокировку или управление состоянием мьютекса.
- Очень удобен для использования в ситуациях с несколькими мьютексами.

```cpp
std::mutex mtx1, mtx2;
void critical_section() 
{
    std::scoped_lock lock(mtx1, mtx2);
    //
    // <-- Код в критической секции, защищённый двумя мьютексами
    //
}
```
💡
Вспоминая пример из главы про **deadlock**, можно переписать его теперь вот так:

```cpp
std::mutex mtx1;
std::mutex mtx2;

void thread1() 
{
    std::scoped_lock lock(mtx1, mtx2); 				// Захват обоих мьютексов атомарно
    std::cout << "Thread 1 has locked both mutexes.\n";
}

void thread2() 
{
    std::scoped_lock lock(mtx1, mtx2); 				// Захват обоих мьютексов атомарно
    std::cout << "Thread 2 has locked both mutexes.\n";
}

// int main...
```

## shared_mutex & shared_lock
> shared_mutex предназначен для ситуации, когда **лишь 1 поток может записывать данные**, а  **остальные потоки могут только лишь читать эти данные.**

```cpp
#include <iostream>
#include <thread>
#include <shared_mutex>
#include <map>
#include <string>

std::shared_mutex cache_mutex;
std::map<int, std::string> cache;

using shared_lock_t = std::shared_lock<std::shared_mutex>;
using unique_lock_t = std::unique_lock<std::unique_mutex>;

std::string GetFromCache(int key) 
{
    //
    // Захват в режиме shared (для чтения)
    //

    shared_lock_t lock(cache_mutex); 
    if (cache.find(key) != cache.end()) {
        return cache[key]; 			// If found by key
    } 
    else {
        return "Not found";
    }
}

void UpdateCache(int key, const std::string& value) 
{
    //
    // Захват в режиме exclusive (для записи)
    // Это единственное место где поток может записывать что-то
    // .. пока в остальных местах потоки только читают
    // .. т.е. все потоки свободно могут читать данные не блокирую друг друга
    // .. но записывающий поток может временно запретить всем читать
    // .. пока записывает что-то, а после записи разрешат всем читать дальше
    //

    unique_lock_t lock(cache_mutex);
    cache[key] = value;
}

void reader_thread(int key) 
{
    std::cout << "Reader thread: " << GetFromCache(key) << std::endl;
}

void writer_thread(int key, const std::string& value) 
{
    UpdateCache(key, value);
    std::cout << "Writer thread: updated key " << key  
              << " with value " << value << std::endl;
}

int main() 
{
    //
    // Обновление кэша
    //

    std::thread writer1(writer_thread, 1, "Data1");
    writer1.join();

    //
    // Параллельное чтение данных из кэша
    //

    std::thread reader1(reader_thread, 1);
    std::thread reader2(reader_thread, 1);
    reader1.join();
    reader2.join();

    //
    // Попытка обновления кэша другим потоком
    //

    std::thread writer2(writer_thread, 1, "NewData1");
    writer2.join();

    //
    // Чтение обновлённых данных
    //

    std::thread reader3(reader_thread, 1);
    reader3.join();

    return 0;
}
```

Если бы в примере использовался обычный мьютекс, то чтение происходило бы куда дольше из-за того, что чтение каждым потоком блокировало бы остальные потоки.

## recursive_mutex
> Это мьютекс, который может быть захвачен одним и тем же потоком несколько раз без блокировки самого себя. Полезен в ситуациях, когда функция, уже захватившая мьютекс, вызывает другую функцию, которая пытается захватить тот же мьютекс.

```cpp
std::recursive_mutex rec_mtx;

void RecursiveFunction(int count) 
{
    if (count <= 0) {
      return;
    } 
    
    std::lock_guard<std::recursive_mutex> lock(rec_mtx);
    std::cout << "Count: " << count << std::endl;
    
    RecursiveFunction(count - 1);
}

int main() 
{
    std::thread t1(RecursiveFunction, 3);
    t1.join();
}

```

## …add
Реализация мьютексов в других контекстах и с другими инструкциями куда больше, например в Win32 Api есть SRWLock, в UNIX - pthread_mutex, futex, pthread_rwlocck и т.п.
