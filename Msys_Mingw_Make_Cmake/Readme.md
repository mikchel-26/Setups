# MSYS2

Скачать установщик с официального сайта https://www.msys2.org. Устанавливать лучше в корень какого-то из дисков.

## Обновление пакетного менеджера pacman

Запустить `MSYS2 MSYS` из Пуска.

```msys
pacman -Suy
```

# GCC UCRT64

Для MSYS существует несколько окружений, различающихся toolchin-ами и версиями библиотек:
* MINGW64 и MINGW32
* CLANG32, CLANG64, CLANGARM64
* UCRT64 - один из наиболее современных, будем его использовать.

```msys
pacman -S mingw-w64-ucrt-x86_64-gcc
```

При проверке, почему-то ошибка, хотя mingw64 спокойно определялась.

```msys
gcc --version
-bash: gcc: command not found
```

Проверка из utrc64 успешна

```utrc64
$ gcc --version
gcc.exe (Rev1, Built by MSYS2 project) 14.2.0
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```


Установка группы пакетов base-devel, которая включает make и дополнительные инструменты, необходимые для компиляции из исходного кода. 

```msys
pacman -S base base-devel
```

Добавляем в переменную среды `Path` путь `D:\msys64\ucrt64\bin`.  
После этого gcc становится доступным из командной строки Windows.

```cmd
C:\Users\user>gcc --version
gcc (Rev1, Built by MSYS2 project) 14.2.0
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

# Make

Устанавливаем.

```msys
pacman -S make
```

Проверяем

```msys
$ make --version

GNU Make 4.4.1
Built for x86_64-pc-msys
Copyright (C) 1988-2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

Добавляем в переменную среды `Path` путь `D:\msys64\usr\bin`.  
После этого make становится доступным из командной строки Windows.

```cmd
C:\Users\user>make --version
GNU Make 4.4.1
Built for x86_64-pc-msys
Copyright (C) 1988-2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

# Cmake

Установим через оф. сайт `https://cmake.org`. Нет уверенности в том, что версия из MSYS2 поддерживает компилятор Visual Studio.  
В переменную среды `Path` путь к Cmake установщик предлагает добавить автоматически (соглашаемся).

Проверяем

```cmd
C:\Users\user>cmake --version
cmake version 3.30.3

CMake suite maintained and supported by Kitware (kitware.com/cmake)
```


Файл main.c

```c
#include <stdio.h>

int main()
{
    printf("Hello world!\r\n");
    return 0;
}
```

Файл CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(HelloWorld)

add_executable(HelloWorld src/main.c)
```

Создаем папку build_vs для решения Visual Studio.  
Переходим в неё.  
Генерируем решение ссылаясь на директорию на один уровень выше `..`, где лежит основной CMakeLists.txt.  
Билдим проект из текущей директории.  
Запускаем файл .exe.

```cmd
mkdir build_vs
cd build_vs
cmake ..
cmake --build .
.\Debug\HelloWorld.exe
```

Проверяем сборку в Visual Studio. Открываем `HelloWorld.sln`. В свойствах решения выбран пункт `Один запускаемый поект`, в нём необходимо в выпадающем списке поменять на `HelloWorld`. Пересобираем решение и запускаем.

Создаем папку build_gcc для решения msys gcc.  
Переходим в неё.  
Генерируем решение ссылаясь на директорию на один уровень выше `..`, где лежит основной CMakeLists.txt.  
Билдим проект из текущей директории.  
Запускаем файл .exe.

```cmd
mkdir build_gcc
cd build_gcc
cmake .. -G "MSYS Makefiles"
cmake --build .
.\HelloWorld.exe
```