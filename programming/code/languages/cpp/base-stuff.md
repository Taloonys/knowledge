# Синтаксические справки

>Каждая операция должна закрываться `;`
>Область видимости обозначается `{ }`
>Условия должны быть в `()`
>Закомментировать строку `//` 
>Закомментировать область `/* текст */`

```cpp
void QWidget::keyPressEvent(QKeyEvent *event) // Да, Qt
{
    switch (event->key()) // пример комментария
    {
    case KeyboardKey::KEY_F1:
        emit closeRequest(); // emit - Qt метод
    case KeyboardKey::KEY_F2:
		{
				int x = 5;
        break;
		}
    } /* switch end */
}
```

<aside>
💡 Область видимости - область, в которой созданные в ней “сущности” видят друг друга, видят переменные из области видимости “выше” и уничтожаются при выходе из текущей области видимости. Ниже пример.

</aside>

```cpp
void func(){int func_x = 0;}
int global_x;

int main() // основное тело программы
{
	global_x; // видит
	func_x; // не видит
	if(global_x = 0){
		int temp = 255;
		global_x; // видит
  }
	temp; // не видит, т.к. temp живёт 
				// только в области видимости if
}
```

---

# Типы данных

<aside>
💡 С++ - это ЯП со статической типизаций, а это значит, что при объявлении переменных необходимо указывать тип, чтобы компилятор понимал, сколько байт сразу будет выделено под переменную.

объявление переменной - int a;
инициализация переменной - a = 5;
объявление и инициализация - int a = 5;

</aside>

Ниже представлены типы данных, кол-во занимаемых ими байт, аналогичные им имена (например, __int8 равносилен char), и диапазон значений.

| **`int`** | 4 | **`signed`** | От -2 147 483 648 до 2 147 483 647 |
| --- | --- | --- | --- |
| **`unsigned int`** | 4 | **`unsigned`** | От 0 до 4 294 967 295 |
| **`__int8`** | 1 | **`char`** | От -128 до 127 |
| **`unsigned __int8`** | 1 | **`unsigned char`** | От 0 до 255 |
| **`__int16`** | 2 | **`short`**, **`short int`**, **`signed short int`** | От -32 768 до 32 767 |
| **`unsigned __int16`** | 2 | **`unsigned short`**, **`unsigned short int`** | От 0 до 65 535 |
| **`__int32`** | 4 | **`signed`**, **`signed int`**, **`int`** | От -2 147 483 648 до 2 147 483 647 |
| **`unsigned __int32`** | 4 | **`unsigned`**, **`unsigned int`** | От 0 до 4 294 967 295 |
| **`__int64`** | 8 | **`long long`**, **`signed long long`** | От -9 223 372 036 854 775 808 до 9 223 372 036 854 775 807 |
| **`unsigned __int64`** | 8 | **`unsigned long long`** | От 0 до 18 446 744 073 709 551 615 |
| **`bool`** | 1 | нет | **`false`** или **`true`** |
| **`char`** | 1 | нет | От -128 до 127 по умолчанию От 0 до 255 при компиляции с помощью [`/J`](https://learn.microsoft.com/ru-ru/cpp/build/reference/j-default-char-type-is-unsigned?view=msvc-170) |
| **`signed char`** | 1 | нет | От -128 до 127 |
| **`unsigned char`** | 1 | нет | От 0 до 255 |
| **`short`** | 2 | **`short int`**, **`signed short int`** | От -32 768 до 32 767 |
| **`unsigned short`** | 2 | **`unsigned short int`** | От 0 до 65 535 |
| **`long`** | 4 | **`long int`**, **`signed long int`** | От -2 147 483 648 до 2 147 483 647 |
| **`unsigned long`** | 4 | **`unsigned long int`** | От 0 до 4 294 967 295 |
| **`long long`** | 8 | none (но эквивалентно **`__int64`**) | От -9 223 372 036 854 775 808 до 9 223 372 036 854 775 807 |
| **`unsigned long long`** | 8 | none (но эквивалентно **`unsigned __int64`**) | От 0 до 18 446 744 073 709 551 615 |
| **`enum`** | непостоянно | нет |  |
| **`float`** | 4 | нет | 3,4E +/- 38 (7 знаков) |
| **`double`** | 8 | нет | 1,7E +/- 308 (15 знаков) |
| **`long double`** | то же, что и **`double`** | нет | То же, что **`double`** |
| **`wchar_t`** | 2 | **`__wchar_t`** | От 0 до 65 535 |
> **Типы с плавающей запятой** при считывании с файла, например или записи туда должны учитывать текущую **локаль**. Под локалью устанавливается регион и язык, например, в США в качестве разделения дробный числе является “.”, а в России “,”. Также для таких чисел под каждой системой могут быть разные стандарты приведения, т.к. хранить целиком дробное число порой невозможно. По умолчанию этим стандартом является IEEE 754. Где число с плавающей запятой является 32-битным контейнером.

> В системной разработке лучше всего использовать фиксированные типы
---

# Операции (операторы)

## Арифметические

| + | Добавляет два операнда | A + B даст 30 |
| --- | --- | --- |
| - | Вычитает второй операнд с первого | A - B даст -10 |
| * | Умножает оба операнда | A * B даст 200 |
| / | Делит числитель на де-числитель | B / A даст 2 |
| % | Оператор модуля и остаток после целочисленного деления | B% A даст 0 |
| ++ | Оператор приращения увеличивает целочисленное значение на единицу | A ++ даст 11 |
| - | Уменьшает целочисленное значение на единицу | A-- даст 9 |

## Реляционные

| == | Проверяет, равны ли значения двух операндов или нет, если да, то условие становится истинным. | (A == B) не соответствует действительности. |
| --- | --- | --- |
| знак равно | Проверяет, равны ли значения двух операндов или нет, если значения не равны, условие становится истинным. | (A! = B) истинно. |
| > | Проверяет, превышает ли значение левого операнда значение правого операнда, если да, тогда условие становится истинным. | (A> B) неверно. |
| < | Проверяет, является ли значение левого операнда меньше значения 
правильного операнда, если да, тогда условие становится истинным. | (A <B) истинно. |
| > = | Проверяет, превышает ли значение левого операнда значение правого операнда, если да, тогда условие становится истинным. | (A> = B) неверно. |
| <= | Проверяет, является ли значение левого операнда меньше или равно 
значению правильного операнда, если да, тогда условие становится 
истинным. | (A <= B) истинно. |

## **Логические**

| && | Вызывается логическим оператором AND. Если оба операнда отличны от нуля, условие становится истинным. | (A && B) является ложным. |
| --- | --- | --- |
| || | Вызывается логическим оператором ИЛИ. Если любой из двух операндов отличен от нуля, тогда условие становится истинным. | (A || B) истинно. |
| ! | Вызывается логическим оператором NOT. Используется для изменения 
логического состояния операнда. Если условие истинно, то логический 
оператор NOT сделает ложным. | ! (A && B) истинно. |

## **Побитовые**

| & | Двоичный оператор AND копирует бит в результат, если он существует в обоих операндах. | (A & B) даст 12, что составляет 0000 1100 |
| --- | --- | --- |
| | | Двоичный оператор OR копирует бит, если он существует в любом из операндов. | (A | B) даст 61, который равен 0011 1101 |
| ^ | Оператор двоичного XOR копирует бит, если он установлен в один операнд, но не тот и другой. | (A ^ B) даст 49, который равен 0011 0001 |
| ~ | Binary Ones Оператор дополнения является унарным и имеет эффект «flipping» бит. | (~ A) даст -61, что составляет 1100 0011 в форме дополнения 2 из-за подписанного двоичного числа. |
| << | Двойной левый оператор сдвига.Значение левых операндов перемещается влево на количество бит, заданных правым операндом. | A << 2 даст 240, что составляет 1111 0000 |
| >> | Двоичный оператор правого сдвига. Значение левых операндов перемещается вправо на количество бит, заданных правым операндом. | A >> 2 даст 15, что составляет 0000 1111 |

## **Присваивания**

| знак равно | Простой оператор присваивания, присваивает значения из правых операндов в левый операнд. | C = A + B присваивает значение A + B в C |
| --- | --- | --- |
| + = | Оператор Add AND присваивания, Он добавляет правый операнд в левый операнд и присваивает результат левому операнду. | C + = A эквивалентно C = C + A |
| знак равно | Subtract AND assign operator, вычитает правый операнд из левого операнда и присваивает результат левому операнду. | C - = A эквивалентно C = C - A |
| знак равно | Оператор умножения и присваивания, Он умножает правый операнд на левый операнд и присваивает результат левому операнду. | C * = A эквивалентно C = C * A |
| знак равно | Оператор Divide AND assign. Он делит левый операнд на правый операнд и присваивает результат левому операнду. | C / = A эквивалентно C = C / A |
| знак равно | Модуль и оператор присваивания, он принимает модуль с использованием двух операндов и присваивает результат левому операнду. | C% = A эквивалентно C = C% A |
| << = | Оператор сдвига слева и. | C << = 2 совпадает с C = C << 2 |
| >> = | Оператор правой смещения и назначения. | C >> = 2 совпадает с C = C >> 2 |
| знак равно | Побитовый И оператор присваивания. | C & = 2 является таким же, как C = C & 2 |
| ^ = | Побитовое исключающее ИЛИ и оператор присваивания. | C ^ = 2 является таким же, как C = C ^ 2 |
| | = | Побитовое включение оператора OR и присваивания. | C | = 2 совпадает с C = C |2 |

## Другие (почему-то операторы)

| **sizeof** | Возвращает размер переменной. Например, sizeof (a), где 'a' является целым числом и будет возвращать 4. |
| --- | --- |
| **Condition ? X : Y** | Если Условие истинно, то оно возвращает значение X, иначе возвращает значение Y. |
| **,** | Вызывает последовательность операций. Значение всего выражения 
запятой - это значение последнего выражения списка, разделенного 
запятыми. |
| **. (dot) and -> (arrow)** | Используются для ссылки на отдельных членов классов, структур и союзов. |
| **Cast** | Преобразуют один тип данных в другой. Например, int (2.2000) вернет 2. |
| **&** | Возвращает адрес переменной. Например, & a; даст фактический адрес переменной. |
| ***** | Является указателем на переменную. Например * var; будет указывать на переменную var. |

---

# Объекты потоков ввода и вывода

> В С и С++ параллельно работают потоки ввода и вывода данных. (На самом деле так и в терминале и вообще довольно распространено) Для работы с ними в С++ существуют объекты, указанные ниже:
> 
- cin стандартный входной поток (stdin в С)
- cout стандартный выходной поток (stdout в С)
- cerr стандартный поток вывода сообщений об ошибках (stderr в С)

Для того, чтобы получить данные из потока, используется оператор >>, для добавления в поток <<.

<aside>
💡 Для ассоциации, cin - конвейер, на который что-то запихнули (входной поток), а мы можем с него что-то забрать, т.е. >> взять с конвейера. Та же ассоциация с cout, на него можно только что-то положить, т.е. << положить на него данные, а они потом уедут на отображение.

</aside>

---

# Условия и условные операторы

> Операторы: `==`, `!=`, `>`, `<`, `<=`, `>=`
> Логические операции: `&&`, `||`, `!`
> ИЛИ ключевые слова `and`, `or`, `not` (доступны вроде пока лишь на gcc)

Условная конструкция **if** - если условие true, то выполняется.

```cpp
if (логическое_выражение)
{
    инструкции
} 
else if (логическое выражение)
{
    инструкции
}
else
{ 
    инструкции
}

// Можно сократить, если выполняется только 1 инструкция
if (логическое_выражение)
    инструкция
else if (логическое выражение)
    инструкция
else
    инструкция
```

**Тернарный оператор** - сокращённая запись if-else. 

```cpp
логическое выражение ? если true : если false

cout << "5 больше 1? - " << (5 > 1 ? "Да" : "Нет")
```

---

# Инкременты и декременты

> Вместо x = x + 1; можно написать инкремент x++;
Вместо x = x - 1; можно написать декремент x--;
Есть префиксная операция ++x/--x и постфиксная операция x++/--x, они отличаются приоритетами операции, постфиксная операция ниже по приоритету.
> 

---

# Сокращённые арифметические операции

> Запись простых арифметических операций можно сокращать.
> 

| a = a + b | a += b |
| --- | --- |
| a = a * b | a *= b |
| a = a - b | a -= b |
| a = a / b | a /= b |

---

# Приоритет операций

> У операций есть приоритет выполнения, например, что выдаст запись int x = 5; int у = ++x++; ? 
Выдаст ошибку компиляции, ибо ++х++ нельзя писать теперь. Однако как у них порядок выполнения?
Ниже приведены примеры. Так же стоит учитывать такой параметр как ассоциативность. Если она справа налево, то это как читать справа налево. Сначала учитывается приоритет группы ассоциативности справа налево, а потом приоритет групп в ассоциативности слева направо.
> 

Сначала мы смотрим на ассоциативность операций с одинаковым приоритетом, чтобы определить порядок их выполнения. Если операции имеют одинаковую ассоциативность, то смотрим на их приоритетность. Операции с более высоким приоритетом выполняются раньше операций с более низким приоритетом. Если операции имеют одинаковый приоритет и ассоциативность, то их выполнение происходит слева направо.

| Описание оператора | Оператор | Альтернатива |  |
| --- | --- | --- | --- |
| **Приоритет группы 1, без ассоциативности** |  |  |  |
| [Разрешение области](https://learn.microsoft.com/ru-ru/cpp/cpp/scope-resolution-operator?view=msvc-170) | [`::`](https://learn.microsoft.com/ru-ru/cpp/cpp/scope-resolution-operator?view=msvc-170) |  |  |
| **Приоритет группы 2, ассоциативность слева направо** |  |  |  |
| [Выбор члена для указателей (объект или указатель)](https://learn.microsoft.com/ru-ru/cpp/cpp/member-access-operators-dot-and?view=msvc-170) | [`.` или `->`](https://learn.microsoft.com/ru-ru/cpp/cpp/member-access-operators-dot-and?view=msvc-170) |  |  |
| [Индекс массива](https://learn.microsoft.com/ru-ru/cpp/cpp/subscript-operator?view=msvc-170) | [`[]`](https://learn.microsoft.com/ru-ru/cpp/cpp/subscript-operator?view=msvc-170) |  |  |
| [Вызов функции](https://learn.microsoft.com/ru-ru/cpp/cpp/function-call-operator-parens?view=msvc-170) | [`()`](https://learn.microsoft.com/ru-ru/cpp/cpp/function-call-operator-parens?view=msvc-170) |  |  |
| [Постфиксный инкремент](https://learn.microsoft.com/ru-ru/cpp/cpp/postfix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) | [`++`](https://learn.microsoft.com/ru-ru/cpp/cpp/postfix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) |  |  |
| [Постфиксный декремент](https://learn.microsoft.com/ru-ru/cpp/cpp/postfix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) | [`--`](https://learn.microsoft.com/ru-ru/cpp/cpp/postfix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) |  |  |
| [Имя типа](https://learn.microsoft.com/ru-ru/cpp/cpp/typeid-operator?view=msvc-170) | [`typeid`](https://learn.microsoft.com/ru-ru/cpp/cpp/typeid-operator?view=msvc-170) |  |  |
| [Постоянное преобразование типа](https://learn.microsoft.com/ru-ru/cpp/cpp/const-cast-operator?view=msvc-170) | [`const_cast`](https://learn.microsoft.com/ru-ru/cpp/cpp/const-cast-operator?view=msvc-170) |  |  |
| [Динамическое преобразование типа](https://learn.microsoft.com/ru-ru/cpp/cpp/dynamic-cast-operator?view=msvc-170) | [`dynamic_cast`](https://learn.microsoft.com/ru-ru/cpp/cpp/dynamic-cast-operator?view=msvc-170) |  |  |
| [Повторно интерпретируемое преобразование типа](https://learn.microsoft.com/ru-ru/cpp/cpp/reinterpret-cast-operator?view=msvc-170) | [`reinterpret_cast`](https://learn.microsoft.com/ru-ru/cpp/cpp/reinterpret-cast-operator?view=msvc-170) |  |  |
| [Статическое преобразование типа](https://learn.microsoft.com/ru-ru/cpp/cpp/static-cast-operator?view=msvc-170) | [`static_cast`](https://learn.microsoft.com/ru-ru/cpp/cpp/static-cast-operator?view=msvc-170) |  |  |
| **Приоритет группы 3, ассоциативность справа налево** |  |  |  |
| [Размер объекта или типа](https://learn.microsoft.com/ru-ru/cpp/cpp/sizeof-operator?view=msvc-170) | [`sizeof`](https://learn.microsoft.com/ru-ru/cpp/cpp/sizeof-operator?view=msvc-170) |  |  |
| [Префиксный инкремент](https://learn.microsoft.com/ru-ru/cpp/cpp/prefix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) | [`++`](https://learn.microsoft.com/ru-ru/cpp/cpp/prefix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) |  |  |
| [Префиксный декремент](https://learn.microsoft.com/ru-ru/cpp/cpp/prefix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) | [`--`](https://learn.microsoft.com/ru-ru/cpp/cpp/prefix-increment-and-decrement-operators-increment-and-decrement?view=msvc-170) |  |  |
| [Дополнение к одному](https://learn.microsoft.com/ru-ru/cpp/cpp/one-s-complement-operator-tilde?view=msvc-170) | [`~`](https://learn.microsoft.com/ru-ru/cpp/cpp/one-s-complement-operator-tilde?view=msvc-170) | **`compl`** |  |
| [Логическое не](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-negation-operator-exclpt?view=msvc-170) | [`!`](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-negation-operator-exclpt?view=msvc-170) | **`not`** |  |
| [Унарное отрицание](https://learn.microsoft.com/ru-ru/cpp/cpp/unary-plus-and-negation-operators-plus-and?view=msvc-170) | [`-`](https://learn.microsoft.com/ru-ru/cpp/cpp/unary-plus-and-negation-operators-plus-and?view=msvc-170) |  |  |
| [Унарный плюс](https://learn.microsoft.com/ru-ru/cpp/cpp/unary-plus-and-negation-operators-plus-and?view=msvc-170) | [`+`](https://learn.microsoft.com/ru-ru/cpp/cpp/unary-plus-and-negation-operators-plus-and?view=msvc-170) |  |  |
| [Взятие адреса](https://learn.microsoft.com/ru-ru/cpp/cpp/address-of-operator-amp?view=msvc-170) | [`&`](https://learn.microsoft.com/ru-ru/cpp/cpp/address-of-operator-amp?view=msvc-170) |  |  |
| [Косвенное обращение](https://learn.microsoft.com/ru-ru/cpp/cpp/indirection-operator-star?view=msvc-170) | [`*`](https://learn.microsoft.com/ru-ru/cpp/cpp/indirection-operator-star?view=msvc-170) |  |  |
| [Создание объекта](https://learn.microsoft.com/ru-ru/cpp/cpp/new-operator-cpp?view=msvc-170) | [`new`](https://learn.microsoft.com/ru-ru/cpp/cpp/new-operator-cpp?view=msvc-170) |  |  |
| [Уничтожение объекта](https://learn.microsoft.com/ru-ru/cpp/cpp/delete-operator-cpp?view=msvc-170) | [`delete`](https://learn.microsoft.com/ru-ru/cpp/cpp/delete-operator-cpp?view=msvc-170) |  |  |
| [Cast](https://learn.microsoft.com/ru-ru/cpp/cpp/cast-operator-parens?view=msvc-170) | [`()`](https://learn.microsoft.com/ru-ru/cpp/cpp/cast-operator-parens?view=msvc-170) |  |  |
| **Приоритет группы 4, ассоциативность слева направо** |  |  |  |
| [Указатель на член (объекты или указатели)](https://learn.microsoft.com/ru-ru/cpp/cpp/pointer-to-member-operators-dot-star-and-star?view=msvc-170) | [`.*` или `->*`](https://learn.microsoft.com/ru-ru/cpp/cpp/pointer-to-member-operators-dot-star-and-star?view=msvc-170) |  |  |
| **Приоритет группы 5, ассоциативность слева направо** |  |  |  |
| [Умножение](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) | [`*`](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) |  |  |
| [Отдел](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) | [`/`](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) |  |  |
| [Модуль](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) | [`%`](https://learn.microsoft.com/ru-ru/cpp/cpp/multiplicative-operators-and-the-modulus-operator?view=msvc-170) |  |  |
| **Приоритет группы 6, ассоциативность слева направо** |  |  |  |
| [Сложение](https://learn.microsoft.com/ru-ru/cpp/cpp/additive-operators-plus-and?view=msvc-170) | [`+`](https://learn.microsoft.com/ru-ru/cpp/cpp/additive-operators-plus-and?view=msvc-170) |  |  |
| [Вычитание](https://learn.microsoft.com/ru-ru/cpp/cpp/additive-operators-plus-and?view=msvc-170) | [`-`](https://learn.microsoft.com/ru-ru/cpp/cpp/additive-operators-plus-and?view=msvc-170) |  |  |
| **Приоритет группы 7, ассоциативность слева направо** |  |  |  |
| [Сдвиг влево](https://learn.microsoft.com/ru-ru/cpp/cpp/left-shift-and-right-shift-operators-input-and-output?view=msvc-170) | [`<<`](https://learn.microsoft.com/ru-ru/cpp/cpp/left-shift-and-right-shift-operators-input-and-output?view=msvc-170) |  |  |
| [Сдвиг вправо](https://learn.microsoft.com/ru-ru/cpp/cpp/left-shift-and-right-shift-operators-input-and-output?view=msvc-170) | [`>>`](https://learn.microsoft.com/ru-ru/cpp/cpp/left-shift-and-right-shift-operators-input-and-output?view=msvc-170) |  |  |
| **Приоритет группы 8, ассоциативность слева направо** |  |  |  |
| [Меньше чем](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) | [`<`](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) |  |  |
| [Больше чем](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) | [`>`](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) |  |  |
| [Меньше или равно](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) | [`<=`](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) |  |  |
| [Больше или равно](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) | [`>=`](https://learn.microsoft.com/ru-ru/cpp/cpp/relational-operators-equal-and-equal?view=msvc-170) |  |  |
| **Приоритет группы 9, ассоциативность слева направо** |  |  |  |
| [Равенство](https://learn.microsoft.com/ru-ru/cpp/cpp/equality-operators-equal-equal-and-exclpt-equal?view=msvc-170) | [`==`](https://learn.microsoft.com/ru-ru/cpp/cpp/equality-operators-equal-equal-and-exclpt-equal?view=msvc-170) |  |  |
| [Неравенство](https://learn.microsoft.com/ru-ru/cpp/cpp/equality-operators-equal-equal-and-exclpt-equal?view=msvc-170) | [`!=`](https://learn.microsoft.com/ru-ru/cpp/cpp/equality-operators-equal-equal-and-exclpt-equal?view=msvc-170) | **`not_eq`** |  |
| **Приоритет ассоциативности слева направо в группе 10** |  |  |  |
| [Побитовое И](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-and-operator-amp?view=msvc-170) | [`&`](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-and-operator-amp?view=msvc-170) | **`bitand`** |  |
| **Приоритет группы 11, ассоциативность слева направо** |  |  |  |
| [Побитовое исключающее ИЛИ](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-exclusive-or-operator-hat?view=msvc-170) | [`^`](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-exclusive-or-operator-hat?view=msvc-170) | **`xor`** |  |
| **Приоритет группы 12, ассоциативность слева направо** |  |  |  |
| [Побитовое ИЛИ](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-inclusive-or-operator-pipe?view=msvc-170) | [`|`](https://learn.microsoft.com/ru-ru/cpp/cpp/bitwise-inclusive-or-operator-pipe?view=msvc-170) | **`bitor`** |  |
| **Приоритет группы 13, ассоциативность слева направо** |  |  |  |
| [Логическое И](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-and-operator-amp-amp?view=msvc-170) | [`&&`](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-and-operator-amp-amp?view=msvc-170) | **`and`** |  |
| **Приоритет группы 14, ассоциативность слева направо** |  |  |  |
| [Логическое ИЛИ](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-or-operator-pipe-pipe?view=msvc-170) | [`||`](https://learn.microsoft.com/ru-ru/cpp/cpp/logical-or-operator-pipe-pipe?view=msvc-170) | **`or`** |  |
| **Приоритет группы 15, ассоциативность справа налево** |  |  |  |
| [Условная логика](https://learn.microsoft.com/ru-ru/cpp/cpp/conditional-operator-q?view=msvc-170) | [`? :`](https://learn.microsoft.com/ru-ru/cpp/cpp/conditional-operator-q?view=msvc-170) |  |  |
| [Передача прав и обязанностей](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание умножения](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`*=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание деления](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`/=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Назначение модуля](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`%=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание сложения](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`+=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание вычитания](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`-=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание сдвига влево](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`<<=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Присваивание сдвига вправо](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`>>=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) |  |  |
| [Назначение побитового И](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`&=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | **`and_eq`** |  |
| [Назначение побитового включающего ИЛИ](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`|=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | **`or_eq`** |  |
| [Назначение побитового исключающего ИЛИ](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | [`^=`](https://learn.microsoft.com/ru-ru/cpp/cpp/assignment-operators?view=msvc-170) | **`xor_eq`** |  |
| [Выражение Throw](https://learn.microsoft.com/ru-ru/cpp/cpp/try-throw-and-catch-statements-cpp?view=msvc-170) | [`throw`](https://learn.microsoft.com/ru-ru/cpp/cpp/try-throw-and-catch-statements-cpp?view=msvc-170) |  |  |
| **Приоритет группы 16, ассоциативность слева направо** |  |  |  |
| [Запятая](https://learn.microsoft.com/ru-ru/cpp/cpp/comma-operator?view=msvc-170) | [,](https://learn.microsoft.com/ru-ru/cpp/cpp/comma-operator?view=msvc-170) |  |  |

---

# Некоторые особенности строк

> Как можно заметить, строка не имеет фиксированного кол-ва байт, только лишь на каждый элемент. Как тогда понимать при взаимодействии строкой, где её конец? Для этого существует **терминирующий ноль** “\0”, чаще всего в компиляторах нет необходимости в его указании, однако, пусть послужить лишним напоминанием. Так же строка в некоторым смысле равносильна массиву (об этом позже) char’ов, попытка проведение операций между этими двумя типами довольно безопасная. Для терминологии… сложение двух строк называется **конкатенацией**.
> 

---

# Указатели и адреса

> **Указатель** - тип, который указывает на область памяти… вот прям буквально указывает, можно даже представить стрелку или палец. Так как каждая переменная (и не только) расположена в какой-то области памяти, то у неё должен быть свой адрес, чтобы программа могла к ней обращаться.
> 

```cpp
int a = 5; // 4 байтовая переменная типа целого числа
int* pa; // указатель типа int!
pa = &a; // теперь указатель хранит адрес переменной a
// & - оператор взятия **адреса** (амперсант)
int b = *pa; // разименования указателя, т.к. указатель 
//содержит адрес, а не само значение по адресу
cout << b; // 5

(*pa)++; // изменяем значение по адресу
cout << a; // 6, т.к. ссылается на тот же адрес, что и pa
```

<aside>
💡 С указателем возможны многие арифметические операции, однако не стоит забывать, что какие-нибудь pa + 1 вернёт адрес элемента из последующего (правого) в области памяти элемента, например.

</aside>

<aside>
💡 [Терминология] 
Raw pointer - указатель в чистом виде
Fancy pointer - некоторый объект, который ведёт себя как указатель, но им явно не является

</aside>

---

# Циклы

<aside>
💡 Работает сокращённая запись, если несколько инструкций, то обязательно {}. Если только 1, то можно без области видимости {}.

</aside>

```cpp
while(логическое выражение) // пока условие выполняется
	инструкции // делать
```

```cpp
while(логическое выражение) do // пока условие выполняется
	инструкции // делать
```

```cpp
 do // сделать 1 раз, а потом проверит, можно ли делать снова
	инструкции 
while(логическое выражение)
```

```cpp
/* создаётся счётчик i внутри цикла, пока он тикает
 цикл выполняется */
for(int i = 0; i > 0; i++) 
	инструкции
```

```cpp
// цикл перебирает каждый объект obj в некотором множестве objects
for(int obj : objects) 
	инструкции
```

```cpp
// цикл перебирает каждый объект obj в некотором множестве objects
foreach(int obj, objects) //  
	инструкции
```

> *Ключевое слово* **break** - прерывает текущий цикл, если цикл в цикле, то прерывается только тот цикл, в котором break вызвался.
> 

> *Ключевое слово* **continue** - прерывает текущую итерацию цикла и передаёт управление циклу, т.е. тот переходит к следующей итерации.
> 

<aside>
🦍 В каких-то случаях можно использовать for (auto& [key, value] : weirdContainer), это просто подсказка формата записи на будущее.

</aside>

---

# Switch оператор

> Работает как условие, только с несколькими вариантами. Берётся сравниваемое значение и сравнивается с разными case’ами, если равно кому-то, то блок этого case’а и выполняется. switch-case работает как **метки**. Т.е. выполнение программы перескакивает на case и идёт до ключевого слова break или иного другого “триггера”. Если не поставить break (где угодно), то программа может выполнить и блок следующего case’а. ****
> 


💡 Метки(go to), говорят, что никогда не стоит их использовать, но мы же понимаем, что раз они существуют… при вызове `goto` программа переходит к метке.
`goto <название метки>` - означает переход к метке
`<название метки>`: - куда метка перейдёт (обратно не вернётся сама

```cpp
switch(сравниваемое значение)
{
case <с чем сравниваем>:
	//если равны, то выполняется этот случай
	break;

case 2: // если значение в скобках switch'а равно 2
{
	int x = 5; // переменную можно объявить только 
					   // в области видимости
} break;

default: // если никакой из случаев не сработал
	break;
}
```

---

# Массивы

> Это набор элементов. Можно представить, что каждый элемент массива расположен в своей отдельной коробке, т.е. если с одной коробки удалить элемент, то в неё ничего не сместится, это будет пустая коробка. В списках/контейнерах, произойдёт же смещение. Стоит помнить, что переменная массива, указателем как бы не является, но в контексте многих ЯП вполне себе нисходит до указателя на первый (точнее нулевой) элемент массива. Это по факту fancy pointer на ряду с итераторами. Индексация элементов, как всегда, начинается с 0.
> 

```cpp
/* Как можно задавать масссивы */
int arr[10];
int arr[] {1,2,3}; 
int arr[] = {1,2,3};
int arr[4]{}; //будут нули
int arr[4]{1,2}; //будет 1,2,0,0
```

Некоторые детали:

- [] - количество элементов от 1 до n, но обращение к элементам идёт с 0
- переменная под массив хранит ссылку на первый элемент массива (т.е. поинтер/указатель), другие его переменные лежат рядом по порядку
- массив можно выводить в for'e в виде cout << *(arr+i); это разыменование поинтера см часть1.,
если без разыменования, то будут адреса каждого элемента
- sizeof(arr) - возвращает занимаемую под массив память в байтах, для выяснения кол-ва элементов: sizeof(arr)/sizeof(int) если массив типа int
- двумерный массив arr[][*колонки всегда вроде надо указывать*], ещё форма записи arr[][]{{1, 2, 3}, {4, 5, 6}};
- [количество строк][количество столбцов]

---

# Функции

> Блок инструкций, которым можно воспользоваться в нужных местах программы. Функция должна быть объявлена до её использования. Если же очень хочется дизайна, то существует **прототип функции**, можно задать название функции где-то заранее, но без {}. А его реализацию написать в любом другом месте.

<aside>
💡 Обязательно стоит знать, что передача в аргументы функции происходит с помощью операции **копирования!!!** Для того, чтобы передать функции переменную на изменение, необходимо в аргументах функции (при объявлении) добавлять адрес или указатель.

</aside>

```cpp
<тип возвращаемого значения> <название функции>(<входные аргументы функции>)
{
	инструкции
}
```

```cpp
bool isItOk(); // пример экземляра

int sum(int a, int b) 
{
	return a + b;
}

int sum(int a, int b, int c) // перегрузка функции
{
	return a + b + c; 
}

void newCout(int i = 5) // задание параметра по умолчанию
{ // если при вызове функции ничего не передаётся, то i = 5
	cout << i;
}

void incrementForMe(int &i) // передача по адресу
{
	i++;
}

void incrementForMeP(int* pi) // передача по указателю
{
	i++;
}

int main()
{
	cout << sum(5, 6) // 11
	cout << sum(5, 6, 7) // 18, вызов перегруженной функции
	isItOk(); // false
	newCout(); // 5
	int b = 0;
	incrementForMe(b); cout << b;  // 1
	incrementForMe(&b); cout << b; // 2
}

bool isItOk() // реализация функции, экземпляр которой был выше
{
   return false;
}
```

<aside>
💡 Первая строчка функции (с возвращаемым типом, названием функции и входными аргументами) называется **сигнатурой**.

</aside>

<aside>
💡 Само название функции по факту является **ссылкой на начало инструкций**  функции, но внутри контекста Си и С++ может преобразовываться как **указатель** (см. также тему про callback). ****Попытка взять адрес от имени функции будет равносильна попытке взять адрес от переменной типа массив (напомню, что она является указателем на первый элемент массива) - т.е. какие-то компиляторы воспринимают это нормально, а какие-то, в теории, могут ругаться.

</aside>

<aside>
💡 Если создать функцию, название который идентично уже существующей, но входные параметры, например, другие, то это называется **перегрузкой функции**.

</aside>
### Немного рекурсии

> Это методология вызова функции внутри неё самой же. Забивает стек. Первым делом надо думать об условии выхода из рекурсии.
> 

```cpp
int doSth(int num){
	if (5 < num < 10)
		return 7; // по факту выход из рекурсии
	return num + doSth(num);
}
```

При работе с рекурсиями активно упоминают работу стека. При сборке проекта разработчиком вручную задаётся размер стека заранее (или автоматически компилятором), он фиксированный. Стек работает по принципу последний зашёл - первый вышел… примерно как стопка тарелок. Когда происходит рекурсия - в стек забиваются адреса вызовов и возвратов функции, т.е. стек забивается каждым таким последующим вызовом рекурсивной функции, что приводит к известному stack overflow. 

Использовать рекурсию стоит редко, обычно для: генерации, деревьев, графов. Также стоит узнать про `tail recursion`.

---

# Wow moment

Неожиданно, в С++ можно передавать массив не как по указателю на перый элемент, а как ссылку на массив, по ней можно и нормально считать размерность массива

```cpp
int arr[5]{0, 1, 2, 3, 4};

void func(int arr[]);        -> call by -> func(arr);    -> pass pointer
void func(int* arr, size);   -> call by -> func(arr, 5); -> pass pointer + size

void func(int (&arr)[size]); -> call by -> func(arr[5]); -> pass reference + size
```