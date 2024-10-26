> **Приведение типов** - преобразование одного типа к другому (ну а что вы ещё ожидали?).
## const_cast

> Cнимает или добавляет константность и volatile (называются cv-qualifiers)…
**проверка на этапе компиляции**
> 

## static cast

> Самое безопасное преобразование одного типа в другой. Большая часть проверок основана на базовых правилах языка.
> 

**Проверка на этапе компиляции**

[static_cast conversion - cppreference.com](https://en.cppreference.com/w/cpp/language/static_cast)

## dynamic_cast

> Не полиморфное преобразование. Необходимо для upcasting и downcasting → преобразование указателя дочернего класса к родительскому и наоборот.
> 

**Проверка на этапе выполнения программы**

[dynamic_cast conversion - cppreference.com](https://en.cppreference.com/w/cpp/language/dynamic_cast)

## reinterpret_cast

> Преобразование от одного указателя к другому.
> 

**Проверка на этапе компиляции**

Основные проверки:

- Что происходит каст указателя к указателю
- Учитывает наличие cv-квалификаторов

## C-style cast

> Синтаксически перекочевавший метод из С. Компилятор поочерёдно пытается перебрать следующие касты:
1. const_cast
2. static_cast
3. static_cast + const_cast
4. reinterpret_cast
5. reinterpret_cast + const cast
> 

<aside>
💡

Из-за вот такого перебора возникает неоднозначность, какой каст сработает + перебор кастов занимает больше времени чем использование нужного каста сразу.

</aside>

```cpp
// Все касты помимо C-style выглядят так
int a = 5;
double b = static_cast<int>(a);

// C-style cast
b = (double)a;
```

## …additions

> Естественно, что разные классы и фреймворки имеют ещё свои как-то описанные касты, они не входят в стандарт, описывать их не буду
> 

<aside>
💡 Не стоит забывать про сопоставимость структур при касте. Возможно смещение по байтам, если этого не учитывать.

</aside>