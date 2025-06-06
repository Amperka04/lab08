# lab04
## Домашнее задание

**Студента группы ИУ8-21**
**Барш Иван**

Репозиторий с файлами: https://github.com/Amperka04/lab04.git

<details>
<summary>предыдущее задание (lab03)</summary>

1. Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter

Содержимое файла CMakeLists.txt в /formatter_lib:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_lib)

set(SOURCE_LIB formatter.cpp)

add_library(formatter ${SOURCE_LIB} formatter.h)
```

Запустили CMake для генерации файлов сборки:

```
cmake ..
```

Вывод: 

```CMake Deprecation Warning at CMakeLists.txt:40 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/bivan/Amperka04/workspace/projects/lab03/formatter_lib 
```

Собрали проект командой ```make```

Вывод: 

```
[ 50%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```



2. У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter

В директории lab03 отредактировали файл CMakeLists.txt:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
cmake_minimum_required(VERSION 3.4)

project(lab03)

include_directories("formatter_lib/" "formatter_ex_lib/" "solver_lib/")

add_subdirectory(formatter_ex_lib)
add_subdirectory(formatter_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)
add_subdirectory(hello_world_application)

```

В директории lab03/formatter_ex_lib создали CMakeLists.txt с следующим содержимым:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex_lib_pr)

set(SOURCE_LIB formatter_ex.cpp)

add_library(formatter_ex ${SOURCE_LIB} formatter_ex.h)

target_link_libraries(formatter_ex formatter)

```

Аналогично предыдущему заданию запустили CMake для генерации файлов сборки, и cобрали проект командой ```make```

Вывод:
```
[ 25%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 75%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```



3. Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

    hello_world, которое использует библиотеку formatter_ex;
    solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.

В директории lab03/solver_lib создали CMakeLists.txt с следующим содержимым:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(solver_lib)
set(SOURCE_LIB solver.cpp)
add_library(solver_l STATIC ${SOURCE_LIB})
```

В директории lab03/solver_application создали CMakeLists.txt с следующим содержимым:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver)

add_executable(solva equation.cpp)

target_link_libraries(solva formatter_ex)

target_link_libraries(solva solver_l)
```

В директории lab03/hello_world_application создали CMakeLists.txt с следующим содержимым:
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(print_hw)

add_executable(hello hello_world.cpp)

target_link_libraries(hello formatter_ex)
```

Аналогично предыдущему заданию запустили CMake для генерации файлов сборки, и cобрали проект командой ```make```

Вывод:
```
[ 20%] Built target formatter
[ 40%] Built target formatter_ex
[ 50%] Building CXX object solver_lib/CMakeFiles/solver_l.dir/solver.cpp.o
[ 60%] Linking CXX static library libsolver_l.a
[ 60%] Built target solver_l
[ 70%] Building CXX object solver_application/CMakeFiles/solva.dir/equation.cpp.o
[ 80%] Linking CXX executable solva
[ 80%] Built target solva
[ 90%] Building CXX object hello_world_application/CMakeFiles/hello.dir/hello_world.cpp.o
[100%] Linking CXX executable hello
[100%] Built target hello
```
</details>

Скопировали репозиторий lab03 в lab04:
```sh
git clone https://github.com/${GITHUB_USERNAME}/lab03 lab04
git remote remove origin 
git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
```

Сделали папку для GitHub Actions:
```sh
.github/workflows
```

В ней же создали файл для автосборки ```.github/workflows/build.yml```
и прописали в нем:
```sh
name: GitHub Actions 
run-name: ${{ github.actor }} test
on: [push]
jobs:
  Test:
    runs-on: ubuntu-latest
    steps:
    - name: Check out repository code
      uses: actions/checkout@v4
    - name: Вывод содержимого директории
      run: |
        ls ${{ github.workspace }}
    - name: CMake Action
      uses: threeal/cmake-action@v2.1.0
    - name: Компиляция
      run: cmake -H. -Bbuild
    - name: Компиляция2
      run:  cmake --build build
    - name: Вывод содержимого директории
      run: |
        ls ${{ github.workspace }}
    - name: Тест
      working-directory: ./build/hello_world_application
      run: ./hello
  Test_win:
    runs-on: windows-latest
    steps:
    - name: Check out repository code
      uses: actions/checkout@v4
    - name: Вывод содержимого директории
      run: dir ${{ github.workspace }}
    - name: Setup CMake
      uses: threeal/cmake-action@v2.1.0
    - name: Компиляция
      run: cmake --build build --config Release
    - name: Вывод содержимого директории
      run: dir ${{ github.workspace }}\build
    - name: Тест
      working-directory: ./build/hello_world_application/Release
      run: .\hello.exe
```

Запушили в гит:
```sh
git add .github/workflows/build.yml
git add README.md
git commit -m "Добавили GitHub Actions"
git push origin main
```
