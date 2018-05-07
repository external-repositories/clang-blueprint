# clang-blueprint

| Build Status |                                                                                |
|--------------|--------------------------------------------------------------------------------|
| Travis       | [![Travis Build Status][travis-build-status-svg]][travis-build-status]         |

Example [CMake](https://cmake.org/)  project that incorporates awesome 
[Clang](https://clang.llvm.org/) tooling.

Current status: *Work in progress*.

## Requirements

### Ubuntu 18.04

```bash
$ sudo apt install gcc g++ clang clang-tidy clang-format cmake cppcheck doxygen graphviz
```

## Build

Use `-DCMAKE_BUILD_TYPE=Release` or `-DCMAKE_BUILD_TYPE=Debug` to specify release or debug builds.

### GCC

```bash
$ cd build
$ cmake ..
$ make
```

### Clang

```bash
$ cd build
$ cmake -DCMAKE_CXX_COMPILER=clang++ ..
$ make
```

### Treat warnings as errors

Use `-DWERROR=On` option to treat compile warnings as errors.

```bash
$ cd build
$ cmake -DERROR=On ..
$ make
/home/user/GitHub/clang-blueprint/src/danger/bad_examples.cpp:13:18: error: array index
      3 is past the end of the array (which contains 2 elements)
      [-Werror,-Warray-bounds]
    std::cout << a[3];
                 ^ ~
...
```

## Run

Navigate into the `build` directory.

### Application

```bash
$ ./clang-blueprint
```

### Unit tests

Unit tests are written using the [Catch2](https://github.com/catchorg/Catch2/) unit testing
framework.

```bash
$ ./unit-test
```

## Build Docs

Documentation is built using [Doxygen](www.doxygen.org/). To configure how the docs are built,
modify [`docs/Doxyfile`](docs/Doxyfile).

```bash
$ cd docs
$ doxygen
$ firefox html/index.html
```

## Clang Tools

Navigate into the `build` directory.

### Clang-Tidy

[Clang-Tidy](http://clang.llvm.org/extra/clang-tidy/) is configured using the 
[`.clang-tidy`](.clang-tidy) configuration file. Modify this file to control which checks should 
be run and configure parameters for certain checks.

```bash
$ cmake ..
$ make clang-tidy
Scanning dependencies of target clang-tidy
95 warnings generated.
7477 warnings generated.
/home/user/GitHub/clang-blueprint/src/main.cpp:10:28: warning: 
    parameter 'argc' is unused [misc-unused-parameters]
int32_t main(const int32_t argc, const char* argv[]) {
                           ^~~~~
                            /*argc*/
/home/user/GitHub/clang-blueprint/src/main.
...
```

### [AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html)

```bash
$ cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_CXX_COMPILER=clang++ -DADDRESS_SANITIZER=On ..
$ make
$ ./unit_test
==25797==ERROR: AddressSanitizer: heap-use-after-free on address 0x61400000024c at 
pc 0x000000616ecf bp 0x7ffd7aab4b30 sp 0x7ffd7aab4b28
...
```

### [UndefinedSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)

```bash
$ cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_CXX_COMPILER=clang++ -DUNDEFINED_SANITIZER=On ..
$ make
$ ./unit_test
/home/user/GitHub/clang-blueprint/test/danger/test_bad_examples.cpp:17:7: 
runtime error: signed integer overflow: 2147483647 + 1 cannot be represented in type 'int'
...
```

### Clang-Format

[Clang-Format](https://clang.llvm.org/docs/ClangFormat.html) is configured using 
the [`.clang-format`](.clang-format) configuration file. Modify this file to control how source 
files should be formatted.

To demonstrate `clang-format` in action, first modify [`src/main.cpp`](src/main.cpp) from

```cpp
    return EXIT_SUCCESS;
```

To

```cpp
    return           EXIT_SUCCESS;
```

Next, run `clang-format` on the project.

```bash
$ cmake ..
$ make clang-format
```

[`src/main.cpp`](src/main.cpp) will be reformatted properly to

```cpp
    return EXIT_SUCCESS;
```

## Cppcheck

To run [Cppcheck](http://cppcheck.sourceforge.net/) on the project

```bash
$ cmake ..
$ make cppcheck
[/home/user/GitHub/clang-blueprint/src/main.cpp:14]: (error) Array 'a[2]' accessed at index 3, 
    which is out of bounds.
...
```

<!-- Badges -->
[travis-build-status]: https://travis-ci.org/johnthagen/clang-blueprint
[travis-build-status-svg]: https://travis-ci.org/johnthagen/clang-blueprint.svg?branch=master
