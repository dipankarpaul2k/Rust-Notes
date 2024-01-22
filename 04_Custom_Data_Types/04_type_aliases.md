# Type Aliases

In Rust, type aliases are created using the `type` keyword. They provide a way to create a new name for an existing type, making code more readable and expressive. **Type aliases do not introduce new types, they are simply alternative names for existing types.**

Here's the basic syntax for creating a type alias:

```rust
type NewTypeName = ExistingType;
```

Now, let's look at some examples and scenarios where type aliases can be useful:

## Simplifying Complex Types

```rust
type Point = (f64, f64);

fn distance(p1: Point, p2: Point) -> f64 {
    ((p2.0 - p1.0).powi(2) + (p2.1 - p1.1).powi(2)).sqrt()
}

fn main() {
    let point1: Point = (0.0, 0.0);
    let point2: Point = (3.0, 4.0);
    
    let dist = distance(point1, point2);
    println!("Distance: {}", dist);
}
```

In this example, a type alias `Point` is introduced to represent a 2D point using the existing tuple type `(f64, f64)`. The `distance` function calculates the Euclidean distance between two points, making the code more readable.

## Enhancing Code Readability

```rust
struct UserId(u32);
struct UserName(String);

type User = (UserId, UserName);

fn display_user(user: User) {
    let (id, name) = user;
    println!("User ID: {}, Name: {}", id.0, name.0);
}

fn main() {
    let user: User = (UserId(42), UserName("John Doe".to_string()));
    display_user(user);
}
```

A type alias `User` is defined as a tuple of `UserId` and `UserName`. This enhances code readability by providing a more descriptive name for the tuple, and the `display_user` function demonstrates how to work with the aliased type.

## Simplifying Generic Code

```rust
type Result<T> = std::result::Result<T, String>;

fn divide(a: i32, b: i32) -> Result<f64> {
    if b == 0 {
        Err("Cannot divide by zero".to_string())
    } else {
        Ok(a as f64 / b as f64)
    }
}

fn main() {
    match divide(10, 2) {
        Ok(result) => println!("Result: {}", result),
        Err(err) => println!("Error: {}", err),
    }
}
```

A type alias `Result<T>` is introduced to represent a result type with a generic value `T` and an error message as a `String`. This simplifies the code by providing a concise name for the result type, and the `divide` function uses this type alias to return a result of division.

## Type Alias for Struct

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

type Square = Rectangle;

fn calculate_area(rect: &Rectangle) -> u32 {
    rect.width * rect.height
}

fn main() {
    let square = Square { width: 5, height: 5 };
    println!("Area of the square: {}", calculate_area(&square));
}
```

In this example, we create a type alias `Square` for the `Rectangle` struct. The function `calculate_area` works with both `Rectangle` and `Square` types, providing flexibility and clarity.

## Type Alias for Enum

**Example 1:**
```rust
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

type Semaphore = TrafficLight;

fn display_semaphore_color(color: &Semaphore) {
    match color {
        Semaphore::Red => println!("Stop!"),
        Semaphore::Yellow => println!("Slow down."),
        Semaphore::Green => println!("Go!"),
    }
}

fn main() {
    let traffic_light = Semaphore::Green;
    display_semaphore_color(&traffic_light);
}
```

In this example, a type alias `Semaphore` is created for the `TrafficLight` enum. The `display_semaphore_color` function takes a `Semaphore` as an argument, allowing for more descriptive code.

**Example 2:**
```rust
enum FileState {
    CannotAccessFile,
    FileOpenedAndReady,
    NoSuchFileExists,
    SimilarFileNameInNextDirectory,
}

fn give_filestate(input: &FileState) {
    use FileState::{
        CannotAccessFile as NoAccess,
        FileOpenedAndReady as Good,
        NoSuchFileExists as NoFile,
        SimilarFileNameInNextDirectory as OtherDirectory
    };
    match input {
        NoAccess => println!("Can't access file."),
        Good => println!("Here is your file"),
        NoFile => println!("Sorry, there is no file by that name."),
        OtherDirectory => println!("Please check the other directory."),
    }
}

fn main() {}
```

We can also use `as` to change the name. So now you can write `OtherDirectory` instead of `FileState::SimilarFileNameInNextDirectory`.

> **Note:** Type aliases do not introduce new types, they are simply alternative names for existing types.

These examples showcase how type aliases in Rust can be utilized to improve code organization, readability, and expressiveness. They are particularly useful in scenarios where existing types or trait objects can benefit from a more descriptive name, leading to cleaner and more understandable code.