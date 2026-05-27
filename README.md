# Отчёт к лабораторной работе №7
Подготовка окружения
```bash
export GITHUB_USERNAME=Toray-lab
alias gsed=sed                                    
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
```

Клонирование lab06 и настройка нового репозитория
```bash
git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
cd projects/lab07
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```

Добавление HunterGate
```bash
$ mkdir -p cmake/Hunter
$ wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake
$ gsed -i '/cmake_minimum_required(VERSION 3.4)/a\

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)
' CMakeLists.txt
```

Удаление встроенного GTest и переход на пакет из Hunter
```bash
$ git rm -rf third-party/gtest
$ gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
' CMakeLists.txt
$ gsed -i 's/add_subdirectory(third-party/gtest)//' CMakeLists.txt
$ gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt
```

Первая сборка и тесты
```bash


$ ls -la $HOME/.hunter
total 12
drwxrwxr-x  3 vlad vlad 4096 May 27 17:27 .
drwx------ 20 vlad vlad 4096 May 27 17:27 ..
drwxrwxr-x  5 vlad vlad 4096 May 27 17:28 _Base
```

Работа с локальной копией Hunter
```bash
$ git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter
Cloning into '/home/vlad/projects/hunter'...
remote: Enumerating objects: 56028, done.
remote: Counting objects: 100% (282/282), done.
remote: Compressing objects: 100% (75/75), done.
remote: Total 56028 (delta 238), reused 208 (delta 206), pack-reused 55746 (from 3)
Receiving objects: 100% (56028/56028), 14.51 MiB | 1.86 MiB/s, done.
Resolving deltas: 100% (35095/35095), done.
$ export HUNTER_ROOT=$HOME/projects/hunter
$ rm -rf _builds
$ cmake -H. -B_builds -DBUILD_TESTS=ON
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/vlad/projects/hunter
-- [hunter] [ Hunter-ID: xxxxxxx | Toolchain-ID: d1e9b55 | Config-ID: dde7d8a ]
-- [hunter] GTEST_ROOT: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Install (ver.: 1.15.2)
-- [hunter] Building GTest
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/google/googletest/archive/v1.15.2.tar.gz'
-- [download 1% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 13% complete]
-- [download 15% complete]
-- [download 17% complete]
-- [download 19% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 24% complete]
-- [download 26% complete]
-- [download 27% complete]
-- [download 29% complete]
-- [download 31% complete]
-- [download 32% complete]
-- [download 34% complete]
-- [download 36% complete]
-- [download 38% complete]
-- [download 40% complete]
-- [download 43% complete]
-- [download 45% complete]
-- [download 46% complete]
-- [download 48% complete]
-- [download 49% complete]
-- [download 50% complete]
-- [download 52% complete]
-- [download 54% complete]
-- [download 56% complete]
-- [download 58% complete]
-- [download 60% complete]
-- [download 62% complete]
-- [download 63% complete]
-- [download 64% complete]
-- [download 66% complete]
-- [download 68% complete]
-- [download 70% complete]
-- [download 72% complete]
-- [download 73% complete]
-- [download 74% complete]
-- [download 75% complete]
-- [download 76% complete]
-- [download 77% complete]
-- [download 79% complete]
-- [download 80% complete]
-- [download 82% complete]
-- [download 84% complete]
-- [download 86% complete]
-- [download 87% complete]
-- [download 88% complete]
-- [download 90% complete]
-- [download 92% complete]
-- [download 94% complete]
-- [download 95% complete]
-- [download 97% complete]
-- [download 98% complete]
-- [download 99% complete]
-- [download 100% complete]
-- verifying file...
       file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
     dst='/home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No update step for 'GTest-Release'
[ 25%] No patch step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (0.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtest.a
[ 25%] Built target gtest
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_main.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmock.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
  SHA1='568d58e26bd4e838449ca7ab8ebc152b3cbd210d'
-- extracting...
     src='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
     dst='/home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No update step for 'GTest-Debug'
[ 75%] No patch step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (0.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtestd.a
[ 25%] Built target gtest
[ 37%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_maind.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmockd.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmockd.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock_maind.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtestd.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest_maind.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest)
-- [hunter] Cache saved: /home/vlad/projects/hunter/_Base/Cache/raw/849f707d4726efc553f4719b2301e898bb6035d8.tar.bz2
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (59.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab07/_builds
$ cmake --build _builds
[ 25%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test
```

Просмотр конфигураций Hunter
```bash
$ git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter
Cloning into '/home/vlad/projects/hunter'...
remote: Enumerating objects: 56028, done.
remote: Counting objects: 100% (282/282), done.
remote: Compressing objects: 100% (75/75), done.
remote: Total 56028 (delta 238), reused 208 (delta 206), pack-reused 55746 (from 3)
Receiving objects: 100% (56028/56028), 14.51 MiB | 1.86 MiB/s, done.
Resolving deltas: 100% (35095/35095), done.
$ export HUNTER_ROOT=$HOME/projects/hunter
$ rm -rf _builds
$ cmake -H. -B_builds -DBUILD_TESTS=ON
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/vlad/projects/hunter
-- [hunter] [ Hunter-ID: xxxxxxx | Toolchain-ID: d1e9b55 | Config-ID: dde7d8a ]
-- [hunter] GTEST_ROOT: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Install (ver.: 1.15.2)
-- [hunter] Building GTest
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/google/googletest/archive/v1.15.2.tar.gz'
-- [download 1% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 13% complete]
-- [download 15% complete]
-- [download 17% complete]
-- [download 19% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 24% complete]
-- [download 26% complete]
-- [download 27% complete]
-- [download 29% complete]
-- [download 31% complete]
-- [download 32% complete]
-- [download 34% complete]
-- [download 36% complete]
-- [download 38% complete]
-- [download 40% complete]
-- [download 43% complete]
-- [download 45% complete]
-- [download 46% complete]
-- [download 48% complete]
-- [download 49% complete]
-- [download 50% complete]
-- [download 52% complete]
-- [download 54% complete]
-- [download 56% complete]
-- [download 58% complete]
-- [download 60% complete]
-- [download 62% complete]
-- [download 63% complete]
-- [download 64% complete]
-- [download 66% complete]
-- [download 68% complete]
-- [download 70% complete]
-- [download 72% complete]
-- [download 73% complete]
-- [download 74% complete]
-- [download 75% complete]
-- [download 76% complete]
-- [download 77% complete]
-- [download 79% complete]
-- [download 80% complete]
-- [download 82% complete]
-- [download 84% complete]
-- [download 86% complete]
-- [download 87% complete]
-- [download 88% complete]
-- [download 90% complete]
-- [download 92% complete]
-- [download 94% complete]
-- [download 95% complete]
-- [download 97% complete]
-- [download 98% complete]
-- [download 99% complete]
-- [download 100% complete]
-- verifying file...
       file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
     dst='/home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No update step for 'GTest-Release'
[ 25%] No patch step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (0.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtest.a
[ 25%] Built target gtest
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_main.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmock.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
  SHA1='568d58e26bd4e838449ca7ab8ebc152b3cbd210d'
-- extracting...
     src='/home/vlad/projects/hunter/_Base/Download/GTest/1.15.2/568d58e/v1.15.2.tar.gz'
     dst='/home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No update step for 'GTest-Debug'
[ 75%] No patch step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/cache.cmake
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (0.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtestd.a
[ 25%] Built target gtest
[ 37%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_maind.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmockd.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmockd.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgmock_maind.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-assertion-result.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtestd.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/libgtest_maind.a
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/vlad/projects/hunter/_Base/xxxxxxx/d1e9b55/dde7d8a/Build/GTest)
-- [hunter] Cache saved: /home/vlad/projects/hunter/_Base/Cache/raw/849f707d4726efc553f4719b2301e898bb6035d8.tar.bz2
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Configuring done (59.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab07/_builds
$ cmake --build _builds
[ 25%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test
```

Просмотр конфигураций Hunter
```bash

$ cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest
  hunter_default_version(GTest VERSION 1.7.0-hunter-6)
  hunter_default_version(GTest VERSION 1.15.2)
$ cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake
# Copyright (c) 2013, Ruslan Baratov
# All rights reserved.

# !!! DO NOT PLACE HEADER GUARDS HERE !!!

include(hunter_add_version)
include(hunter_cacheable)
include(hunter_download)
include(hunter_pick_scheme)
include(hunter_cmake_args)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter.tar.gz"
    SHA1
    1ed1c26d11fb592056c1cb912bd3c784afa96eaa
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-1"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-1.tar.gz"
    SHA1
    0cb1dcf75e144ad052d3f1e4923a7773bf9b494f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-2"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-2.tar.gz"
    SHA1
    e62b2ef70308f63c32c560f7b6e252442eed4d57
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-3"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-3.tar.gz"
    SHA1
    fea7d3020e20f059255484c69755753ccadf6362
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-4"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-4.tar.gz"
    SHA1
    9b439c0c25437a083957b197ac6905662a5d901b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-5"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-5.tar.gz"
    SHA1
    796804df3facb074087a4d8ba6f652e5ac69ad7f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-6"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-6.tar.gz"
    SHA1
    64b93147abe287da8fe4e18cfd54ba9297dafb82
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-7"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-7.tar.gz"
    SHA1
    19b5c98747768bcd0622714f2ed40f17aee406b2
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-8"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-8.tar.gz"
    SHA1
    ac4d2215aa1b1d745a096e5e3b2dbe0c0f229ea5
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-9"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-9.tar.gz"
    SHA1
    8a47fe9c4e550f4ed0e2c05388dd291a059223d9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-10"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-10.tar.gz"
    SHA1
    374e6dbe8619ab467c6b1a0b470a598407b172e9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-11"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-11.tar.gz"
    SHA1
    c6ae948ca2bea1d734af01b1069491b00933ed31
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p2
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p2.tar.gz"
    SHA1
    93148cb8850abe78b76ed87158fdb6b9c48e38c4
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p5
    URL https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p5.tar.gz
    SHA1 3325aa4fc8b30e665c9f73a60f19387b7db36f85
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p6
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p6.tar.gz"
    SHA1
    f57096bd01c6f8cbef043b312d4d1e82f29648b6
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p7
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p7.tar.gz"
    SHA1
    4fe083a96d7597f7dce6f453dca01e1d94a1e45b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p8
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p8.tar.gz"
    SHA1
    1cdd396b20c8d29f7ea08baaa49673b1c261f545
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p9
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p9.tar.gz"
    SHA1
    a345f16cb610e0b5dfa7778dc2852b784cfede5b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p10
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p10.tar.gz"
    SHA1
    1d92c9f51af756410843b13f8c4e4df09e235394
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.8.0-hunter-p11"
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz"
    SHA1
    76c6aec038f7d7258bf5c4f45c4817b34039d285
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.8.1"
    URL
    "https://github.com/google/googletest/archive/release-1.8.1.tar.gz"
    SHA1
    152b849610d91a9dfa1401293f43230c2e0c33f8
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.10.0"
    URL
    "https://github.com/google/googletest/archive/release-1.10.0.tar.gz"
    SHA1
    9c89be7df9c5e8cb0bc20b3c4b39bf7e82686770
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.10.0-p0"
    URL
    "https://github.com/hunter-packages/googletest/archive/v1.10.0-p0.tar.gz"
    SHA1
    f7c72be12120e018f53cda0e0daa26fab5da7dfc
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.10.0-p1"
    URL
    "https://github.com/hunter-packages/googletest/archive/v1.10.0-p1.tar.gz"
    SHA1
    06a1f667f200ff94d38b608e44c3c8061c7b8f2f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.11.0"
    URL
    "https://github.com/google/googletest/archive/release-1.11.0.tar.gz"
    SHA1
    7b100bb68db8df1060e178c495f3cbe941c9b058
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.12.1"
    URL
    "https://github.com/google/googletest/archive/release-1.12.1.tar.gz"
    SHA1
    cdddd449d4e3aa7bd421d4519c17139ea1890fe7
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.13.0"
    URL
    "https://github.com/google/googletest/archive/v1.13.0.tar.gz"
    SHA1
    bfa4b5131b6eaac06962c251742c96aab3f7aa78
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.14.0"
    URL
    "https://github.com/google/googletest/archive/v1.14.0.tar.gz"
    SHA1
    2b28c2a3a30d86b1759543ef61fac3c4d69f8c4c
)

hunter_add_version(
        PACKAGE_NAME
        GTest
        VERSION
        "1.15.2"
        URL
        "https://github.com/google/googletest/archive/v1.15.2.tar.gz"
        SHA1
        568d58e26bd4e838449ca7ab8ebc152b3cbd210d
)


if(HUNTER_GTest_VERSION VERSION_LESS 1.8.0 OR HUNTER_GTest_VERSION VERSION_GREATER_EQUAL 1.11.0)
  set(_gtest_license "LICENSE")
else()
  set(_gtest_license "googletest/LICENSE")
endif()

# gtest_force_shared_crt prevents GoogleTest from modifying options
# rather than forcing it to use shared libraries
hunter_cmake_args(
    GTest
    CMAKE_ARGS
    HUNTER_INSTALL_LICENSE_FILES=${_gtest_license}
    gtest_force_shared_crt=TRUE
)

hunter_pick_scheme(DEFAULT url_sha1_cmake)
hunter_cacheable(GTest)
hunter_download(PACKAGE_NAME GTest PACKAGE_INTERNAL_DEPS_ID 1)
```
Переопределение версии GTest
```bash
$ mkdir cmake/Hunter
$ cat > cmake/Hunter/config.cmake <<EOF
> hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
```

Добавление демонстрационной программы
```bash
$ mkdir demo
$ cat > demo/main.cpp <<EOF
> #include <print.hpp>

#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
EOF
$ gsed -i '/endif()/a\
> add_executable(demo \${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp)
target_link_libraries(demo print)
install(TARGETS demo RUNTIME DESTINATION bin)
' CMakeLists.txt
```

Использование Polly для кросс-платформенной сборки
```bash
$ mkdir tools
$ git submodule add https://github.com/ruslo/polly tools/polly
Cloning into '/home/vlad/Toray-lab/workspace/projects/lab07/tools/polly'...
remote: Enumerating objects: 6578, done.
remote: Counting objects: 100% (29/29), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 6578 (delta 21), reused 17 (delta 17), pack-reused 6549 (from 2)
Receiving objects: 100% (6578/6578), 1.68 MiB | 1.41 MiB/s, done.
Resolving deltas: 100% (4551/4551), done.
$ tools/polly/bin/polly.py --test
Python version: 3.13
Build dir: /home/vlad/Toray-lab/workspace/projects/lab07/_builds/default
Execute command: [
  `which`
  `cmake`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "which" "cmake"

/usr/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.31.6

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/home/vlad/Toray-lab/workspace/projects/lab07/_builds/default`
  `-DCMAKE_TOOLCHAIN_FILE=/home/vlad/Toray-lab/workspace/projects/lab07/tools/polly/default.cmake`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "-H." "-B/home/vlad/Toray-lab/workspace/projects/lab07/_builds/default" "-DCMAKE_TOOLCHAIN_FILE=/home/vlad/Toray-lab/workspace/projects/lab07/tools/polly/default.cmake"

-- [polly] Used toolchain: Default
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
-- Configuring done (1.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab07/_builds/default
Execute command: [
  `cmake`
  `--build`
  `/home/vlad/Toray-lab/workspace/projects/lab07/_builds/default`
  `--`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "--build" "/home/vlad/Toray-lab/workspace/projects/lab07/_builds/default" "--"

[ 50%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
Run tests
Execute command: [
  `ctest`
]

[/home/vlad/Toray-lab/workspace/projects/lab07/_builds/default]> "ctest"

*********************************
No test configuration file found!
*********************************
Usage

  ctest [options]

-
Log saved: /home/vlad/Toray-lab/workspace/projects/lab07/_logs/polly/default/log.txt
-
Generate: 0:00:02.250671s
Build: 0:00:01.904848s
Test: 0:00:00.164077s
-
Total: 0:00:04.323833s
-
SUCCESS
$ tools/polly/bin/polly.py --toolchain clang-cxx14
Python version: 3.13
Build dir: /home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14
Execute command: [
  `which`
  `cmake`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "which" "cmake"

/usr/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.31.6

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14`
  `-GUnix Makefiles`
  `-DCMAKE_TOOLCHAIN_FILE=/home/vlad/Toray-lab/workspace/projects/lab07/tools/polly/clang-cxx14.cmake`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "-H." "-B/home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14" "-GUnix Makefiles" "-DCMAKE_TOOLCHAIN_FILE=/home/vlad/Toray-lab/workspace/projects/lab07/tools/polly/clang-cxx14.cmake"

-- [polly] Used toolchain: clang / c++14 support
-- The C compiler identification is Clang 19.1.7
-- The CXX compiler identification is Clang 19.1.7
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/clang - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/clang++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.9s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14
Execute command: [
  `cmake`
  `--build`
  `/home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14`
  `--`
]

[/home/vlad/Toray-lab/workspace/projects/lab07]> "cmake" "--build" "/home/vlad/Toray-lab/workspace/projects/lab07/_builds/clang-cxx14" "--"

[ 50%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
-
Log saved: /home/vlad/Toray-lab/workspace/projects/lab07/_logs/polly/clang-cxx14/log.txt
-
Generate: 0:00:02.971221s
Build: 0:00:02.182855s
-
Total: 0:00:05.154748s
-
SUCCESS
```
