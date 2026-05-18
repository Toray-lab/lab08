# Отчёт к лабораторной работе №5
Подготовка окружения
```bash
$ export GITHUB_USERNAME=Toray-lab
$ export GITHUB_USERNAME=Toray-lab
$ alias gsed=sed
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/Toray-lab/workspace ~/Toray-lab/workspace
$ source scripts/activate
```

Клонирование предыдущей работы и смена remote
```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab06
Cloning into 'projects/lab06'...
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 16 (delta 2), reused 10 (delta 1), pack-reused 0 (from 0)
Receiving objects: 100% (16/16), 10.67 KiB | 3.56 MiB/s, done.
Resolving deltas: 100% (2/2), done.
$ cd projects/lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
```

Добавление GTest
```bash
$ mkdir third-party
$ git submodule add https://github.com/google/googletest third-party/gtest
Cloning into '/home/vlad/Toray-lab/workspace/projects/lab06/third-party/gtest'...
remote: Enumerating objects: 28627, done.
remote: Counting objects: 100% (64/64), done.
remote: Compressing objects: 100% (48/48), done.
remote: Total 28627 (delta 32), reused 16 (delta 16), pack-reused 28563 (from 2)
Receiving objects: 100% (28627/28627), 13.78 MiB | 6.16 MiB/s, done.
Resolving deltas: 100% (21268/21268), done.
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..
Note: switching to 'release-1.8.1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
$ git add third-party/gtest
$ git commit -m"added gtest framework"
[main 6525442] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```

Настройка CMakeLists.txt для поддержки тестов
```bash
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
> option(BUILD_TESTS "Build tests" OFF)
> ' CMakeLists.txt

$ cat >> CMakeLists.txt <<EOF
>
> if(BUILD_TESTS)
>   enable_testing()
>   add_subdirectory(third-party/gtest)
>   file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
>   add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
>   target_link_libraries(check \${PROJECT_NAME} gtest_main)
>   add_test(NAME check COMMAND check)
> endif()
> EOF
```

Создание теста
```bash
$ mkdir tests
$ cat > tests/test1.cpp <<EOF
> #include <print.hpp>
> #include <gtest/gtest.h>
>
> TEST(Print, InFileStream)
> {
>   std::string filepath = "file.txt";
>   std::string text = "hello";
>   std::ofstream out{filepath};
>   print(text, out);
>   out.close();
>
>   std::string result;
>   std::ifstream in{filepath};
>   in >> result;
>
>   EXPECT_EQ(result, text);
> }
> EOF
```

Сборка и запуск тестов
```bash
$ cmake -H. -B_build -DBUILD_TESTS=ON
CMake Warning (dev) in CMakeLists.txt:
  No project() command is present.  The top-level CMakeLists.txt file must
  contain a literal, direct call to the project() command.  Add a line of
  code such as

    project(ProjectName)

  near the top of the file, but after cmake_minimum_required().

  CMake is pretending there is a "project(Project)" command on the first
  line.
This warning is for project developers.  Use -Wno-dev to suppress it.

CMake Warning (dev) in CMakeLists.txt:
  cmake_minimum_required() should be called prior to this top-level project()
  call.  Please see the cmake-commands(7) manual for usage documentation of
  both commands.
This warning is for project developers.  Use -Wno-dev to suppress it.

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
CMake Deprecation Warning at third-party/gtest/CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at third-party/gtest/googlemock/CMakeLists.txt:42 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Deprecation Warning at third-party/gtest/googletest/CMakeLists.txt:49 (cmake_minimum_required):
  Compatibility with CMake < 3.10 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value.  Or, use the <min>...<max> syntax
  to tell CMake that the project requires at least <min> but has been updated
  to work with policies introduced by <max> or earlier.


CMake Warning (dev) at third-party/gtest/googletest/cmake/internal_utils.cmake:239 (find_package):
  Policy CMP0148 is not set: The FindPythonInterp and FindPythonLibs modules
  are removed.  Run "cmake --help-policy CMP0148" for policy details.  Use
  the cmake_policy command to set the policy and suppress this warning.

Call Stack (most recent call first):
  third-party/gtest/googletest/CMakeLists.txt:84 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found PythonInterp: /usr/bin/python3 (found version "3.13.5")
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
CMake Warning (dev) in CMakeLists.txt:
  No cmake_minimum_required command is present.  A line of code such as

    cmake_minimum_required(VERSION 3.31)

  should be added at the top of the file.  The version specified may be lower
  if you wish to support older CMake versions for this project.  For more
  information run "cmake --help-policy CMP0000".
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (3.4s)
CMake Warning (dev) at CMakeLists.txt:7 (add_executable):
  Policy CMP0003 should be set before this line.  Add code such as

    if(COMMAND cmake_policy)
      cmake_policy(SET CMP0003 NEW)
    endif(COMMAND cmake_policy)

  as early as possible but after the most recent call to
  cmake_minimum_required or cmake_policy(VERSION).  This warning appears
  because target "check" links to some libraries for which the linker must
  search:

    Project

  and other libraries with known full path:

    /home/vlad/Toray-lab/workspace/projects/lab06/_build/third-party/gtest/googlemock/gtest/libgtest_main.a

  CMake is adding directories in the second list to the linker search path in
  case they are needed to find libraries from the first list (for backwards
  compatibility with CMake 2.4).  Set policy CMP0003 to OLD or NEW to enable
  or disable this behavior explicitly.  Run "cmake --help-policy CMP0003" for
  more information.
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab06/_build
$ cmake --build _build
[ 16%] Built target print
[ 33%] Built target gtest
[ 50%] Built target gtest_main
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library libgmock.a
[ 83%] Built target gmock
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
$ cmake --build _build --target test
Running tests...
Test project /home/vlad/Toray-lab/workspace/projects/lab06/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.02 sec
$ _build/check
Running main() from /home/vlad/Toray-lab/workspace/projects/lab06/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
$ cmake --build _build --target test -- ARGS=--verbose
Running tests...
UpdateCTestConfiguration  from :/home/vlad/Toray-lab/workspace/projects/lab06/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/vlad/Toray-lab/workspace/projects/lab06/_build/DartConfiguration.tcl
Test project /home/vlad/Toray-lab/workspace/projects/lab06/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/vlad/Toray-lab/workspace/projects/lab06/_build/check
1: Working Directory: /home/vlad/Toray-lab/workspace/projects/lab06/_build
1: Test timeout computed to be: 10000000
1: Running main() from /home/vlad/Toray-lab/workspace/projects/lab06/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (1 ms)
1: [----------] 1 test from Print (1 ms total)
1:
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (1 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.02 sec
```

Обновление документации и CI
```bash
$ gsed -i 's/lab04/lab06/g' README.md
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml
$ gsed -i '/cmake --build _build --target install/a\
> - cmake --build _build --target test -- ARGS=--verbose
> ' .travis.yml
```

Проверяем конфигурацию Travis, коммитим, пушим
```bash
$ travis lint
Hooray, .travis.yml looks valid :)
$ git add .travis.yml tests
$ git add -p
$ git commit -m"added tests"
[main 4f4e15d] added tests
 135 files changed, 13365 insertions(+), 10 deletions(-)
 create mode 100644 CMakeLists.txt
 create mode 100644 _build/CMakeCache.txt
 create mode 100644 _build/CMakeFiles/3.31.6/CMakeCCompiler.cmake
 create mode 100644 _build/CMakeFiles/3.31.6/CMakeCXXCompiler.cmake
 create mode 100755 _build/CMakeFiles/3.31.6/CMakeDetermineCompilerABI_C.bin
 create mode 100755 _build/CMakeFiles/3.31.6/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 _build/CMakeFiles/3.31.6/CMakeSystem.cmake
 create mode 100644 _build/CMakeFiles/3.31.6/CompilerIdC/CMakeCCompilerId.c
 create mode 100755 _build/CMakeFiles/3.31.6/CompilerIdC/a.out
 create mode 100644 _build/CMakeFiles/3.31.6/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100755 _build/CMakeFiles/3.31.6/CompilerIdCXX/a.out
 create mode 100644 _build/CMakeFiles/CMakeConfigureLog.yaml
 create mode 100644 _build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 _build/CMakeFiles/Makefile.cmake
 create mode 100644 _build/CMakeFiles/Makefile2
 create mode 100644 _build/CMakeFiles/TargetDirectories.txt
 create mode 100644 _build/CMakeFiles/check.dir/DependInfo.cmake
 create mode 100644 _build/CMakeFiles/check.dir/build.make
 create mode 100644 _build/CMakeFiles/check.dir/cmake_clean.cmake
 create mode 100644 _build/CMakeFiles/check.dir/compiler_depend.internal
 create mode 100644 _build/CMakeFiles/check.dir/compiler_depend.make
 create mode 100644 _build/CMakeFiles/check.dir/compiler_depend.ts
 create mode 100644 _build/CMakeFiles/check.dir/depend.make
 create mode 100644 _build/CMakeFiles/check.dir/flags.make
 create mode 100644 _build/CMakeFiles/check.dir/link.d
 create mode 100644 _build/CMakeFiles/check.dir/link.txt
 create mode 100644 _build/CMakeFiles/check.dir/progress.make
 create mode 100644 _build/CMakeFiles/check.dir/tests/test1.cpp.o
 create mode 100644 _build/CMakeFiles/check.dir/tests/test1.cpp.o.d
 create mode 100644 _build/CMakeFiles/cmake.check_cache
 create mode 100644 _build/CMakeFiles/print.dir/DependInfo.cmake
 create mode 100644 _build/CMakeFiles/print.dir/build.make
 create mode 100644 _build/CMakeFiles/print.dir/cmake_clean.cmake
 create mode 100644 _build/CMakeFiles/print.dir/cmake_clean_target.cmake
 create mode 100644 _build/CMakeFiles/print.dir/compiler_depend.internal
 create mode 100644 _build/CMakeFiles/print.dir/compiler_depend.make
 create mode 100644 _build/CMakeFiles/print.dir/compiler_depend.ts
 create mode 100644 _build/CMakeFiles/print.dir/depend.make
 create mode 100644 _build/CMakeFiles/print.dir/flags.make
 create mode 100644 _build/CMakeFiles/print.dir/link.txt
 create mode 100644 _build/CMakeFiles/print.dir/progress.make
 create mode 100644 _build/CMakeFiles/print.dir/src/print.cpp.o
 create mode 100644 _build/CMakeFiles/print.dir/src/print.cpp.o.d
 create mode 100644 _build/CMakeFiles/progress.marks
 create mode 100644 _build/CTestTestfile.cmake
 create mode 100644 _build/Makefile
 create mode 100644 _build/Testing/Temporary/CTestCostData.txt
 create mode 100644 _build/Testing/Temporary/LastTest.log
 create mode 100755 _build/check
 create mode 100644 _build/cmake_install.cmake
 create mode 100644 _build/file.txt
 create mode 100644 _build/libprint.a
 create mode 100644 _build/third-party/gtest/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 _build/third-party/gtest/CMakeFiles/progress.marks
 create mode 100644 _build/third-party/gtest/CTestTestfile.cmake
 create mode 100644 _build/third-party/gtest/Makefile
 create mode 100644 _build/third-party/gtest/cmake_install.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/DependInfo.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/build.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/cmake_clean.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/cmake_clean_target.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/compiler_depend.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/compiler_depend.ts
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/depend.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/flags.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/link.txt
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/progress.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o.d
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/DependInfo.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/build.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/cmake_clean.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/cmake_clean_target.cmake
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/compiler_depend.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/compiler_depend.ts
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/depend.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/flags.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/link.txt
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/progress.make
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o.d
 create mode 100644 _build/third-party/gtest/googlemock/CMakeFiles/progress.marks
 create mode 100644 _build/third-party/gtest/googlemock/CTestTestfile.cmake
 create mode 100644 _build/third-party/gtest/googlemock/Makefile
 create mode 100644 _build/third-party/gtest/googlemock/cmake_install.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/Export/0c08b8e77dd885bfe55a19a9659d9fc1/GTestTargets-noconfig.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/Export/0c08b8e77dd885bfe55a19a9659d9fc1/GTestTargets.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/DependInfo.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/build.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/cmake_clean.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/cmake_clean_target.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/compiler_depend.internal
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/compiler_depend.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/compiler_depend.ts
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/depend.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/flags.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/link.txt
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/progress.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o.d
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/DependInfo.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/build.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/cmake_clean.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/cmake_clean_target.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/compiler_depend.internal
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/compiler_depend.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/compiler_depend.ts
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/depend.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/flags.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/link.txt
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/progress.make
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o.d
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CMakeFiles/progress.marks
 create mode 100644 _build/third-party/gtest/googlemock/gtest/CTestTestfile.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/Makefile
 create mode 100644 _build/third-party/gtest/googlemock/gtest/cmake_install.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/GTestConfig.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/GTestConfigVersion.cmake
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/gmock.pc
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/gmock_main.pc
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/gtest.pc
 create mode 100644 _build/third-party/gtest/googlemock/gtest/generated/gtest_main.pc
 create mode 100644 _build/third-party/gtest/googlemock/gtest/libgtest.a
 create mode 100644 _build/third-party/gtest/googlemock/gtest/libgtest_main.a
 create mode 100644 _build/third-party/gtest/googlemock/libgmock.a
 create mode 100644 _build/third-party/gtest/googlemock/libgmock_main.a
 create mode 100644 file.txt
 create mode 100644 include/print.hpp
 create mode 100644 src/print.cpp
 create mode 100644 tests/test1.cpp
$ git push main
Username for 'https://github.com': Toray-lab
Password for 'https://Toray-lab@github.com':
Everything up-to-date
$ travis login --auto
/home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:334:in `format': wrong number of arguments (given 5, expected 1..3) (ArgumentError)
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:315:in `store_error'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:235:in `rescue in execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:200:in `execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /usr/local/bin/travis:25:in `load'
        from /usr/local/bin/travis:25:in `<main>'
/home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:91:in `hub_tokens': undefined method `each' for nil (NoMethodError)

        hub.fetch(host, []).each do |entry|
                           ^^^^^
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:58:in `possible_tokens'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:42:in `each_token'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:37:in `with_token'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/login.rb:30:in `login'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/login.rb:52:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:208:in `execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /usr/local/bin/travis:25:in `load'
        from /usr/local/bin/travis:25:in `<main>'
$ travis enable
Detected repository as Toray-lab/lab06, is this correct? |yes| yes
Toray-lab/lab06: enabled :)
$ mkdir artifacts
```
Создание CPackConfig.cmake
```bash
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF

$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF

$ cat >> CPackConfig.cmake <<EOF
set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF

$ cat >> CPackConfig.cmake <<EOF
set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF

$ cat >> CPackConfig.cmake <<EOF
set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF

$ cat >> CPackConfig.cmake <<EOF
include(CPack)
EOF

$ cat >> CMakeLists.txt <<EOF
include(CPackConfig.cmake)
EOF
```

Обновление README и создание лицензии
```bash
$ gsed -i 's/lab05/lab06/g' README.md
$ cat > LICENSE <<EOF
MIT License
Copyright (c) $(date +%Y) ${GITHUB_USERNAME}
Permission is hereby granted...
EOF
```

Коммит и тегирование
```bash
$ git add .
$ git commit -m"added cpack config"
[main a3099fb] added cpack config
 6 files changed, 64 insertions(+), 18 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
 create mode 100644 LICENSE
$ git tag v0.1.0.0
$ git push origin main -
-tags
Username for 'https://github.com': Toray-lab
Password for 'https://Toray-lab@github.com':
Enumerating objects: 195, done.
Counting objects: 100% (195/195), done.
Delta compression using up to 4 threads
Compressing objects: 100% (111/111), done.
Writing objects: 100% (195/195), 996.51 KiB | 43.33 MiB/s, done.
Total 195 (delta 62), reused 184 (delta 59), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (62/62), done.
To https://github.com/Toray-lab/lab06
 * [new tag]         v0.1.0.0 -> v0.1.0.0
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/Toray-lab/lab06'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
$ git push origin main --force
Username for 'https://github.com': Toray-lab
Password for 'https://Toray-lab@github.com':
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/Toray-lab/lab06
 + d5eec4d...a3099fb main -> main (forced update)
```

Настройка Travis CI
```bash
$ travis login --auto
/home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:334:in `format': wrong number of arguments (given 5, expected 1..3) (ArgumentError)
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:315:in `store_error'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:235:in `rescue in execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:200:in `execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /usr/local/bin/travis:25:in `load'
        from /usr/local/bin/travis:25:in `<main>'
/home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:91:in `hub_tokens': undefined method `each' for nil (NoMethodError)

        hub.fetch(host, []).each do |entry|
                           ^^^^^
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:58:in `possible_tokens'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:42:in `each_token'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/tools/github.rb:37:in `with_token'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/login.rb:30:in `login'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/login.rb:52:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli/command.rb:208:in `execute'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.local/share/gem/ruby/3.3.0/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /usr/local/bin/travis:25:in `load'
        from /usr/local/bin/travis:25:in `<main>'
$ travis enable
Detected repository as Toray-lab/lab06, is this correct? |yes| yes
Toray-lab/lab06: enabled :)
```

Локальная генерация пакетов
```bash
$ cmake -H. -B_build
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
-- Configuring done (2.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab06/_build
$ cmake --build _build
[ 50%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
$ cd _build
$ cpack -G "TGZ"
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print []
CPack: Create package
CPack: - package: /home/vlad/Toray-lab/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
```

Сохранение артефактов
```bash
$ mkdir -p ../artifacts
$ mv *.tar.gz ../artifacts/
$ cd ..
$ tree artifacts
artifacts
└── print-0.1.0.0-Linux.tar.gz

1 directory, 1 file
```
