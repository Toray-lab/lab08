# Отчёт к лабораторной работе №3
Настройка окружения
```bash
$ export GITHUB_USERNAME=Toray-lab
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/Toray-lab/workspace ~/Toray-lab/workspace
$ source scripts/activate
```
Клонирование lab02 как основы для lab03
```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
Cloning into 'projects/lab03'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 10 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (10/10), 4.60 KiB | 4.60 MiB/s, done.
Resolving deltas: 100% (1/1), done.
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
$ git branch -m main
$ git push -u origin main
Username for 'https://github.com': Toray-lab
Password for 'https://%D0%95%D1%89Toray-lab@github.com':
remote: Invalid username or token. Password authentication is not supported for Git operations.
fatal: Authentication failed for 'https://github.com/Toray-lab/lab03.git/'
git push -u origin main
```
Компиляция без CMake
```bash
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
print.o
$ nm print.o | grep print
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
$ ar rvs print.a print.o
ar: creating print.a
a - print.o
$ file print.a
print.a: current ar archive
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
example1.o
$ g++ example1.o print.a -o example1
$ nm example2.o
0000000000000000 V DW.ref.__gxx_personality_v0
                 U __gxx_personality_v0
0000000000000000 T main
                 U _Unwind_Resume
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED2Ev
0000000000000000 n _ZNSt15__new_allocatorIcED5Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZSt21ios_base_library_initv
0000000000000000 r _ZStL19piecewise_construct
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
hello
$ rm -rf example1.o example2.o print.o print.a example1 example2 log.txt
```
Создание CMakeLists.txt
```bash
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF

$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF

$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```
Создание сборки CMake 
```bash
$ cmake -H. -B_build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
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
-- Configuring done (1.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab03/_build
$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
ls -la _build/libprint.a
-rw-rw-r-- 1 vlad vlad 2110 Apr 22 17:36 _build/libprint.a
```
Добавление исполняемых файлов в CMakeLists.txt
```bash
$ cat >> CMakeLists.txt <<EOF
add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF
target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```
Пересборка всех целе  
```bash
$ cmake --build _build
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab03/_build
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[ 66%] Linking CXX executable example1
[ 66%] Built target example1
[ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[100%] Linking CXX executable example2
[100%] Built target example2
$ cmake --build _build --target print
[100%] Built target print
$ cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2
$ cmake --build _build --target print
[100%] Built target print
$ cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2
```
Запуск собранных примеров
```bash
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -f log.txt
```
Установка проекта
```bash
$ git clone https://github.com/tp-labs/lab03 tmp
Cloning into 'tmp'...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Receiving objects: 100% (91/91), 1.02 MiB | 2.71 MiB/s, done.
Resolving deltas: 100% (41/41), done.
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
$ cat CMakeLists.txt
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
```
Соберём установку в папку _install
```bash
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab03/_build
$ cmake --build _build --target install
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/vlad/Toray-lab/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/vlad/Toray-lab/workspace/projects/lab03/_install/include
-- Installing: /home/vlad/Toray-lab/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/vlad/Toray-lab/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/vlad/Toray-lab/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
$ tree _install
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

4 directories, 4 files
```
Фиксируем изменения в git
```bash
$ git add CMakeLists.txt
$ git commit -m "added CMakeLists.txt with install"
[main 075dd30] added CMakeLists.txt with install
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
$ git push origin main
```
