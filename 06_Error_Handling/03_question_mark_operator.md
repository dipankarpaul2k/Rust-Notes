# Propagating Errors in Rust

In Rust, error handling is a crucial aspect of writing robust and reliable code. One common pattern for error handling is propagating errors, which involves passing errors up the call stack rather than handling them immediately. This allows errors to be handled at higher levels of abstraction where more context is available.

## The `?` Operator in Rust

The `?` operator in Rust is a shorthand for propagating errors in functions that return a `Result` type. It can only be used within functions that return `Result` or `Option`. When used, the `?` operator essentially unwraps the `Result`, returning the inner value if it's `Ok`, or propagating the error if it's `Err`.

**Syntax and Usage**

The syntax for the `?` operator is simple: it follows the expression that returns a `Result`. If the expression evaluates to `Ok`, the value is returned, and if it evaluates to `Err`, the error is immediately returned from the function.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_file_contents(filename: &str) -> Result<String, io::Error> {
    let mut file = File::open(filename)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

fn main() {
    match read_file_contents("example.txt") {
        Ok(contents) => println!("File contents: {}", contents),
        Err(err) => eprintln!("Error reading file: {}", err),
    }
}
```

In this example, the `read_file_contents` function attempts to open a file and read its contents. If any error occurs during this process, such as the file not existing or being unable to read from it, the error is propagated back to the caller using the `?` operator.

## Bubble Up Multiple Errors

Sometimes, a function can return multiple error types. In such cases, you can use the `Box<dyn Error>` trait object to wrap different error types into a single type.

```rust
use std::error::Error;

fn multiple_errors() -> Result<(), Box<dyn Error>> {
    let result1: Result<(), io::Error> = Err(io::Error::new(io::ErrorKind::Other, "Error 1"));
    let result2: Result<(), std::fmt::Error> = Err(std::fmt::Error {});

    result1?;
    result2?;

    Ok(())
}
```

In summary, propagating errors using `?` allows for concise error handling and improves code readability. Additionally, when dealing with functions that return multiple error types, using `Box<dyn Error>` helps to wrap different error types into a single type for easier management.