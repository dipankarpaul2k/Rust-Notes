<!-- omit in toc -->
# Data Types

- [Scalar Types](#scalar-types)
  - [Integer Types:](#integer-types)
  - [Floating-Point Types:](#floating-point-types)
  - [Boolean Type:](#boolean-type)
  - [Character Type:](#character-type)
- [Compound Types](#compound-types)
  - [Array Type:](#array-type)
  - [Tuple Type:](#tuple-type)
- [Some other types](#some-other-types)
  - [String Type:](#string-type)
  - [Slice Type:](#slice-type)
  - [Reference Types:](#reference-types)


Every value in Rust is of a certain data type, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: 

1. Scalar
2. Compound

Keep in mind that Rust is a statically typed language, which means that it must know the types of all variables at compile time. The compiler can usually infer what type we want to use based on the value and how we use it. 

## Scalar Types

A scalar type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. You may recognize these from other programming languages.

### Integer Types:

- An integer is a number without a fractional component.
- The default integer type is `i32`.
- Signed integers: `i8`, `i16`, `i32`, `i64`, `i128`, `isize`
- Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
- Each variant can be either signed(+/-) or unsigned(+) and has an explicit size.

Example:
```rust
let integer: i32 = 42;
let unsigned_integer: u64 = 100;
```

- Each signed variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n - 1</sup> - 1 inclusive, where n is the number of bits that variant uses.
- Unsigned variants can store numbers from 0 to 2<sup>n</sup> - 1.
- The `isize` and `usize` types depend on the architecture of the computer your program is running on.

### Floating-Point Types:

- `f32`: 32-bit floating point
- `f64`: 64-bit floating point, default.

Example:
```rust
let float: f64 = 3.14;
```
- Floating-point numbers are represented according to the IEEE-754 standard. The f32 type is a single-precision float, and f64 has double precision.

### Boolean Type:

- `bool`: Represents true or false values.
- Booleans are one byte in size.

Example:
```rust
let is_true: bool = true;
```

### Character Type:

- `char`: Represents a single Unicode character.
- We can also store special characters like `$`,`@` and `&`, etc. using the character type.

Example:
```rust
let character: char = 'A';
```

> Notably, wea can also store numbers as characters using single quotes. For example,
>
> ```rust
> let num_char: char = '5';
> ```
> Here `5` is a character not a number.

## Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

### Array Type:

- Fixed-size, homogeneous data structure.
- Every element of an array must have the same type.
- Arrays are useful when you want your data allocated on the stack rather than the heap or when you want to ensure you always have a fixed number of elements.  

Example:
```rust
let a: [i32; 3] = [1, 2, 3];

let first = a[0];  // Accessing array element
let second = a[1];
```
- Here, i32 is the type of each element. After the semicolon, the number 3 indicates the array contains three elements.
- You can also initialize an array to contain the same value for each element.
  
Example:
```rust
let numbers = [3; 5];

println!("numbers = {:?}", numbers);  // [3, 3, 3, 3, 3]
```

**Mutable Array in Rust**

In Rust, an array is immutable, which means we cannot change its elements once it is created.

However, we can create a mutable array by using the mut keyword before assigning it to a variable.

Example:
```rust
fn main() {
    let mut numbers: [i32; 5] = [1, 2, 3, 4, 5];
    
    println!("original array = {:?}", array); // [1, 2, 3, 4, 5]
    
    // change the value of the 3rd element in the array
    numbers[2] = 0;
    
    println!("changed array = {:?}", numbers); // [1, 2, 0, 4, 5]
}
```

We changed the element at index `2` (third element) from `3` to `0`. This is possible because we have created the numbers array as mutable.

> **Note:** Values inside an array can only be modified but cannot be deleted because the size of the array is fixed after initialization.

### Tuple Type:

- Tuples group together elements of different types into one compound type. 
- Fixed-size, heterogeneous data structure.
- Oonce declared, they cannot grow or shrink in size.
  
Example:
```rust
let tup: (i32, f64, &str) = (42, 3.14, "tuple");
```
- We can break down tuples into smaller variables, known as destructuring.

Example:
```rust
let (x, y, z) = tup;

println!("The value of y is: {y}");  // prints 3.14
```

> **Note:** Destructuring a tuple is also known as **tuple unpacking.**

- We can also access a tuple element directly by using a period (.) followed by the index of the value.

Example:
```rust
let first = tup.0;  // 42
let second = tup.1; // 3.14

println!("The value of y is: {y}");
```
The tuple without any values has a special name, unit. This value and its corresponding type are both written () and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.

**Mutable Tuple**

In Rust, a tuple is immutable by default, which means we cannot change its elements once it is created.

However, we can create a mutable array by using the mut keyword before assigning it to a variable.

Example:
```rust
fn main() {
    // initialize a mutable tuple
    let mut mountain_heights = ("Everest", 8848, "Fishtail", 6993);
    
    println!("Original tuple = {:?}", mountain_heights);
    // prints => ("Everest", 8848, "Fishtail", 6993)
    
    // change 3rd and 4th element of a mutable tuple
    mountain_heights.2 = "Lhotse";
    mountain_heights.3 = 8516;
    
    println!("Changed tuple = {:?}", mountain_heights);
    // prints => ("Everest", 8848, "Lhotse", 8516)
}
```

Here, we create a mutable tuple named mountain_heights. We then change its `2nd` and `3rd` tuple index.

> **Note:** You can only change the element of a tuple to the same type as when it was created. Changing data types is not allowed after tuple creation.

## Some other types

We wiil learn about them in detail later.

### String Type:

- Represents a sequence of characters.
- The default string type is a string slice `&str`.
- Strings can be either string slices or owned strings `String`. 
- String slices are often used for string literals, while `String` is used for dynamically allocated strings.
  
Example:
```rust
let string_slice: &str = "Hello, Rust!";
let owned_string: String = String::from("Rust is awesome!");
```   

### Slice Type:

- Represents a view into a portion of a contiguous sequence. 
- Slice must reference an existing array or slice.
- Slices are dynamically sized and provide a flexible way to work with arrays.

Example:
```rust
let array = [1, 2, 3, 4, 5];
let slice: &[i32] = &array[1..4]; // includes elements from index 1 to 3
   
// Accessing the elements of the slice
for num in slice {
   println!("Number: {}", num);
}   
```

### Reference Types:

- References allow you to borrow values without taking ownership. 
- Reference must be explicitly created by borrowing.
- `&T`: Immutable reference, used for read-only access.
- `&mut T`: Mutable reference, used for read-write access.

Example:
```rust
let x = 42;
let reference: &i32 = &x;
let mutable_reference: &mut i32 = &mut x;
```
- **Rules for reference:** 
    - Multiple immutable references to the same data are allowed.
    - Only one mutable reference to a piece of data is allowed within a specific scope.

Example:
```rust
let x = 10;
let mut y = 10;
    
// Creating an immutable reference to x
let reference: &i32 = &x;

// Creating a mutable reference to y
let mutable_reference: &mut i32 = &mut y;
 
// Modifying the value through the mutable reference
*mutable_reference += 5;
    
// Accessing the value through the reference
println!("Value through immutable reference: {}", *reference);  // 10
println!("Value through mutable reference: {}", y);  // 15
```
- The `*` operator is used to dereference the reference and access the underlying value.

These are the basic data types in Rust. Understanding and using these types effectively will help you write safe and expressive Rust code.

For more information read the [rust book](https://doc.rust-lang.org/book/ch03-02-data-types.html).