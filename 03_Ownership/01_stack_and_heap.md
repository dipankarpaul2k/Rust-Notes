<!-- omit in toc -->
# Stack and Heap

In Rust, the stack and heap are memory areas your code uses at runtime. While usually handled automatically, Rust, being memory-safe, requires some awareness of these. Concepts like *ownership*, *references*, and *borrowing* ensure memory safety. To grasp these, it's crucial to know how to allocate and deallocate memory in the stack and heap.

## Stack Memory

The Stack can be thought of as a stack of plates. When we add more plates, we add them on the top of the pile. When we need a plate, we take one from the top.

The stack inserts values in order. It gets them and removes the values in the opposite order.

- Adding data is called **pushing onto the stack**
- Removing data is called **popping off the stack**

This phenomenon is called **Last In, First Out (LIFO)** in programming.

Data stored on the stack must have a fixed size during compile time. Rust, by default, allocates memory on the stack for primitive types.

Let's visualize how memory is allocated and deallocated on the stack with an example.

Example:
```rust
fn main() {
    let x = 111;
    foo();
}

fn foo() {
    let y = 999;
    let z = 333;
}
```

When `main()` function executes, we allocate a single 32-bit integer (`x`) to the stack frame.

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 0           | x        | 111       |


In the table, the "Address" column refers to the memory address of the RAM. The "Name" column refers to the variable, and the "Value" column refers to the variable's value.

When `foo()` is called a new stack frame is allocated. The `foo()` function has two variable, `y` and `z`.

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 2           | z        | 333       |
| 1           | y        | 999       |
| 0           | x        | 111       |


> **Note:** The numbers 0, 1, and 2 are not actual memory addresses.

After `foo()` is completed, its stack frame is deallocated.

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 0           | x        | 111       |

Finally, `main()` is completed, and everything goes away.

Rust automatically does allocation and deallocation of memory in and out of the stack.

## Heap Memory

As opposed to the stack, most of the time, we will need to pass variables to different functions and keep them alive for longer than a single function's execution. This is when we can use the heap memory.

We can allocate memory on the heap using the `Box<T>` type.

Example:
```rust
fn main() {
    let x = Box::new(100);
    let y = 222;
    
    println!("x = {x}, y = {y}");
}
```

Here

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 1           | y        | 222       |
| 0           | x        | -->       |

Like before, we allocate two variables, `x` and `y`, on the stack. However, the value of `x` is allocated on the heap when `Box::new()` is called. Thus, the actual value of `x` is a `pointer` to the heap.

The memory now looks like this:

**Stack memory**

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 1           | y        | 222       |
| 0           | x        | -->5476   |

**Heap memory**

| **Address** | **Name** | **Value** |
| ----------- | -------- | --------- |
| 5476        |          | 100       |

Here, the variable `x` holds a pointer to the address `→5678` (an arbitrary address used for demonstration). Heap can be allocated and freed in any order. Thus it can end up with different addresses and create holes between addresses.

So when `x` goes away, it first frees the memory allocated on the heap.

Once the `main()` is completed, we free the stack frame, and everything goes away, freeing all the memory.

We can make the memory live longer by transferring ownership where the heap can stay alive longer than the function which allocates the `Box`.

## Differences between Stack and Heap

<table>
    <thead>
        <tr>
            <th>Stack</th>
            <th>Heap</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Accessing data in the stack is faster. </td>
            <td>Accessing data in a heap is slower.</td>
        </tr>
        <tr>
            <td>Managing memory in the stack is predictable and trivial.</td>
            <td>Managing memory for the heap (arbitrary size) is non-trivial.</td>
        </tr>
        <tr>
            <td>Rust stack allocates by default.</td>
            <td>Box is used to allocate to the heap.</td>
        </tr>
        <tr>
            <td>Primitive types and local variables of a function are allocated on the stack.</td>
            <td>Data types that are dynamic in size, such as String, Vector, Box, etc., are allocated on the heap.</td>
        </tr>
    </tbody>
</table>

---

For more information read the [rust book](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html).