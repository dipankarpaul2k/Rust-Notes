# Ownership

Rust manages computer memory through **ownership rules** without memory leaks and runtime slowness. Unlike other languages with garbage collection or manual memory allocation, Rust's compiler enforces ownership rules. If violated, the program won't compile, ensuring efficient memory management.

## Variable Scope

A scope is an area within the code block for which a variable is valid. In Rust, the scope of a variable defines its ownership.

Example:
```rust
// `name` is invalid and cannot be used here 
// because it's outside the scope of code block{}
{ // code block starts here
    let name = String::from("hello rust!");   // `name` is valid from this point forward
    
    // do stuff with `name`
} // code block ends
// this scope ends, `name` is no longer valid and cannot be used
```

Here the variable `name` is only available inside the code block, i.e., between the curly braces `{}`. We cannot use the `name` variable outside the closing curly brace.

**Whenever a variable goes out of scope, its memory is freed.**

## Ownership Rules in Rust

Rust has some ownership rules. We must keep these rules in mind while working with Rust:

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

## Data Move

Sometimes, we might not want a variable to be dropped at the end of the scope. Instead, we want to transfer ownership of an item from one variable to another.

Here's an example to understand data movement and ownership rules in Rust.
```rust
fn main() {
    // rule no. 1 
    // String value has an owner variable `fruit1`
    let fruit1 = String::from("Banana");
    
    // rule no. 2
    // only one owner at a time
    // ownership moves to another variable `fruit2`
    let fruit2 = fruit1;
    
    // rule no. 3
    // error, out of scope, value is dropped
    println!("fruit1 = {}", fruit1); // prints → error
    // cannot print variable fruit1 because ownership has moved
    
    // print value of fruit2 on the screen
    println!("fruit2 = {}", fruit2); // prints → Banana
}
```

Here, `fruit1` was the owner of the String.

A `String` stores data both on the `stack` and the `heap`. This means that when we bind a `String` to a variable `fruit1`, the memory representation looks like this:

**Stack**
| Name     | Value |
|----------|-------|
| ptr      | -->   |
| length   | 6     |
| capacity | 6     |

**Heap**
| Index | Value |
|-------|-------|
| 0     | B     |
| 1     | a     |
| 2     | n     |
| 3     | a     |
| 4     | n     |
| 5     | a     |




[![](https://mermaid.ink/img/pako:eNoljj0LwkAQRP_KsZVCAtZXCIqFhTZJ6VksdxtzJPfBukEl5L97mqmG4TG8GWxyBBq6Mb1sjyzq0pioSg63VtAOKgvfVV3vj5szYVY-Onqr3XaFoIJAHNC78jH_NgPSUyADulSHPBgwcSkcTpLaT7SghSeqYMoOhU4eH4wBdIfjs6zkvCS-rlJ_t-ULqB01Cg?type=png)](https://mermaid.live/edit#pako:eNoljj0LwkAQRP_KsZVCAtZXCIqFhTZJ6VksdxtzJPfBukEl5L97mqmG4TG8GWxyBBq6Mb1sjyzq0pioSg63VtAOKgvfVV3vj5szYVY-Onqr3XaFoIJAHNC78jH_NgPSUyADulSHPBgwcSkcTpLaT7SghSeqYMoOhU4eH4wBdIfjs6zkvCS-rlJ_t-ULqB01Cg)

## Data Copy

Primitive types like Integers, Floats and Booleans don't follow the ownership rules. These types have a known size at compile time and are stored entirely on the stack, so copies of the actual values are quick to make.

Example:
```rust
fn main() {
    let x = 11;
    
    // copies data from x to y
    // ownership rules are not applied here 
    let y = x;

    println!("x = {x}, y = {y}"); // prints x = 11, y = 11
}
```

Here, `x` variable can be used afterward, unlike a `move` without worrying about ownership, even though `x` is assigned to `y`.

This copying is possible because of the `Copy` trait available in primitive types in Rust. When we assign `x` to `y`, a copy of the data is made.

> **Note:** A `trait` is a way to define shared behavior in Rust. We will discuss the traits later.

