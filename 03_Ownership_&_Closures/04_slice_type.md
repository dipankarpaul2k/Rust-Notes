# The Slice Type

A Rust slice is a data type used to access portions of data stored in collections like arrays, vectors and strings. **A slice is a kind of reference, so it does not have ownership.**

## Slices with Arrays:

```rust
fn main() {
    let my_array = [1, 2, 3, 4, 5];

    // Creating a slice from the array
    let my_slice = &my_array[1..4]; // Takes elements from index 1 to 3 (4 is exclusive)

    println!("Original Array: {:?}", my_array); // prints the original array
    println!("Slice: {:?}", my_slice); // prints [2, 3, 4]
}
```

In this example, `my_slice` is a reference to a portion of the `my_array`.

```rust
let my_slice = &my_array[1..4]; // Takes elements from index 1 to 3 (4 is exclusive)
}
```

## Omit Indexes of a Rust Slice

While slicing a data collection, Rust allows us to omit either the start index or the end index or both from its syntax.

```rust
// syntax
let slice = &var[start_index..end_index]; // start from `start_index` and goes up to `end_index`(exclusive)
let slice = &var[start_index..=end_index]; // start from `start_index` and goes up to `end_index`(inclusive)
```
**Omitting the Start Index of a Slice**

```rust
let slice = &var[..3];
```
This means the slice starts from index `0` and goes up to index `3` (exclusive). It is equivalent to &numbers[0..3].

**Omitting the End Index of a Slice**
```rust
let slice = &var[2..];
```
 This means the slice starts from index `2` and goes up to the end of collection.

**Omitting both Start and End Index of a Slice**
```rust
let slice = &var[..];
```
This means the slice starts from index `0` and goes up to the end of collection and will reference the whole collection.

## Mutable Slices

We can create a mutable slice by using the `&mut` keyword. Once the slice is marked as mutable, we can change values inside the slice.

Example:
```rust
fn main() {
    // mutable array
    let mut colors = ["red", "green", "yellow", "white"];
    
    println!("original array = {:?}", colors);

    // mutable slice
    let sliced_colors = &mut colors[1..3];
    
    println!("original slice = {:?}", sliced_colors);

    // change the value of the original slice at the first index
    sliced_colors[1] = "purple";

    println!("changed slice = {:?}", sliced_colors);
    println!("changed array = {:?}", colors);
}
```
Output:
```terminal
original array = ["red", "green", "yellow", "white"]
original slice = ["green", "yellow"]
changed slice = ["green", "purple"]
changed array = ["red", "green", "purple", "white"]
```

Here, we have created a mutable array `colors`. Then, we have created a mutable slice `sliced_colors` with `&mut array[1..3]`.

Now, we can change the content of the mutable slice by `sliced_colors[1] = "purple"`.

## Slices with Strings:

```rust
fn main() {
    let my_string = String::from("Hello, Rust!");

    // Creating a slice from the string
    let my_slice = &my_string[7..11]; // Takes characters from index 7 to 10 (11 is exclusive)

    println!("Original String: {}", my_string); // Prints the original string
    println!("Slice: {}", my_slice); // Rust
}
```

In this example, `my_slice` is a reference to a substring of `my_string`.

## Where Slices Should be Used:

1. **Avoiding Unnecessary Copying:**
   Slices are useful when you only need to work with a part of a collection without copying the data. This is more efficient than creating a new collection with the same elements.

2. **Passing Subsets of Data:**
   Slices are commonly used when passing parts of a collection to functions. Instead of passing the entire collection, you can pass a reference to a slice.

## Examples Demonstrating the Usefulness of Slices:


### Without Slices:

```rust
fn sum_elements(data: &[i32]) -> i32 {
    let mut sum = 0;
    for &num in data {
        sum += num;
    }
    sum
}

fn main() {
    let my_array = [1, 2, 3, 4, 5];

    // Unnecessarily passing the entire array
    let total = sum_elements(&my_array);

    println!("Total: {}", total);
}
```

### With Slices:

```rust
fn sum_elements(data: &[i32]) -> i32 {
    let mut sum = 0;
    for &num in data {
        sum += num;
    }
    sum
}

fn main() {
    let my_array = [1, 2, 3, 4, 5];

    // Passing only the required slice
    let total = sum_elements(&my_array[1..4]);

    println!("Total: {}", total);
}
```

In the first example without slices, the entire array is passed to the `sum_elements` function, which may be inefficient. In the second example with slices, only the relevant part of the array is passed, avoiding unnecessary copying and improving efficiency.

### Example 2: Sorting Part of an Array

```rust
fn main() {
    let mut numbers = [5, 2, 8, 1, 9, 3, 7];

    // Sort a portion of the array using a slice
    numbers[2..5].sort();

    println!("Sorted Array: {:?}", numbers);
}
```

This example demonstrates sorting a subset of an array using a mutable slice.

### Example 3: Splitting a String
```rust
fn main() {
    let data = "apple,orange,banana";

    // Split the string into parts using a slice
    let parts: Vec<&str> = data.split(',').collect();

    println!("Fruits: {:?}", parts);
}
```

Here, we split a string into parts using a slice, creating a vector of substrings.

In summary, slices are beneficial when working with subsets of data to avoid unnecessary copying and to pass portions of a collection to functions. They improve code readability, performance, and help prevent errors related to unnecessary data duplication.

