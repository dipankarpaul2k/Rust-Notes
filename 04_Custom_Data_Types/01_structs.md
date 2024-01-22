# Structs in Rust

In Rust, a **struct** (short for *structure*) is a composite data type that allows you to group together variables of different data types under a single name. Structs are useful for organizing and representing data in a more structured manner.

With structs, you can create your own type. You will use structs all the time in Rust because they are so convenient. Structs are created with the keyword `struct`. The name of a struct should be in **PascalCase** -- it is a programming naming convention where the first letter of each compound word in a variable is capitalized and there is no gap between words. If you write a struct in all lowercase, the compiler will yell you.

Generally there are three kinds of structs, 

## Unit Struct

First is a **unit struct**. Unit means "doesn't have anything". For a unit struct, you just write the name and a semicolon(`;`).

```rust
struct FileDirectory;
fn main() {}
```
Unit structs can be useful when you need to implement a trait on some type but don’t have any data that you want to store in the type itself.

## Tuple Struct

The second is a **tuple struct**, or an unnamed struct. It is "unnamed" because you only need to write the types, not the field names. Tuple structs are good when you need a simple struct and don't need to remember names.

```rust
struct RgbColour(u8, u8, u8);

fn main() {
    // Make a colour out of RGB (red, green, blue)
    let my_colour = RgbColour(50, 0, 50); 
}
```

## Named Struct

The third type is the **named struct**. This is probably the most common struct. In this struct you declare field names and types inside a `{}` code block. Note that you don't write a semicolon(`;`) after a named struct.

```rust
// Define a named struct Point
struct Point {
    x: i32,
    y: i32,
}
```

In this example, `Point` is a struct with two fields, `x` and `y`, both of type `i32`. You separate fields by commas(`,`) in a named struct. For the last field you can add a comma or not - it's up to you. 

## Instantiating a Struct
```rust
// Create an instance of the Point struct
let origin = Point { x: 0, y: 0 };
```

Here, we have initialized the values of the x and y fields of the Point struct. This process of initializing the values of struct fields is known as an **instantiation** of a struct.

> **Note:** Think of struct definition as a template, and the struct instances fill in that template with data.

## Accessing Struct Fields

**If its a tuple struct:**
```rust
println!("The second part of the colour is: {}", my_colour.1); // prints: 0
```
You can access the fields of the tuple struct by using the index of the fields.

**If its a named struct:**
```rust
println!("x: {}, y: {}", origin.x, origin.y);
```
You can access the fields of the named struct by using the name of the fields.

**Destructuring Fields of a Struct:**

Destructuring is the process of breaking down fields of a data type (array, tuple, etc.) into smaller variables. We can break down the struct fields into smaller variables as well.

```rust
// destructuring the origin, an instance of the Point struct
let Point { x , y } = origin;
```
On the left side, we are making `let` declarations for the `Point` struct with field `x` and `y`. On the right side, we assign the instantiated struct of the `Point`, `origin`.

Now, we access the `x` and `y` fields using the field names directly.
- `x` instead of `origin.x`
- `y` instead of `origin.y`

However, you should note that the name of the variables while destructuring should be the same as the name of the fields.

## Mutable Struct

To create a mutable struct, we use the `mut` keyword while declaring the struct variable.

Example:
```rust
// instantiate Point struct to a mutable structure variable
let mut origin2 = Point { x: 0, y: 0 };

// change the value of x field in mutable origin2 struct
origin2.x = 5;
```

## Struct Update Syntax

It’s often useful to create a new instance of a struct that includes most of the values from another instance, but changes some. You can do this using **struct update syntax.**

Example:
```rust
struct User {
    active: bool,
    username: String,
    email: String,
    login_count: u64,
}

fn main() {
    let user1 = User {
        active: true,
        username: String::from("someuser"),
        email: String::from("user1@example.com"),
        login_count: 1,
    };

    let user2 = User {
        email: String::from("user2@example.com"),
        ..user1
    };
}
```
The syntax `..` specifies that the remaining fields not explicitly set should have the same value as the fields in the given instance.

In this example, the `..user1` part in `user2` variable means to use the remaining fields values from `user1` for the new instance `user2`.

## Structs vs Tuples

While tuples are also used for grouping data, there are key differences:

- **Named Fields**: Structs have named fields, making it more self-documenting.
- **Field Access**: Struct fields are accessed by name, whereas tuples use positional indexing.

## Associated Functions and Methods

### Associated Functions

Associated functions are functions that are associated with a struct but do not take an instance of the struct type (`self`) as their first parameter. They are often used for creating instances of the type. Associated functions are defined using `impl` blocks.

Example:
```rust
impl Point {
    // Associated function to create a Point from coordinates
    fn new(x: i32, y: i32) -> Point {
        Point { x, y }
    }
}
// Usage of the associated function
let point = Point::new(3, 4);
```

> **Note:** `impl` in Rust is short for **"implementation"** and is used to define implementations for traits, associated functions, and methods for a type. It allows you to specify behavior associated with a particular type.
> 

### Methods

Methods are functions associated with a struct that take `self` (an instance of the struct) as their first parameter. Methods are used to define behavior specific to instances of the type. They are also defined using `impl` blocks.

Example:
```rust
impl Point {
    // Method to calculate the distance from the origin
    fn distance_from_origin(&self) -> f64 {
        ((self.x.pow(2) + self.y.pow(2)) as f64).sqrt()
    }
}
// Usage of the method
let distance = point.distance_from_origin();
```

**Full Example:**

```rust
// Define a struct named Point
struct Point {
    x: i32,
    y: i32,
}

// Implement associated functions and methods for the Point struct
impl Point {
    // Associated function to create a Point from coordinates
    fn new(x: i32, y: i32) -> Point {
        Point { x, y }
    }

    // Method to calculate the distance from the origin
    fn distance_from_origin(&self) -> f64 {
        ((self.x.pow(2) + self.y.pow(2)) as f64).sqrt()
    }
}

fn main() {
    // Create an instance of the Point struct
    let origin = Point { x: 0, y: 0 };

    // Accessing individual fields of a struct
    println!("x: {}, y: {}", origin.x, origin.y);

    // Create an instance using the associated function
    let point = Point::new(3, 4);

    // Use the method to calculate the distance from the origin
    let distance = point.distance_from_origin();

    println!("Distance from the origin: {}", distance);
}
```

### Methods with Multiple Parameters

In Rust, methods for structs can have multiple parameters, just like regular functions. The first parameter of a method is always `self`(either immutable or mutable), which represents an instance of the struct. Additional parameters can be added as needed.

Example:
```rust
struct Rectangle {
    width: f64,
    height: f64,
}

impl Rectangle {
    // Asssociate function to create a square
    fn square(size: f64) -> Self {
        Self {
            width: size,
            height: size,
        }
    }

    // Method with multiple parameters
    fn resize(&mut self, new_width: f64, new_height: f64) {
        self.width = new_width;
        self.height = new_height;
    }
}

fn main() {
    // Create an instance of the Rectangle struct
    let mut rect = Rectangle { width: 10.0, height: 20.0 };
    // Create an square with the Rectangle struct
    let square = Rectangle::square(10.0);

    // Print the original values
    println!("Original Rectangle: width - {}, height - {}", rect.width, rect.height);

    // Call the method with multiple parameters to resize the rectangle
    rect.resize(15.0, 25.0);

    // Print the updated values
    println!("Resized Rectangle: width - {}, height - {}", rect.width, rect.height);
}
```

When calling a method with multiple parameters, you pass the additional parameters in the method call. The `self` parameter is implicitly passed by Rust when you call the method on an instance of the struct.

Remember to use `&self` for methods that only read the instance and `&mut self` for methods that modify the instance. In the example, the `resize` method modifies the instance, so it takes `&mut self`.

## When to use Structs

Structs are a better choice when:

Structs (short for structures) in Rust are a fundamental building block for creating custom data types. They are useful in various scenarios where you need to organize and encapsulate data. Here are several situations where using structs is particularly beneficial:

1. **Grouping Related Data:**
   - Structs are ideal when you want to group together related pieces of data under a single name. This helps in organizing and maintaining a clean structure in your code.

2. **Encapsulation and Abstraction:**
   - Structs allow you to encapsulate the internal details of a data structure and expose a well-defined interface. This abstraction helps in managing complexity and isolating implementation details.

3. **Representing Real-World Entities:**
   - Use structs to model real-world entities or concepts in your program. For example, if you are building a game, you might have structs representing characters, items, or locations.

4. **Custom Types with Named Fields:**
   - Structs provide a way to create custom types with named fields, making the code more self-explanatory and reducing the likelihood of errors caused by the misuse of data.

5. **Immutability and Borrowing:**
   - Structs can be defined as immutable, allowing you to create instances that cannot be modified after creation. This supports a more functional programming style and helps prevent unintended changes.

6. **Initialization with Named Fields:**
   - Structs enable you to initialize data using named fields, which makes the code more readable and self-documenting. This is especially useful when dealing with structs that have many fields.

7. **Pattern Matching and Destructuring:**
   - Structs can be used with pattern matching to destructure and extract values. This is valuable when working with complex data structures and handling different cases.

8. **Default Values and Builders:**
   - Structs can have default values for their fields, and builders can be implemented to facilitate the creation of instances with a subset of fields initialized.

9. **Tuple Structs for Lightweight Types:**
   - Tuple structs are a variant of structs that have unnamed fields, providing a lightweight way to create custom types without naming each field. They are useful for simple scenarios where the fields have a clear order.

10. **Ownership and Lifetimes:**
    - Structs play a crucial role in managing ownership and lifetimes in Rust. They allow you to define data ownership relationships and ensure memory safety through the borrowing system.

11. **Implementation of Methods:**
    - Structs can have methods associated with them, allowing you to define behavior that is specific to instances of the struct. This promotes encapsulation and code organization.


In summary, Structs in Rust allow you to create custom types tailored to your needs. They enable you to organize related data together, providing clarity to your code. Using `impl` blocks, you can define functions and methods associated with these types, allowing you to specify the behavior of instances of your structs.