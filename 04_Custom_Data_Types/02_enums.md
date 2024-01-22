# Enums in Rust

Enums, short for enumerations, are a feature in Rust that is useful for creating custom types to represent a finite set of related options or states. They are especially handy when you want to group related data and provide a clear, expressive way to work with it.

They look very similar to a `struct`, but are different. Here is the difference:

- Use a `struct` when you want one thing **AND** another thing.
- Use an `enum` when you want one thing **OR** another thing.

So structs are for **many things** together, while enums are for **many choices** together.

## Basic Enum Definition

To declare an enum, write `enum` and use a code block(`{}`) with the options, separated by commas. Just like a struct, the last part can have a comma or not.

Example:
```rust
// Define a simple enum named 'Color' with three variants
enum Color {
    Red,
    Green,
    Blue,
}
```

Here, `Color` is an enum with three variants: `Red`, `Green`, and `Blue`. Each variant is treated as a distinct value of the `Color` type.

## Using Enums to Group Data

Enums are a better way to group data when you have a set of related options that can be one of a limited number of choices. For example, if you were representing the status of an order, you could use an enum like this:

```rust
enum OrderStatus {
    Pending,
    Shipped,
    Delivered,
    Cancelled,
}
```

## Defining Enums with Data

You can associate data with each variant of an enum. This is useful when you want to attach additional information to each variant:

```rust
enum Shape {
    Circle(f64),    // Variant with a single f64 parameter
    Rectangle(f64, f64),  // Variant with two f64 parameters
    Square(f64),
}

fn calculate_area(shape: Shape) -> f64 {
    match shape {
        Shape::Circle(radius) => 3.14 * radius * radius,
        Shape::Rectangle(width, height) => width * height,
        Shape::Square(side) => side * side,
    }
}

fn main() {
    let circle = Shape::Circle(2.0);
    let rectangle = Shape::Rectangle(3.0, 4.0);
    let square = Shape::Square(2.5);

    println!("Area of Circle: {}", calculate_area(circle));
    println!("Area of Rectangle: {}", calculate_area(rectangle));
    println!("Area of Square: {}", calculate_area(square));
}
```

## Enums with Methods (Associated Functions)

You can define associated functions (methods) for enums using the `impl` keyword. Let's extend the `Color` enum to include an associated function:

```rust
enum Color {
    Red,
    Green,
    Blue,
}

impl Color {
    // Associated function for the Color enum
    fn description(&self) -> &str {
        match self {
            Color::Red => "This is the color red.",
            Color::Green => "This is the color green.",
            Color::Blue => "This is the color blue.",
        }
    }
}

fn main() {
    let red = Color::Red;
    let green = Color::Green;
    let blue = Color::Blue;

    println!("{}", red.description());
    println!("{}", green.description());
    println!("{}", blue.description());
}
```

In this example, the `description` method is associated with the `Color` enum, and it returns a string describing each color variant.

## Importing Enums with `*`

You can also **import** an enum so you don't have to type so much. For example, in the above code, we have to type `Color::` every time we use enum `Color`. Here we can use **import** so we can type less. To import everything, write `*`. 

> **Note:** it's the same key as `*` for dereferencing but is completely different.

Now the code will look like
```rust
enum Color {
    Red,
    Green,
    Blue,
}

use Color::*; // We imported everything in Color. Now we can just write Red, Green and Blue.

impl Color {
    // Associated function for the Color enum
    fn description(&self) -> &str {
        match self {
            Red => "This is the color red.", // We don't have to write Color:: anymore
            Green => "This is the color green.",
            Blue => "This is the color blue.",
        }
    }
}

fn main() {
    let red = Red;
    let green = Green;
    let blue = Blue;

    println!("{}", red.description());
    println!("{}", green.description());
    println!("{}", blue.description());
}
```

Here, the `use Color::*;` statement allows us to refer to the enum variants directly without prefixing them with `Color::`. This can make the code more concise and readable, especially when dealing with enums with numerous variants.

## Enum Variants as Integers

In Rust, each variant of an enum is assigned a numeric value, starting from 0 and incrementing by 1 for each subsequent variant. This can be useful in certain scenarios where you want to represent the enum as an integer or need to perform numerical operations based on the enum variants.

Example:
```rust
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

fn main() {
    // Enum variants as integers
    println!("Red: {}", TrafficLight::Red as u32); // Output: 0
    println!("Yellow: {}", TrafficLight::Yellow as u32); // Output: 1
    println!("Green: {}", TrafficLight::Green as u32); // Output: 2
}
```

In this example, we're explicitly casting the enum variants to integers using `as u32`. This can be handy, for instance, when you need to store the enum variant in a numeric format.

When working with enums, the `match` statement is commonly used to handle different variants. The integer representation of enum variants can be used in match arms as well:

Example:
```rust
enum Day {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday,
}

fn main() {
    let today = Day::Wednesday;

    match today as i32 {
        0..=4 => println!("It's a weekday!"),
        5..=6 => println!("It's the weekend!"),
        _ => println!("Invalid day"),
    }
}
```

In this example, we're using the integer representation of the `Day` enum variants in a match statement to distinguish between weekdays and weekends.

You can also assign numbers to the variants as you wish using the `=` operator. You don't have to give all of them a number. But if you don't, Rust will just add 1 from the previous arm before to give it a number.

Example:
```rust
enum Star {
    BrownDwarf = 10,
    RedDwarf = 50,
    YellowStar = 100,
    RedGiant = 1000,
    DeadStar, // It will be 1001 automatically
}
```

## When to Use Enums

Enums in Rust are a versatile tool that can be employed in various scenarios to improve code structure, expressiveness, and type safety. Here are several situations where using enums is particularly beneficial:

1. **Representing a Finite Set of Options:**
   - Enums are ideal when you have a fixed and limited set of related options or states. This makes the code more readable and self-documenting.

2. **Pattern Matching and Control Flow:**
   - Enums work well with pattern matching (`match` statements), providing a concise and expressive way to handle different cases or states.

3. **Error Handling:**
   - Enums can be used to define custom error types, allowing for more informative and structured error handling. This is especially useful when dealing with multiple potential failure scenarios.

4. **Grouping Related Data:**
   - Enums with associated data allow you to group related pieces of information together. This is useful when you need to represent multiple variations of a concept.

5. **Defining State Machines:**
   - Enums are valuable for modeling state machines where an object can be in one of several states, and transitions between states are well-defined.

6. **Replacing Booleans with More Descriptive Types:**
   - Instead of using a boolean to represent a binary choice, consider using an enum with two variants. This provides better readability and extensibility.

7. **Enhancing Type Safety:**
   - Enums contribute to type safety by clearly specifying the possible values a variable can take. This helps catch errors at compile time rather than runtime.

8.  **Extension through Associated Functions and Methods:**
    - Enums can have associated functions and methods, allowing you to encapsulate behavior related to the enum. This promotes encapsulation and code organization.

In summary, enums in Rust are a powerful tool for defining custom types that can represent a set of related options. They allow you to group data, attach information to each variant, and define associated functions (methods) for more functionality. Enums are particularly useful in situations where you have a well-defined set of choices or states.