# Algorithms with STL
- Всё это находится в `<algorithm>` или ` <numeric>`
- Если для кого-то нет примера с предикатом, то почти наверняка метод всё же позволяет его задать
- Возможно, где-то пропустил иные варианты методов
- Если не указана сложность, то скорее всего она основана на примитивном алгоритме и составляет, условно O(N) по времени.

### Tips
- try Haskell, функциональное программирование как смысл жизни


# for_each
> Вызывает функцию для каждого элемента последовательности

```cpp
std::vector v { 3, 2, 3, 4 };
auto print = [](int x) { std::cout << x; };
std::for_each(v.begin(), v.end(), print);
```

# search, search_n
> search → Ищет вхождение одной последовательности в другую последовательность.
> search_n → Возвращает итератор на начало последовательности из n одинкаовых элементов или end.


```cpp
auto it = search_n(data.begin(), data.end(), howMany, value);
```

# missmatch
> Возвращает пару итераторов на первое несовпадение элементов двух последовательностей.

```cpp
std::vector x { 1, 2 };
std::vector y { 1, 2, 3, 4 };

auto pair = std::mismatch(x.begin(), x.end(), y.begin());

// pair.first == x.end()
// pair.second = y.begin() + 2
```

# count, count_if
> Вернуть счётчик (на основе условия) на основе некоторого диапазона.
```cpp
std::vector x {1, 2, 3};
std::count_if(x.begin(), x.end(), [](auto val) {
  return val % 2 == 0;                                // Признак чётности числа
});  
```

# transform
> Преобразовывает данные одной последовательности по заданному алгоритму в данные иной последовательности.
```cpp
char to_uppercase(unsigned char c) {
    return std::toupper(c);
}

std::transform(hello.cbegin(), hello.cend(), hello.begin(), to_uppercase);
```

# find, find_if, find_if_not
> Возвращает итератор на найденный элемент. Сложность по времени в худшем случае O(N).
```cpp
std::vector x {1, 2, 3};

std::find(x.begin(), x.end(), 3);                     // Найти 3

std::find_if(x.begin(), x.end(), [](auto val) {       // Найти по условию предиката
  return val % 2 == 0;                                // Признак чётности числа
});  
```

# accumulate, reduce
> Возвращает применяемую функцию на весь диапазон, по умолчанию -> суммирует.
- accumulate считает всё по порядку друг за другом
- reduce можно вынести в параллелизм и требует лишь примитивной связности списка
```cpp
std::vector x {1, 2, 3};

int sum = std::accumulate (x.begin(), x.end(), 0);     // `0` is like an init value for inner "summ param"
```

# partial_sum
> Позволяет применять кумулятивные(накапливаемые) вычисления и выдавать их в другой контейнер (или сразу в поток).
см примеры: https://en.cppreference.com/w/cpp/algorithm/partial_sum

# adjacent_difference
> Позволяет проводить вычисление на основе смежных элементов (и выводить в некоторый контейнер)
```cpp
#include <iostream>
#include <algorithm>
#include <numeric>
#include <iterator>


int compute(int x, int y) {
    return x + y;
}


int main() {
    std::vector v = { 1, 2, 3, 5, 9, 11, 12 };
    
    std::adjacent_difference(v.begin(), v.end(), 
                             std::ostream_iterator<int>(std::cout, " "),                   // "Трюк" с выводом результата сразу поток, а не в переменную
                             compute);
}

// получим 1 3 5 8 14 20 23
// 0 + 1 = 1, 1 + 2 = 3, 2 + 3 = 5 ... 11 + 12 = 23
```

# shuffle, random_shuffle
> Помогает мешать (порядок) значения в заданном диапазоне
https://en.cppreference.com/w/cpp/algorithm/random_shuffle

# reverse
> Реверсирует значения в заданном диапазоне
```cpp
    std::vector<int> v = {1, 2, 3, 4, 5};
    std::reverse(v.begin(), std::prev(v.end(), 2));   // реверсируем от (начала) до (конец-2)
    for (const auto& val : v) printf("%d ", val);     // 3 2 1 4 5
```

# all_of, any_of, none_of
> Возвращает элементы bool, если все элементы подходят под условие, хотя бы какое-то подходит или ни один не подходит соответственно.
https://en.cppreference.com/w/cpp/algorithm/all_any_none_of

# copy, copy_if
> Копирует элементы из одного диапазона в другой (copy_if для функциональщины соответственно)
https://en.cppreference.com/w/cpp/algorithm/copy

# iota
> Позволяет заполнить диапазон значений по предсказуемой последовательности. (0 1 2 3..., a b c d ...)
```cpp
    std::vector<int> v(5);                            // вектор из 5 элементов (не путать с capacity)
    std::iota(v.begin(), v.end(), 0);                 // заполняем от 0 до конца
    for (const auto& val : v) printf("%d ", val);     // 0 1 2 3 4
```
- есть аналог в С++20 в либе std::views::iota, но там про ranges и покруче

# generate, generate_n
> Позволяет заполнять значения сразу в некоторый диапазон
```cpp
    std::vector<int> v(5);
    std::generate(v.begin(), v.end(), [n = 0] () mutable { return n++; } );            // эквивалент std::iota
```

# fill, fill_n
> Позволяет заполнить диапазон значениями
```cpp
    std::vector<int> v(5);
    std::fill(v.begin(), v.end(), 5);            // v = { 5 5 5 5 5 }
```

# inlcusive_scan, exlusive_scan
> Примерно то же самое, что и `partial_sum`, но применямые функции не будут строго ассоциативными ( (a+b) + c == (a+c) + b, вспоминаем, что в передаваемой лямбде 2 входных параметра в предикате может быть)
> exclusive_scan же просто не включает не включает i-й элемент в i-ю "сумму". (т.е. i0 и in, например, не будут включаться в результат)
https://en.cppreference.com/w/cpp/algorithm/inclusive_scan
https://en.cppreference.com/w/cpp/algorithm/exclusive_scan
