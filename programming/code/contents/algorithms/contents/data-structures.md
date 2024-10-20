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