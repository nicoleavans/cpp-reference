### Templates
A template defines:
- a family of classes
- a family of functions
- an alias to a family of types 
- a family of variables
- a concept 

The definition of a template must be visible at the point of implicit instantiation, so template libraries typically provide all template definitions in the headers.

A template is a construct that generates an ordinary type or function at compile time based on user arguments. A function template example:

```
#include <iostream>

template <typename T>
    T minimum(const T& x, const T& y) {
        return x < y ? x : y; //ternary operator
    }

int main(){
    std::cout << minimum(4, 10) << '\n';
    std::cout << minimum(3.8, 3.7);
	
	return 0;
}
```
The typename keyword is a placeholder for other variable types, and the compiler will replace instances of T with user specified types or deduce them. 

In the example above, only type T was established, but there is no functional limit, multiple can be declared separated by commas. 

It is also possible to define a template that takes an abitrary number of zero or more type parameters using the ellipsis operator.

## Concepts

In C++20, the [concepts](https://en.cppreference.com/w/cpp/concepts) header was introduced, providing definitions that can be used to perform compile-time validation of template arguments. 

| Function  | Definition |
| ------------- | ------------- |
| same_as | specifies that a type is the same as another type  |
| convertible_to  |  specifies that a type is implicitly convertible to another type   |
| derived_from | specifies that a type is derived from another type |
| integral | specifies that a type is an integral type | 
| floating_point | specifies that a type is a floating-point type | 
| swappable | specifies that a type can be swapped | 
| destructible | specifies than an object of the type can be destroyed | 
| constructible_from |  specifies that a variable of the type can be constructed from or bound to a set of argument types | 
| move_constructible |  specifies that an object of a type can be move constructed |
| copy_constructible |  specifies that an object of a type can be copy constructed and move constructed |


### Sources

- https://en.cppreference.com/w/cpp/language/templates
- https://docs.microsoft.com/en-us/cpp/cpp/templates-cpp?view=msvc-170
- https://en.cppreference.com/w/cpp/concepts

## References

- https://www.cppstories.com/2021/concepts-intro/
- https://en.cppreference.com/w/cpp/language/parameter_pack
