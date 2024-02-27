# Generics in Rust

Generics in Rust allows us to write code that can operate on different types without having to write separate implementations for each type. It helps us write code that can handle values of any type in a type-safe and efficient way. They enable you to write functions, structs, enums, and traits that can work with any data type.

With the help of generics, we can define placeholder types for our methods, functions, structs, enums and traits.

## Syntax

The syntax for defining generics in Rust uses angle brackets (`<>`) to declare generic type parameters. For example, a generic function to find the maximum of two values:

```rust
use std::cmp::PartialOrd;

fn max<T: PartialOrd>(a: T, b: T) -> T {
    if a > b {
        a
    } else {
        b
    }
}

fn main() {
    let bigger = max(10, 0);
    println!("{}", bigger);
}
```

In this example, `T` is a generic type parameter. The `T: PartialOrd` clause specifies that `T` must implement the `PartialOrd` trait, which allows for comparison.

## Using Generics

You can use generics with functions, structs, enums, and traits. For example,

### Generic Struct

```rust
#[derive(Debug)]
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn new(x: T, y: T) -> Self {
        Point { x, y }
    }
}

fn main() {
    // initializing a generic struct with i32 data type
    let int_point = Point::new(1,2);

    // initializing a generic struct with f32 data type
    let float_point = Point::new(1.1,2.2);

    println!("int_point: {:?}", int_point);
    println!("float_point: {:?}", float_point);
}
```

Here, `Point` is a struct that can hold values of any type `T`.

### Generic Functions

We can also create functions with generic types as parameter(s).

The syntax,
```rust
// generic function with single generic type
fn my_function<T>(x: T, y: T) -> T {
    // function body
    // do something with `x` and `y`
}

// generic function with multiple generic types
fn my_function<T, U>(x: T, y: U) {
    // function body
    // do something with `x` and `y`
}
```
Here, `<T>` in the function definition signifies a generic function over type `T`. Similarly, `<T, U>` signifies a generic function over type `T` and `U`.

Example:
```rust
use std::cmp::PartialOrd;

fn max<T: PartialOrd>(a: T, b: T) -> T {
    if a > b {
        a
    } else {
        b
    }
}

fn main() {
    let bigger = max(10, 0);
    println!("{}", bigger);
}
```

In this example, the type parameter `T` is declared with the syntax `<T: PartialOrd>`, which means that `T` can be any type that implements the `PartialOrd` trait.

The `PartialOrd` trait provides methods for comparing values of a type, such as `<` and `>`. This feature of Rust is called Trait bounds. If we don't use `<T: PartialOrder>`, Rust will throw a compile error: `error[E0369]: binary operation '<' cannot be applied to type 'T'`.

## Benefits of Generics

- **Code Reusability**: Write generic functions and structs that can be used with different types.
- **Type Safety**: Generics in Rust ensure that types are checked at compile time, preventing runtime errors.
