# Pattern Matching

Pattern matching in Rust is primarily done using the `match` keyword. We have already seen the use of `match` multiple times in `struct` and `enum` notes. It is a powerful and expressive feature that allows us to match patterns against values and execute corresponding code based on the matched pattern. Patterns can be used to destructure values, perform comparisons, and control the flow of your program.

## Basic Syntax

```rust
match value {
    pattern1 => {
        // code to execute when value matches pattern1
    }
    pattern2 => {
        // code to execute when value matches pattern2
    }
    _ => {
        // code to execute for any other case (default case)
    }
}
```

## Matching values

We can pattern match against the value of a variable. This is useful if our code wants to take some action based on a particular value.

Example:
```rust
fn main() {
    let x = 42;

    match x {
        0 => println!("Zero"),
        1 | 2 => println!("One or Two"),
        3..=9 => println!("Three to Nine (inclusive)"),
        _ => println!("Others"),
    }
}
```

Notice that we also match against underscore `_`. The **catch-all patterns**, `_` placeholder has a special meaning in pattern matching, if all the other patterns do not match, it defaults to `_`.

## Matching Enums

Pattern matching is extensively used to match enum variants.

Example:
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}

fn main() {
    let penny = Coin::Penny;
    println!("Value of Penny: {} cents", value_in_cents(penny));
}
```

## Destructuring Tuples

We can also use pattern matching to destructure the tuples.

Example:
```rust
fn main() {
    let point = (3, 4);

    match point {
        (0, 0) => println!("Origin"),
        (x, 0) => println!("On x-axis at {}", x),
        (0, y) => println!("On y-axis at {}", y),
        (x, y) => println!("At coordinates: ({}, {})", x, y),
    }
}
```

## Matching Option and Result Type

The most common case for pattern matching is with `Option` and `Result` enum types. Both the `Option` and `Result` type have two variants and are mainly used for error handling.

**Option type has:**

- Some(T) → a value with type T
- None → to indicate failure with no value

**Result type has:**

- Ok(T) → operation succeeded with value T
- Err(E) → operation failed with an error E

> **Note:** We will discuss `Option` and `Result` types in the error handling notes.

## Matches Are Exhaustive

There’s one other aspect of `match` we need to discuss, the arms patterns must cover all possibilities.

Example:
```rust
fn main() {
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            Some(i) => Some(i + 1), // error
            // non-exhaustive patterns: `None` not covered
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
}
```

Here the code will not compile. We didn’t handle the `None` case, so this code will cause a bug. If we try to compile this code, we’ll get an error. Rust knows that we didn’t cover every possible case, and even knows which pattern we forgot!

## Usage and When to Use

- **Used for control flow:** `match` is often used instead of long chains of `if-else` statements, providing a more concise and readable way to express branching logic.

- **Destructuring:** It's useful for destructuring complex data types like enums, tuples, and structs to access their individual components.

- **Exhaustiveness:** The compiler ensures that all possible cases are handled, preventing accidental oversight.

## Pitfalls
- **Missing Cases:** If all possible cases are not covered, the compiler will issue a warning. Ensure you cover all cases or use a wildcard `_` pattern if some cases can be ignored.

- **Shadowing:** Be cautious when using the same variable name within different arms of the match statement, as it can lead to unintentional shadowing.

- **Order Matters:** The order of patterns in a `match` statement matters. Patterns are checked from top to bottom, and the **first match is executed**.

- **Complexity:** While powerful, complex `match` statements with many patterns can reduce readability. In such cases, consider refactoring code or using other constructs like `if let` for simpler cases.

## `match` alternatives in Rust

In Rust, `if let`, `let else`, and `while let` are constructs that provide a concise way to handle specific patterns or conditions without the verbosity of a full `match` statement.

### 1. `if let`:

The `if let` syntax allows you to match a single pattern and execute code if the pattern matches.

Example:
```rust
fn main() {
    let value = Some(42);

    if let Some(x) = value {
        println!("Got a value: {}", x);
        // output: Got a value: 42
    } else {
        println!("Got None");
    }
}
```
Here, the `if let` expression is matching on the `value` variable and binding the value of `Some` variant to the `x` variable.

**Usage:**
- Use `if let` when you are interested in a specific pattern and want to handle that case, and optionally, have a fallback for other cases.

**Pitfalls:**
- Be aware that `if let` only handles one specific pattern. If you need to handle multiple patterns, consider using `match`.

### 2. `let else`:

The `let else` syntax allows you to handle the "None" case explicitly when using the `Option` type.

Example:
```rust
fn main() {
    let value1 = Some(42);
    let value2 = None;

    let x = check_value(&value1);

    let y = check_value(&value2);

    println!("Value of x: {}", x); // Output: Value of x: 42
    println!("Value of y: {}", y); // Output: Value of y: 0
}

fn check_value(value: &Option<i32>) -> i32 {
    if let Some(v) = value {
        *v
    } else {
        0
    }
}
```

**Usage:**
- Use `let else` when you want to assign a default value or perform a fallback action when the result is `None`.

**Pitfalls:**
- Like `if let`, `let else` only handles one specific pattern. If you need to handle multiple patterns, consider using `match`.

### 3. `while let`:

The `while let` syntax allows you to repeatedly execute code as long as a pattern matches.

Example:
```rust
fn main() {
    let mut stack = Vec::new();
    stack.push(Some(42));
    stack.push(Some(25));
    stack.push(None);

    while let Some(value) = stack.pop() {
        match value {
            Some(x) => println!("Popped value: {}", x),
            None => println!("No value"),
        }
    }
}
```

Output:
```terminal
No value
Popped value: 25
Popped value: 42
```

The order of the output corresponds to the reverse order of items being popped from the stack.

**Usage:**
- Use `while let` when you want to repeatedly perform an action as long as a certain pattern is matched.

**Pitfalls:**
- Be cautious with infinite loops. Ensure that the loop eventually terminates by modifying the loop variable or using a `break` statement.

In summary, pattern matching is a fundamental part of Rust that contributes to its safety and expressiveness. It's a versatile tool that can simplify code and make it more robust.