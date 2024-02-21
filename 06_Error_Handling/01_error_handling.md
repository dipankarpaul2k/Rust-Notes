# Rust Error Handling

Error handling is a fundamental part of any programming language. In Rust, error is an unexpected behavior or event in a program that will produce an unwanted output.

Errors are of two categories:

- Unrecoverable Errors
- Recoverable Errors

## Unrecoverable Errors

Unrecoverable errors are errors from which a program stops its execution. These are typically handled using the `panic!` macro. Unrecoverable errors indicate that something has gone fundamentally wrong with the program, such as a logic error or a critical resource being unavailable. When a **panic** occurs, the program will unwind the stack, clean up resources, and then terminate.

Here's an example of using `panic!` to handle an unrecoverable error.

```rust
fn main() {
    let divisor = 0;
    if divisor == 0 {
        panic!("Cannot divide by zero");
    }

    println!("Hello, World!");

    let result = 10 / divisor;
    println!("Result: {}", result);
}
// Output:
// Hello, World!
// thread 'main' panicked at 'Cannot divide by zero', /tmp/G8FP5jHb2u/main.rs:5:9
```

In this example, if `divisor` is zero, the program will panic with the message "Cannot divide by zero." The panic will unwind the stack, and the program will terminate.

Notice that the program still runs the expressions above panic! macro. We can still see `Hello, World!` printed to the screen before the error message.

The `panic!` macro takes in an error message as an argument.

## Recoverable Errors

Recoverable errors are errors that won't stop a program from executing. Most errors are recoverable, and we can easily take action based on the type of error.

Recoverable errors are typically handled using the `Result` type. When a function can fail, it can return a `Result` type that indicates either success (`Ok`) or failure (`Err`). Here's an example of how you might handle a recoverable error using `Result`.

```rust
use std::fs::File;
use std::io::Error;

fn open_file(path: &str) -> Result<File, Error> {
    match File::open(path) {
        Ok(file) => Ok(file),
        Err(err) => Err(err),
    }
}

fn main() {
    let file_path = "example.txt";
    match open_file(file_path) {
        Ok(file) => println!("File opened successfully"),
        Err(err) => println!("Error opening file: {}", err),
    }
}
```

In this example, the `open_file` function attempts to open a file at the given path. If the file is successfully opened, it returns `Ok(file)`. If an error occurs, it returns `Err(err)`. In the `main` function, we match on the result of `open_file` to handle both cases.

## The Result Enum

In Rust, the `Result` enum is used to represent the result of an operation that can either succeed (`Ok`) and return a value, or fail (`Err`) and return an error. The `Result` enum is defined as follows.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

- The `T` type parameter represents the type of the value returned on success.
- The `E` type parameter represents the type of the error returned on failure.

Here's a simple example demonstrating the `Result` enum in action.

```rust
use std::fs::File;
use std::io::Error;

fn open_file(path: &str) -> Result<File, Error> {
    match File::open(path) {
        Ok(file) => Ok(file),
        Err(err) => Err(err),
    }
}

fn main() {
    let file_path = "example.txt";
    match open_file(file_path) {
        Ok(file) => println!("File opened successfully"),
        Err(err) => println!("Error opening file: {}", err),
    }
}
```

In this example, the `open_file` function attempts to open a file at the given path. It returns a `Result<File, Error>`, where `File` is the type of the value returned on success (the opened file) and `Error` is the type of the error returned on failure.

The `match` statement in the `main` function is used to handle the `Result` returned by `open_file`. If `open_file` returns `Ok(file)`, the file is successfully opened and a message is printed. If `open_file` returns `Err(err)`, an error occurred while opening the file, and the error message is printed.

The `Result` enum is a powerful tool for handling errors in Rust, providing a clear and concise way to represent the outcome of operations that can fail.

## The Option Enum

In Rust, the `Option` enum is used to represent the presence or absence of a value. It is commonly used in scenarios where a value might be missing, such as when looking up a key in a dictionary or when parsing user input. The `Option` enum is defined as follows:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

- The `T` type parameter represents the type of the value that might be present.
- The `Some` variant contains the actual value.
- The `None` variant indicates that no value is present.

Here's a simple example demonstrating the `Option` enum in action:

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 {
        None
    } else {
        Some(a / b)
    }
}

fn main() {
    let result = divide(10.0, 5.0);
    match result {
        Some(value) => println!("Result: {}", value),
        None => println!("Cannot divide by zero"),
    }
}
```

In this example, the `divide` function calculates the result of dividing two numbers. If the divisor (`b`) is zero, the function returns `None` to indicate that the division is not possible. Otherwise, it returns `Some(result)` with the actual result of the division.

The `match` statement in the `main` function is used to handle the `Option` returned by `divide`. If `divide` returns `Some(value)`, the result is printed. If `divide` returns `None`, a message indicating that division by zero is not possible is printed.

The `Option` enum is a powerful tool for handling optional values in Rust, providing a safe and concise way to deal with the absence of a value.

## Difference between Result and Option enum in Rust

The `Result` and `Option` enums in Rust are both used to represent the possibility of absence, but they are used in different contexts and have different purposes:

1. **`Result<T, E>`**:
   - Represents the result of an operation that can succeed (`Ok(T)`) and return a value of type `T`, or fail (`Err(E)`) and return an error of type `E`.
   - Typically used for operations that can fail in a recoverable way, such as file I/O, network operations, or parsing.
   - Forces the developer to handle errors explicitly, making error handling more robust and predictable.

2. **`Option<T>`**:
   - Represents the possibility of a value being present (`Some(T)`) or absent (`None`).
   - Typically used for optional values, where the absence of a value is a valid and expected outcome, rather than an error.
   - Provides a safe and concise way to handle optional values without using null or undefined.

In summary,

- **Purpose**: `Result` is used for operations that can fail with an error, while `Option` is used for optional values where absence is not considered an error.
- **Variants**:
  - `Result`: `Ok(T)` for success and `Err(E)` for error.
  - `Option`: `Some(T)` for a present value and `None` for absence.
- **Error Handling**: `Result` forces explicit error handling, while `Option` provides a way to handle optional values without the need for error handling.

In general, `Result` is used for operations where failure is expected and should be handled, while `Option` is used for cases where a value may or may not be present, and the absence of a value is not an exceptional condition.