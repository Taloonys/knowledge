
---
# Немного сложный для самого начала тутор (лекции ИТМО)

[About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)

---

* [Introduction info](info.md)
* [Base stuff](programming/code/contents/code-languages/cpp/contents/base-stuff.md)
* [Strings](strings.md)
* [keywords](keywords.md)
* [OOP](programming/code/contents/code-languages/cpp/contents/oop.md)
* [namespaces-and-aliases](namespaces-and-aliases.md)
* [Logical/binary constant (+mutable)](logical-binary-immutability.md)
* [Directives](directives.md)
* [Generics/Templates](generics.md)
* [[collections]]
* [Iterators](Iterators.md)
* [POD-structures, standard layouts](pods-and-standard_layouts.md)
* [Casting](cast.md)
* [Functional objects and handling](functional.md)
* [Operators overriding](operators-overriding.md)
* [Exceptions](exceptions.md)
* [Macroses](macro.md)
* [Rvalue/lvalue](rvalue-lvalue.md)
* [RAII](raii.md)
* [[idioms]]
* [Multithreading](multithreading.md)
* STL (Standard Library)
	* [Forbiden Stl functions ](forbiden-stl-functions.md)
	* [Algorithms with STL](stl-algorithms.md)
---

# Boost

- boost::asio
- boost::interprocess
    - shared memory
    - mapped object

---

# GSL

> Не уверен, что есть смысл описывать эту библиотеку, т.к. стандарты языка слегка изменились. Это некоторого рода имплементация С++ от Microsoft. Непосредственно относится к источнику ниже C++ CoreGuidelines.

---

# ==Wonderful resources to learn from==

**Google style code** → Очень приближен к нормальному код стайлу за исключением некоторых моментов для поддержки старыми компиляторами, но от этого стоит базово отталкиваться

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Self_contained_Headers)

**Git.CoreGuidelines** → Материал с полезными советами (некоторые не однозначные, многое для аккуратности, но она не везде нужна)

[C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

**Cpp-kt(Лекции с кафедры ИТМО)** → упомянуто после оглавления (важно, что рассматриваются некоторые краеугольные случаи или внутренности чего-то)

[About & Links - C++ course notes](https://cpp-kt.github.io/cpp-notes/course.html)

msu-cpp(Лекции с института Ломоносова) → Довольно обширный (с точки рассмотрения, но не описания) материал. **В основном, много (довольно понятного) кода**

[GitHub - mtrempoltsev/msu_cpp_lectures: C++ lectures](https://github.com/mtrempoltsev/msu_cpp_lectures/tree/master)

---

# Doxygen

Одно из средств документирования кода (потом может ещё сформироваться полезная документация на основе этого)

https://www.doxygen.nl/manual/docblocks.html

---
# CMake

[CMake](cmake.md)