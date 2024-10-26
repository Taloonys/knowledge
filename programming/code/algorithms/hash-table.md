Абстрактный тип данных, суть которой заключается в хранении любых значений по системе <**ключ**> - <**значение**>. Реализация хэш таблицы может быть разной.
![Untitled](image-storage/Untitled%202%201.png)

В качестве **ключа** могут быть любые типы, но такие типы будут храниться в таблице в качестве хэша. Значение, указанное ключом, **хэшируется** - т.е. **преобразуются данные любой длины в данные фиксированной длины**.

С указанной выше формулировкой становится ясна одна проблема, кол-во ключей в таком случае будет ограничено, ситуация, когда требуемый хэш соответствует двум значениям называется **коллизией**. Решение подобной проблемы зависит от реализации, можно, например, в качестве ключа иметь связный список, где под нужной позицией будут храниться 2 ключа, хэши которых совпадают.

![Untitled](image-storage/Untitled%203%201.png)

Например, в ситуации со связными списками может возникать проблема, что на 1 хэш элементе может быть огромный список ключей, что увеличит сложность алгоритма поиска.. для борьбы с этим может использоваться расширение хэш таблицы, обычно после ~75% хэш таблица расширяется хэш пустыми значениями, старые ячейки остаются.

<aside>
💡 Отличие от хэшсета в том, что каждый **бакет** содержит эту пару ключ-значение целиком.

</aside>

![Untitled](image-storage/Untitled%204%201.png)

### **Использованные источники**

[https://www.youtube.com/watch?v=cWbuK7C13HQ](https://www.youtube.com/watch?v=cWbuK7C13HQ)
[https://www.youtube.com/watch?v=rPp46idEvnM&ab_channel=СашаЛукин](https://www.youtube.com/watch?v=rPp46idEvnM&ab_channel=%D0%A1%D0%B0%D1%88%D0%B0%D0%9B%D1%83%D0%BA%D0%B8%D0%BD)