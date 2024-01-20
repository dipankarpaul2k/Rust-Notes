<!-- omit in toc -->
# Variables and Mutability

## Variables
- In Rust, variables are created using the `let` keyword.
- Variables are `immutable` by default, meaning their values cannot be changed once assigned.
  
Example:
```rust
let x = 5; // immutable variable
```

## Mutability
- To make a variable mutable, you can use the `mut` keyword.
  
Example:
```rust
let mut y = 10; // mutable variable
y = 15; // valid because y is mutable
```

## Shadowing
- Rust allows shadowing, where you can redeclare a variable with the same name, effectively creating a new variable that hides the previous one. 
 >Rustaceans say that the first variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable.
  
Example:
```rust
let z = 20;
let z = z + 5; // shadows the previous z
```
- The difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the let keyword again, we can change the type of the value but reuse the same name.

## Constants
- Constants are declared using the `const` keyword.
- Constants must have a specified type, and their values cannot be changed.
- You aren’t allowed to use `mut` with constants. Constants aren’t just immutable by default, they’re always immutable.
  
Example:
```rust
const PI: f64 = 3.14;
```

## Type Annotations

- While Rust can often infer the variable type, you can explicitly specify it using a type annotation.(We will talk more about the type annotation in the next section.)
  
Example:
```rust
let message: &str = "Hello, Rust!"; // type annotation for a string reference
```

## Example combining mutability, shadowing, and constants

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

---

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html).