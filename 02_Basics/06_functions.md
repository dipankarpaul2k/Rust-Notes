<!-- omit in toc -->
# Functions

In Rust code, functions play a crucial role. You’ve already seen one of the most important functions in the language: the `main` function, which is the entry point of the rust programs.

Snake case is the conventional style for naming functions and variables in Rust. This style involves using all lowercase letters and separating words with underscores.

Functions in Rust are declared using the `fn` keyword. They can have parameters, a return type, and a block of code defining their behavior.

- [Example:](#example)
- [Statements and Expressions:](#statements-and-expressions)
  - [Statements:](#statements)
  - [Expressions:](#expressions)
- [Return Values:](#return-values)
- [Comments:](#comments)


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

In function signatures, you must declare the type of each parameter. 

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