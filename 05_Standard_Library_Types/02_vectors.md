# Vectors in Rust

In Rust, collections are data structures that allow you to store and manipulate multiple values. The standard library provides several collection types, and one of them is a vector.

A vector is a dynamically resizable array, and it is part of the standard library. Vectors are implemented as a contiguous block of memory, and they can grow or shrink in size as needed. Vectors in Rust are represented by the `Vec<T>` type, where `T` is the type of elements the vector will contain.

## Creating a Vector

You can create a new vector in Rust using the `Vec::new()` constructor or using the `vec!` macro for initialization:

```rust
// Using Vec::new()
let mut v1: Vec<i32> = Vec::new(); // initialize with []

// Using vec! macro
let v2 = vec![1, 2, 3, 4, 5];

// Initializing a vector of zeros
let vec = vec![0; 5];
assert_eq!(vec, [0, 0, 0, 0, 0]);
```

**Printing vectors**

The `{:?}` format specifier is used with the `println!` macro to print the vector using the `Debug` trait. The `Debug` trait is automatically implemented for types if you derive `Debug` for them.

```rust
println!("{v1:?}");
println!("{v2:?}");
```

## Updating a Vector

You can add elements to a vector using the `push` method:

```rust
let mut v = Vec::new();
v.push(1);
v.push(2);
v.push(3);
```

## Reading Elements

You can access elements in a vector using indexing:

```rust
let v = vec![10, 20, 30, 40, 50];
let third_element = v[2];
println!("Third element: {}", third_element);
```

## Comparison between Arrays and Vectors

Arrays and vectors are both used for storing collections of elements in Rust, but they have some key differences in terms of size flexibility, ownership, and functionality.

### Arrays:

1. **Fixed Size:**
   - Arrays have a fixed size that is determined at compile time.
   - The size is part of the type signature, and it cannot be changed during runtime.

2. **Ownership:**
   - Arrays in Rust have a fixed size and are often allocated on the stack.
   - Ownership of an array is transferred entirely when it is moved or passed as a function argument.

3. **Mutability:**
   - The entire array is mutable or immutable; you cannot selectively make individual elements mutable.

4. **Usage:**
   - Arrays are typically used when the number of elements is known at compile time and will not change.

Example:
```rust
// Fixed-size array with type annotation
let array: [i32; 5] = [1, 2, 3, 4, 5];
```

> **Note:** An array of type [i32; 5] and [i32; 3] are different, lengths is a part of the array type annotation.

### Vectors:

1. **Dynamic Size:**
   - Vectors have a dynamic size and can grow or shrink during runtime.
   - The size is not part of the type signature and is determined at runtime.

2. **Ownership:**
   - Vectors are heap-allocated by default, and ownership can be moved or borrowed.
   - Vectors can be resized without changing the ownership characteristics.

3. **Mutability:**
   - Individual elements within a vector can be mutable, allowing for more fine-grained control.

4. **Usage:**
   - Vectors are used when the number of elements is not known at compile time or may change during program execution.

Example:
```rust
// Dynamic-size vector
let mut vector: Vec<i32> = vec![1, 2, 3, 4, 5];
vector.push(6);  // Adding an element
```

### Commonalities:

- Both arrays and vectors store elements of the same type.
- They support indexing to access individual elements(starting from index 0).
- They can be iterated over using loops or iterators.

### Decision Factors:

- Use arrays when the size is fixed and known at compile time.
- Use vectors when the size may change during runtime or is unknown at compile time.
- Vectors provide more flexibility but may have a slight performance overhead compared to arrays due to dynamic resizing.

In the end, the choice between arrays and vectors depends on the specific requirements of your program regarding size flexibility, ownership, and mutability.

## Methods on Vectors

These are the methods that are commonly implemented by Vectors.

### `append` method:

Appends elements from another vector(collection) to the end of a vector.

```rust
let mut vec = vec![1, 2, 3];
vec.append(&mut vec![4, 5]);
println!("{:?}", vec);  // Output: [1, 2, 3, 4, 5]
```

### `clear` method:

Removes all elements from a vector.

```rust
let mut vec = vec![1, 2, 3];
vec.clear();
println!("{:?}", vec);  // Output: []
```

### `contains` method:

The `contains` method checks if a specific element exists in the vector. It returns a boolean value indicating whether the element is present or not.

```rust
let vec = vec![1, 2, 3];
let contains_two = vec.contains(&2);

if contains_two {
    println!("Vector contains 2");
} else {
    println!("Vector does not contain 2");
}
```

### `drain` method:

Removes and yields a range of elements from the vector.

```rust
let mut vec = vec![1, 2, 3, 4, 5];
let drained: Vec<_> = vec.drain(1..4).collect();
println!("{:?}", drained);  // Output: [2, 3, 4]
```

> Collect: The `collect` method is used to transform an iterator into a collection. It allows you to collect the elements of an iterator into a variety of data structures, including vectors.

### `get` method:

The `get` method retrieves an element from a vector at a specified index, returning an `Option` that contains either a reference to the element or `None` if the index is out of bounds.

```rust
let vec = vec![1, 2, 3];
let element = vec.get(1);
   
match element {
    Some(value) => println!("Element at index 1: {}", value),
    None => println!("Index out of bounds"),
}
```

### `get_mut` method:

The `get_mut` method retrieves a mutable reference to an element in the vector at a specified index. It returns an `Option` that contains either the mutable reference or `None` if the index is out of bounds.

```rust
let mut vec = vec![1, 2, 3];

if let Some(mut_ref) = vec.get_mut(1) {
    *mut_ref = 10;
    println!("{:?}", vec);  // Output: [1, 10, 3]
} else {
    println!("Index out of bounds");
}
```

### `insert` method:

Inserts an element at a specified index in the vector.

```rust
let mut vec = vec![1, 2, 3];
vec.insert(1, 4);
println!("{:?}", vec);  // Output: [1, 4, 2, 3]
```

> **Note:** Inserting an element will shift all other values after specified index in the vector by one (+1 index).

### `is_empty` method:

Returns true if the vector is empty.

```rust
let vec = Vec::<i32>::new();
println!("{}", vec.is_empty());  // Output: true
```

### `iter` method:

Provides an iterator over the elements of the vector, returning an iterator that yields the reference to the elements of the vector.

```rust
let v = vec![1, 2, 3];
for i in v.iter() {
    println!("{}", i);
}
```

### `len` method:

Returns the number of elements in the vector.

```rust
let vec = vec![1, 2, 3];
println!("{}", vec.len());  // Output: 3
```

### `new` method:

Creates a new, empty vector.

```rust
let vec: Vec<i32> = Vec::new();
```

### `push` method:

Adds an element to the end of a vector.

```rust
let mut vec = vec![1, 2, 3];
vec.push(4);
println!("{:?}", vec);  // Output: [1, 2, 3, 4]
```

### `pop` method:

Removes and returns the last element from a vector.

```rust
let mut vec = vec![1, 2, 3];
let popped = vec.pop();
println!("{:?}", popped);  // Output: Some(3)
```

### `remove` method:

Removes the element at the specified index and returns it.

```rust
let mut vec = vec![1, 2, 3];
let removed = vec.remove(1);
println!("{:?}", removed);  // Output: 2
```

> **Note:** Removing an element will shift all other values after specified index in the vector by one (-1 index).

### `splice` method:

Removes a range of elements and replaces them with another collection.

```rust
let mut vec = vec![1, 2, 3, 4, 5];
let new_elements = vec![10, 11];
vec.splice(1..4, new_elements);
println!("{:?}", vec);  // Output: [1, 10, 11, 5]
```

### `split_off` method:

Splits the vector into two at the given index.

```rust
let mut vec = vec![1, 2, 3, 4, 5];
let new_vec = vec.split_off(2);
println!("{:?}", vec);       // Output: [1, 2]
println!("{:?}", new_vec);   // Output: [3, 4, 5]
```

### `truncate` method:

Shortens the vector to the specified length.

```rust
let mut vec = vec![1, 2, 3, 4, 5];
vec.truncate(2);
println!("{:?}", vec);  // Output: [1, 2]
```

> **Note:** Panic situations occur if you try to access an index that is out of bounds or if the vector is expected to have a certain capacity but doesn't. Always check the Rust documentation for specific details on panic situations for each method.

---

For more information read [Rust Book](https://doc.rust-lang.org/book/ch08-01-vectors.html).