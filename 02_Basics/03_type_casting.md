# Type Casting

Type casting allows us to convert variables of one data type to another. In Rust, we use the `as` keyword to perform type casting.

## Floating point to integer

Example:
```rust
fn main() {
    // assign a floating point f64 value to decimal variable
    let decimal: f32 = 64.31;

    // convert decimal variable to u16 integer type using as keyword
    let integer = decimal as u16;

    println!("decimal = {}", decimal);
    println!("integer = {}", integer);
}
```

Here, decimal as u16; expression converts f64 floating-point type to u16 integer type.

We are converting data from one type to another type manually using the `as` keyword. This type of type casting is also known as Explicit Type Casting.

## Character to integer

Example:
```rust
fn main() {
    let character: char = 'A';

    // convert char type to u8 integer type
    let integer = char as u8;

    println!("character = {}", character);
    println!("integer = {}", integer);
}
```

In Rust, the `char` data type is internally stored as a Unicode Scalar Value. Unicode Scalar Value is simply the numeric representation of a character in Unicode standard also known as a code point.

The Unicode value of character `A` is `65`. So, we get the output of `65` when converting the character `A` to an integer.

## Integer to character

We can also convert integer type to a character type.

Example:
```rust
fn main() {
    // only u8 integer data type can be converted into char
    let integer: u8 = 65;
  
    // convert integer to char using the as keyword
    let character = integer as char;

    println!("integer = {}" , integer);
    println!("character = {}", character);
}
```

In the example above, the integer value `65` is the Unicode code for character `A`. Thus after type casting, we get character `A` as the output. Every character has an Unicode code associated with it.

## Error while converting integer to character

We are only allowed to use `u8` integers while performing type casting between integer and character. If we use any other integer type and convert it to a character, we will get an error. For example,

Example:
```rust
fn main() {
    let integer: i32 = 65;
  
    // convert integer to char using the as keyword
    let character = integer as char;  //! invalid cast
    // only `u8` can be cast as `char`, not `i32`

    println!("integer = {}" , integer);
    println!("character = {}", character);
}
```

Here, we have used `i32` data type instead of `u8`. Hence we get an error.

It's because Unicode Scalar Values are small integer numbers and fit in the range of `u8` data type.

## Boolean to integer

Example:
```rust
fn main() {
    let boolean1: bool = false;
    let boolean2: bool = true;
  
    // convert boolean type to integer
    let integer1 = boolean1 as i32;
    let integer2 = boolean2 as i32;

    println!("boolean1 = {}", boolean1);
    println!("boolean1 = {}", boolean2);
    println!("integer1 = {}", integer1);
    println!("integer2 = {}", integer2);
}
```

Here, boolean data type `false` and `true` are converted to integer `0` and `1` respectively.

## Limitations of Type Casting
There are limitations while performing type casting in Rust. Not all data types are converted to one another.

For example, we cannot convert a floating type to a character.

```rust
fn main() {
    let decimal: f32 = 65.321;
  
    // convert float to char data type
    let character = decimal as char;  //! invalid cast
    //! only `u8`` can be cast as `char`, not `f32`

    println!("decimal = {}", decimal);
    println!("character = {}", character);
}
```

Here, we have tried to convert the `float` type to `char`, hence we get an error. The error says that Rust is expecting a `u8` data type for conversion not `f32`.
