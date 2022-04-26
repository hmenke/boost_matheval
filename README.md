# Boost Matheval

[![Linux](https://github.com/hmenke/boost_matheval/actions/workflows/linux.yml/badge.svg)](https://github.com/hmenke/boost_matheval/actions/workflows/linux.yml)
[![Windows](https://github.com/hmenke/boost_matheval/actions/workflows/windows.yml/badge.svg)](https://github.com/hmenke/boost_matheval/actions/workflows/windows.yml)
[Documentation](https://hmenke.github.io/boost_matheval/doxygen/html/)
[Coverage](https://hmenke.github.io/boost_matheval/coverage/html/)

This library uses [Boost.Spirit](http://www.boost.org/libs/spirit/index.html)
(QI for C++98 and X3 for C++14) and
[Boost.Fusion](http://www.boost.org/libs/fusion/index.html) (and
[Boost.Phoenix](http://www.boost.org/libs/phoenix/index.html) with C++98) to
parse and evaluate mathematical expressions.

The examples below use the X3 variant of Boost Matheval.

### Motivating example 1

```cpp
#include <iostream>
#include "matheval.hpp"

int main() {
    std::cout << matheval::parse("1+1") << '\n';
}
```
Outputs
```
2
```

### Motivating example 2

```cpp
#include <iostream>
#include <map>
#include "matheval.hpp"

int main() {
    std::map<std::string,double> symtab;
    symtab.emplace(std::make_pair("x",  2));
    symtab.emplace(std::make_pair("y", -1));
    std::cout << matheval::parse("cbrt(x/2 + sqrt(x**2/4 + y**3/24))",symtab) << '\n';
}
```
Outputs
```
1.25548
```

### Motivating example 3

We can also evaluate an expression multiple times without paying the
cost of parsing again.
```cpp
#include <iostream>
#include <map>
#include "matheval.hpp"

int main() {
    matheval::Parser parser;
    parser.parse("x + 1");
    std::cout << parser.evaluate({std::make_pair("x",1)}) << ' '
              << parser.evaluate({std::make_pair("x",2)}) << '\n';
}
```
Outputs
```
2 3
```

### Build instructions

To build Boost Matheval, just follow the usual CMake workflow.
```bash
mkdir build
cd build
cmake ..
make         # build the library and the examples
make check   # build and run the tests
```

### Requirements and Limitations

General:

* C++14 compatible compiler, i.e. GCC >= 4.8, Clang, Visual Studio 2015.
* Boost.Spirit, Boost.Phoenix, and Boost.MathConstants.
* No support for ternary functions (e.g. `if`).
* No support for complex numbers.

### Alternatives

* [GNU libmatheval](https://www.gnu.org/software/libmatheval/) is a C
  library built using the parser generator YACC with about the same
  scope as Boost Matheval.
* [ExprTk](http://www.partow.net/programming/exprtk/) is a true
  monster with almost 40000 lines in a single header file.  It
  implements a complete state machine including things like logical
  operations, control structures, and even file IO.  Compilation time
  is even longer than with Boost Matheval.

### License

Distributed under the [Boost Software License, Version 1.0](http://boost.org/LICENSE_1_0.txt).
