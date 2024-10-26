# Include

> Подключает файл к текущему

```cpp
#include <vector> // подключает stl vector.h
```

💡 Если имя в `<>`, то идёт поиск по “стандартным путям”, например, stl’ные элементы явно являются стандартными…
Остальные файлы заключаются в `"anyothermodule.h"`

# Pragma

## pragma once

> Защищает header файл (.h) от повторного включения
> 

```cpp
#pragma once
```

## pragma warning

[https://learn.microsoft.com/ru-ru/cpp/preprocessor/warning?view=msvc-170](https://learn.microsoft.com/ru-ru/cpp/preprocessor/warning?view=msvc-170)

# Define

> “Определяет параметр для замены”, см. также макросы
> 

```cpp
#define CONTSTANT_VAR 5

int main()
{
	printf("Constant value is %d", CONSTANT_VAR);
}
```

<aside>
💡 Для набора большого кол-ва define’ов стоит использовать отдельный файл в проекте.

</aside>

# Ifdef, ifndef, elif, endif

> Условная логика для define
> 

```cpp
#ifdef _WIN32
	using service_t = windows::service;
#elif defined(__linux__)
	using service_t = linux::service;
#else 
	using service_t = macos::service;
#endif
```

| include                                  | подключения модуля (по файлу)                                                                                                                     |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| define <имя>(аргументы)                  | макрос замены, заменяет имя в коде на то, что будет написано в нём.<br>Просто пример: <br>`#define GLOBAL_NUM 5` - заменит в коде GLOBAL_NUM на 5 |
| ifdef, ifndef, elif, ifdef, ifndef, elif | `#define` с if-else логикой                                                                                                                       |
| pragme once                              |  защита header файла от повторного включения                                                                                                      |
| pragma push/pragma pop                   |                                                                                                                                                   |
