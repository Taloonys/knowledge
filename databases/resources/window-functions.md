* Понравился в качестве источника примеров: https://youtu.be/phIR9W0yIaE?si=cG5hcgOwSogI9U_r

> Оконная функция - функци, которая может выполнять операции с выделенным набором строк (**окном** или же **partition**).
# Syntax
```sql
-- Использование внутри SELECT FROM чисто для примера
SELECT
    столбцы,
    оконная_функция()                             -- функция начинается тут
		[необязательно выражение для фильтрации]  -- можно не указывать
	    OVER (                                    -- ключевое слово для неё
	        [PARTITION BY столбец1, столбец2] 
	        [ORDER BY столбец3] 
	        [ROWS/RANGE ограничение_окна]
    ) AS имя_столбца
FROM таблица
```
# Evaluation order
![](Pasted%20image%2020250506212758.png)
# Types
![](image-storage/Pasted%20image%2020250506212339.png)
## Агрегирующие функции
> Агрегируют значения по указанному столбцу без свёртки как у `GROUP BY`!
![](image-storage/Pasted%20image%2020250506212226.png)
* Ключевой синтаксис - `OVER` + `PARTITION BY`
![](image-storage/Pasted%20image%2020250506212541.png)
## Ранжирующие функции
* Ключевой синтаксис - `OVER` + `ORDER BY`
```sql
-- Микро заметка: RANK оставляет "дыры" (1,2,2,4), DENSE_RANK — нет (1,2,2,3)
SELECT 
    student, score,
    RANK() OVER (ORDER BY score DESC) AS rank
FROM exam_results;
```
## Функции смещения
> Позволяют перемещаться по таблице и обращаться к соседним или крайним в таблице строкам. 
* Ключевой синтаксис - `OVER` + `PARTITION BY` + `ORDER BY`
```sql
-- Сравнение зарплаты с предыдущей в отделе
SELECT 
    name, department, salary,
    LAG(salary) OVER (PARTITION BY department ORDER BY hire_date) AS prev_salary
FROM employees;
```