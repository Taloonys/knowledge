---
tags: [code, algorithms]
aliases: [Оглавление]
---

# Оглавление
* [Algo-math](algo-math.md)
* [Big O notation](big-o-notation.md)
* [Data structures](data-structures.md)
* [Common patterns](common-patterns.md)
* [Sorting methods1 - old](sorting.md)
* [Sorting algorithms2 - new](sortings.md)
* [Too advanced (never gonna be used in interviews)](adv-algo.md)
# Real cases
| Общий концепт                                   | Про что                                              | обычно черз..<br>(убрать потом)   | мои попытки...             |
| ----------------------------------------------- | ---------------------------------------------------- | --------------------------------- | -------------------------- |
| **LRU Cache / LFU Cache**                       | Реализация кеша с вытеснением по частоте или времени | `HashMap + List / Heap`           | [here](lru-lfu-caching.md) |
| **Min Stack / Max Stack**                       | Стек с возможностью быстро узнать минимум/максимум   | `Stack + Stack`                   |                            |
| **Sliding Window / Moving Average**             | Окно фиксированной длины по данным потока            | `Deque`                           |                            |
| **Top K Elements / Frequent Words**             | Часто встречающиеся элементы                         | `HashMap + Min-Heap`              |                            |
| **Median of Data Stream**                       | Поддержание медианы при добавлении элементов         | `Max-Heap + Min-Heap`             |                            |
| **Autocomplete / Prefix Search**                | Поиск по префиксу                                    | `Trie`                            |                            |
| **Union-Find (Disjoint Set)**                   | Определение связных компонент                        | `Parent Array + Path Compression` |                            |
| **Scheduler / Task ordering**                   | Топологическая сортировка, приоритеты                | `Graph + Heap`                    |                            |
| **Rate Limiter**                                | Ограничение запросов в единицу времени               | `Queue / Heap + Timestamp`        |                            |
| **Consistent Hashing**                          | Балансировка по нодам                                | `Ring + Binary Search`            |                            |
| **Least Recently Used Stack / Browser History** | Undo/Redo                                            | `Two Stacks`                      |                            |
| **Randomized Set / GetRandom() in O(1)**        | Поддержка случайного выбора                          | `Vector + HashMap`                |                            |
| **Interval Merging / Calendar Booking**         | Обработка пересечений отрезков                       | `Sort + PriorityQueue`            |                            |
| **Design Twitter / News Feed**                  | Мини-социальная сеть                                 | `Heap + HashMap + LinkedList`     |                            |
