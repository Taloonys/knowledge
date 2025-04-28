# Operators overriding

> На всякий случай, напомню, что есть ещё перегрузка функций и методов, где 2 одинаковые по названию функции имеют разное кол-во и типы входных данных в сигнатуре. Есть ещё переопределение методов в дочерних классах, которое позволяет нам для, например, ребёнка изменить функционал уже существующей с функционалом функции или метода в контексте родителя.
> 

Начнём с перегрузки методов/функций. Обычно **перегрузка метода** обозначается ключевым словом **override**, но это не обязательно.

```cpp
class Parent{
public:
	void doParent()
	{cout << "Why doesn't my child have 100000$?";}
}

class Child : Parent{
//...
}

int main(){ 
	Child child;
	child.doParent(); // "Why doesn't my child have 100000$?"
} 
```

```cpp
class Parent{
public:
	void doParent()
	{cout << "Why doesn't my child have 100000$?";}
}

class Child : Parent{
public:
void doParent() override // перегрузка
	{cout << "well...";}
}

int main(){ 
	Child child;
	child.doParent(); // "well..."
} 
```

Перегрузка оператора синтаксически похожа на перегрузку функции. Ниже представлены шаблоны написания перегрузки операторов. Где ReturnType(ClassType) - возвращаемый тип, Op - сам символ оператора, right_operand и left_operand - как значение до = и после (соответственно).

```cpp
/* Формальная запись */
// бинарный оператор
ReturnType operator Op(Type right_operand);
// унарный оператор
ClassType& operator Op();

/* Полная запись */
// бинарный оператор
ReturnType operator Op(const ClassType& left_operand, Type right_operand);
// альтернативное определение, где класс, для которого создается оператор, представляет правый операнд
ReturnType operator Op(Type left_operand, const ClassType& right_operand);
// унарный оператор
ClassType& operator Op(ClassType& obj);
```

```cpp
class Integer // пример перегрузки +
{
private:
    int value;
public:
    Integer(int i): value(i) 
    {}
    const Integer operator+(const Integer& rv) const {
        return (value + rv.value);
    }
};
```

**Примеры перегрузки унарных операторов:**

```cpp
class Integer
{
private:
    int value;
public:
    Integer(int i): value(i) 
    {}

    //унарный +
    friend const Integer& operator+(const Integer& i);

    //унарный -
    friend const Integer operator-(const Integer& i);

    //префиксный инкремент
    friend const Integer& operator++(Integer& i);

    //постфиксный инкремент
    friend const Integer operator++(Integer& i, int);

    //префиксный декремент
    friend const Integer& operator--(Integer& i);

    //постфиксный декремент
    friend const Integer operator--(Integer& i, int);
};

//унарный плюс ничего не делает.
const Integer& operator+(const Integer& i) {
    return i.value;
}

const Integer operator-(const Integer& i) {
    return Integer(-i.value);
}

//префиксная версия возвращает значение после инкремента
const Integer& operator++(Integer& i) {
    i.value++;
    return i;
}

//постфиксная версия возвращает значение до инкремента
const Integer operator++(Integer& i, int) {
    Integer oldValue(i.value);
    i.value++;
    return oldValue;
}

//префиксная версия возвращает значение после декремента
const Integer& operator--(Integer& i) {
    i.value--;
    return i;
}

//постфиксная версия возвращает значение до декремента
const Integer operator--(Integer& i, int) {
    Integer oldValue(i.value);
    i.value--;
    return oldValue;
}
```

**Примеры перегрузки унарных операторов:**

```cpp
class Integer
{
private:
    int value;
public:
    Integer(int i): value(i) 
    {}
    friend const Integer operator+(const Integer& left, const Integer& right);

    friend Integer& operator+=(Integer& left, const Integer& right);

    friend bool operator==(const Integer& left, const Integer& right);
};

const Integer operator+(const Integer& left, const Integer& right) {
    return Integer(left.value + right.value);
}

Integer& operator+=(Integer& left, const Integer& right) {
    left.value += right.value;
    return left;
}

bool operator==(const Integer& left, const Integer& right) {
    return left.value == right.value;
}
```

<aside>
💡 Для присваиваний и подобной логике операторов необходимо возвращать адрес. Не все операторы доступны для переопределения… например, override "=" не наследуется. Через ключевое слово Friend можно иногда целенаправленно переопределять бинарники, но в целом всё эта тема ломает читабельность кода почти всегда, разве что << и >> можно трогать.

</aside>

💡 В С++ дозволен вызов оператора как функции

```cpp
template<typename MemberPtr>
void RemovePptr(MemberPtr p)
{
	return p.operator->();
}
```