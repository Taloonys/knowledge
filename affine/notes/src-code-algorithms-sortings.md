---
tags: [code, algorithms]
aliases: [Bubble sort]
---

# Bubble sort
> - Сравниваются соседние элементы
> - наибольший из двух "всплывает" наверх (т.е. ставится, например, на вторую позицию в паре)
> - проход по всему массиву идёт несколько раз

![](../../../images/code__algorithms__1_uIng6XTBgXVGMF-y1O1GjQ.png)
* В худшем и среднем случае: **O(n²)**
* В лучшем (уже отсортированный массив): **O(n)**
```python
def bubble_sort(arr):
	n = len(arr)
	for i in range(n):                                    # 'top traverse
		for j in range(n - i - 1):                        # 'from current traverse
			# n - i => cuz every iteration last `i` elements would be greatest
			# n - i - 1 => cuz below we use `j + 1` 
		    if arr[j] > arr[j + 1]:                       # comparison
                arr[j], arr[j + 1] = arr[j + 1], arr[j]   # less, greater
```
# Selection sort
> - Находим минимальный элемент в массиве, ставим его в начало
> - ищем следующий минимальный элемент в оставшейся части массива - ставим следующим
> - и т.д.

![](../../../images/code__algorithms__selection-sort.png)
* Всегда вроде **O(n²)**
```python
def selection_sort(arr):
	n = len(arr)
	for i in range(n):                                   # 'top travers
		tmp_min = i                                      # fixate first current min
		for j in range(i + 1, n):                        # 'from next pos traverse
			if arr[j] < arr[tmp_min]:                    # found smallest
				tmp_min = j                              # last sorted min
			arr[tmp_min], arr[j] = arr[j], arr[tmp_min]  # swap target positions
```
# Insertion sort
> - Предполагаем, что первый элемент отсортирован
> - Смотрим следующий, если он меньше, то вставляем его в начало
> - Если следующий элемент больше, то считаем, что он на правильном месте
> - Делаем до конца

![](../../../images/code__algorithms__Insertion-Sorting-Algorithm.webp)
* Худший случай: **O(n²)**
* Лучший случай (почти отсортировано): **O(n)**
```python
def insertion_sort(arr):
	n = len(arr)
	for i in range(1, n):  # assume that first is sorted -> thus skipped
		current = arr[i]   # save main iterator
		j = i - 1          # j is previos
		while j >= 0 and j > сurrent:  # actaul bounds and prev is smaller
			arr[j+1] = arr[j]          # 1-st stage of swap
			j -= 1                     # index back
		arr[j+1] = current             # 2-nd stage of swap
```
# Merge sort
> - Делим массив пополам
> - Если можно делим его пополам дальше
> - Все такие разложенныые кусочки мы постепенно мёрджим с сортировкой обратно

![](../../../images/code__algorithms__7.png)
* Все случаи: **O(n log n)**
* Но вот по памяти уже **O(n)**
* Порядок равных элементов относительно друг друга сохраняется
```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2   # divide array
        left = arr[:mid]      # left part
        right = arr[mid:]     # right part

        merge_sort(left)      # recursive calling for both parts
        merge_sort(right)     # ..

        i = j = k = 0
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                arr[k] = left[i]
                i += 1
            else:
                arr[k] = right[j]
                j += 1
            k += 1

        while i < len(left):
            arr[k] = left[i]; i += 1; k += 1
        while j < len(right):
            arr[k] = right[j]; j += 1; k += 1
```
# Quick sort
> - В массиве выбирается некоторый опорный элемент (**pivot**), выбор может быть любого рода
> - На основе опорного элемента массив делится на части, где в одной все меньше pivot, а во второй все больше pivot
> - Повторяется выбор с этим самым выбором и разбиением от pivot
> - Результатом является цепочка вот этих вот оставшихся одиноких опорных значений после кучи рекурсивных вызовов

![](../../../images/code__algorithms__seqqs.drawio.svg)
- Средний случай: **O(n log n)**
- Худший случай (если pivot плох): **O(n²)**
```cpp
//
// Code from geeksforgeeks.org
//

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // choose the pivot
  
    // шndex of smaller element and indicates 
    // the right position of pivot found so far
    int i = low - 1;

    // Traverse arr[low..high] and move all smaller
    // elements on left side. Elements from low to 
    // i are smaller after every iteration
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    // move pivot after smaller elements and
    // return its position
    swap(arr[i + 1], arr[high]);  
    return i + 1;
}

// the QuickSort function implementation
void quickSort(vector<int>& arr, int low, int high) {
  
    if (low < high) {
      
        // pi is the partition return index of pivot
        int pi = partition(arr, low, high);

        // recursion calls for smaller elements
        // and greater or equals elements
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```
# Counting sort
> - Берём массив чисел, в котором нам **важно заранее знать диапазон значений**
> - Составляем массив частот повторений каждого числа, где индекс - само число, а значение по индексу - сколько раз встретили в исходном массиве
> - Проходимся по исходному массиву и считаем кто сколько раз встретился, заполняя массив частот
> - Далее просто восстанвливаем массив на основе встреченных индексов и в том кол-ве, в котором запомнили
* К слову, все сортировки ранее были "*сравнивающими*", т.к. использовали `<` и `>`, здесь же мы их не используем, поэтому эта сортировка и далее - "не *сравнивающие*"
![](../../../images/code__algorithms__1_lmzbRxK0FNgxzu8owsm4cA.png)
* O(n + k), где k - диапазон значений
```python
def counting_sort(arr):
    if not arr:
        return arr

    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val + 1

    count = [0] * range_val

    for num in arr:
        count[num - min_val] += 1

    for i in range(1, range_val):
        count[i] += count[i - 1]

    output = [0] * len(arr)
    for num in reversed(arr):
        count[num - min_val] -= 1
        output[count[num - min_val]] = num

    return output

```
# Bucket sort
> - Создаётся пачка бакетов
> - Значения попадают в разные бакеты
> - Внутри бакетов происходит сортировка этих самых групп значений (обычно insertion sort)
> - Происходит сбор значений со всех бакетов, они будут в уже отсортированном виде

![](../../../images/code__algorithms__bucket-sort-with-integer-elements.webp)
* O(n + d), где d - дисперсия значений (например 0..9 -> d = 10)
```python
def bucket_sort(arr):
    if not arr:
        return arr
        
    bucket_count = 10
    min_val, max_val = min(arr), max(arr)
    bucket_size = (max_val - min_val) / bucket_count

    buckets = [[] for _ in range(bucket_count)]
    for num in arr:
        index = int((num - min_val) / bucket_size)
        if index == bucket_count:  # edge case
            index -= 1
            
        buckets[index].append(num)

    for bucket in buckets:
        insertion_sort(bucket)

    return [num for bucket in buckets for num in bucket]

```
# Radix sort
> Фактически bucket sort, только применяется дважды, сначала по младшим разрядам чисел, а потом по старшим разрядам чисел

![](../../../images/code__algorithms__Radix-Sort.jpg)

```python
def counting_sort(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10

    for num in arr:
        index = num // exp
        count[index % 10] += 1

    for i in range(1, 10):
        count[i] += count[i - 1]

    for num in reversed(arr):
        index = num // exp
        output[count[index % 10] - 1] = num
        count[index % 10] -= 1

    for i in range(n):
        arr[i] = output[i]


def radix_sort(arr):
    max_num = max(arr)
    exp = 1
    while max_num // exp > 0:
        counting_sort(arr, exp)
        exp *= 10
```
* только для integers, можно адаптировать для floats некоторыми трюками
* быстрее quick sort на элементах больше 100 (8-bit base) и на элементах больше 10000 (16-bit base)
