# Modern C++ Tutorial Notes

## Constants

### `constexpr`
C++11 provides 
`constexpr`
to let the user explicitly declare that the function or object contructor will become a constant expression at compile time. The keyword explicitly tells the compiler to verify the function or object should be a compile-time constant expression. An example featuring recursion:

```c++
#include <iostream>

constexpr int fibonacci(const int n) {
    return n == 1 || n == 2 ? 1 : fibonacci(n-1) + fibonacci(n-2);
}

int main(){

    std::cout << fibonacci (4) << '\n';
    return 0;
}
```

Starting with C++14, the 
`constexpr`
function can use simple statements such as local variables, loops, and branches internally, such that we could write the
`fibonacci`
function defined above as such instead:

```c++
constexpr int fibonacci(const int n) {
    if(n == 1) return 1;
    if(n == 2) return 1;
    return fibonacci(n-1) + fibonacci(n-2);
}
```

C++ 17 introduces the 
`constexpr`
keyword into the
`if`
statement, allowing us to declare the condition of a constant expression:

```c++
#include <iostream>

template<typename T>
auto print_type_info(const T& t) {
    //if t is an int, else
    if constexpr (std::is_integral<T>::value) {
        return t + 1;
    } else {
        return t + 0.001;
    }
}
int main() {
    std::cout << print_type_info(5) << std::endl;
    std::cout << print_type_info(3.14) << std::endl;
}
```

## Variables and Initialization
### Initializer List
Take a class Foo with two initialzation values - two integers:

```c++
#include <iostream>
#include <vector>

class Foo {
public:
    int value_a;
    int value_b;
    Foo(int a, int b) : value_a(a), value_b(b) {}
};

int main() {
    // before C++11
    int arr[3] = {1, 2, 3};
    Foo foo(1, 2);
    std::vector<int> vec = {1, 2, 3, 4, 5};

    std::cout << "arr[0]: " << arr[0] << std::endl; 
    std::cout << "foo:" << foo.value_a << ", " << foo.value_b << std::endl; 
    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << std::endl; 
    }
    return 0;
}
```

In C++11, the library 
`initializer_list`
was added, allowing the constructor or other function to use an initialization list like a parameter. It provides a bridge between normal arrays and POD (plain ol' data) initialization methods, such that:

```c++
#include <iostream>
#include <initializer_list>
#include <vector>

class MagicFoo {
public:
    std::vector<int> vec;
    MagicFoo(std::initializer_list<int> list) {
        for (std::initializer_list<int>::iterator it = list.begin();
             it != list.end(); ++it)
            vec.push_back(*it);
    }
};
int main() {
    // after C++11
    MagicFoo magicFoo = {1, 2, 3, 4, 5};

    std::cout << "magicFoo: \n";
    for (std::vector<int>::iterator it = magicFoo.vec.begin(); it != magicFoo.vec.end(); ++it) 
        std::cout << *it << std::endl;
}
```

The initialization list can also be used as a formal parameter of a normal function.

### Structured Binding and Tuples
Structured bindings provide functionality similar to the multiple return values provided in other languages. C++11 added a 
`std::tuple`
container for constructing a tuple, and C++17 completed its functionality. Example:

```c++
#include <iostream>
#include <tuple>

std::tuple<int, double, std::string> f() {
    return std::make_tuple(1, 2.3, "456");
}

int main() {
    auto [x, y, z] = f();
    std::cout << x << ", " << y << ", " << z << std::endl;
    return 0;
}
```

## Type Inference
C++11 introduces the keyworsd
`auto`
and 
`decltype`
to implement type derivation, letting the compiler deduce the type of the variable.

### `auto`
Some common usages of auto:

```c++
#include <iostream>

int main() {
    auto i = 5;       //i as int
    auto j = "hello"; //j as string
    std::cout << i << '\n' << j << '\n';
    return 0;
}
```

Since C++20, 
`auto`
can even be used as function arguments. Consider:

```c++
#include <iostream>

int add(auto x, auto y) {
    return x+y;
}

int main() {
    auto i = 5; // type int
    auto j = 6; // type int
    std::cout << add(i, j) << std::endl;
    return 0;
}
```

### `decltype`
The 
`decltype`
keyword yields the type of a specified expression. Together with the
`auto`
, it is useful for writing template libraries. 

```c++
#include <iostream>

int main() {
    auto x = 1;
    //z is instantiated as the same type of x
    decltype(x) z;
    //is_same returns true if the two compared types are the same 
    //whether it is two variables or a variable and a type
    if (std::is_same<decltype(x), int>::value)
        std::cout << "type x == int" << std::endl;
    if (std::is_same<decltype(x), float>::value)
        std::cout << "type x == float" << std::endl;
    if (std::is_same<decltype(x), decltype(z)>::value)
        std::cout << "type z == type x" << std::endl;
    return 0;
}
```

### `decltype(auto)`
Mainly used to derive the return type of a forwarding function or package, which does not require us to explicitly specify the parameter expression of 
`decltype`
. Consider the example when we need to wrap the following two functions:

```c++
std::string  lookup1();
std::string& lookup2();
```

In C++11:

```c++
std::string look_up_a_string_1() {
    return lookup1();
}
std::string& look_up_a_string_2() {
    return lookup2();
}
```

With 
`decltype(auto)`
, we can let the compiler do the parameter forwarding:

```c++
decltype(auto) look_up_a_string_1() {
    return lookup1();
}
decltype(auto) look_up_a_string_2() {
    return lookup2();
}
```

## Control Flow
### Range-Based `for` loop
C++11 introduces a range-based iterative method, and we can write loops that are as concise as Python:

```c++
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {1, 2, 3, 4};
    if (auto itr = std::find(vec.begin(), vec.end(), 4); itr != vec.end()) *itr = 4;
    for (auto element : vec)
        std::cout << element << std::endl; // read only
    for (auto &element : vec) {
        element += 1;                      // writeable
    }
    for (auto element : vec)
        std::cout << element << std::endl; // read only
}
```

## Templates
### Extern Templates
In traditional C++, templates are instantiated by the compiler only when they are used. This results in an increase in compile time due to repeated instantiations. Also, we have no way to tell the compiler not to trigger the instantiation of the template.

To this end, C++11 introduces an external template that allows us to explicitly tell the compiler when to instantiate.

```c++
template class std::vector<bool>;          // force instantiation
extern template class std::vector<double>; // should not instantiate in current file
```

### Variadic Templates
In traditional C++, both a class template and function tempate could only accept a fixed set of template parameters as specified; C++11 added a new representation, allowing any number, template parameters of any category, and no need to fix the number of parameters when defining:

```c++
template<typename... Ts> class Foo;
```

We can pass any number of arguments, then they parameters are unpacked. So far ther is no simple way to process the parameter package but there are a few processing methods:

#### Recursive Template Function

Recursion is the most classic approach. This method continually recursively passes template parameters to the function, thereby recursively traversing all template parameters:

```c++
#include <iostream>
template<typename T0>
void printf1(T0 value) {
    std::cout << value << std::endl;
}
template<typename T, typename... Ts>
void printf1(T value, Ts... args) {
    std::cout << value << std::endl;
    printf1(args...);
}
int main() {
    printf1(1, 2, "123", 1.1);
    return 0;
}
```

#### Initialize List Expansion
Recursive template functions are standard practice, but the obvious drawback is that you must define a function that terminates recursion. An example:

```c++
template<typename T, typename... Ts>
auto printf3(T value, Ts... args) {
    std::cout << value << std::endl;
    (void) std::initializer_list<T>{([&args] {
        std::cout << args << std::endl;
    }(), value)...};
}
```

In this code, the initialization list provided and the properties of the Lambda expression are additionally used. 

By initializing the list, 
`(lambda expression, value)...`
will be expanded. Due to the appearance of the comma expression, the previous lambda expression is executed first, and the output of the parameter is completed. To avoid compiler warnings, we can explicitly convert 
`std::initializer_list`
to
`void`
.

### Fold Expression
The feature of the variable length parameter can be brought into the expression:

```c++
#include <iostream>
template<typename ... T>
auto sum(T ... t) {
    return (t + ...);
}
int main() {
    std::cout << sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) << std::endl;
}
```

## Object-Oriented
### Delegate Constructor
C++11 introduces the concept of a delegate construct, which allows the constructor to call another constructor in a constructor of the same class, simplifying the code:

```c++
#include <iostream>
class Base {
public:
    int value1;
    int value2;
    Base() {
        value1 = 1;
    }
    Base(int value) : Base() { // delegate Base() constructor
        value2 = value;
    }
};

int main() {
    Base b(5);
    std::cout << b.value1 << std::endl;
    std::cout << b.value2 << std::endl;
}
```

## Inheritance Constructor
In traditional C++, constructors need to pass arguments one by one if they need inheritance, which leads to inefficiency. C++11 introduces the concept of inheritance constructors using the keyword
`using`
:

```c++
#include <iostream>
class Base {
public:
    int value1;
    int value2;
    Base() {
        value1 = 1;
    }
    Base(int value) : Base() { // delegate Base() constructor
        value2 = value;
    }
};
class Subclass : public Base {
public:
    using Base::Base; // inheritance constructor
};
int main() {
    Subclass s(3);
    std::cout << s.value1 << std::endl;
    std::cout << s.value2 << std::endl;
}
```



# Compilation Environment
Use
`clang++`
compiler, with compilation flag
`-std=c++20`
.

# Sources
* [The Modern C++ Tutorial](https://github.com/changkun/modern-cpp-tutorial) by Ou Changkun
* https://docs.microsoft.com/en-us/cpp/cpp/decltype-cpp?view=msvc-170