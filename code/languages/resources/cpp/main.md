# Немного сложный для самого начала тутор (лекции ИТМО)

[About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)

# Оглавление
* [Introduction info](resources/info.md)
* [Base stuff](resources/base-stuff.md)
* [Strings](resources/strings.md)
* [keywords](keywords.md)
* [OOP](resources/oop.md)
* [namespaces-and-aliases](resources/namespaces-and-aliases.md)
* [Logical/binary constant (+mutable)](resources/logical-binary-immutability.md)
* [Directives](resources/directives.md)
* [Generics/Templates](resources/generics.md)
* [collections](resources/collections.md)
* [Iterators](resources/Iterators.md)
* [POD-structures, standard layouts](resources/pods-and-standard_layouts.md)
* [Casting](resources/cast.md)
* [Functional objects and handling](resources/functional.md)
* [Operators overriding](resources/operators-overriding.md)
* [Exceptions](resources/exceptions.md)
* [Macroses](resources/macro.md)
* [Rvalue/lvalue](resources/rvalue-lvalue.md)
* [RAII](resources/raii.md)
* [idioms](resources/idioms.md)
* [Multithreading](resources/multithreading.md)
* STL (Standard Library)
	* [Forbiden Stl functions ](resources/forbiden-stl-functions.md)
	* [Algorithms with STL](resources/stl-algorithms.md)

# Boost

- boost::asio
- boost::interprocess
    - shared memory
    - mapped object

# GSL

> Не уверен, что есть смысл описывать эту библиотеку, т.к. стандарты языка слегка изменились. Это некоторого рода имплементация С++ от Microsoft. Непосредственно относится к источнику ниже C++ CoreGuidelines.

# ==Wonderful resources to learn from==

**Google style code** → Очень приближен к нормальному код стайлу за исключением некоторых моментов для поддержки старыми компиляторами, но от этого стоит базово отталкиваться

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Self_contained_Headers)

**Git.CoreGuidelines** → Материал с полезными советами (некоторые не однозначные, многое для аккуратности, но она не везде нужна)

[C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

**Cpp-kt(Лекции с кафедры ИТМО)** → упомянуто после оглавления (важно, что рассматриваются некоторые краеугольные случаи или внутренности чего-то)

[About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)

msu-cpp(Лекции с института Ломоносова) → Довольно обширный (с точки рассмотрения, но не описания) материал. **В основном, много (довольно понятного) кода**

[GitHub - mtrempoltsev/msu_cpp_lectures: C++ lectures](https://github.com/mtrempoltsev/msu_cpp_lectures/tree/master)

# Doxygen
Одно из средств документирования кода (потом может ещё сформироваться полезная документация на основе этого)

https://www.doxygen.nl/manual/docblocks.html

# CMake
[CMake](resources/cmake.md)

# Когда мы видим С...
> Довольно часто приходится переписывать или взаимодействовать с С кодом с его ужасной философией древних времён. Стоит помнить некоторые моменты при взаимодейтсвии с ним.

- В Си нет **ООП**, соответственно всякие вызовы методов класса следуют превратить в `обычные функции` или `static функции`
- В Си нет **исключений**, т.е. всё перед экспортом в Си нужно обернуть в `try-catch`, однако в Си под Windows есть `try-except-final-leave`
- В С++ подобное недопустимо, а в Си - без проблем:
  ```cpp
  char buffer[128];
  (int*)buffer = 10;		// В С++ после каста мы получаем rvalue, ему ничего нельзя присвоить

  char* cur = buffer[20];
  char* p   = (int*)cur++;	// В С++ после каста мы получаем rvalue, его нельзя модифицировать инкрементом
  ```
- В Си есть только С-style cast
- В Си нет неймспейсов
- В Си нет `new-delete`, только `malloc-free`, а они возвращают `void*`, следует об этом помнить
- В Си возможен неявный каст от `void*` к чему угодно (но не рекомендуется), в С++ - нет такого
- В Си `void` имеет размерность сопоставимую с машинным словом (x16 - 2 байта, х32 - 4 байта, х64 - 8 байт), при этом `void*` размерности по стандарту не имеет, но расширениями компиляторов это можно обойти. В С++ похожая замена для этого `uintptr_t`
