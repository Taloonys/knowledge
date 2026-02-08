---
tags: [code, languages]
aliases: [Language]
---

# Language
* Динамическая строгая типизация
* Интерпретированный, но есть и компиляционные механизмы типа CPython
* python2 и python3 - сильно разные вещи
* `.ipynb` - как смысл жизни для аналитиков и млщиков
* Сборщик мусора на основе подсчёта ссылок
# Import packages
* Есть пакетный менеджер pip для установки всяких пакетов
```bash
pip install pandas
# or
py -m pip install pandas
# `-m` stands for module
```
* Важно пользоваться виртуальный окружением при работе с пакетами, которые не хочется оставлять прям у себя на пк
```bash
# установить бы venv через pip
py -m venv ./.venv
# создастся папка .venv в текущей директории, можно назвать как угодно

./.venv/Scripts/Activate.bat
# в терминале должна появиться подпись (venv)
```
# Package transfer to end-user
* Старый добрый pip
```bash
### Сохранение зависимостей
# Сохранить зависимости в файл через pip
pip freeze > requirements.txt 
# ИЛИ можно использовать pigar либу, которая будет строго импорты записывать, а не всё подряд

### Использование
# Создать виртуальное окружение (опционально)
py -m venv .venv
./.venv/start.sh

pip install -r requirements.txt
```
* setup.py
* pyproject.toml + poema
# FAQs / nature diff
## chain comparison
```python
55 == True is True
# posssible thought
(55 == True) is True
# BUT for python it's equal to
(55 == True) and (True is True)
```
## inheritance theme
```python
class A:
    def hello(self):
        print("A")

class B(A):
    def hello(self):
        print("B")

class C(A):
    def hello(self):
        print("C")

class D(B, C):
    pass

D().hello()
# Python use virtual inheritance here, 
# so it will use first possible candidate for diamond inheritance problem
# output >> B
```
## hashing in collections
> Python has mutable and immutable objects
```python
{("a", [1, 2]): "value"}
# [] - is list, list is immutable by nature
# immutable objects aren't hashable
# >>> this line is invalid
```
## mutable/immutable stuff
> Python interpreter often tries to:
> * re-use even mutable objects
> * refer same immutable object to the same place in process' virtual memory 
```python
def append_to_list(item, my_list=[]):    
	my_list.append(item)    
	return my_list
	
print(append_to_list(1))
print(append_to_list(2))

# 'my_list' is mutable, it will be reused here
# >>> [1]
# >>> [1, 2]
```
* btw **better practice** instead of **function default args** is:
```python
def append_to_list(item, my_list=None):    
	if my_list is None:        
		my_list = []    
		
	my_list.append(item)   
		
	return my_list
```
## comprehension
> List/set/dict comprehension is built-in python feature for short and concise "array-like stuff" generation.
```python
print([x for x in range(3)])   # list comprehension
print((x for x in range(3)))   # generator
print({x for x in range(3)})   # set comprehension

# also generator comprehension is lazy
# .. it's print won't return 0, 1, 2

print({x: x*2 for x in range(3)})   # dict comprehension
```
* generator is mostly used for "iterate over once", use comprehensions instead in other cases, cuz generator can't do some basic stuff