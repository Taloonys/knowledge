 > Это математическая абстракция реальной системы любой природы, объекты которой обладают парными связями. 
> Каждый **узел графа** может называться **вершиной**, **связи между узлами** графа называются **рёбрами**.
> **Весом ребра** называется значение, поставленное для ребра, в некоторой степени интерпретируемое как “**длина ребра**” (см. рис. ниже).

![Untitled](image-storage/Untitled%2019.png)

Иллюстрация примеров из задач:

![Untitled](image-storage/Untitled%2020.png)

Графы могут характеризоваться следующими свойствами:

- **Ориентированный граф** (все рёбра имеют некоторое одно направление)
    
    ![Untitled](image-storage/Untitled%2021.png)
    
- **Неориентированный граф** (все рёбра имеют 2 направления)
    
    ![Untitled](image-storage/Untitled%2022.png)
    
- **Связный граф** (между любой парой узлов есть *хотя бы* 1 путь)
    
    ![Untitled](image-storage/Untitled%2023.png)
    
- **Несвязный граф** (между хотя бы одной парой узлов нет пути/ребра)
    
    ![Untitled](image-storage/Untitled%2024.png)
    
- Почему нам не хватает представления в виде матрицы/2D-массива?
    
    Ниже представлены примеры того, как это можно было сделать, и что тут не так…
    
    ![Untitled](image-storage/Untitled%2025.png)
    
    ![Untitled](image-storage/Untitled%2026.png)
    
    ![Untitled](image-storage/Untitled%2027.png)
    

## Построение графа

**Узел** будет представлять в себе элемент, хранящий в себе:

- Значение узла (может быть любого типа, естественно)
- Список рёбер (которые к нем примыкают)
- Список родителей  (узлы, которые по уровню “выше”)

![Untitled](image-storage/Untitled%2028.png)

**Ребро** будет представлять собой абстрактный элемент, хранящий в себе:

- Узел, на который ведёт это ребро (на 1, если однонаправленное ребро, на 2 если, двунаправленное)
- Вес ребра

![Untitled](image-storage/Untitled%2029.png)

Целиком граф будет представлять из себя хэш-таблицу, со значением-ключом узла и ссылкой  на сам узел.

![Untitled](image-storage/Untitled%2030.png)

![Untitled](image-storage/Untitled%2031.png)

## Методологии поиска путей и обхода

### Поиск в глубину

> В общем это выглядит как обход слева-направо из пункта с деревьями. Создаётся вторая хэш-таблица для сохранения пройденных узлов, чтобы не попасть в петли и происходит обход “вниз” по каждой “ветке” слева-направо.
> 

![Untitled](image-storage/Untitled%2032.png)

И если граф несвязный, то добавляем “обёртку”, где мы проходим каждый узел без исключения.

![Untitled](image-storage/Untitled%2033.png)

Выше проиллюстрированы примеры обхода с помощью рекурсивного подхода, если же граф огромный, то с таким подходом вполне возможно переполнение стека, тогда стоит обернуть рекурсивный вызов своим собственным стеком, который будет очищаться, если по текущей ветке обхода мы не находим результат (ссылаясь на механизм наличия ребёнка/родителя).

![Untitled](image-storage/Untitled%2034.png)

Подобным методом возможен поиск пути до требуемого элемента, однако данный метод не позволяет эффективно найти самый короткий путь, т.к. им придётся перебирать сначала все пути, а потом уже искать по сформированной хэш-таблице путей самый короткий…

### Поиск в ширину

> Данный метод позволяет найти кратчайший путь путём обхода каждого “уровня” графа и всё так же основываясь на механизме наличия у узла ребёнка. Для такого метода так же заводится вспомогательная структура данных как очередь, которая записывает туда каждый “уровень” графа. В цикле обхода с очередью исключается родитель только тогда, когда у него есть ребёнок, тогда в очередь записывается ребёнок с уровня ниже вместо родителя, далее происходит переход на следующий элемент рассматриваемого уровня и т.д.

![Untitled](image-storage/Untitled%2035.png)

Для сохранения истории обхода узлов в целях поиска кратчайшего пути можно использовать обёртку  как в иллюстрации ниже:

![Untitled](image-storage/Untitled%2036.png)

<aside>
💡 Поиск в ширину позволяет найти кратчайший путь до требуемого элемента, однако он так же обходит весь граф, хоть и в меньшей степени, чем поиск в глубину. Одной из методологий ускорения поиска в обоих случаях (в глубину и ширину) можно использовать усложнённый **алгоритм двунаправленного поиска пути**, где описанные ранее поиски будут происходить из стартовой точки графа и конечной одновременно.

</aside>

### Алгоритм Дейкстеры

> Данный алгоритм придуман для поиска кратчайшего пути на основе оценки веса рёбер. 
> **Важным условием** работы алгоритма является то, что **веса всех рёбер графа** **положительны**.
> 

В данном алгоритме сначала происходит оценка весов прилегающих рёбер, на наименьший по весу ребру происходит переход к следующему узлу и так вплоть до конечной точки. Результаты весов рёбер между точками сохраняются. 

После же на всякий случай проверяются оставшиеся не рассмотренные пути.

![Untitled](image-storage/Untitled%2037.png)

Чтобы узнать, по каким узлам мы проходили, происходит оценка того результата суммарного веса рёбер пути, по которому мы прошли, и сравнивается с весами прилегающих рёбер конечной точки, и происходит такой же проход в обратном порядке до стартовой точки, только запоминаются пройденные узлы.

![Untitled](image-storage/Untitled%2038.png)

![Untitled](image-storage/Untitled%2039.png)

В алгоритме с самого начала все узлы графа сохраняются в список не пройденных, а весом для рёбер для прохода до каждого узла берётся самого максимальное в графе.

![Untitled](image-storage/Untitled%2040.png)

Далее происходит проход до конечной точки.

![Untitled](image-storage/Untitled%2041.png)

И проход обратно до стартовой точки.

![Untitled](image-storage/Untitled%2042.png)

- Использованные источники
    [https://www.youtube.com/watch?v=VehB3eglQMQ&ab_channel=AlekOS](https://www.youtube.com/watch?v=VehB3eglQMQ&ab_channel=AlekOS)