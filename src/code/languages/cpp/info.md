---
tags: [code, languages, cpp]
aliases: [C++]
---

# C++

> Это компилируемый, статически типизированный мультипарадигменный язык программирования (ЯП). Особенностью плюсов является смесь низкоуровневого подхода (контроль над памятью из С) и высокоуровневого программирования.

## Компилируемый
Это означает, что написанный код переводится(транслируется) компилятором в машинный код, который потом полностью испольняется. Со стороны пользователя это значит, что код сначала проверится, а потом только запустится. Интерпретируемые, например, в real time исполняют каждую текущую строчку.

## Статически типизированный
Статическая типизация - параметр подпрограммы, при котором возвращаемый значения фукнций/методов/переменных определяется типом ещё в момент их объявления. В динамический типизации x будет int(целое число), только когда ему будет присвоена 5, в статический типизации необходимо заранее указать тип х при его объявлении - int x; x = 5.

## Мультипарадигменный
Содержит больше 2 (или 3) парадигм. 

- Обобщённое программирование
- Метапрограммирование
- Объектно-ориентированное программирование
- Процедурное программирование

# Как из ничего чё т запустить…
Первым делом понадобится компилятор нашего кода, для винды это Gcc/G++(MinGw) или Clang для любителей посидеть в старбакс. 

- Для винды туториал:
    1. ***MinGW official website is now down (as of 24th March 2021). Hence, instead of their official website, download from [MinGW SourceForge page](https://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download)***
    2. Look for **mingw-get-setup.exe** for downloading. Download it and launch the installer. Accept the terms and move on.
    3. You'll now see that the installer is connecting to the Internet and downloading a lot of tiny and small files. Wait till it ends.
    4. Right when it ends (which won't take long), you'll be presented a window with title **MinGW Installation Manager**. You should be in the 'Basic Setup' tab by default when it launches. If not, click on **Basic Setup**.
    5. Out of the numerous check boxes presented to you on the right side, tick "**mingw32-gcc-g++-bin**". If you are prompted with a menu, click on Mark for Install.
    6. Then on the top left corner click on **Installation > Apply Changes**. And wait while it downloads a billion files and installs them.
    
    ![https://res.cloudinary.com/practicaldev/image/fetch/s--rPjjXRTg--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/buyja9obbjs1x7p8h6cd.png](https://res.cloudinary.com/practicaldev/image/fetch/s--rPjjXRTg--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/buyja9obbjs1x7p8h6cd.png)
    
    1. Now you gotta edit your "Environment Variables" as well, so that gcc works in cmd no matter the file location.
    2. For that go to **Windows Explorer > Right click on This PC > Properties > Advanced system settings > Environment Variables** or you could just search for "Environment Variables" in Windows Search...
    3. At the bottom "System Variables" panel, look for a Variable named "Path" and double click on it. Some systems show a good UI for adding a New Path easily (by clicking New), else you just need to add ; at the end and add the following path
        
        ```
        C:\MinGW\bin
        
        ```
        
        ### (This is assuming you didn't manually change any installation paths and went with just clicking 'Next' during installation)
        
    4. Click on OK, and OK and close the other windows. Open a Command Prompt Terminal and try typing `gcc --version` and press Enter.
        
        If you get something like
        
        ```
        gcc (MinGW.org GCC Build-2) 9.2.0
        Copyright (C) 2019 Free Software Foundation, Inc.
        This is free software; see the source for copying conditions.
        There is NO warranty; not even for MERCHANTABILITY or FITNESS
        FOR A PARTICULAR PURPOSE.
        
        ```
        
        gcc has been successfully installed in your PC. Enjoy!
        

Далее с помощью команд собираем проект (cpp файлик) с помощью gcc/g++, микро тутор по нему тут:

[Compiling with g++ - GeeksforGeeks](https://www.geeksforgeeks.org/compiling-with-g-plus-plus/)