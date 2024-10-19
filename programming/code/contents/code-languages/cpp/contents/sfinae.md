# SFINAE tools

---

# enable_if<bool, T>

> Включает  опцию инстанцирования, если true
> 

```cpp
//
// Possible implementation
//
template<bool B, class T = void> // если Т нет - enable_if<>::type вернёт void
struct enable_if {};

// if B = true, then template function will looks like it has typedef 
template<class T>
struct enable_if<true, T> 
{ 
	typedef T type; 
};

// таким образом, если у такой развёртки взять ::type, то оно вернёт T
```

<aside>
💡 Как это работает?
Если мы указываем template<typename = std::enable_if_t<…> >, то у нас происходит обращение к типу, являющийся результатом выражения enable_if_t<>(или std::enable_if<..>::type), если тип не определяется, то происходит “ошибка”, поэтому такой вариант шаблона исключается из кандидатов на использование.
Если std::enable_if_t<…> указывать в возвращаемой функции, то логика исключения из кандидатов для инстанцирования будет такой же.

</aside>

# true_type/false_type

> std::true_type и std::false_type - являются специальными описателями true и false в template контексте, с помощью которых можно решать, какие специализации, например, можно использовать при решении об инстанцировании
> 

```cpp
template<typename T>
struct is_integral : std::false_type {};

template <>
struct is_integral<int> : std::true_type {};

template <>
struct is_integral<long> : std::true_type{};

template<typename T>
std::enable_if_t<is_integral<T>::value> DedicateType(T value)
{
	printf("It's an integral type");
}

template<typename T>
std::enable_if_t<!is_integral<T>::value> DedicateType(T value)
{
	printf("It's not an integral type");
}

//
// usage
//

DedicateType(5); // it's an integral type
DedicateType(4.5); // it's not an integral type
DedicateType("HelloWorld"); // it's not an integral type
```

<aside>
💡

Вызываем DedicateType(4.5) 
→ в std::enable_if_t<> есть проверка по is_integral<T>::value 
→ видим, что для is_integral<double> нет никакой специализации 
→ т.е. наш случай относится к общему is_integral 
→ is_integral наследуется от std::false_type 
→ значит is_integral<>::value будет равен false 
→ мы попадаем под !is_integral 
→ происходит соответствующий вызов printf("It's not an integral type");

</aside>

# bool_constant/integral_constant

```cpp
template<typename T>
struct is_integral 
	: std::false_type 
{ };

template<typename T>
struct is_integral<T> 
	: std::bool_constant<std::is_integral_v<T>> 
{ };
```

```cpp
using two_t = std::integral_constant<int, 2>;
using four_t = std::integral_constant<int, 4>;
 
static_assert(not std::is_same_v<two_t, four_t>);
static_assert(two_t::value * 2 == four_t::value, "2*2 != 4");
```

# disjuntcion/conjuction

> Аналоги дизъюнкции и конъюнкции для **исключительно шаблонов типов**
> 

```cpp
template<class...> 
struct conjunction 
	: std::true_type 
	{ };

template<class B1> 
struct conjunction<B1> 
	: B1 
{ };

template<class B1, class... Bn>
struct conjunction<B1, Bn...> 
    : std::conditional<bool(B1::value), conjunction<Bn...>, B1>::type 
{ };
```

# decay

> Возвращает из передаваемого типа непосредственно тип, отбрасывая cv-квалификаторы, ссылки, поинтеры и т.п.
> 

```cpp
void SetText(const std::string& str) // const &
{
	std::decay<str> decaied_str; // decaied_str is std::string
}
```

# is_same

> Сравнивает типы
> 

```cpp
template<typename Label,
		     typename = std::enable_if_t<std::is_same<Label, std::decay<Label>>>>
void SetText(Label* l, const std::string& text)
{
	l->setText(text);
}
```

# is_convertable

# common_type

# is_base_of

# auto

> Позволяет автоматически определять тип на основе “явных признаков”
> 

# decltype

> Позволяет определить тип выражения
> 

# type_identity (C++20, but easily writtable)

# declval

> Добавляет к передаваемому типу rvalue-ссылку, значения в скобках имеет nonevaluated context, т.е. значение внутри вычисляет лишь при попытке вызвать
> 

<aside>
💡 [https://en.cppreference.com/w/cpp/language/expressions#Potentially-evaluated_expressions](https://en.cppreference.com/w/cpp/language/expressions#Potentially-evaluated_expressions)

</aside>

```cpp
struct Default
{ // has default constructor
    int foo() const { return 1; } 
};
 
struct NonDefault
{
    NonDefault() = delete; // doesn't have default constructor
    int foo() const { return 1; }
};
 
int main()
{
	//
	// type of n1 is int
	//
  decltype(Default().foo()) n1 = 1;                   
  
  //
  // error: no default constructor 
  //
  decltype(NonDefault().foo()) n2 = n1;       
          
  //
  // type of n2 is int
  // it's not constructable, but we can call and decide what type it would return
  //
  decltype(std::declval<NonDefault>().foo()) n2 = n1; 
	std::cout << "n1 = " << n1 << '\n'
            << "n2 = " << n2 << '\n';
}
```

# void_t

> Специальный шаблон, где определён void как void_t. Но с этим есть некоторые трюки…
> 

[https://www.cyberforum.ru/cpp-beginners/thread3078420.html](https://www.cyberforum.ru/cpp-beginners/thread3078420.html)

# remove_cv

> откидывает cv квалификаторы
> 

# addressof

> Берёт **уже сконструированный объект** и возвращает ссылку на него.
> 

# to_address (C++20)

> Берёт указатель и возвращает указатель… Суть в том, что это нужно для случая, когда объект, который принимает функция, **ещё даже не сконструирован**.
> 

# remove_reference

> Удаляет ссылку
> 

# is_pointer

> Проверяет, указатель ли передаваемый объект
> 

# add_pointer

> Добавляет **типу** указатель
> 

# type_identity (C++20)

# is_invocable

# make_unsigned

> Возвращает беззнаковый вариант возвращаемого типа
> 

**Зачем нам такое вообще? Например для такого**

```cpp
template<typename Diff>
inline constexpr auto max_possible_value = Diff{static_cast<std::make_unsigned_t<Diff>>(-1) >> 1};
    
template<typename Diff>
inline constexpr auto min_possible_value = Diff{-max_possible_value<Diff> - 1};

```