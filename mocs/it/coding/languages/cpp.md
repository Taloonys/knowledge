---
tags:
  - mocs
  - code
  - languages
  - cpp
aliases:
  - cpp
---
- [Up/Back](../languages.md)
# Немного сложный для самого начала тутор (лекции ИТМО)
[About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)
# Оглавление
* [Introduction info](../../../../src/code/languages/cpp/info.md)
* [Base stuff](../../../../src/code/languages/cpp/base-stuff.md)
* [Strings](../../../../src/code/languages/cpp/strings.md)
* [keywords & attributes](../../../../src/code/languages/cpp/keywords-attributes.md)
* [OOP](../../../../src/code/languages/cpp/oop.md)
* [namespaces-and-aliases](../../../../src/code/languages/cpp/namespaces-and-aliases.md)
* [Logical/binary constant (+mutable)](../../../../src/code/languages/cpp/logical-binary-immutability.md)
* [Directives](../../../../src/code/languages/cpp/directives.md)
* [Templates or metaprogramming](../../../../src/code/languages/cpp/templates.md)
* [collections](../../../../src/code/languages/cpp/collections.md)
* [Iterators](../../../../src/code/languages/cpp/Iterators.md)
* [POD-structures, standard layouts](../../../../src/code/languages/cpp/pods-and-standard_layouts.md)
* [Casting](../../../../src/code/languages/cpp/cast.md)
* [Functional objects and handling](../../../../src/code/languages/cpp/functional.md)
* [Operators overriding](../../../../src/code/languages/cpp/operators-overriding.md)
* [Exceptions](../../../../src/code/languages/cpp/exceptions.md)
* [Macro](../../../../src/code/languages/cpp/macro.md)
* [Rvalue/lvalue](../../../../src/code/languages/cpp/rvalue-lvalue.md)
* [RAII](../../../../src/code/languages/cpp/raii.md)
* [idioms](../../../../src/code/languages/cpp/idioms.md)
* [Multithreading](../../../../src/code/languages/cpp/multithreading.md)
* STL (Standard Library)
	* [Forbiden Stl functions ](../../../../src/code/languages/cpp/forbiden-stl-functions.md)
	* [Algorithms with STL](../../../../src/code/languages/cpp/stl-algorithms.md)
---
# Boost
- boost::asio
- boost::interprocess
    - shared memory
    - mapped object

# GSL
> Не уверен, что есть смысл описывать эту библиотеку, т.к. стандарты языка слегка изменились. Это некоторого рода имплементация С++ от Microsoft. Непосредственно относится к источнику ниже C++ CoreGuidelines.

# ==Wonderful resources to learn from==

* ***Google style code** → Очень приближен к нормальному код стайлу за исключением некоторых моментов для поддержки старыми компиляторами, но от этого стоит базово отталкиваться
	* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Self_contained_Headers)

* ***Git.CoreGuidelines** → Материал с полезными советами (некоторые не однозначные, многое для аккуратности, но она не везде нужна)
	* [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

* ***Cpp-kt(Лекции с кафедры ИТМО)** → упомянуто после оглавления (важно, что рассматриваются некоторые краеугольные случаи или внутренности чего-то)
	* [About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)

* msu-cpp(Лекции с института Ломоносова) → Довольно обширный (с точки рассмотрения, но не описания) материал. **В основном, много (довольно понятного) кода**
	* [GitHub - mtrempoltsev/msu_cpp_lectures: C++ lectures](https://github.com/mtrempoltsev/msu_cpp_lectures/tree/master)
# Doxygen
Одно из средств документирования кода (потом может ещё сформироваться полезная документация на основе этого)
* https://www.doxygen.nl/manual/docblocks.html
# CMake
[CMake](../../../../src/code/languages/cpp/cmake.md)
# Когда мы видим С...
> Довольно часто приходится переписывать или взаимодействовать с С кодом с его ужасной философией древних времён. Стоит помнить некоторые моменты при взаимодейтсвии с ним.

- В Си нет **ООП**, соответственно всякие вызовы методов класса следуют превратить в `обычные функции` или `static функции`
- В Си нет **исключений**, т.е. всё перед экспортом в Си нужно обернуть в `try-catch`, однако в Си под Windows есть `try-except-final-leave`
- В С++ подобное недопустимо, а в Си - без проблем:
  ```cpp
//
// В С++ после каста мы получаем rvalue, ему ничего нельзя присвоить
//

char buffer[128];
(int*)buffer = 10;

//
// В С++ после каста мы получаем rvalue, его нельзя модифицировать инкрементом
//

char* cur = buffer[20];
char* p   = (int*)cur++;	

//
// А вот так можно, но конкретно тут это UB
//

(AnyClass*)buffer->inner_value = 50;   
  ```
- В Си есть только С-style cast
- В Си нет неймспейсов
- В Си нет `new-delete`, только `malloc-free`, а они возвращают `void*`, следует об этом помнить
- В Си возможен неявный каст от `void*` к чему угодно (но не рекомендуется), в С++ - нет такого
- В Си `void` имеет размерность сопоставимую с машинным словом (x16 - 2 байта, х32 - 4 байта, х64 - 8 байт), при этом `void*` размерности по стандарту не имеет, но расширениями компиляторов это можно обойти. В С++ похожая замена для этого `uintptr_t`

# Precompiled header
> `.h` файлы не попадают в компиляцию, только `.c`, `.cpp`, существует древний трюк для ускорения компиляции - скомпилировать хэдер заранее, дабы при перекомпиляции некоторых других `.c/.cpp` файлов все эти неизменённые хэдеры не перекомпилировались повторно. Такие файлы помечаются как `.pch` (PreCompiledHeader), но есть и другие варианты вроде...
https://en.wikipedia.org/wiki/Precompiled_header
# double ptr philosophy
```cpp
void Function(int** buffer) {
	if (*buffer == nullptr) {
		*buffer = new int[size];
	}
}

int* ptr = nullptr;
Function(ptr);
// ptr now point to buffer
delete[ptr], ptr = nullptr;
```
# Opaque pointer
> Opaque - прозрачный. Суть метода заключается в, например, объявлении указателя на структуру, у которой нет реализации в нашем контексте.
> Например клиент кода видит эту дичь `typedf struct Structure* str_ptr_t`, а описания Structure нигде нет, он может сам указать описание, а может это описание где-то в другом контексте сокрыто.
* https://stackoverflow.com/questions/7553750/what-is-an-opaque-pointer-in-c
* В С++ под похожее определение попадает идиома *Pimpl*, в хэдере указан указатель на какой-то внутренний тип, а всё его описание где-то внутри cpp.
