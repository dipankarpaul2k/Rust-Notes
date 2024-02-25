# Rust unwrap() and expect()

In Rust, the `unwrap()` ard `expect()` are utility methods that work with `Option` and `Result` types in Rust.

## `unwrap()` method

The `unwrap` method is used to extract the value from a `Result` or `Option` type. It returns the inner value if it exists, or it panics if the value is a `None` (for `Option`) or an `Err` variant (for `Result`). For example,

```rust
fn divide(x: f64, y: f64) -> Result<f64, &'static str> {
    if y == 0.0 {
        Err("Division by zero!")
    } else {
        Ok(x / y)
    }
}

fn main() {
    let result = divide(10.0, 2.0);
    let value = result.unwrap(); // This will return the inner value (5.0) if the result is Ok, or panic if it's Err.
    println!("Result: {}", value); // Output: 5.0
}
```

In this example, the `divide` function returns a `Result<f64, &'static str>`, representing either the result of the division or an error message if the division by zero occurs. `unwrap` is used in `main` to extract the inner value (5.0 in this case) from the `Result`. If the `Result` is an `Err`, `unwrap` will panic and the program will crash.

While `unwrap` is convenient, it's also risky because it can cause your program to panic if used incorrectly. It's often better to use methods like `unwrap_or`, `unwrap_or_else`, or `unwrap_or_default`, which provide safer alternatives by allowing you to specify a default value or a function to call in case of an error.

### `unwrap_or(default)`

This method returns the inner value if it exists, or the provided default value otherwise. It does not panic.

```rust
let some_option: Option<i32> = None;
let result = some_option.unwrap_or(42);
println!("Result: {}", result); // Output: 42
```

### `unwrap_or_else(f)`

This method behaves similarly to `unwrap_or`, but instead of providing a default value directly, you provide a function that will be called to produce the default value when needed.

```rust
let some_option: Option<i32> = None;
let result = some_option.unwrap_or_else(|| {
    println!("Calculating default value...");
    42
});
println!("Result: {}", result); // Output: Calculating default value... Result: 42
```

### `unwrap_or_default()`

This method is available only for types that implement the `Default` trait. It returns the inner value if it exists, or the default value of the type otherwise. This is a more concise way of providing a default value without specifying it explicitly.

```rust
let some_option: Option<i32> = None;
let result = some_option.unwrap_or_default();
println!("Result: {}", result); // Output: 0 (default value for i32)
```

**When to use which method:**

- Use `unwrap` when you're confident that the `Result` or `Option` will always contain a value, and it's okay for your program to panic otherwise (e.g., in a development or testing scenario).
  
- Use `unwrap_or` when you want to provide a default value that is simple and doesn't require additional computation.

- Use `unwrap_or_else` when you want to provide a default value that requires some computation or when you want to defer the computation until it's actually needed.

- Use `unwrap_or_default` when you want to provide a default value that is the default value of the type itself and doesn't require any additional computation.

## `expect()` method

The `expect` method is similar to `unwrap`, but it allows you to provide a custom error message when a panic occurs. This can be helpful for debugging, as the error message will provide more context about why the program panicked. For example,

```rust
fn divide(x: f64, y: f64) -> Result<f64, &'static str> {
    if y == 0.0 {
        Err("Division by zero!")
    } else {
        Ok(x / y)
    }
}

fn main() {
    let result = divide(10.0, 0.0);
    let value = result.expect("Failed to divide. DIVISION BY ZERO IS NOT ALLOWED."); // Custom error message for panic
    println!("Result: {}", value); // This will never be reached in this example
}
```

In this example, if the `divide` function is called with `y` as `0.0`, it will return an error. When `expect` is called on this `Result`, it will panic with the message `"Failed to divide. DIVISION BY ZERO IS NOT ALLOWED."`.

> **Note:** `expect` messages should be specific, concise. You can also describe `panic` situations(like "DIVISION BY ZERO IS NOT ALLOWED").

**When to use `expect`:**

- Use `expect` when you want to provide a custom error message to be displayed if a `panic` occurs. This can make it easier to understand why the program panicked, especially during debugging.

- It's often a good practice to use `expect` instead of `unwrap` when you expect an operation to fail under certain conditions, but you still want to provide a helpful error message in case it does fail. This helps in identifying and fixing issues in your code.

## `map_err` method
Other than `unwrap` and `expect`, there are anoher useful method called `map_err`. This method transforms the error variant of a `Result` into a new error variant. It takes a closure that accepts the current error value and returns a new error value. If the `Result` is an `Ok`, the closure is not called, and the `Ok` variant is returned unchanged. For example,

```rust
let result: Result<i32, &str> = Err("error");
let mapped_result = result.map_err(|err| format!("Custom error: {}", err));
println!("{:?}", mapped_result); // Output: Err("Custom error: error")
```

Use `map_err` when you want to transform the error variant of a `Result` into a different type of error or modify the error message.

In summary, even though `unwrap` and `expect` is easy to use, it is generally discouraged. Instead, prefer to use pattern matching and handle the `Err` case explicitly, or call `unwrap_or`, `unwrap_or_else`, or `unwrap_or_default`.