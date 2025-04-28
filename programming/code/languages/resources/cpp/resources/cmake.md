--- 

# How to

## Make asset

dir:

- my_project
- CMakeLists.txt
- main.cpp
- module
    - CMakeLists.txt
    - main.cpp

## Use cmake command

В директории dir написать cmake с нужными аргументами. cmake сработает, когда увидит CMakeLists.txt в указанной директории. CMakeLists.txt самый верхний содержит cmake_minimim_required и project. Команда cmake применяется только к нему.

# command line arguments

> for cmake command
> 
> 
> ```cpp
> -DCMAKE_BUILD_TYPE=Debug
> -DCMAKE_INSTALL_PREFIX=/D:/somewhere/dir
> -DCMAKE_INSTALL_PREFIX="/D:/somewhere/dir,/D:/somewhereelse/dir"
> -DCMAKE_PREFIX_PATH=/D:/from-where-pick-files-to-look
> -DCMAKE_CUSTOM_PARAM=TRUE
> ```
> 

## set param by -D

Do it before —build

## set where to place files for future build by -B

Do it before —build

```cpp
cmake -Bbuild_dir
```

## set what dir with make-files to build by  --build

```cpp
cmake --build build_dir
```

## do install

```cpp
cmake --target install
cmake --build buid_dir --target install
```

<aside>
💡 install не всегда (чаще всего) замещает файлы в месте назначения

</aside>

# Inside CMakeLists.txt

> Задача системы или даже отдельных команд собрать **target** с указанными именами

## cmake_minimum_required

главный cmake начинается с этой строчки и project

## project

указывается название проекта, название является так же target’ом

## set

указывает определённые переменные и их значения

## find_package

ищет в известных ему путях и указанному CMAKE_PREFIX_PATH пакеты (а точнее .cmake файлы, обычно лежат где-то в share)

## add_executable

применяется только в единицам трансляции (.cpp), объединяет их в 1 исполняемый файл

## target_compile_definitions

для текущего cmake-unit’а указывает заранее известные для кода переменные (cmake-unit - папка в которой написан текущий CMakeLists.txt)

## target_link_libraries

указание для линкера, с какими библиотеками надо связать target

## target_link_dirictories

указание для линкера, какой директорию с библиотеками целиком надо связать с target

## target_include_dirictories

позволяет из указанной директории спокойно include’ить (в коде) файлы для указанного target

## add_subdirectory

добавляет в текущий CMakeLists.txt “ссылку” на суб-директорию с со своим CMakeLists.txt

## conditional expression $<…>

синтаксис как в шаблонах: 
типа `SomeWeirdType<T>::integer_type`
→ если внутри указан integer_type, то извлекаем его, так же и со значениями

```makefile
target_include_directories(tgt PRIVATE /opt/include/$<CXX_COMPILER_ID>)

target_compile_definitions(tgt PRIVATE
  $<$<VERSION_LESS:$<CXX_COMPILER_VERSION>,4.2.0>:OLD_COMPILER>
)

 $<$<BOOL:${UNIX}>:rt> # если UNIX система, тогда извлекаем rt библу
```