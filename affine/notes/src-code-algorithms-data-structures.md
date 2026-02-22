---
tags: [code, algorithms]
aliases: [Hash ring]
---

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
* [Linked list](linked-list.md)/ Deque / Queue / Stack
  

# Hash ring
> Используется, в основном, для сессий и в рамках cdn.
https://www.geeksforgeeks.org/consistent-hashing/
