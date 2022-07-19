## Modern C++ Tutorial Notes
Covering Chapter 3 of the [The Modern C++ Tutorial](https://github.com/changkun/modern-cpp-tutorial) by Ou Changkun

# Language Runtime Enhancements

## Lambda Expression
Lambda expressions provide a feature like anonymous functions. Anonymous functions are used when a functin is needed, but you don't want to use a name to call a function. There are many possible scenarios like this. Thus, anonymous functions are almost standard in modern programming languages.

### Basics
The basic syntax of a Lambda expression is as follows:

```c++
[capture list] (parameter list) mutable(optional) exception attribute -> return type {
// function body
}
```

The capture list can be understood as a type of parameter. The internal function body of a lambda expression cannot use variables outside the body of the function by default. At this time, the capture list can serve to transfer external data. According to the behavior passed, the capture list is divided into the following types:

#### 1. Value Capture
Similar to parameter passing, the value capture is based on the fact that the variable can be copied, except that the captured variable is copied whne the lambda expression is created, not when it is called:

```c++
void lambda_value_capture() {
    int value = 1;
    auto copy_value = [value] {
        return value;
    };
    value = 100;
    auto stored_value = copy_value();
    std::cout << "stored_value = " << stored_value << std::endl;
    // At this moment, stored_value == 1, and value == 100.
    // Because copy_value has copied when its was created.
}
```

#### 2. Reference Capture
Similar to a reference pass, the reference capture saves the reference and the value changes:

```c++
void lambda_reference_capture() {
    int value = 1;
    auto copy_value = [&value] {
        return value;
    };
    value = 100;
    auto stored_value = copy_value();
    std::cout << "stored_value = " << stored_value << std::endl;
    // At this moment, stored_value == 100, value == 100.
    // Because copy_value stores reference
}
```

#### 3. Implicit Capture
Manually writing a capture list is sometimes very complicated. At this point, you can write a
`&`
or 
`=`
to the compiler to declare the reference or value capture.

To summarize, capture provides the ability for lambda expresions to use external values. The four most common forms of capture list are:
* `[]` empty capture list
* `[name1, name2, ...]` captures a series of variables
* `[&]` reference capture, let the compiler deduce the reference list by itself
* `[=]` value capture, let the compiler deduce the value list by itself


#### 4. Expression Capture
The value captures and reference captures mentioned prior are variables that have been declared in the outer scope, so these capture methods capture the lvalue, not rvalue.

C++14 gives us the convenience of allowing the captured members to be initialized with arbitrary expressions, which allows the capture of rvalues. The type of the captured variable being declared is judged according to the expression, and the judgement is the same as using
`auto`
:

```c++
#include <iostream>
#include <memory>  // std::make_unique
#include <utility> // std::move

void lambda_expression_capture() {
    auto important = std::make_unique<int>(1);
    auto add = [v1 = 1, v2 = std::move(important)](int x, int y) -> int {
        return x+y+v1+(*v2);
    };
    std::cout << add(3,4) << std::endl;
}
```

In the above code, 
`important`
is an exclusive pointer that cannot be caught by value capture using
`=`
. At this time we need to transfer it to the rvalue and initialize it in the expression. 

### Generic Lambda
Previously we mentioned that the
`auto`
keyword cannot be used in the parameter list because it would conflict with the functionality of the template. But lambda expressions are not regular functions. Without further specification on the typed parameter list, lambda expressions cannot utilize templates. Fortunately, this trouble only exists in C++11, starting with C++14, the formal parameters of the lambda function can use the 
`auto`
keyword to utilize template generics:

```c++
void lambda_generic() {
    auto generic = [](auto x, auto y) {
        return x+y;
    };

    std::cout << generic(1, 2) << std::endl;
    std::cout << generic(1.1, 2.2) << std::endl;
}
```

## Function Object Wrapper
### `std::function`
The essence of a Lambda expresion is an object of a class type (called a closure type) that is similar to a function object type (called a closure object). When the capture list of a Lambda expression is empty, the closure object can also be converted to a function pointer value for delivery:

```c++
#include <iostream>
using foo = void(int);  // function pointer
void functional(foo f) {
    f(1);
}
int main() {
    auto f = [](int value) {
        std::cout << value << std::endl;
    };
    functional(f);  // call by function pointer
    f(1);           // call by lambda expression
    return 0;
}
```

The above code gives two different forms of invocation, one is to call Lambda as a function type, and the other is to directly call a Lambda expression. In C++11, these concepts are unified. The type of object that can be called is collectively called the callable type. This type is introduced by 
`std::function`
.
C++11
`std::function`
is a generic, polymorphic function wrapper whose instances can store, copy, and call any target entity that can be called. It is essentially a container of functions, a type-safe package of entities. When we have a container for functions, we can more easily handle functions and function pointers as objects:

```c++
#include <functional>
#include <iostream>

int foo(int para) {
    return para;
}

int main() {
    // std::function wraps a function that take int paremeter and returns int value
    std::function<int(int)> func = foo;

    int important = 10;
    std::function<int(int)> func2 = [&](int value) -> int {
        return 1+value+important;
    };
    std::cout << func(10) << std::endl;
    std::cout << func2(10) << std::endl;
}
```

### `std:bind` and `std::placeholder`
#### `std:bind`
Used to bind the parameters of the function call. It solves the requirement that we may not always be able to get all the parameters of a function at one time. Through this function, we can specify a placeholder parameter bound to the function in advance, and then complete the call after the parameters are complete:

```c++
int foo(int a, int b, int c) {
    ;
}
int main() {
    // bind parameter 1, 2 on function foo,
    // and use std::placeholders::_1 as placeholder for the first parameter.
    auto bindFoo = std::bind(foo, std::placeholders::_1, 1,2);
    // when call bindFoo, we only need one param left
    bindFoo(1);
}
```

> Tip: Note the magic of the `auto` keyword. We may not be familiar with the return type of a function, but we can circumvent this problem by using the keyword.

## rvalue Reference
To understand what the rvalue reference is about, you must have a clear understanding of lvalue and rvalue.

**lvalue**, left value, is the value to the left of the assignment symbol. To be precise, an lvalue is a persistent object that still exists after an expression (not necessarily an assignment expression).

**rvalue**, right value, the value on the right refers to the temporary object that no longer exists after the expression ends. 

In C++11, the concept of rvalue values is further divided into prvalue and xvalue.

**prvalue**, pure rvalue, such as
`10`
or
`true`
; either the result of the evalueation is equivalent to a literal or anonymous temporary object. Temporary variables returned by non-references, generated by operation expressions, original literals, Lambda expressions, and requires expressions are all pure rvalue values. (string literals are not prvalues, a string literal is an lvalue with type
`const char` 
array).

Consider:

```c++
#include <type_traits>

int main() {
    // Correct. The type of "01234" is const char [6], so it is an lvalue
    const char (&left)[6] = "01234";

    // Assert success. It is a const char [6] indeed. Note that decltype(expr)
    // yields lvalue reference if expr is an lvalue and neither an unparenthesized
    // id-expression nor an unparenthesized class member access expression.
    static_assert(std::is_same<decltype("01234"), const char(&)[6]>::value, "");

    // Error. "01234" is an lvalue, which cannot be referenced by an rvalue reference
    // const char (&&right)[6] = "01234";
}
```

However, an array can be implicitly converted to a corresponding pointer The result, if not an lvallue reference, is an rvalue (xvalue if the result is an rvalue reference, prvalue otherwise)

```c++
const char*   p    = "01234"; // Correct. "01234" is implicitly converted to const char*
const char*&& pr   = "01234"; // Correct. "01234" is implicitly converted to const char*, which is a prvalue.
// const char*& pl = "01234"; // Error. There is no type const char* lvalue
```

**xvalue**, expiring value, is the concept proposed by C++11 to introduce rvalue references, a value that is destroyed but can be moved. 

```c++
std::vector<int> foo() {
    std::vector<int> temp = {1, 2, 3, 4};
    return temp;
}

std::vector<int> v = foo();
```

The return value generated by 
`foo()`
is used as a temporary value. Once copied by
`v`
, it will be destroyed immediately, and cannot be obtained or modified. The xvalue defines behavior in which temporary values ​​can be identified while being able to be moved.

After C++11, the compiler does some work for us, where the lvalue
`temp`
is subjected to implicit rvalue conversion, equivalent to
`static_cast<std::vector<int> &&>(temp)`
, where
`v` 
here moves the value returned by
`foo`
locally.

# Sources
* [The Modern C++ Tutorial](https://github.com/changkun/modern-cpp-tutorial) by Ou Changkun
* https://en.cppreference.com/w/cpp/language/value_category