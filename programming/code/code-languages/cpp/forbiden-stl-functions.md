--- 

| Память/массивы данных          | memset                                    |                                                                                                                     |
| ------------------------------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
|                                | memcpy                                    |                                                                                                                     |
|                                | fill                                      |                                                                                                                     |
| Работа со строками и символами | isspace                                   | Проверяет, является ли символ whitespace’ом (что-то типа пробела или пустого символа)                               |
|                                | isdigit                                   | Проверяет, является символ цифрой или нет                                                                           |
|                                | stoi                                      | преобразует последовательность символов в str в значение типа int и возвращает значение                             |
|                                | itoa                                      | конвертирует целое число num в строчный эквивалент и помещает результат в строку, на которую указывает параметр str |
|                                | `strstr(const char *s2, const char *s1)`  | Возвращает указатель на первое вхождение строки s1 в строку s2. Если совпадений нет, возвращает NULL                |
|                                | `strtok (char *, const char *)`           | Позволяет разбить строку на отдельные части (лексемы)                                                               |
|                                | `strncpy(char *, const char *, size_t n)` | Копирует n символов второй строки в первую                                                                          |
|                                | `strspn (const char *, const char *)`     | Возвращает длину начала первой строки, в которую входят символы, из которых состоит вторая строка                   |
|                                | strcat                                    | к концу первого аргумента присоединяет второй                                                                       |
|                                | strncat                                   | присоединяет n символов второй строки к первой                                                                      |
|                                | `strchr (const char *, int c)`            | Возвращает указатель на первое вхождение символа с в строку. Возвращает `NULL`, если такого символа в строке нет    |
| Другое                         | swap                                      | меняет значения объектов                                                                                            |
|                                | move                                      | `remove_reference` с объекта и `static_cast` к `rvalue-ссылке`                                                      |
|                                | addressof                                 | извлекает адресс                                                                                                    |