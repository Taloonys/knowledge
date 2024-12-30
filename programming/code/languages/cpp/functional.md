# Functors

> **Функторы** или **функциональные объекты**, т.е. экземпляр класса, в котором переопределён оператор(), т.е. такой объект теперь работает как вызов функции, только объект может “хранить состояние”.
> 

```cpp
class SimpleFunctor {
    std::string name_;
public:
    SimpleFunctor(const char *name) : name_(name) {}
    void operator()() 
			{ std::cout << "Oh, hello, " << name_ << endl; }
};

int main() {
    SimpleFunctor sf("catonmat");
    sf();  // выводит "Oh, hello, catonmat"
}
```

<aside>
💡 Чаще всего функторы в С++ используются в качестве предикатов, псевдозамыканий или функций сравнения в алгоритмах STL. Честно говоря, это издержки обычной логики, которую решили назвать как-то особенно…

</aside>

# Callback-функции

> **Callback** - метод передачи функции в качестве аргумента, например, другой функции. Ниже приведён пример использования. Напомню, что само имя функции является ссылкой на начало инструкций, однако в контексте С++ может преобразовываться к указателю.
> 

```cpp
std::string dataFromBd(){
    return "Data from BD";
}

std::string dataFromMe(){
    return "Data from Me";
}

std::string dataFromServer(){
    return "Data from Server";
}

// callBack имя будет хранить в себе имя функции.
void showMsg(std::string (*callBack)()) 
{
    cout << callBack();
}

int main()
{
    showMsg(dataFromMe); // можно сделать лист функций для красоты

		// отдельный пример про callBack
		void(*callBackHolder)() = dataDromBd
		cout << callBackHolder();
    return 0;
}
```

💡 Переменную, хранящую указатель на инструкции, синтаксически можно представить вот так:
`int (*) (int, int)` → где может быть и так → int **sum**(int, int)
Также возможна запись такого рода: `int(int,int)`, например, что равносильно случаю выше…
Куда удобнее всё это записывать и обёртывать в std::function<int(int,int)> sum. Такое доступно с С++11 в header’е

```cpp
//...
// callBack имя будет хранить в себе имя функции, которая должна возвращать std::string и ничего не принимать на вход
void showMsg(std::function<std::string ()> callBack)
{
    cout << callBack();
}
//..
```

# Lambdas

> Грубо говоря, это выполняемое тело функции, которую можно вставить куда угодно в коде, хоть в сложение или куда-либо ещё. Но стоит помнить, что лямбда - тоже, грубо говоря, отдельный тип. Так же каждая написанная лямбда - *уникальна*, это необходимо знать для того, чтобы потом пытаться делать, например, листы лямбд.
> 

Изначально лямбда понятия не имеет, что находится за её пределами, для этого надо ей указывать, что можно "захватить", чтобы лямбда видела это внутри своего блока кода {}. Лямбда обязательно начинается с [], общий синтаксис выглядит так:  

- [] - это область захвата, можно захватывать переменные [x,y] по значению (если в теле лямбды менять такие значения, то во вне они не изменятся), а можно захватывать по ссылке [&x, &y] (очевидно, что так при работе с ними в теле лямбды значения во вне уже изменятся)
- () - работают как для аргументов функции и можно использовать внутри них ключевые слова по типу mutable
- ->Type - указание того, что лямбда должна вернуть… можно не писать, если случай будет предсказуем для компилятора (например, вернуть 5, компилятор поймёт, что речь идёт про int).
- {} - область видимости лямбды

<aside>
💡 [=] и [&] захватывают все значения из внешней области видимости по значению и по ссылкам соответственно, но такое делать настоятельно НЕ рекомендуется
[this] захватывает текущий класс по ссылке, вполне норм

</aside>

<aside>
💡 Лямбды **можно запоминать в новых версиях С++**: 
auto MyLambda = [](){};, в таком случае MyLambda имеет тип лямбда, для вызова самой лямбды  надо писать MyLambda(); т.е. как вызов функции.

</aside>

```cpp
int a = 5;
int b = 10;
auto sumLambda = [=](){return a + b;}
cout << sumLambda(); // 15
```

Ещё лямбды можно форматировать вот так…

```cpp
auto lambda = [lifetime = shared_from_this(),
               handler  = std::move(handler),
               /*...*/]{}
```

## Терминология

- **Функциональным объектом** называется объект, в котором переопределён оператор вызова ()
    
    ```cpp
    template<typename AnyFd>
    struct CloseSth 
    {
    	void operator()(AnyFd fd) 
    	{
    		fd.close();
    	}
    }
    ```
    
- **Предикатом** называется функциональный объект, результатом работы которого является boolean результат
    
    ```cpp
    // Такая лямбда в методе сортировки - предикат
    list.sort(..., []->bool(int first, int second){
    	return first > second;
    }
    ```
    
- **Замыканием** называется анонимная функция, ссылающаяся в своём теле на переменные, не содержащиеся в области захвата
    
    ```cpp
    // a и b - входные аргументы, они не захватываются, lambda - замыкание
    auto lambda = [this](int a, int b){
    	return a + b;
    }
    ```