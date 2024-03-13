# Operators

In Rust, operators are symbols that represent computations or operations on values. They can be used for arithmetic, logical, bitwise, and other operations.

## Arithmetic Operators
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

## Assignment Operators
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

## Comparison Operators
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

## Logical Operators
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
