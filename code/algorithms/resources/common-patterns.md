https://www.youtube.com/watch?v=RYT08CaYq6A&t=30s
https://www.youtube.com/watch?v=DjYZk8nrXVY&t=487s

**SHOULD SPLIT IN FILES**
# Базовая терминология
* **указатель, pointer, ptr ->** не обязательно именно указатель на память, в контексте структур данных это **может быть и просто индекс**
* **монотонный ряд ->** **ряд отсортирован в порядке возрастания или убывания**
* **аккумулирование ->** процесс накапливания "мощности", например временная переменная для хранения промежуточной суммы где-нибудь - накапливает сумму значений
# Prefix sum array
* Когда нужно **найти сумму подмассива чисел**
> По сути мы заранее формируем ряд из накапливаемой суммы (допустим P), а потом мы возвращаем разность значений из нового массива по индексам 
> `P[i] - P[j-1]`
>* где `-1` из логики про диапазоны `[включительно, исключая)` офк

```cpp
#include <vector>

class NumArray {
    std::vector<int> nums_;

public:
    NumArray(vector<int>& nums) 
        : nums_(nums.size())
    {
        //
        // For example we have array: 1, 2, 3, 4, 5
        // Here we form an array of accumulating character: 1, 3, 6, 10, 15
        // i.e. we accumulate bubble-sum values -> 0+1 = 1 , 1+2 = 3 , 1+2+3 = 6 ...
        // 
        std::partial_sum(nums.begin(), nums.end(), nums_.begin());
    }
    
    int sumRange(int left, int right) 
    {
        //
        // We already summed an array, so now we just use subtraction (formula) to get sum of a range of elements
        // sum[i, j] = P[j] - P[i-1]
        //

        return left == 0 
            ? nums_[right] 
            : nums_[right] - nums_[left - 1];           // <-- to be accurate, THIS is formula usage
    }
};
```

# Binary Search
* Поиск чисел
* **ТОЛЬКО** для **ОТСОРТИРОВАННЫХ** или **МОНОТОННЫХ** функций
* **временная сложность -> O(log n)**
> - Выбираются 3 позиции `low`(самое левое), `high`(самое правое), `mid` (прям середина)
> - Сравнивается желаемое значение и **mid**
> - Если **mid** (например) больше желаемого -> всё, что ниже **mid** - отбрасывается
> - Теперь **low** указывает на ранее **mid**, **mid** пересчитывается
> - Так делается, пока не найдём нужное

```python
def binary_search(nums: list[int], target: int) -> int:
    low, high = 0, len(nums) - 1

    while low <= high:
        mid = (low + high) // 2  # calc or recalc mid

        if nums[mid] == target:  # found
            return mid
        if nums[mid] < target:
            low = mid + 1        # shift low ~ drop anything <mid
        else:
            high = mid - 1       # shift high ~ drop anything >mid

    return -1


# Example
nums = [1, 2, 5, 9]
print(binary_search(nums, 5))  # -> 2
```
### tips
* вместо `mid = (low + high) // 2` для защиты от переполнения (например int) можно использовать `mid = low + (high - low) // 2`
# Find substring in string
## Boyer-Moore search
* Чем меньше длина подстроки -> тем ближе алгортим к О(n), предполагается, что для нас это плохо
* Формирование вспомогательной хэшмапы может быть дороже, чем простой перебор, если изначальная строка небольшая
> - Создаём хэшмапу со смещениями вида `char : смещение`
> - Смещение считается по формуле -> `shift = max(1, substring.len - i - 1)`, где `i` -  текущий индекс подстроки
> - Если буквы повторяются при формировании таблицы, то оставляем то значение, которое меньше всего выходит
> - Проходимся алгоритмом по строке, начиная сравнение с последней буквы подстроки
> - Если совпало - смотрим дальше
> - Если не совпало - смотрим на таблицу смещений и смещаем просмотр на нужное значение

```python
def boyer_moore_search(text: str, pattern: str) -> int:
    len_text, len_pattern = len(text), len(pattern)
    if len_pattern == 0:
        return 0

    # Python short way to build <char : shift> table
    shift_table = { char: max(1, len_pattern - i - 1) for i, char in enumerate(pattern) }

    i = 0
    while i <= len_text - len_pattern:
        sub_i = len_pattern - 1
        while sub_i >= 0 and text[i + sub_i] == pattern[sub_i]:
            sub_i -= 1  # *char hit*

        if sub_i < 0: # found
            return i
        else:         # *char miss*
	        # -> here we shift "main ptr" `i` using shift table
	        # -> bad_char is last *char hit* char in substring
            bad_char = text[i + len_pattern - 1]
            i += shift_table.get(bad_char, len_pattern)

    return -1


# Example
print(boyer_moore_search("HERE IS A SIMPLE EXAMPLE", "EXAMPLE"))  # -> 17 index
```
# Two pointers
> Уменьшаем кол-во проходов за счёт образования 2 "параллельных" обходов
## Classic
* **Обычно O(n)**
> Часто бывает, когда нужно найти **пару**, **палиндром**, двухсумму и т.п. 

```python
def find_pair_with_sum(sorted_nums: list[int], target: int) -> tuple[int, int] | None:
    """Find any pair of values with target sum in array"""
    left = 0
    right = len(sorted_nums) - 1

    while left < right:
        # main calculation
        current_sum = sorted_nums[left] + sorted_nums[right]

        if current_sum == target:  # hit
            return (sorted_nums[left], sorted_nums[right])
        if current_sum < target:   # miss
            left += 1  # shift left
        else:
            right -= 1 # shift right

    return None


# Example
nums = [1, 2, 3, 4, 6, 8, 9]
print(find_pair_with_sum(nums, 10))  # -> (2, 8)
```
## Fast-slow concept
> Один указатель, например, идёт по 1 шагу, а второй, например по 2
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None


def find_middle_node(head: Node) -> Node:
    slow_pointer = head
    fast_pointer = head

    # fast -> 2 steps per iteration
    # slow -> 1 step per iteration
    while fast_pointer and fast_pointer.next:
        slow_pointer = slow_pointer.next
        fast_pointer = fast_pointer.next.next

    return slow_pointer


# Example
# 1 → 2 → 3 → 4 → 5
a, b, c, d, e = Node(1), Node(2), Node(3), Node(4), Node(5)
a.next, b.next, c.next, d.next = b, c, d, e

middle = find_middle_node(a)
print(middle.value)  # -> 3 is middle
```
### sliding window
> Скользящее окно. 
> Используется, когда нужно найти **наибольшую/наименьшую последовательность** чего-либо

```python
def longest_subarray_under_sum(nums: list[int], max_sum: int) -> int:
	"""Find longest subarray with each-element-sum <= max_sum"""
    left = 0
    current_sum = 0
    longest = 0

    for right in range(len(nums)):
        current_sum += nums[right]

        while current_sum > max_sum:  # beyond limit
            current_sum -= nums[left] # eleminate value passed by left
            left += 1                 # shift `left ptr`

        longest = max(longest, right - left + 1)

    return longest


# Example
nums = [1, 2, 3, 4, 2, 1, 1]
print(longest_subarray_under_sum(nums, 7))  # -> 3
# 1, 2, 3
# 4, 2, 1
# 2, 1, 1
```
### Cycle detection
> Иногда называют Floyd's algorithm
> Суть в **проверке на цикличность** чего-либо.
```python
def has_cycle(head: Node) -> bool:
    slow_pointer = head
    fast_pointer = head

    while fast_pointer and fast_pointer.next:
        slow_pointer = slow_pointer.next
        fast_pointer = fast_pointer.next.next

        if slow_pointer == fast_pointer:
            return True   # Yep - it's cyclic

    return False


# Example with cycle
a, b, c, d = Node(1), Node(2), Node(3), Node(4)
a.next, b.next, c.next, d.next = b, c, d, b  # b -> ... -> d -> b === cycle

print(has_cycle(a))  # -> True
```
# Linked list in-place reversal
> Нужно развернуть в обратную сторону связный список. Делается это обычно так:
> - Создаётся/иммитируется пустая **dummy нода**
> - Возникают понятия **prev**, **curr**, **next**
> - Первая нода указывает на **dummy ноду**
> - Остальные просто `node.next = node.prev`
> - Возвращается указатель на именно первую ноду
```python
def reverse_linked_list(head: Node) -> Node:
	prev = None
	current = head
	
	while current is not None:
		next = current.next   # save next-node-ptr in temp
		
		current.next = prev   # reverse links
		prev = current        # now curr-node-ptr is considered prev 
		
		current = next        # move curr-node-ptr next
		
	return current            # now current is on the end of original list -> head
```
### tips
* Почти все задачи на linked list требуют использования **dummy ноды с null**
# Monotonic stack
> Приём в котором мы **сохраняем на стэке влияющие на ответы значения** (или их индексы).
> Часто вопрос **связан с поиском "найти ближайшее большее/меньшее"**.
```python
def next_greater_elements(nums: list[int]) -> list[int]:
	"""
	Find next greater element for every single element in nums.
	All results are returned in list as well.
	If there are no -> emplace -1.
	"""
    result = [-1] * len(nums) # filled output list wiht [-1, -1, -1...]
    stack = []                # function main "engine", contains INDEXES

    for right in range(len(nums)):
        # left & right -> related to `nums[int]` !!!
        # right ~ current
        
        # while stack is NOT empty 
        # .. + current element > the last in stack (stack top)
        while stack and nums[right] > nums[stack[-1]]:  # *hit*
            # remember, that we should emplace next greater value 
            # .. in the same place as value-waiter
            
            # stack.pop() - was the index of value
            # .. that was "waiting" for next greated value
            result[stack.pop()] = nums[right]  # emplace into result
            
        stack.append(right)   # casually adding everything

    return result


# Example
nums = [2, 1, 2, 4, 3]
print(next_greater_elements(nums))  # -> [4, 2, 4, -1, -1]
```
# Find K elements
> Brute force здесь - просто отсортировать и извлечь, но сортировка на больших массивах - дорогая операция
## K largest -> min-heap
* **O(k log n)** при `k << n` (размера кучи)
>Весь **массив засовываем в кучу** (min-heap) и просто pop() k-элементов
```python
import heapq

def k_smallest_elements(nums: list[int], k: int) -> list[int]:
    heapq.heapify(nums)                             # convert to heap
    
    return [heapq.heappop(nums) for _ in range(k)]  # extract k times
```
## K smallest -> max-heap
> Весь **массив делаем отрицательным и помещаем в кучу** (max-heap) и pop() k-элементов
```python
import heapq

def k_largest_elements_v2(nums: list[int], k: int) -> list[int]:
    max_heap = [-n for n in nums]                            # to negative
    heapq.heapify(max_heap)                                  # convert to heap
    
    return [-heapq.heappop(max_heap) for _ in range(k)]      # exract k times
```
* Есть ещё другой подход: делать min-heap с push-ом значений, но как только размер кучи будет достигать `k` -> делаем после вставки pop() из кучи, таким образом всякие минимальные значения тут будут извлекаться
```python
import heapq

def k_largest_elements(nums: list[int], k: int) -> list[int]:
    min_heap = []

    for num in nums:
        heapq.heappush(min_heap, num)
        
        if len(min_heap) > k:                # size outrange
            heapq.heappop(min_heap)          # drop heap's min value
            
    return sorted(min_heap, reverse=True)    # now we have only max values in heap
```
## K most frequent
> .. и в итоге через min-heap.
> Мы просто параллельно заводим хэшмапу, которая хранит `число : кол-во повторений`
* Многие ЯП умеют сравнивать tuples (ШОК), в 90% случаев этого достаточно
```python
import heapq
from collections import Counter

def k_most_frequent(nums: list[int], k: int) -> list[int]:
    freq = Counter(nums)            # hashmap -> num : counter 
    min_heap = []

    for num, count in freq.items():
        heapq.heappush(min_heap, (count, num)) # here freq would be compared first
        if len(min_heap) > k:                  # beyond range -> try drop
            heapq.heappop(min_heap)            # pop least frequency

    # descended order for frequency
    return [num for count, num in sorted(min_heap, reverse=True)]
```
# Basic graph/tree traversal
> Немного базовые понятия для обхода графов и деревьев...
## DFS
>**DFS** -> концепт для графов и деревьев, где обход идёт приоритетно в глубину, затем "откат" до некоторого узла и далее обход следующей ветки вглубь
* найти путь между 2 нодами
* проверить граф на цикличность
* найти топологический порядок в DAG
* подсчитать кол-во связанных узлов графа
### Recursive approach
```python
def dfs_recursive(graph, v, visited_nodes):
	visited_nodes.add(v)                  # remember vertex as visited
	print(v, end=" ")
	
	for neighbour in graph[v]:                           # traverse graph
		if neighbour not in visited_nodes:               # "we wasn't here"
			self.dfs_recursive(neighbour, visited_nodes) # recurse call for that 
	
	
#
# execution like:
#		
visited_nodes = set()
#...
dfs_recursive(graph, starting_point, visited_nodes)
```
### Iterative approach
* Берём стартовую ноду, помещаем её в стэк
* Извлекаем в цикле ноды со стэка **(main point)**
* Начинаем обход графа через for
* Нас интересуют только незнакомые вершины, их мы "помечаем" как "посещённые" (ну что-нибудь с ней делаем)
* Смотрим от этой вершины следующие через вложенный for
* Если вершину не посещали -> кладём в стэк **(main point)**
* Со стэка можно взять только последнюю положенную, поэтому пока есть куда вложиться -> мы будем постоянно брать в цикле все вершины на уровень ниже, если они есть
```python
def dfs_iterative(graph, v, visited_nodes): 
	stack = [v]                           # place starting point in stack
	
	while stack:                          # cycle
		vertex = stack.pop()              # extract vertex
		if vertex not in visited_nodes:   # "we wasn't here"
			visited_nodes.add(vertex)     # remember vertex
			print(vertex, end=" ")
			
			for neighbour in reversed(graph[vertex]):
				# we use `reverse` for better emplace values in stack..
				# cuz if we skip `reverse` -> pop()'s will return traversed order
				# .. and ofc it would corrupt our main traverse order
				if neighbour not in visited_nodes:  # "we wasn't here"
					stack.append(neighbour)         # remember vertex
					
					
#
# execution like:
#	
visited_nodes = set()
#...
dfs_iterative(graph, starting_point, visited_nodes)
```
### tip
* Важно отличать, что здесь мы используем **стэк как "буфер" с LIFO извлечением** из вершин графа
## BFS
> **BFS** -> концепт для графов и деревьев, где обход идёт приорететно по текущему уровню, и только потом идёт спуск до следующего уровня
* поиск кратчайшего пути
* обход по уровням сверху вниз
* поиск всех соединённых вершин
* поиск наименьшего кол-ва преобразований из одного слова в другое
```python
def bfs(graph, start)
	visited = set()                     # container where we remember visited nodes
	queue = deque([start])              # result output
	
	visited.add(start)                  # remember start node
	
	while queue:
		vertex = queue.popleft()           # extract node
		print(vertex, end=" ")
		
		for neighbour in graph[vertex]:
			if neighbour not in visited:   # "we wasn't here"
				visited.add(neighbour)     # 
				queue.append(neighbour)
```
* Берём стартовую вершину, помещаем её в очередь и помечаем как пройденную
* В цикле вытаскиваем из очереди вершины графа **(main point)**
* От этих вершин, соответственно, осматриваем следующие вершины (вложенный for)
* Нас интересуют только незнакомые вершины
* Незнакомые вершины мы помечаем как пройденные и помещаем в очередь **(main point)**
* Т.к. мы используем очередь, то мы берём вершины в том же порядке, что помещали их, таким образом мы приоритетно будет осматривать текущий уровень
### tip
* Важно отличать, что здесь мы используем **очередь как "буфер" с FIFO извлечением** из вершин графа
# Binary tree traversal
> Обход дерева по разным правилам:
* **Pre-order (NLR)**: Node -> Left -> Right 
* **In-order (LNR)**: Left -> Node -> Right
* **Post-order (LRN)**: Left -> Right -> Node

> Пример для NLR ниже. Для остальных просто меняется порядок вызовов функций.
```python
def preorder_traversal(node: BinaryTreeNode):
    if not node:
        return
        
    print(node.value, end=" ")   # Node
    preorder(node.left)          # Left
    preorder(node.right)         # Right```
```
* Есть ещё **Level-order (BFS)** -> тут **обход идёт по уровням**
	* т.е. в каждой итерации описывается текущий уровень (слева - направо), а потом переход к следующему уровню
	* В NLR, LNR, LRN - уже внутри одной итерации идёт попытка сразу опуститься глубже, т.е. они **DFS**, а level-order - **BFS**
```python
from collections import deque

def level_order(root: BinaryTreeNode):
    if not root:
        return
        
    queue = deque([root])             # used container for storing everything
    while queue:
        node = queue.popleft()        # pick leftmost level
        
        print(node.value, end=" ")
        
        if node.left:                 # pick it's children
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```
* Для сравнения обходов, вот у нас есть дерево:
```markdown
        1
      /   \
     2     3
    / \   /
   4  5  6
```

| Traversal order  | description                                              | Тип обхода | Associative                                           | Output      |
| ---------------- | -------------------------------------------------------- | ---------- | ----------------------------------------------------- | ----------- |
| Pre-order (NLR)  | Корень → Лево → Право                                    | DFS        | "Всегда идём налево"                                  | 1 2 4 5 3 6 |
| In-order (LNR)   | Лево → Корень → Право                                    | DFS        | "Проходимся вертикалью"<br>(слева направо)            | 4 2 5 1 6 3 |
| Post-order (LRN) | Лево → Право → Корень                                    | DFS        | "Начинаем с дочерних"<br>(слева направо, снизу веерх) | 4 5 2 6 3 1 |
| Level-order      | По уровням сверху вниз <br>(внутри уровня слева направо) | BFS        | "Идём строго сверху вниз"                             | 1 2 3 4 5 6 |
# Matrix traversal
> Очень полезно считать матричные вопросы - вопросами про графы, тут часто хватает DFS и BFS
## DFS (recursive approach)
```python
def num_islands_dfs(grid: list[list[str]]) -> int:
	""" Return islands number using DFS"""
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    visited = set()

	# 0 - water
	# 1 - land
    def dfs(r, c):
	    """Recursive DFS"""
        if (r < 0 or c < 0 or r >= rows or c >= cols  # matrix bounds check
            or grid[r][c] == '0'                      # if we are on water
            or (r, c) in visited):                    # or if visited field
            
            return

        visited.add((r, c))  # mark as visited
        
        # try mark closest lands as visited
        # DFS(recursive) core idea
        dfs(r + 1, c)  # up
        dfs(r - 1, c)  # down
        dfs(r, c + 1)  # right
        dfs(r, c - 1)  # left

    islands = 0
    for r in range(rows):
        for c in range(cols):
	        # if land and no visited - increment
            if grid[r][c] == '1' and (r, c) not in visited:
                dfs(r, c)        # analyze current a closest to it land fields
                islands += 1     # well.. such encounter means -> it's a new island

    return islands
```
## BFS approach
```python
def num_islands_bfs(grid):
	""" Return islands number using BFS"""
    if not grid:
        return 0

    rows, cols = len(grid), len(grid[0])
    visited = set()

    def bfs(r, c):
        queue = deque([(r, c)])              # BFS core idea
        
        while queue:
            row, col = queue.popleft()
            
            for delta_row, delta_col in [(1,0), (-1,0), (0,1), (0,-1)]: 
                # delta ~ shift
                # nr/nc -> actaul row/col + in-cycle shift 
                nr, nc = row + delta_row, col + delta_col
                
                # 0 - water
				# 1 - land
                if (0 <= nr < rows and 0 <= nc < cols      # matrix borders
	                and grid[nr][nc] == '1'                # interested in land's
	                and (nr, nc) not in visited):          # want unknown field
                    visited.add((nr, nc))          # mark as visited
                    queue.append((nr, nc))         # BFS core idea

    count = 0
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1' and (r, c) not in visited:
	            # find unknown land -> i.e. new island + mark as visited
                visited.add((r, c))
                bfs(r, c)
                count += 1

    return count

``` 
## tip
* Полезно считать матричные вопросы - вопросами на графы
# Backtracking
> Концепт поиска решения, если некий "путь" не ведёт к решению, то мы откатываемся "назад", т.е. мы сохраняем историю выбором, по которой и откатываемся
* найти всевозможные пути/комбинации/варианты/расстановки"
* наверное, это паттерн где почти всегда один и тот же паттерн кода:
```python
def backtrack(path, options):
    if условие_готовности:
        сохранить(path)
        return

    for выбор in options:
        сделать(выбор)
        backtrack(новый_path, новые_options)
        отменить(выбор)                         # <- rollback
```
* примерными задачами часто являются судоку и n-queens
## permutations example
> permutation - перестановка
* **используем set** для уникальных комбинаций, т.к. мы именно переставляем
```python
def permute(nums, path=None, used=None):
	"""
	Print all possible permutations.
	
	Arguments:
		path -> current working state/solution
		used -> container for permute-combos we tried for current solution
		nums -> victim array
	"""
    if path is None:
        path = []
        
    if used is None:
        used = set()

    if len(path) == len(nums):  # we've reached the end with this path
	    # that's a main job, we focus on meeting this condition
        print(path)             # <- basicly it's a function output 
        return                  # don't forget - we have recursion

    for num in nums:
        if num not in used:                      # if we didn't tried that option
	        # here we create another updated path for next permute() call
	        # we reached condition above? - clear current `used`
	        # .. and start from new next num
            used.add(num)                        # mark as "tried"
            permute(nums, path + [num], used)    # update solution path
            used.remove(num)                     # <- rollback!
```
## combinations
> Примерно то же самое, что и перестановки (permutations), только **вместо set у нас просто массив**
```python
def combine(nums, k, start=0, path=None):
	"""
	Find (and print) all combinations with k-length in nums
	
	Arguments:
		nums -> victim array
		k -> target len (main condition)
		start -> index in nums, from where we start making solution, we need this in recursive calls
		path -> current solution
	"""
    if path is None:
        path = []

    if len(path) == k:  # main condition
        print(path)     # <- function output
        return          # don't forget - we have recursion

    for i in range(start, len(nums)): 
        path.append(nums[i])               # we can repeat values
        combine(nums, k, i + 1, path)
        path.pop()
```
## subsets
> Примерно то же самое, что и комбинации (combinations), **только у нас нет условия**, по сути.. return'ом здесь будет являться естественное завершение функции
```python
def subsets(nums, start=0, path=None):
	"""
	Find (and print) all subsets of nums 
	
	Arguments:
		nums -> victim array
		start -> index in nums, from where we start making solution, we need this in recursive calls
		path -> current solution
	"""
    if path is None:
        path = []

    print(path)

    for i in range(start, len(nums)):  
	    # don't forget, that after path.pop() we still have in some recurse calls
	    # .. we still have next `i` iteration
        path.append(nums[i])
        subsets(nums, i + 1, path)
        path.pop()
```
# Dynamic programming
> Концепт, при котором мы разбиваем сложную задачу на маленькие под-задачи (**overlapping problems**), результат под-задач мы можем запоминать и переиспользовать повторно.
> Так образуется оптимальное решение на основе оптимальных под-задач - **optimal substructure**.
* fibonacci numbers
* kadane's algorithm
* 0/1 knapsack 
* longest common subsequence (LCS)
* longest increasing subsequence (LIS)
* subset sum problem
* matrix chain multiplication
## fibonacci
> Самая яркая концепция - tail recursion в, например, числах Фибоначчи. 
> Когда вместо рекурсивного вызова функции (что, по сути, "развернуть вычисления до N-го элемента, а потом схлопнуть одним сложным длительным вычислением суммы сразу всего ряда) мы решаем сразу вычислять суммы с самого начала ряда и двигаться к концу, т.е. мы  делаем мелкие вычисления и аккумулируем результат.
* Было так:
```python
def fib(n):
	""" Fib calc -> O(2^n) """
    if n <= 1:
        return n                 # end of recursion
        
    return fib(n-1) + fib(n-2)   # huge recursion stack
```
* Стало так:
```python
def fib_memo(n, memo=None):
	""" 
	Fib calc (tail recursion) -> O(n)
	
	Arguments:
		n - how many fib calcs do we need
		memo - output array with fib calcs 
	"""
    if memo is None:
        memo = {}
        
    if n in memo:
        return memo[n]
        
    if n <= 1:   # same target condition, but we use it "at start"
        return n
        
    # memo is also called here as `accumulator` 
    # .. usually it's a single value instead of container of all calcs
    # .. it accumulates sub-task calculations and is dropped into next sub-tasks
    # .. next sub-tasks do like: (memo := memo + k), and drop further
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    
    return memo[n]
```
## 0/1 knapsack 
> Есть `n` предметов, у каждого есть вес (weights) и ценность (values).  
> Надо выбрать подмножество, чтобы **максимизировать ценность**,  не превышая лимит по весу `W`.
* создаём строку из (W+1) нулей
* множим её до (n+1) размерности
* имеем теперь матрицу (n+1) х (W+1)
```python
dp = [
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0], 
]
# column = Weights
# rows = possible items
```
* по циклам проходим по кол-ву элементов и по весам (двумерный обход)
* ...
```python
def knapsack(weights, values, W):
    n = len(weights)
    dp = [ [0]*(W+1) for _ in range(n+1) ]

    for i in range(1, n+1):
        for w in range(1, W+1):
            if weights[i-1] <= w:
				dp[i][w] = max(
				    dp[i-1][w],                       # dont' pick item
				    dp[i-1][w - weight[i]] + value[i] # pick item
				)
            else:
                dp[i][w] = dp[i-1][w]                 # too heavy
                
    return dp[n][W]
```

# TODO
* Доописать dp
* Разбить всё на файлы