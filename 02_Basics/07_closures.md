# Closures in Rust

Closures in Rust are anonymous functions that can capture variables from their surrounding environment. They are useful for encapsulating behavior that can be passed around as arguments to other functions or stored in data structures. Closures provide flexibility and conciseness, especially in scenarios where defining a separate named function is unnecessary.

## Syntax

In Rust, closures are defined using the `|args| body` syntax, where `args` represent the parameters and `body` represents the implementation. Closures can capture variables from their enclosing scope, making them powerful and flexible constructs.

## Defining a Closure

A basic closure in Rust can be defined as follows.

```rust
// define a closure to print a text
let print_text = || println!("Hello World!");
```

In the above example, we have created a closure that prints the text **"Hello World!"**. Here,

- `print_text` - variable to store the closure
- `||` - start of a closure
- `println!("Hello World!")` - body of the closure

## Calling a Closure

Closures can be called just like regular functions.

```rust
// call the closure
print_text(); // Output: Hello World!
```

## Closure with Parameters

Closures can take parameters just like functions.

```rust
let multiply = |x, y| x * y;
let result = multiply(3, 4);
println!("Result: {}", result); // Output: Result: 12
```

## How the Closure Works

Closures in Rust capture their environment by reference by default. This means they can access variables from the scope in which they are defined. This behavior enables closures to carry context with them.

## Multi-line Closure

Multi-line closures can be defined using curly braces `{}`.

```rust
let add_and_print = |x, y| {
    let sum = x + y;
    println!("Sum: {}", sum);
    sum
};

let result = add_and_print(3, 4);
println!("Result: {}", result);
```

## Closure Environment Capturing

Closures capture variables from their surrounding environment. They can capture variables by reference or by value, depending on the situation and the specified capturing mode.

```rust
fn main() {
    let num = 100;
    
    // A closure that captures the num variable
    let print_num = || println!("Number = {}", num);
    
    print_num(); // Output: Number = 100
}
```

Here, the closure bound to `print_num` uses the variable `num` which was not defined in it. This is known as **closure environment capturing**.

## Closure Environment Capturing Modes

Closures can capture variables in different modes: `Fn`, `FnMut`, and `FnOnce`. Each mode governs how the closure interacts with the variables it captures, determining whether it borrows them immutably, mutably, or consumes them.

### `Fn` (Immutable Borrowing)

Borrows variables by reference. The `Fn` mode borrows variables from the enclosing scope immutably. This means the closure can read the variables but cannot modify them. The closure retains a reference to the captured variables, allowing multiple invocations without altering the original values.

```rust
let x = 5;

// immutable closure
let print_x = || println!("x: {}", x);

print_x(); // Output: x: 5
print_x(); // Output: x: 5
```

In this example, the closure `print_x` captures the variable `x` using the `Fn` mode. It can access `x` and print its value, but it cannot modify `x`.

This mode of capture is also known as **Capture by Immutable Borrow**.

### `FnMut` (Mutable Borrowing)

Borrows variables by mutable reference. The `FnMut` mode allows closures to borrow variables mutably. This mode enables closures to modify the captured variables. Unlike `Fn`, `FnMut` closures can change the values of the captured variables, but they still retain a reference to them.

```rust
let mut x = 5;

// mutable closure
let mut increment_x = || {
    x += 1;
    println!("Incremented x: {}", x);
};

increment_x(); // Output: Incremented x: 6
increment_x(); // Output: Incremented x: 7
println!("x: {}", x); // Output: x: 7
```

In this example, the mutable closure `increment_x` captures the variable `x` using the `FnMut` mode. It can modify `x`, incrementing its value, and print the updated value.

**Note:** Mutable closure, this means no other references of the variable `x` can exist unless the closure is used. For example,

```rust
fn main() {
    let mut x = 5;

    // mutable closure
    let mut increment_x = || {
        x += 1;
        println!("Incremented x: {}", x);
    };

    let y = &x;
    // Error: cannot borrow `x` as immutable because it is also borrowed as mutable

    increment_x(); // Output: Incremented x: 6

    let z = &x; // No error, as mutable closure is already used
}
```

This mode of capture is also known as **Capture by Mutable Borrow**.

### `FnOnce` (Only Allows A Single Call)

The `FnOnce` mode consumes variables, taking ownership of them. Closures in this mode can only be called once because they transfer ownership of the captured variables. After the first invocation, the closure becomes unusable.

```rust
let x = vec![1, 2, 3];
let print_and_consume_x = || {
    let new_x = x; // Move happens here
    // this value implements `FnOnce`, which causes it to be moved when called
    println!("Consumed x: {:?}", new_x);
};
// x has been consumed and cannot be used again

print_and_consume_x(); // Output: Consumed x: [1, 2, 3]
print_and_consume_x(); 
// Error: closure cannot be invoked more than once 
// because it moves the variable `x` out of its environment
// println!("x: {:?}", x); // Error: value borrowed here after move
```

In this example, the closure `print_and_consume_x` captures the variable `x` using the `FnOnce` mode. Then it moves the variable `x` to a new variable `new_x` inside the closure. It prints the contents of `new_x` and consumes it, preventing any further use of `x` after the first invocation. 

This mode of capture is also known as **Capture by Move**.

These capturing modes provide flexibility in how closures interact with their captured variables, allowing Rust developers to express complex behaviors while maintaining the language's safety guarantees.

## Difference Between Functions and Closures

The main difference between functions and closures in Rust is that closures can capture their environment, meaning they can access variables from the scope in which they are defined. Functions, on the other hand, operate in a more isolated manner.

<table>
    <tr>
        <td>Functions</td>
        <td>Closures</td>
    </tr>
    <tr>
        <td>Named And Standalone; Defined With `Fn` Keyword</td>
        <td>Anonymous; Defined Inline Using `|args| body` syntax</td>
    </tr>
    <tr>
        <td>Cannot Capture Variables From Surrounding Scope</td>
        <td>Can Capture Variables From Surrounding Scope</td>
    </tr>
    <tr>
        <td>Less Flexible; Fixed Behavior And Signature</td>
        <td>More Flexible; Can Capture Context And Adapt Behavior</td>
    </tr>
    <tr>
        <td>Fixed Syntax With Named Parameters And Return Type</td>
        <td>Flexible Syntax; Parameters And Return Type Inferred</td>
    </tr>
    <tr>
        <td>Can Be Reused Across Different Contexts</td>
        <td>Typically Used For Short-Lived, Context-Dependent Behavior</td>
    </tr>
    <tr>
        <td>Suitable For Encapsulating Reusable Logic</td>
        <td>Suitable For Ad-Hoc Behavior And Small Tasks</td>
    </tr>
    <tr>
        <td>No Ownership Transfer; Operate On Borrowed References</td>
        <td>Can Consume Or Borrow Variables, Transferring Ownership If Needed</td>
    </tr>
    <tr>
        <td>Fixed Lifetime Determined By Their Scope</td>
        <td>May Capture Variables With Different Lifetimes</td>
    </tr>
</table>

## Using Closure as a Function Argument

You can also use closure as a function argument. For example,

```rust
fn apply_operation<F>(operation: F, a: i32, b: i32) -> i32
where
    F: Fn(i32, i32) -> i32,
{
    operation(a, b)
}

fn main() {
    let add = |x, y| x + y;
    let result_add = apply_operation(add, 5, 3);
    println!("Addition: {}", result_add); 
    // Output: Addition: 8

    let multiply = |x, y| x * y;
    let result_multiply = apply_operation(multiply, 5, 3);
    println!("Multiplication: {}", result_multiply); 
    // Output: Multiplication: 15
}
```

In this example:

- The `apply_operation` function takes three arguments: `operation`, `a`, and `b`. The `operation` argument is a closure that accepts two `i32` parameters and returns an `i32`. The `a` and `b` arguments are the operands for the operation.
- The `main` function demonstrates using `apply_operation` with two different closures: one for addition and another for multiplication.
- Both closures are defined inline using the `|x, y| ...` syntax.
- The `apply_operation` function is generic over the type `F`, which is constrained to be a closure that takes two `i32` parameters and returns an `i32`. This constraint is expressed using the `where F: Fn(i32, i32) -> i32` clause.

This example demonstrates the power of closures as function arguments in Rust. By accepting closures as parameters, functions gain the flexibility to perform different operations based on the behavior encapsulated within the closures. This approach promotes code reusability and enables developers to write more concise and expressive code.

---

For more information read [Rust book](https://doc.rust-lang.org/book/ch13-01-closures.html).