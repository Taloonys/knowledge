---
tags: [code, languages, cpp]
aliases: [const world]
---

# const world

```cpp
const T  // -> константный тип

//
// Если const слева от *, то константые данные
// Если const справа от *, то константный сам указатель
// * т.е. указатель не может потом начать указывать на что-то другое
//

const T* // -> указатель на константные данные
T* const // -> константный указатель на данные
const T* const  // -> константный указатель на константные данные
T const * const // -> константный указатель на константные данные

//
// ссылка сама по себе указывает только на что-то одно, т.е. она всегда const по существу
//

const T& // -> ссылка на константные данные

// 
// using-alias хитрожопый слегка
//

using ptr_t = T*;
const ptr_t // -> T* const -> константный указатель на данные

// 
// classes and methods
//

template<typename T>
class Base 
{
public: 
	void SquareOwnParam() const // помечено, что метод ничего не меняет в классе
	{ // махинации и использования не-const методов/объектов запрещены
		CheckNothing();    // Error, uses non-const method
		param_ *= param_;  // Error, param_ - внутреннее состояние класса (т.е. член класса)
	}
	
	void CheckNothing() {} // non-const
	
private:
	T param_;
};

// 
// Weird ideas
//

const T const x; // -> второй const проигнорируется

using CINT = const int;
const CINT ccх = 8; // -> та же ситуация, компилятор на такое выдаст варнинг
```

# Binary const

> По факту бинарная константность означает неизменяемость данных в их битовом представлении
> 

```cpp
const int a = 5; // то, что лежит под a - никак не изменить в памяти
```

# Logical const and mutable purpose

> Подразумевает неизменяемость видимого состояния объекта даже если во внутренней реализации какого-то, например, const метода происходят изменения каких-то состояний полей.
> 

Из примера в начале:

```cpp
// Сам пример
template<typename T>
class Base 
{
public: 
	void SquareOwnParam() const // помечено, что метод ничего не меняет в классе
	{ // махинации и использования не-const методов/объектов запрещены
		CheckNothing();    // Error, uses non-const method
		param_ *= param_;  // Error, param_ - внутреннее состояние класса (т.е. член класса)
	}
	
	void CheckNothing() {} // non-const
	
private:
	T param_;
};
```

```cpp
template<typename T>
class Base 
{
public: 
	void SquareOwnParam() const // помечено, что метод ничего не меняет в классе
	{
		CheckNothing();    // Still error, uses non-const method
		param_ *= param_;  // Ok, param_ is mutable
	}
	
	void CheckNothing() {} // non-const
	
private:
	mutable T param_;
};
```

> Т.е. ключевое слово **mutable** позволяет, не меняя логическую константность объекта, менять его внутреннее состояние. Похоже на костыль, не правда ли? Но не совсем, mutable надо использовать только когда это надо:
> 

```cpp
class SomeClass final
{
public:
	void ConstFunc() const 
	{
		std::lock_guard lock(mutex_); // увы, но lock меняет состояние mutex
		/* ...do some read...*/
	} // unlock
		
private:
	std::mutex mutex_; // допустим, что нужен в нескольких местах класса
}
	
```

Снять с метода константность мы логически не можем, но использовать в нём мьютекс мы, увы, обязаны, как вывернуться из этой ситуации?

```cpp
private:
	mutable std::mutex mutex_;
```

Далее лямбда:

```cpp
int main() 
{
	int i     = 2;
	auto ok   = [&i]    { ++i; };  // OK, i захватывается по ссылке
	auto err  = [i]     { ++i; };  // Ошибка: попытка изменения внутренней копии i
	auto err2 = [x{22}] { ++x; };  // Ошибка: попытка изменения внутренней переменной x
}

int main() {
  int i = 2;
  auto ok = [i, x{22}] mutable { 
	  i++; x+=i; 
	};
}
```

> Подведём итог по **mutable** тем, что он **нужен для явного разделения бинарной и логической неизменяемости**…