# Какую задачу решают

> **Шаблоны** - это обобщённое описание функций или типа. По своему принципу выступают как **заменители** типа/объекта.

```cpp
template<class Type> // теперь Type у нас именно какой-то тип
Type _min(Type a, Type b){
    if( a < b){
        return a;
    }
    return b;
}

int main()
{
	double a = 5;
	double b = 10;
	cout << _min(a,b);	// 5

	int a = 5; // другой тип, но тоже будет работать
	int b = 10;
	cout << _min(a,b);	// 5
	return 0;
}
```

> `std::<any_template_func>::type` → `std::<any_template_func>_t`
>`std::<any_template_func>::value` → `std::<any_template_func>_v`


---
# typename vs class

[https://stackoverflow.com/questions/2023977/what-is-the-difference-between-typename-and-class-template-parameters](https://stackoverflow.com/questions/2023977/what-is-the-difference-between-typename-and-class-template-parameters)
Грубо говоря, до 17-ых плюсов была какая-то чародейская магия с шаблонным <шаблонным параметром>.

**Сейчас правильнее использовать typename везде**

# Ключевое слово typename

> **typename** явно указывает, что за ним стоит тип, а не что-то ещё.
> 

```cpp
template<typename T>
class MyClass
{
public:
	void Func()
	{
		// Обращение к вложенному классу SubType в T
		// [Т.к. явно указано, что T::SubType - тип с помощью typename]
		typename T::SubType* ptr; 
		
		// Может вообще произойти попытка умножения SubType на ptr
		// [Компилятор вполне может посчитать, что SubType член класса или функция...]
		T::SubType* ptr; 
	}
```

> Очевидно, что не всегда надо указывать typename

```cpp
/**
 * @brief контейнер из элементов типа T размерностью size
 * @tparam T - тип каждого элемента контейнера
 * @param size[in] - размерность контейнера, указываемая как объект типа std::size_t
 */
template<typename T, std::size_t size>
struct MyArray {};

//
// usage
//
MyArray<int, 20>; 
MyArray<double, 90>;
```

# Двойная трансляция
При попытке скомпилировать шаблон происходят следующие этапы:
- Синтаксическая проверка того, как написан шаблон - а-ля **definition time**
- Попытка подставить подходящих кандидатов - **инстанцирование**

# SFINAE
> Substitution failure is not an error, если использовать шаблоны по стандарту, то на все не валидные подстановки решения будут выдаваться ошибки подстановки (ошибки компиляции), но если использовать SFINAE, то не валидные кандидаты будут просто исключаться из подбора на подстановку. Однако.. **если не будет найдено ни одного кандидата, то будет ошибка компиляции**.

[SFINAE tools](sfinae.md)

💡 Представим, что есть вот такой код:
```cpp
    template<typename It,
             typename /* Enable */ = std::enable_if_t<
                is_span_compitable_iterator_v<It, element_type>>>
    constexpr span(It first, size_type count) noexcept
        : base_t(to_pointer(first, count), count)
    { }
```

НО **is_span_compitable_iterator_v** является частью namespace’а **details**, который никак здесь не указан. Что произойдёт в таком случае? Данный шаблон будет исключён из списка кандидатов, т.к. невозможно инстанцировать is_span_compitable_iterator_v<,>…
Но никакой ошибки непосредственно по этому поводу компилятор не выдаст! (Он просто скажет, скорее всего, что нет подходящего конструктора для вызова уже при использовании). Это некоторая порождающая ошибки особенность шаблонов…

```cpp
    // Working option...
    template<typename It,
             typename /* Enable */ = std::enable_if_t<
                details::is_span_compitable_iterator_v<It, element_type>>>
    constexpr span(It first, size_type count) noexcept
        : base_t(to_pointer(first, count), count)
    { }
```


# if constepxr ()
> if constexpr (true) { } - разрешает включать свой блок в компиляцию
> if constexpr (false) { } - не включает свой блок в компиляцию вообще (т.е. ещё на препроцессе код просто исчезает из файла грубо говоря)

```cpp
template<typename T>
T sum (T a, T b, T enable_sum_threshold_number)
{
	if consexpr ( a < enable_sum_threshold_number && b < enable_sum_threshold_number)
	  return a + b;
	else 
		return 0;
}

// main
sum(2, 2, 3);     // returns 4 -> compiles only: sum(...) { return a + b; } 
                  // doesn't inlcude if() else return 0;
sum(2, 5, 3);     // return 0 -> complies only: sum(...) { return 0; }
                  // prb compiler will through away input args in release build =)
```

# Шаблонозависимые переменные

```cpp
template<typename Protocol>
class Server 
{
private:
	class Session;	
}

template<typename Protocol>
class Server<Protocol>::Session : TCP
{
	using session_info_t = typename Session<Protocol>::session_id;
public:
	Session() {};
}
```

# CTAD (C++17)

> **Class Template Argument Deduction (CTAD)** - механизм, позволяющий компиляторы самостоятельно определять тип для шаблонов **КЛАССА.** (Для функций всё было нормально изначально)

```cpp
// до CTAD
auto tup2 = std::make_tuple(123, 'a', 40.0);
// или
auto tup1 = std::tuple<int, char, double>(123, 'a', 40.0);

// с CTAD
std::tuple tup(123, 'a', 40.0);  // std::tuple<int, char, double>
```

## Some issues

```cpp
template <typename X>
using PairIntX = std::pair<int, X>; // doesn't work with aliases

PairIntX p{1, true};                // не компилируется
```

```cpp
std::pair p{1, 5};              // OK
std::pair<double> q{1, 5};      // ошибка, так нельзя <double, ?>
std::pair<double, int> r{1, 5}; // OK
```

## User deduction guides

> Механизм, позволяющий подсказать компилятору, как решить некоторые вопросы касательно параметров шаблона

```cpp
// синтаксис
[explicit] template-name (parameter-declaration-clause) -> simple-template-id;
```

```cpp
template <typename T>
struct MyVector 
{
  template <typename It>
  MyVector(It from, It to) {...}
};

std::vector<double> dv = {1.0, 3.0, 5.0, 7.0};

// Компилятор пока тупенький и не может точно понять, 
// что лежит под It, чтобы он мог сконструировать структуру MyVector
MyVector v2{dv.begin(), dv.end()}; 
```

Для такого придумали **user deduction guides.**

<aside>
💡 можно указывать вне определения класса… совсем немножечко концепта декорирования…

</aside>

```cpp

template<typename T>
class MyVector { ... };

// поэтому подсказываем компилятору, что если прилетает два итератора
// то 
template <typename It>
MyVector(It, It) -> MyVector<typename std::iterator_traits<It>::value_type>;
```

💡 если поставить **explicit**:

```cpp
template<typename It>
**explicit** MyVector(It, It) -> MyVector<typename std::iterator_traits<It>::value_type>;
```

то такое будет запрещено:
**MyVector v3 = {dv.begin(), dv.end()}; // error**

[Class Template Argument Deduction](https://habr.com/ru/articles/461963/)

# Dependent false

> Специальный приём, позволяющий использовать static_assert(false) внутри шаблона (не чтобы он падал сразу, а чтобы было понятно, что попали в неправильный контекст, иначе обычный static_assert(false) просто сразу упадёт без вопросов откуда куда и зачем.

[How can I create a type-dependent expression that is always false? - The Old New Thing](https://devblogs.microsoft.com/oldnewthing/20200311-00/?p=103553)

# Template CopyConstructor

```cpp
class C {
public:
	C (const C& other_c) {}
	
	template<typename T>
	C (const T& other_tc) {}
};

C c;
C c1(c); // -> would be used simple copy constructor
```

Простое удаление копирующего конструктора не поможет, просто операция копирования будет запрещена, но можно так:

```cpp
class C {
public:
	C (C const volatile&) = delete;
	
	template<typename T>
	C (T const & other_tc) {}
};

C c;
C c1(c); // -> would be used template copy constructor
```

# Concepts (C++20)

> Вот прям всё, что нужно знать про них - вся глава 14

[concepts](https://metanit.com/cpp/tutorial/17.1.php)

# Ellipsis …
💡 ellipsis - (по англ.) - многоточие используется для переменного кол-ва аргументов

```cpp
double average(int count, ...) 
{ }

double average(...)
{ }
```
## Private member access
> Когда у класса-жертвы есть шаблонный метод с параметром, у которого тоже есть шаблон (в примерах ниже будет `std::vector<T>`)
* Когда значение ссылочное:

```cpp
class Victim {
//
// У этого класса будем воровать приватное поле
//
public:
    template <typename T>
    void Foo(std::vector<T>& v) { 
    //
    // Вот этим методом мы воспользуемся
    //
    } 

private:    
    int a_ = 55;                           // Интересуемое приватное поле
};


class Scout {
//
// Наш самописный класс-партизан
//
public:
    int* a = nullptr;
};


//
// Специализацию шаблона функции можно делать вне класса
//
template <>
void Victim::Foo<Scout>(std::vector<Scout>& v) { 
	//
	// В ссылку из-вне помещаем адрес на приватное поле класса
	//
    v[0].a = &(this->a_);
}


int main()
{
    Victim victim;
    Scout scout;

    std::vector<Scout> v = {scout};        // Некрасиво, но такой пример...
    victim.Foo(v);
    std::cout << *(v[0].a);                // Получаем наше приватное поле

    return 0;
}
```
* По значению:
```cpp
class Victim {
//
// Класс-жертва, чьё поле будем вскрывать
//
public:
    template <typename T>
    void foo(std::vector<T> v) { } 

private:    
    int a_ = 55;
};


//
// Исключительно для ассоциации
//
#define VECTOR_IMPOSTER std::vector<int&>

template <>
class VECTOR_IMPOSTER {
//
// Специализация std::vector, в которой мы полностью меняем поведение
//   
public:
    vector(int& val) 
        : val_(val) 
    { }
    
    void SetValue(int& val) { 
        val_ = val;
    }
private:
    int& val_;
};


template <>
void Victim::foo<int&>(VECTOR_IMPOSTER v) { 
	//
	// Можно и не так, это один из способов...
	//
    v.SetValue(this->a_);
}


int main()
{
    Victim f;

    int value_chest = 0;                             // К этой переменной подцепим приватное поле
    VECTOR_IMPOSTER value_hopper = {value_chest};    // С помощью вектора свяжем переменную и приватное поле
     
    f.foo(value_hopper);
    std::cout << value_chest;

    return 0;
}
```
## true_type/false_type и enable_if никогда не надо смешивать!

- **BAD**

```cpp
template<typename It>
struct is_contiguous_iterator 
    : std::false_type 
{ };

template<typename It,
         typename /* Enable */ = std::enable_if_t<
            is_random_access_iterator<It>::value
            && std::is_base_of_v<typename std::iterator_traits<It>::iterator_category, 
                                 typename std::random_access_iterator_tag>
            && std::is_lvalue_reference_v<iter_reference_t<It>
            && std::is_same_v<iter_value_t<It>, 
                              remove_cvref_t<iter_reference_t<It>>
            && (is_pointer_or_nullptr<remove_cvref_t<It>>::value
                || std::is_pointer_v<decltype(std::declval<const It&>() + 0)>)>>
struct is_contiguous_iterator<It> 
    : std::true_type
{ 
    //
    // should satisfy LegacyIterator Requirements
    //
};
```

- **SHOULD BE**

```cpp
template<typename It>
struct is_contiguous_iterator // Это первичный шаблон
    : std::bool_constant<
        is_random_access_iterator<It>::value
        && std::is_base_of_v<typename std::iterator_traits<It>::iterator_category, 
                             typename std::random_access_iterator_tag>
        && std::is_lvalue_reference_v<iter_reference_t<It>
        && std::is_same_v<iter_value_t<It>, 
                          remove_cvref_t<iter_reference_t<It>>
        && (is_pointer_or_nullptr<remove_cvref_t<It>>::value
            || std::is_pointer_v<decltype(std::declval<const It&>() + 0)>)>
{ };
```

💡 Почему struct … : std::bool_constant<…>, а не inline constexpr … → всякие функции по типу std::disjunction и т.п. принимают в себя хорошо именно тип, т.е. к ним хорошо использовать структуру. Но это не ультимативное решение
## template<typename T, typename = void> - контекст void_t использования, не смешивать с enable_if

- **SHOULD BE**

```cpp
template<typename, typename = void>
struct is_iterator 
    : std::false_type 
{ };

template<typename It>
struct is_iterator<It, std::void_t<
    typename std::iterator_traits<It>::difference_type,
    typename std::iterator_traits<It>::value_type,
    typename std::iterator_traits<It>::pointer,
    typename std::iterator_traits<It>::reference,
    typename std::iterator_traits<It>::iterator_category>> 
    : std::true_type 
{ };
```
# Sources
> Шаблоны - слишком обширная тема
* Дэвид Вандервуд, Шаблоны С++