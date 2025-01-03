> Если у вас небольшой и понятный массив, то ничто не мешает взять встроенную функцию языка программирования типа sort (). Она пошуршит каким-то своим алгоритмом и вернёт отсортированный массив, однако, для большого массива данных могут возникать разного рода проблемы, связанных с многопоточностью ПО или же техническими возможностями, например. Для подобного стоит использовать специализированные алгоритмы сортировки и бинарного поиска, дабы оптимизировать весь этот процесс.

# Существует множество видов сортировки, самые стандартные из них:

## **Сортировка пузырьком**
    
Данный вид сравнивает попарно значения. Очевидно, что в каждом проходе по множеству самое большое значение ставится в самый конец, в следующем проходе происходит тоже самое, но уже для множества на 1 элемент меньше (тот самый большой по значения из прошлого прохода). Плохие результаты при работе с большими значениями, только учебный характер.

![Untitled](image-storage/Untitled.png)
    
# **Сортировка выбором**

Данный вид сравнения берёт самый первый элемент множества и попарно сравнивает его со всеми остальными, если в таком сравнении находится элемент меньше, чем первый, то сравнивается уже он со всеми последующими. Если сравниваемый элемент за 1 проход изменился, то он ставится в начало как самый наименьший и исключается из следующего прохода. Наименьшее кол-во операций, если считать перестановки. Т.е. меньше всего перезаписей.

![Untitled](image-storage/Untitled%201.png)
    
## **Сортировка вставками**

В данном виде сортировки сначала берётся первая какая-то часть элементов множества и считается, что она уже отсортирована. Затем берётся самый последний элемент этой части и сравнивается с последующими элементами, если последующий элемент оказывается меньше, то последний элемент “отсортированной” группы меняется местами с таким элементом, а отсортированная группа в следующем проходе уменьшается на 1 позицию, дабы последнее помещённое в неё число встало на своё место внутри отсортированной группы. Если же последующий элемент больше наибольшего элемента отсортированной группы, то размер отсортированной группы увеличивается на 1, а такой элемент становится набольшим в этой группе. Наименьший результат, если считать по кол-ву сравнений. Т.е. полезно если важнее всего чтение.
    

![Untitled](image-storage/Untitled%202.png)

![Untitled](image-storage/Untitled%203.png)
## **Сортировка Хоара**

В данной сортировке берётся какой-то опорный элемент в центре массива, а так же обработка такого множества будет содержать “указатели” на первый и последний элемент массива, задача “указателя” в начале массива искать элементы большие или равные опорному, задача “указателя” в конце массива искать меньшие или равные опорному… такие элементы меняются местами друг с другом, под такое может попасть и сам опорный элемент. Проходы будут длиться до тех пор, пока такие “указатели” не перепрыгнут друг друга. Когда это случится, множество будет разбито на 2 подмножества, где каждый элемент одного подмножества будет, например, меньше элементов другого (3.1.4.2 И 9.3.5.4). Дальше происходит всё то же самое в каждом из этих подмножеств. После 32 элементов работает быстрее, чем прошлые сортировки.

![Untitled](image-storage/Untitled%204.png)

![Untitled](image-storage/Untitled%205.png)

## **Сортировка Ломуто**

Это одна из модификаций сортировки Хоара, в которой опорным элементом будет являться самый последний элемент массива. А “указатели” начала и конца массива будут стартовать с самого начала, только задачей “указателя” начала массива будет поиск элементов строго больше опорного значения, а задачей “указателя” конца массива будет поиск значений всё так же меньше или равных опорному. При разбиении на 2 под-массива опорное значение помещается в самое начало правого под-массива, таким образом все значения этого под-массива будут больше опорного значения, текущее разбиение повторяет описанную выше логику. 

![Untitled](image-storage/Untitled%206.png)

![Untitled](image-storage/Untitled%207.png)