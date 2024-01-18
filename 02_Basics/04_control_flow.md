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