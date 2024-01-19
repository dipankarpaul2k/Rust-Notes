<!-- omit in toc -->
# Rust Notes

Hi! I am Diankar Paul👋

I'm currently learning Rust programming language and here in this repo I will upload all my notes and insights in this language.

Hope, it will help you understand Rust programming language more easily. Keep coding, keep learning and keep developing🙂.

<!-- omit in toc -->
# Table of Contents

- [What is Rust? Why use Rust?](#what-is-rust-why-use-rust)
  - [There are several key features that make Rust stand out:](#there-are-several-key-features-that-make-rust-stand-out)
  - [Environment Setup](#environment-setup)
- [Variables and Mutability](#variables-and-mutability)
  - [Variables:](#variables)
  - [Mutability:](#mutability)
  - [Shadowing:](#shadowing)
  - [Constants:](#constants)
  - [Type Annotations:](#type-annotations)
  - [Example combining mutability, shadowing, and constants:](#example-combining-mutability-shadowing-and-constants)
  - [Does shadowing works on const as well?](#does-shadowing-works-on-const-as-well)
- [Data Types](#data-types)
  - [Scalar Types](#scalar-types)
    - [Integer Types:](#integer-types)
    - [Floating-Point Types:](#floating-point-types)
    - [Boolean Type:](#boolean-type)
    - [Character Type:](#character-type)
  - [Compound Types](#compound-types)
    - [Array Type:](#array-type)
    - [Tuple Type:](#tuple-type)
  - [Some other types](#some-other-types)
    - [String Type:](#string-type)
    - [Slice Type:](#slice-type)
    - [Reference Types:](#reference-types)
- [Type Casting](#type-casting)
  - [Floating point to integer:](#floating-point-to-integer)
  - [Character to integer:](#character-to-integer)
  - [Integer to character:](#integer-to-character)
  - [Error while converting integer to character](#error-while-converting-integer-to-character)
  - [Boolean to integer](#boolean-to-integer)
  - [Limitations of Type Casting](#limitations-of-type-casting)
- [Operators](#operators)
  - [Arithmetic Operators:](#arithmetic-operators)
  - [Assignment Operators:](#assignment-operators)
  - [Comparison Operators:](#comparison-operators)
  - [Logical Operators:](#logical-operators)
- [Control Flow](#control-flow)
  - [if Expressions](#if-expressions)
    - [Basic `if-else`:](#basic-if-else)
    - [`if-else if` ladder:](#if-else-if-ladder)
    - [Using `if` in Assignments:](#using-if-in-assignments)
    - [Ternary Operator (Shorthand for Simple `if-else`):](#ternary-operator-shorthand-for-simple-if-else)
  - [Repetition with Loops](#repetition-with-loops)
    - [`loop`:](#loop)
      - [`loop` inside `loop`:](#loop-inside-loop)
    - [`while`:](#while)
    - [`for`:](#for)
    - [Loop Control Statements:](#loop-control-statements)
- [Functions](#functions)
  - [Example:](#example)
  - [Statements and Expressions:](#statements-and-expressions)
    - [Statements:](#statements)
    - [Expressions:](#expressions)
  - [Return Values:](#return-values)
  - [Comments:](#comments)

---

# What is Rust? Why use Rust?

Rust is a programming language that was designed with a focus on system-level programming, providing a balance between low-level control over hardware resources and high-level abstractions that make programming more productive and safe. It was originally created by Graydon Hoare at Mozilla Research, with contributions from Brendan Eich, the creator of JavaScript and first released in 2010.

## There are several key features that make Rust stand out:

1. **Memory Safety:** One of the main highlights of Rust is its emphasis on memory safety without sacrificing performance. Rust uses a borrowing system to enforce strict ownership and borrowing rules at compile-time, preventing common issues such as null pointer dereferences, dangling pointers, and data races. This helps developers write more reliable and secure code.

2. **Zero-Cost Abstractions:** Rust aims to provide high-level abstractions without introducing runtime overhead. This is often referred to as "zero-cost abstractions." It means that the abstractions used in Rust, such as those provided by the ownership system and high-level constructs, don't come with a runtime performance penalty. The compiler optimizes the code to achieve performance comparable to manual memory management in languages like C or C++.

3. **Concurrency without Data Races:** Rust's ownership system ensures safe concurrent programming by preventing data races. The ownership model allows for concurrent access to data through borrowing and references, and the compiler ensures that multiple threads cannot simultaneously mutate data, reducing the likelihood of concurrency bugs.

4. **Community and Ecosystem:** Rust has a vibrant and active community that contributes to the language's development and maintains a rich ecosystem of libraries and tools. The package manager, Cargo, simplifies dependency management and makes it easy to share and distribute Rust projects.

5. **Versatility:** While Rust is often associated with systems programming, it's a versatile language suitable for a wide range of applications. Its syntax and features support building everything from operating systems and embedded systems to web servers and command-line utilities.

In summary, Rust offers a unique combination of memory safety and zero-cost abstractions, making it an attractive choice for systems programming where performance and reliability are critical. Its design encourages developers to write clean and safe code while providing the flexibility and control needed for low-level programming tasks.

## Environment Setup

Let’s start your Rust journey by installing Rust on Linux, macOS, or Windows. You can find the latest guide in this [link](https://doc.rust-lang.org/book/ch01-01-installation.html).

---

# Variables and Mutability

## Variables:
- In Rust, variables are created using the `let` keyword.
- Variables are `immutable` by default, meaning their values cannot be changed once assigned.
  
Example:
```rust
let x = 5; // immutable variable
```

## Mutability:
- To make a variable mutable, you can use the `mut` keyword.
  
Example:
```rust
let mut y = 10; // mutable variable
y = 15; // valid because y is mutable
```

## Shadowing:
- Rust allows shadowing, where you can redeclare a variable with the same name, effectively creating a new variable that hides the previous one. 
 >Rustaceans say that the first variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable.
  
Example:
```rust
let z = 20;
let z = z + 5; // shadows the previous z
```

- The difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the let keyword again, we can change the type of the value but reuse the same name.

## Constants:
- Constants are declared using the `const` keyword.
- Constants must have a specified type, and their values cannot be changed.
- You aren’t allowed to use `mut` with constants. Constants aren’t just immutable by default, they’re always immutable.
  
Example:
```rust
const PI: f64 = 3.14;
```

## Type Annotations:

- While Rust can often infer the variable type, you can explicitly specify it using a type annotation.(We will talk more about the type annotation in the next section.)
  
Example:
```rust
let message: &str = "Hello, Rust!"; // type annotation for a string reference
```

## Example combining mutability, shadowing, and constants:
```rust
const MAX_POINTS: u32 = 100_000; // same as 100000

fn main() {
    let mut counter = 0; // mutable variable
    counter += 1;

    let counter = counter * 2; // shadows the previous counter, new variable with different type
    // counter += 1; // Uncommenting this line would result in a compilation error, as counter is no longer mutable

    println!("Max Points: {}", MAX_POINTS);
    println!("Counter: {}", counter);
}
```

This example showcases mutability, shadowing, and the use of constants in Rust. It's important to understand these concepts for effective variable management in your Rust programs.

Now you might have a question,

## Does shadowing works on const as well?

No, shadowing does not work on constants (`const`) in Rust. **Once a constant is defined, its value cannot be changed or shadowed.** Constants have a fixed, unchangeable value throughout the scope of their existence.

Example:
```rust
const MAX_POINTS: u32 = 100_000;

// Uncommenting the line below would result in a compilation error
// const MAX_POINTS: u32 = 200_000; // cannot redeclare `MAX_POINTS`
```

Attempting to redeclare or shadow the constant `MAX_POINTS` with a new value will cause a compilation error. If you need a variable with a changeable value, use a mutable variable instead of a constant.

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html).

---

# Data Types

Every value in Rust is of a certain data type, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: 

1. Scalar
2. Compound

Keep in mind that Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it. 

## Scalar Types

A scalar type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. You may recognize these from other programming languages.

### Integer Types:

- An integer is a number without a fractional component.
- The default integer type is `i32`.
- Signed integers: `i8`, `i16`, `i32`, `i64`, `i128`, `isize`
- Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
- Each variant can be either signed(+/-) or unsigned(+) and has an explicit size.

Example:
```rust
let integer: i32 = 42;
let unsigned_integer: u64 = 100;
```

- Each signed variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n - 1</sup> - 1 inclusive, where n is the number of bits that variant uses.
- Unsigned variants can store numbers from 0 to 2<sup>n</sup> - 1.
- The `isize` and `usize` types depend on the architecture of the computer your program is running on.

### Floating-Point Types:

- `f32`: 32-bit floating point
- `f64`: 64-bit floating point, default.

Example:
```rust
let float: f64 = 3.14;
```
- Floating-point numbers are represented according to the IEEE-754 standard. The f32 type is a single-precision float, and f64 has double precision.

### Boolean Type:

- `bool`: Represents true or false values.
- Booleans are one byte in size.

Example:
```rust
let is_true: bool = true;
```

### Character Type:

- `char`: Represents a single Unicode character.
- We can also store special characters like `$`,`@` and `&`, etc. using the character type.

Example:
```rust
let character: char = 'A';
```

> Notably, wea can also store numbers as characters using single quotes. For example,
>
> ```rust
> let num_char: char = '5';
> ```
> Here `5` is a character not a number.

## Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

### Array Type:

- Fixed-size, homogeneous data structure.
- Every element of an array must have the same type.
- Arrays are useful when you want your data allocated on the stack rather than the heap or when you want to ensure you always have a fixed number of elements.  

Example:
```rust
let a: [i32; 3] = [1, 2, 3];

let first = a[0];  // Accessing array element
let second = a[1];
```
- Here, i32 is the type of each element. After the semicolon, the number 3 indicates the array contains three elements.
- You can also initialize an array to contain the same value for each element.
  
Example:
```rust
let numbers = [3; 5];

println!("numbers = {:?}", numbers);  // [3, 3, 3, 3, 3]
```

**Mutable Array in Rust**

In Rust, an array is immutable, which means we cannot change its elements once it is created.

However, we can create a mutable array by using the mut keyword before assigning it to a variable.

Example:
```rust
fn main() {
    let mut numbers: [i32; 5] = [1, 2, 3, 4, 5];
    
    println!("original array = {:?}", array); // [1, 2, 3, 4, 5]
    
    // change the value of the 3rd element in the array
    numbers[2] = 0;
    
    println!("changed array = {:?}", numbers); // [1, 2, 0, 4, 5]
}
```

We changed the element at index `2` (third element) from `3` to `0`. This is possible because we have created the numbers array as mutable.

> **Note:** Values inside an array can only be modified but cannot be deleted because the size of the array is fixed after initialization.

### Tuple Type:

- Tuples group together elements of different types into one compound type. 
- Fixed-size, heterogeneous data structure.
- Once declared, they cannot grow or shrink in size.
  
Example:
```rust
let tup: (i32, f64, &str) = (42, 3.14, "tuple");
```
- We can break down tuples into smaller variables, known as destructuring.

Example:
```rust
let (x, y, z) = tup;

println!("The value of y is: {y}");  // prints 3.14
```

> **Note:** Destructuring a tuple is also known as **tuple unpacking.**

- We can also access a tuple element directly by using a period (.) followed by the index of the value.

Example:
```rust
let first = tup.0;  // 42
let second = tup.1; // 3.14

println!("The value of y is: {y}");
```
The tuple without any values has a special name, unit. This value and its corresponding type are both written () and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.

**Mutable Tuple**

In Rust, a tuple is immutable by default, which means we cannot change its elements once it is created.

However, we can create a mutable array by using the mut keyword before assigning it to a variable.

Example:
```rust
fn main() {
    // initialize a mutable tuple
    let mut mountain_heights = ("Everest", 8848, "Fishtail", 6993);
    
    println!("Original tuple = {:?}", mountain_heights);
    // prints => ("Everest", 8848, "Fishtail", 6993)
    
    // change 3rd and 4th element of a mutable tuple
    mountain_heights.2 = "Lhotse";
    mountain_heights.3 = 8516;
    
    println!("Changed tuple = {:?}", mountain_heights);
    // prints => ("Everest", 8848, "Lhotse", 8516)
}
```

Here, we create a mutable tuple named mountain_heights. We then change its `2nd` and `3rd` tuple index.

> **Note:** You can only change the element of a tuple to the same type as when it was created. Changing data types is not allowed after tuple creation.

## Some other types

We wiil learn about them in detail later.

### String Type:

- Represents a sequence of characters.
- The default string type is a string slice `&str`.
- Strings can be either string slices or owned strings `String`. 
- String slices are often used for string literals, while `String` is used for dynamically allocated strings.
  
Example:
```rust
let string_slice: &str = "Hello, Rust!";
let owned_string: String = String::from("Rust is awesome!");
```   

### Slice Type:

- Represents a view into a portion of a contiguous sequence. 
- Slice must reference an existing array or slice.
- Slices are dynamically sized and provide a flexible way to work with arrays.

Example:
```rust
let array = [1, 2, 3, 4, 5];
let slice: &[i32] = &array[1..4]; // includes elements from index 1 to 3
   
// Accessing the elements of the slice
for num in slice {
   println!("Number: {}", num);
}   
```

### Reference Types:

- References allow you to borrow values without taking ownership. 
- Reference must be explicitly created by borrowing.
- `&T`: Immutable reference, used for read-only access.
- `&mut T`: Mutable reference, used for read-write access.

Example:
```rust
let x = 42;
let reference: &i32 = &x;
let mutable_reference: &mut i32 = &mut x;
```
- **Rules for reference:** 
    - Multiple immutable references to the same data are allowed.
    - Only one mutable reference to a piece of data is allowed within a specific scope.

Example:
```rust
let x = 10;
let mut y = 10;
    
// Creating an immutable reference to x
let reference: &i32 = &x;

// Creating a mutable reference to y
let mutable_reference: &mut i32 = &mut y;
 
// Modifying the value through the mutable reference
*mutable_reference += 5;
    
// Accessing the value through the reference
println!("Value through immutable reference: {}", *reference);  // 10
println!("Value through mutable reference: {}", y);  // 15
```
- The `*` operator is used to dereference the reference and access the underlying value.

These are the basic data types in Rust. Understanding and using these types effectively will help you write safe and expressive Rust code.

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-02-data-types.html).

---

# Type Casting

Type casting allows us to convert variables of one data type to another. In Rust, we use the `as` keyword to perform type casting.

## Floating point to integer:

Example:
```rust
fn main() {
    // assign a floating point f64 value to decimal variable
    let decimal: f32 = 64.31;

    // convert decimal variable to u16 integer type using as keyword
    let integer = decimal as u16;

    println!("decimal = {}", decimal);
    println!("integer = {}", integer);
}
```

Here, decimal as u16; expression converts f64 floating-point type to u16 integer type.

We are converting data from one type to another type manually using the `as` keyword. This type of type casting is also known as Explicit Type Casting.

## Character to integer:

Example:
```rust
fn main() {
    let character: char = 'A';

    // convert char type to u8 integer type
    let integer = char as u8;

    println!("character = {}", character);
    println!("integer = {}", integer);
}
```

In Rust, the `char` data type is internally stored as a Unicode Scalar Value. Unicode Scalar Value is simply the numeric representation of a character in Unicode standard also known as a code point.

The Unicode value of character `A` is `65`. So, we get the output of `65` when converting the character `A` to an integer.

## Integer to character:

We can also convert integer type to a character type.

Example:
```rust
fn main() {
    // only u8 integer data type can be converted into char
    let integer: u8 = 65;
  
    // convert integer to char using the as keyword
    let character = integer as char;

    println!("integer = {}" , integer);
    println!("character = {}", character);
}
```

In the example above, the integer value `65` is the Unicode code for character `A`. Thus after type casting, we get character `A` as the output. Every character has an Unicode code associated with it.

## Error while converting integer to character

We are only allowed to use `u8` integers while performing type casting between integer and character. If we use any other integer type and convert it to a character, we will get an error. For example,

Example:
```rust
fn main() {
    let integer: i32 = 65;
  
    // convert integer to char using the as keyword
    let character = integer as char;  //! invalid cast
    // only `u8` can be cast as `char`, not `i32`

    println!("integer = {}" , integer);
    println!("character = {}", character);
}
```

Here, we have used `i32` data type instead of `u8`. Hence we get an error.

It's because Unicode Scalar Values are small integer numbers and fit in the range of `u8` data type.

## Boolean to integer

Example:
```rust
fn main() {
    let boolean1: bool = false;
    let boolean2: bool = true;
  
    // convert boolean type to integer
    let integer1 = boolean1 as i32;
    let integer2 = boolean2 as i32;

    println!("boolean1 = {}", boolean1);
    println!("boolean1 = {}", boolean2);
    println!("integer1 = {}", integer1);
    println!("integer2 = {}", integer2);
}
```

Here, boolean data type `false` and `true` are converted to integer `0` and `1` respectively.

## Limitations of Type Casting
There are limitations while performing type casting in Rust. Not all data types are converted to one another.

For example, we cannot convert a floating type to a character.

```rust
fn main() {
    let decimal: f32 = 65.321;
  
    // convert float to char data type
    let character = decimal as char;  //! invalid cast
    //! only `u8`` can be cast as `char`, not `f32`

    println!("decimal = {}", decimal);
    println!("character = {}", character);
}
```

Here, we have tried to convert the `float` type to `char`, hence we get an error. The error says that Rust is expecting a `u8` data type for conversion not `f32`.

> There are other ways to convert the data types in Rust, but those are advanced topics and will be discussed later.

---

# Operators

In Rust, operators are symbols that represent computations or operations on values. They can be used for arithmetic, logical, bitwise, and other operations. 

## Arithmetic Operators:
   - `+` (Addition)
   - `-` (Subtraction)
   - `*` (Multiplication)
   - `/` (Division)
   - `%` (Remainder)

Example:
```rust
fn main() {
    let a = 10;
    let b = 3;

    println!("Addition: {}", a + b);    // prints 13
    println!("Subtraction: {}", a - b); // prints 7
    println!("Multiplication: {}", a * b);  // prints 30
    println!("Division: {}", a / b);  // prints 3
    println!("Remainder: {}", a % b);   // prints 1
}
```

Note that, in the above example, we use the `/` operator to divide two integers `10` and `3`. The output of the operation is `3`.

In standard calculation, `10 / 3` gives `3.33`. However, in Rust, when the `/` operator is used with integer(`i32`) values, we get the quotient (integer) as the output.

If we want the actual result, we should use the `/` operator with floating-point(`f32`, `f64`) values.

Example:
```rust
fn main() {
    let a = 10.0;
    let b = 3.0;

    println!("Division: {}", a / b);  // prints 3.33
}
```

## Assignment Operators:
   - `=` (Assignment)
   - `+=` (Add and assign)
   - `-= ` (Subtract and assign)
   - `*=` (Multiply and assign)
   - `/=` (Divide and assign)
   - `%=` (Remainder and assign)

Example:
```rust
fn main() {
    let mut a = 5;

    a += 2; // equivalent to a = a + 2;
    println!("After addition: {}", a);

    a *= 3; // equivalent to a = a * 3;
    println!("After multiplication: {}", a);
}
```

## Comparison Operators:
   - `==` (Equal to)
   - `!=` (Not equal to)
   - `<` (Less than)
   - `>` (Greater than)
   - `<=` (Less than or equal to)
   - `>=` (Greater than or equal to)

Example:
```rust
fn main() {
    let x = 5;
    let y = 7;

    println!("Equal: {}", x == y);
    println!("Not equal: {}", x != y);
    println!("Less than: {}", x < y);
    println!("Greater than: {}", x > y);
    println!("Less than or equal to: {}", x <= y);
    println!("Greater than or equal to: {}", x >= y);
}
```

> **Note:** Comparison operators are also known as **relational operators**.

## Logical Operators:
   - `&&` (Logical AND) returns `true` if both exp1 and exp2 are `true`
   - `||` (Logical OR) returns `true` if any one of the expressions is `true`
   - `!` (Logical NOT) returns `true` if the expression is `false` and returns `false`, if it is `true`

```rust
fn main() {
    let a = true;
    let b = false;

    println!("Logical AND: {}", a && b);
    println!("Logical OR: {}", a || b);
    println!("Logical NOT: {}", !a);
}
```

> **Note:** The logical `AND` and `OR` operators are also called **short-circuiting logical operators** because these operators don't evaluate the whole expression in cases they don't need to.
>
> Example:
> ```rust
> false || true || false
> ```
>
> The `||` operator evaluates to true because once the compiler sees a single true expression, it skips the evaluation and returns true directly.

These are some of the fundamental operators in Rust. Understanding how they work will be crucial as you start writing Rust programs.

---

# Control Flow

The ability to control the flow of code execution based on conditions is fundamental. Rust, a language known for its emphasis on safety and performance, provides constructs such as `if` expressions and `loops` to control the flow of operations.


## if Expressions

### Basic `if-else`:

Example:
```rust
fn main() {
    let number = 42;

    if number > 50 {
        println!("Number is greater than 50");
    } else {
        println!("Number is less than or equal to 50");
    }
}
```

In this example, the condition `number > 50` is checked, and the corresponding block is executed based on whether the condition is `true` or `false`.

It’s also worth noting that the condition in this code must be a bool. If the condition isn’t a bool, we’ll get an error.

Example:
```rust
fn main() {
    let number = 3;

    if number {     // mismatched types, expected `bool`, found integer
        println!("number was three");
    }
}
```

The error indicates that Rust expected a bool but got an integer. Unlike languages such as Ruby and JavaScript, Rust will not automatically try to convert non-Boolean types to a Boolean. You must be explicit and always provide if with a Boolean as its condition.

### `if-else if` ladder:

Example:
```rust
fn main() {
    let grade = 85;

    if grade >= 90 {
        println!("Grade A");
    } else if grade >= 80 {
        println!("Grade B");
    } else if grade >= 70 {
        println!("Grade C");
    } else {
        println!("Grade below C");
    }
}
```

This example demonstrates a chain of `if-else if` statements. The conditions are checked in order, and the first true branch is executed.

### Using `if` in Assignments:

Rust allows using `if` in assignments. The value assigned is the result of the executed block based on the condition.

Example:
```rust
fn main() {
    let is_even = if number % 2 == 0 { true } else { false };
    println!("Is the number even? {}", is_even);
}
```

Note that the return values from each arm of the `if` must be the same type, or we’ll get an error.

Example:
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { "six" };
    // `if` and `else` have incompatible types
    // expected integer, found `&str`
    println!("The value of number is: {number}");
}
```

### Ternary Operator (Shorthand for Simple `if-else`):

In Rust, there is no traditional ternary operator, but this kind of shorthand using `if-else` works similarly.

```rust
fn main() {
    let number = 42;
    let result = if number > 50 { "greater" } else { "less or equal" };
    println!("Number is {}", result);
}
```

>Remember that the conditions must be of type `bool`. Rust does not automatically convert non-boolean values to boolean like some other languages. The condition must explicitly be a boolean expression.

## Repetition with Loops

In Rust, loops allow you to repeatedly execute a block of code until a certain condition is met. There are three primary types of loops in Rust: `loop`, `while`, and `for`.

### `loop`:

The `loop` keyword creates an infinite loop, which continues until explicitly interrupted using `break`.

Example:
```rust
fn main() {
    let mut count = 0;

    loop {
        println!("Count: {}", count);
        count += 1;

        if count == 5 {
            break; // Exit the loop when count reaches 5
        }
    }
}
```

Sometime you may want to use the result of `loop` in your code, to do this, you can add the value you want returned after the `break` expression; that value will be returned out of the loop so you can use it.

Example:
```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}"); // 20
}
```

#### `loop` inside `loop`:

In Rust, you can create nested loops by placing one `loop` inside another one. This can be useful for more complex control flow scenarios. But in this case, `break` and `continue` will apply to the innermost loop at that point. 

To avoid this problem, rust have something called `loop labels`. Loop labels allow you to name loops, and then use the `break` or `continue` statements with a specific label to indicate which loop you want to affect.

>Loop labels must begin with a single quote.

Example:
```rust
'outer: loop {
    // Code inside the outer loop
    'inner: loop {
        // Code inside the inner loop
        
        if some_condition {
            break 'inner;
        }
        
        if another_condition {
            break 'outer;
        }
    }
    // Code after the inner loop
}
// Code after the labeled loops
```

### `while`:

The `while` loop continues executing the block of code as long as the specified condition is `true`.

Example:
```rust
fn main() {
    let mut number = 1;

    while number <= 5 {
        println!("Number: {}", number);
        number += 1;
    }
}
```

This construct eliminates a lot of nesting that would be necessary if you used `loop`, `if`, `else`, and `break`, and it’s clearer. While a condition evaluates to true, the code runs; otherwise, it exits the loop.

### `for`:

The `for` loop iterates over a range, a collection or an iterable object, executing the block of code for each iteration.

Example with range:
```rust
fn main() {
    for number in (1..=3).rev() {   // rev(), to reverse the range
        println!("{number}!");
    }
    println!("LIFTOFF!!!"); // Output: 3 2 1 "LIFTOFF!!!"
}
```

Example with iterable object (e.g., array):
```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];

    for number in numbers.iter() {
        println!("Number: {}", number);
    }
}
```

The `.iter()` method is used to create an iterator over the elements of the array `numbers`. The loop then iterates over each element using this iterator. Using `.iter()`, does not consume the array; it only borrows the elements. This means the array `numbers` can still be used after the loop.

Example without iterator:
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

Here, the array `a` is directly used in the loop. In this case, the loop consumes the array, and each iteration takes `ownership` of each element of the array. If the elements of the array were of a type that does not implement the `Copy` trait, this approach would move the elements out of the array, and the array `a` would be unusable after the loop.

> We will learn about the `Ownership` in the upcoming notes.

Rust's `for` loop is quite versatile, supporting various iterable objects. 

### Loop Control Statements:

- **`break`:** Exits the loop.
- **`continue`:** Skips the rest of the current iteration and proceeds to the next one.

Example with `break` and `continue`:
```rust
fn main() {
    for number in 1..=10 {
        if number % 2 == 0 {
            continue; // Skip even numbers
        }

        println!("Number: {}", number);

        if number == 7 {
            break; // Exit the loop when number is 7
        }
    }
}
```

Loops in Rust offer flexibility and control, ensuring that you can efficiently iterate or repeatedly execute code based on your specific requirements.

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-05-control-flow.html).

---

# Functions

Functions are reusable blocks of code that perform a specific task. In Rust code, functions play a crucial role. You’ve already seen one of the most important functions in the language: the `main` function, which is the entry point of the rust programs.

Snake case is the conventional style for naming functions and variables in Rust. This style involves using all lowercase letters and separating words with underscores.

Functions in Rust are declared using the `fn` keyword. They can have parameters, a return type, and a block of code defining their behavior.

## Example:
```rust
// Function with parameters and a return type
fn add_numbers(a: i32, b: i32) -> i32 {
    a + b
}

// Function with no parameters and no return value
fn greet() {
    println!("Hello, Rust!");
}

fn main() {
    let result = add_numbers(3, 4);
    println!("Result: {result}");

    greet();
}
```

In this example, `add_numbers` is a function that takes two parameters (`a` and `b`) and returns their sum. The `greet` function has no parameters and no return value.

In function, you must declare the type of each parameter. 

> **Note:** Rust code uses a small case as the convention for defining a function name. An extended function name with multiple words will have underscores in between words.

## Statements and Expressions:

### Statements:
- Statements are instructions that perform an action but do not return a value.
- They end with a semicolon `;`.
- Function definitions are also statements.
  
Example:
```rust
let x = 5; // statement
println!("Value of x: {}", x); // statement
```

### Expressions:
- Expressions evaluate to a value.
- They do not end with a semicolon, as the semicolon turns an expression into a statement.
  
Example:
```rust
let y = {
    let a = 3;
    let b = 4;
    a + b // expression without a semicolon, returns the result
};

println!("Value of y: {}", y);
```

In the second example, the block `{}` contains an expression (`a + b`) without a semicolon, making it the value of the block. This value is then assigned to `y`.

In Rust, functions return the result of their last expression implicitly. However, if you use a semicolon after the last expression, it becomes a statement, and the function returns `()` (unit type) implicitly.

Understanding the distinction between statements and expressions is important, especially when dealing with function return values and blocks of code in Rust.

## Return Values:

In Rust, we must declare type of the return value after an arrow (`->`). Unlike some other languages, Rust does not require naming return values explicitly.

Notably, the `return` keyword is optional in functions, the return value of the function is synonymous with the value of the final expression in the block of the body of a function.

If necessary, you can use the `return` keyword to exit a function prematurely and specify a particular value to be returned. However, in most cases, the final expression in the function block serves as the implicit return value. 

Example:
```rust
fn five() -> i32 {
  5   // // This is the implicit return value
}

fn plus_one(x: i32) -> i32 {
  return x + 1;   // Explicit use of return keyword
}

fn main() {
  let x = five();
  let y = plus_one(5);
  println!("The value of x is: {x}");
  println!("The value of y is: {y}"); // 6
}
```

**How to pass by reference in Rust?**

We can use pass by reference to pass a pointer of the variable instead of the actual variable. For example,

```rust
fn main() {
  let word = String::from("hello");

  // passing reference of word variable
  let len = calculate_length(&word);

  println!("The length of '{}' is {}.", word, len);
  // Prinys -> The length of 'hello' is 5.
}

fn calculate_length(s: &String) -> usize {
  s.len()
}
```

Here, we pass the `word` variable as a reference to the function `calculate_length()` with `&word`.

> **Note:** We will talk about reference in details in upcoming notes.

## Comments:

>I know comments are unrelated to functions, but I don't want to make a seperate note for this part.

- In Rust, comments are initiated with two slashes (`//`). 
- For single-line comments, this style continues until the end of the line. 
- Multi-line comments require `//` on each line. 
- Comments can be placed at the end of lines containing code. 

Example:
```rust
fn main() {
  let lucky_number = 7; // I’m feeling lucky today
}
```
- However, the more common style involves putting comments on a separate line above the code they annotate. 

Example:
```rust
fn main() {
  // I’m feeling lucky today
  let lucky_number = 7;
}
```
- Rust also supports documentation comments, denoted by `///`, providing a distinct way to document code for tools and external users.

Example:
```rust
/// This function adds two numbers together.
///
/// # Examples
///
/// ```
/// let result = add_numbers(3, 4);
/// assert_eq!(result, 7);
/// ```
fn add_numbers(a: i32, b: i32) -> i32 {
    a + b
}
```

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html).

---



