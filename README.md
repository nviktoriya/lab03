## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

### Студент
Назарова Виктория, группа ИУ8-21

```sh
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```sh
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
$ nm print.o | grep print
$ ar rvs print.a print.o
$ file print.a
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
$ g++ example1.o print.a -o example1
$ ./example1 && echo
```

```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```

```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```sh
$ cmake -H. -B_build
$ cmake --build _build
```

```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```sh
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

```sh
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -rf log.txt
```

```sh
$ git clone https://github.com/tp-labs/lab03 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```

```sh
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```

```sh
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```

## Report

```sh
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
### Выполнение задания 1
Переносим удалённый репозиторий в локальный:
``` sh
$ mkdir ~/workspace/projects/lab03 && cd ~/workspace/projects/lab03
$ git init
$ git remote add origin https://github.com/nviktoriya/lab03
$ git pull origin master
```
Переходим в директорию *formatter_lib* и создаём в ней файл `CMakeLists.txt`:
``` sh
$ cd formatter_lib
$ touch CMakeLists.txt
```
Редактируем `CMakeLists.txt`:
``` sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(formatter_lib)
add_library(formatter STATIC formatter.h formatter.cpp)
EOF
```
Устанавливаем cmake
``` sh
$ sudo apt update
$ sudo apt install cmake
```
Результат:

Первая команда:
``` sh
Сущ:1 http://ru.archive.ubuntu.com/ubuntu jammy InRelease
Сущ:2 http://ru.archive.ubuntu.com/ubuntu jammy-updates InRelease
Сущ:3 http://ru.archive.ubuntu.com/ubuntu jammy-backports InRelease
Сущ:4 http://security.ubuntu.com/ubuntu jammy-security InRelease
Чтение списков пакетов…
Построение дерева зависимостей…
Чтение информации о состоянии…
Может быть обновлён 321 пакет. Запустите «apt list --upgradable» для показа.
```
Вторая команда:
``` sh
Чтение списков пакетов…
Построение дерева зависимостей…
Чтение информации о состоянии…
Предлагаемые пакеты:
  cmake-doc ninja-build cmake-format
Следующие НОВЫЕ пакеты будут установлены:
  cmake
Обновлено 0 пакетов, установлено 1 новых пакетов, для удаления отмечено 0 пакетов, и 321 пакетов не обновлено.
Необходимо скачать 5 010 kB архивов.
После данной операции объём занятого дискового пространства возрастёт на 21,2 MB.
Пол:1 http://ru.archive.ubuntu.com/ubuntu jammy-updates/main amd64 cmake amd64 3.22.1-1ubuntu1.22.04.2 [5 010 kB]
Получено 5 010 kB за 3с (1 728 kB/s)
Выбор ранее не выбранного пакета cmake.
(Чтение базы данных … 
(Чтение базы данных … 5%
(Чтение базы данных … 10%
(Чтение базы данных … 15%
(Чтение базы данных … 20%
(Чтение базы данных … 25%
(Чтение базы данных … 30%
(Чтение базы данных … 35%
(Чтение базы данных … 40%
(Чтение базы данных … 45%
(Чтение базы данных … 50%
(Чтение базы данных … 55%
(Чтение базы данных … 60%
(Чтение базы данных … 65%
(Чтение базы данных … 70%
(Чтение базы данных … 75%
(Чтение базы данных … 80%
(Чтение базы данных … 85%
(Чтение базы данных … 90%
(Чтение базы данных … 95%
(Чтение базы данных … 100%
(Чтение базы данных … на данный момент установлено 214000 файлов и каталогов.)
Подготовка к распаковке …/cmake_3.22.1-1ubuntu1.22.04.2_amd64.deb …
Распаковывается cmake (3.22.1-1ubuntu1.22.04.2) …
Настраивается пакет cmake (3.22.1-1ubuntu1.22.04.2) …
Обрабатываются триггеры для man-db (2.10.2-1) …
```
Создаём директорию сборки проекта:
``` sh
$ mkdir build && cd build
```
Генерируем файлы сборки CMake:
``` sh
$ cmake ..
```
Результат:
``` sh
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/viktoriya/workspace/projects/lab03/formatter_lib/build
```
Собираем проект:
``` sh
$ cmake --build . --config Release
```
Результат:
``` sh
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
### Выполнение задания 2
Переходим в директорию *formatter_ex_lib* и создаём в ней файл `CMakeLists.txt`:
``` sh
$ cd ~/workspace/projects/lab03/formatter_ex_lib
$ touch CMakeLists.txt
```
В содержимое файла добавляем следующее:
``` sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)
add_library(formatter STATIC ~/workspace/projects/lab03/formatter_lib/formatter.cpp)
target_include_directories(formatter PUBLIC ~/workspace/projects/lab03/formatter_lib)
add_library(formatter_ex STATIC formatter_ex.h formatter_ex.cpp)
target_link_libraries(formatter_ex formatter)
EOF
```
Создаём директорию сборки проекта:
``` sh
$ mkdir build && cd build
```
Генерируем файлы сборки CMake:
``` sh
$ cmake ..
```
Результат:
``` sh
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/viktoriya/workspace/projects/lab03/formatter_ex_lib/build
```
Собираем проект:
``` sh
$ cmake --build . --config Release
```
Результат:
``` sh
[ 25%] Building CXX object CMakeFiles/formatter.dir/home/viktoriya/workspace/projects/lab03/formatter_lib/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 75%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
### Выполнение задания 3
#### Создание `CMakeLists.txt` для приложения *hello_world*
Переходим в директорию *hello_world_application* и создаём в ней файл `CMakeLists.txt`:
``` sh
$ cd ~/workspace/projects/lab03/hello_world_application
$ touch CMakeLists.txt
```
В содержимое файла добавляем следующее:
``` sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(hello_world_application)
add_library(formatter STATIC ~/workspace/projects/lab03/formatter_lib/formatter.cpp)
target_include_directories(formatter PUBLIC ~/workspace/projects/lab03/formatter_lib)
add_library(formatter_ex STATIC ~/workspace/projects/lab03/formatter_ex_lib/formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC ~/workspace/projects/lab03/formatter_ex_lib)
add_executable(hello_world hello_world.cpp)
target_link_libraries(formatter_ex formatter)
target_link_libraries(hello_world formatter_ex)
EOF
```
Создаём директорию сборки проекта:
``` sh
$ mkdir build && cd build
```
Генерируем файлы сборки CMake:
``` sh
$ cmake ..
```
Результат:
``` sh
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/viktoriya/workspace/projects/lab03/hello_world_application/build
```
Собираем проект:
``` sh
$ cmake --build . --config Release
```
Результат: 
``` sh
[ 16%] Building CXX object CMakeFiles/formatter.dir/home/viktoriya/workspace/projects/lab03/formatter_lib/formatter.cpp.o
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/home/viktoriya/workspace/projects/lab03/formatter_ex_lib/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world
```

#### Создание `CMakeLists.txt` для приложения *solver*
Переходим в директорию *solver_application* и создаём в ней файл `CMakeLists.txt`:
``` sh
$ cd ~/workspace/projects/lab03/solver_application
$ touch CMakeLists.txt
```
В содержимое файла добавляем следующее:
``` sh
$ cat >> CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.4)
project(solver_application)
add_library(solver_lib STATIC ~/workspace/projects/lab03/solver_lib/solver.cpp)
target_include_directories(solver_lib PUBLIC ~/workspace/projects/lab03/solver_lib)
add_library(formatter STATIC ~/workspace/projects/lab03/formatter_lib/formatter.cpp)
target_include_directories(formatter PUBLIC ~/workspace/projects/lab03/formatter_lib)
add_library(formatter_ex STATIC ~/workspace/projects/lab03/formatter_ex_lib/formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC ~/workspace/projects/lab03/formatter_ex_lib)
add_executable(solver equation.cpp)
target_link_libraries(formatter_ex formatter)
target_link_libraries(solver formatter_ex)
target_link_libraries(solver solver_lib)
EOF
```
Создаём директорию сборки проекта:
``` sh
$ mkdir build && cd build
```
Генерируем файлы сборки CMake:
``` sh
$ cmake ..
```
Результат:
``` sh
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/viktoriya/workspace/projects/lab03/solver_application/build
```

Собираем проект:
``` sh
$ cmake --build . --config Release
```
Результат:
``` sh
[ 12%] Building CXX object CMakeFiles/solver_lib.dir/home/viktoriya/workspace/projects/lab03/solver_lib/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object CMakeFiles/formatter.dir/home/viktoriya/workspace/projects/lab03/formatter_lib/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 62%] Building CXX object CMakeFiles/formatter_ex.dir/home/viktoriya/workspace/projects/lab03/formatter_ex_lib/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex.a
[ 75%] Built target formatter_ex
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
```

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
