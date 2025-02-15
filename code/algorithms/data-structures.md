**Структура данных** — это способ организации информации для более эффективного использования. В программировании структурой обычно называют набор данных, связанных определённым образом. Например, массив — это структура.
Со структурой можно работать: добавлять данные, извлекать их и обрабатывать, например изменять, анализировать, сортировать. Для каждой структуры данных — свои алгоритмы. Работа программиста — правильно выбирать уже написанные готовые либо писать свои.

Главное свойство структур данных в том, что у любой единицы данных должно быть чёткое место, по которому её можно найти. Как определяется это место и как происходит поиск, зависит от конкретной структуры.

Разные структуры данных имеют разной степени преимущества в того или иного рода операциях, как оценивать ? -> [Big O notation](big-o-notation.md)

Примеры сложности операций некоторых видов структур данных:

| Контейнер \ операция   | Вставка элемента | Удаление элемента | Поиск элемента |
| ---------------------- | ---------------- | ----------------- | -------------- |
| Array                  | O(N)             | O(N)              | O(N)           |
| List                   | O(1)             | O(1)              | O(N)           |
| Sorted array           | O(N)             | O(N)              | O(logN)        |
| Бинарное дерево поиска | O(logN)          | O(logN)           | O(logN)        |
| **Хеш-таблица**        | O(1)             | O(1)              | O(1)           |

* [Hash-set](hash-set.md) / Или просто **set**
* [Hash-table](hash-table.md)
* [Trees](tree.md)
* [Graphs](graph.md)
* Linked list / Deque / Queue / Stack
  
# Linked list
> Каждый элемент (Node) содержит значение и указатель на следующую ноду.
> * Поиск по такому листу всегда O(N) -> т.к. надо по каждому элементу пройтись
> * Удаление и вставка первого элемента O(1) -> т.к. достаточно перезаписать значение head-node'ы.
> * Удаление и вставка элемента в остальных случаях O(n) -> т.к. до элемента надо сначала дойти (по факту это поиск)

```
// Псевдокод
struct Node {
  T    value;    // Текущее значение
  T    *next;    // "Указание" на следующее

  Node()
    : value(), next(nullptr)
  { }

  Node(value, next = nullptr)
    : value (value), next(next)
  { }
};
```

* Есть также `двунаправленный список`, который содержит "указание" не предыдущую ноду.

## Deque
> Двухсвязный список, где элементами могут являться уже последовательности данных.
> Т.е. где-то лежит в памяти вектор на 10 элементов, последний элемент такого вектора ссылается на вектор в другом месте памяти на 60 элементов, у которого последний элемент тоже ссылается на следующий кусок данных и т.п.

## Queue
> Двухсвязный список, работающий по принципу FIFO (First in first out).

## Stack 
> Двухсвязный список, работающий по принципу LIFO (Last in first out).

### Tips
* Когда использовать однонаправленный список? - В явном виде... никогда? Очередь и стэк реализованы с помощью это списка, если нужна какая-то необычная функциональность типа circular buffer (https://en.wikipedia.org/wiki/Circular_buffer), то можно модифицировать linked list
* Когда использовать двунаправленный список? - Одна из явных ассоциаций для применения это страница в бразуере и функциональность <вперёд - назад>. Т.е. работа только с ближайшими значениями от некоторой позиции.
* Когда использовать дек? - Когда нужен вектор, в котором помещение новых данных не вызывает реаллокацию (перекопирование) уже записанных данных.

# Hash ring
> Используется, в основном, для сессий и в рамках cdn.
https://www.geeksforgeeks.org/consistent-hashing/
