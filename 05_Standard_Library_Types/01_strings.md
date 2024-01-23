# Strings in Rust

We’ll first define what we mean by the term *string*. **A string is a collection of characters.** Rust has only one string type in the core language, which is the string literals `str` that is usually seen in its borrowed form `&str`, which are references to some UTF-8 encoded string data stored in the program’s binary and are therefore also *string slices*. And because it is a string slice, `str` doesn't own the string data, it is an immutable view into a sequence of UTF-8 bytes.

The `String` type, which is provided by Rust’s standard library rather than coded into the core language, is a growable, mutable, owned, heap-allocated UTF-8 encoded string type. When Rustaceans refer to **“strings”** in Rust, they might be referring to either the `String` or the string slice `&str` types, not just one of those types. Both String and string slices are UTF-8 encoded.

## String

A `String` is a heap-allocated, mutable, UTF-8 encoded string. You can create a `String` from a string literal or from another `String` using the `to_string` method.

Example:
```rust
let s1: String = "Hello, ".to_string();
let s2: String = String::from("world!");
let s3: String = String::new(); // Dynamic empty string
let combined = s1 + &s2;
// Note: s1 has been moved here and can no longer be used

println!("{}", combined); // Prints: Hello, world!
```

One other way to make a `String` is called `.into()` but it is a bit different because `.into()` isn't just for making a `String`.

Example:
```rust
fn main() {
    let s4 = "Try to make this a String".into(); 
    // ⚠️type annotations needed, consider giving `s4` a type

    let s5: String = "Try to make this a String".into();
    // ✅ Now compiler is happy with `String` type
}
```

## str (String Slice)

A `str` is an immutable reference to a sequence of UTF-8 bytes. It is usually seen as a borrowed form of a `String` or a string literal.

Example:
```rust
let s: &str = "Hello, world!";
let slice: &str = &s[0..5]; // Take a slice from index 0 to 5 (exclusive)

println!("{}", slice); // Prints: Hello
```

You can even write emojis, thanks to UTF-8.

Example:
```rust
fn main() {
    let name = "😂";
    println!("My name is actually {}", name);
    // prints: My name is actually 😂
}
```

## Transforming String to str and vice versa

You can convert a `String` to a `&str` by using the `as_str` method or by using the `&` operator:

Example:
```rust
let my_string: String = String::from("Hello");
let my_str1: &str = my_string.as_str();
let my_str2: &str = &my_string;

println!("{}", my_str1); // Prints: Hello
println!("{}", my_str2); // Prints: Hello
```

Converting a `&str` to `String` can be done using the `to_string` method:

Example:
```rust
let my_str: &str = "World!";
let my_string: String = my_str.to_string();

println!("{}", my_string); // Prints: World!
```

## Concatenate Two Strings

In Rust, there are several ways to concatenate two strings. The choice of method depends on the ownership and borrowing rules of Rust and the specific use case. Here are some common methods:

### Using the `+` Operator

Example:
```rust
fn main() {
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");

    let combined = s1 + &s2;
    // s1 has been moved, and can't be used afterwards

    println!("{}", combined); // Prints: Hello, world!
}
```

Here, the `+` operator is used to concatenate two strings. However, it has a constraint: it consumes ownership of the first string (`s1` in this case). This means you can't use `s1` after the concatenation, as it has been moved.

### Using the `format!` Macro

Example:
```rust
fn main() {
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");

    let combined = format!("{}{}", s1, s2);

    println!("{}", combined); // Prints: Hello, world!
    // Both s1 and s2 are still usable
}
```

The `format!` macro allows you to concatenate strings without taking ownership of any of them. It returns a new `String` with the concatenated content.

### Using the `String::push_str` Method

```rust
fn main() {
    let mut s1 = String::from("Hello, ");
    let s2 = String::from("world!");

    s1.push_str(&s2);

    println!("{}", s1); // Prints: Hello, world!
    // s2 is still usable
}
```

With the `push_str` method, you can append a string slice (`&str`) to a `String` without taking ownership of the second string.

### Using the `String::push` Method

```rust
fn main() {
    let mut s1 = String::from("Hello,");
    let s2 = String::from("world!");

    s1.push(' '); // Adding a space(' ') Char type
    s1.push_str(&s2);

    println!("{}", s1); // Prints: Hello, world!
    println!("{}", s2); // Prints: world!
    // s2 is still usable
}
```

The `push` method can be used to append a single character or string slice to a `String`.

### Using the `&` Operator

```rust
fn main() {
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");

    let combined = s1.clone() + &s2;
    // Clone is used to avoid ownership issues

    println!("{}", combined); // Prints: Hello, world!
    // Both s1 and s2 are still usable
}
```

The `&` operator can be used to borrow a reference to a `String`, allowing you to concatenate without consuming ownership. However, you may need to use `clone()` on the first string to avoid ownership issues.

### Considerations:

- When concatenating strings, be aware of ownership and borrowing rules to avoid issues like moving ownership unintentionally.
- If you need to concatenate strings in a loop or multiple times, consider using a `String` and appending with `push_str` for better performance.

Choose the method that best fits your specific use case and consider factors such as ownership, performance, and readability. Each method has its own set of rules and considerations based on Rust's ownership model.

## Methods on String and str

Both `String` and `str` have a variety of methods for manipulating and working with text. The methods that are available on `str` (string slice) and `String` (heap-allocated string) are not exactly the same, even though they share some common methods. The reason for this difference is that `str` is an immutable view into a sequence of UTF-8 bytes, while `String` is a growable, mutable, heap-allocated UTF-8 encoded string. Some methods include:

**String Methods:**
- `push_str`: Appends a string slice to the end of the `String`.
- `replace`: Replaces a substring with another substring.
- `to_uppercase`/`to_lowercase`: Converts the string to uppercase or lowercase.

Example:
```rust
let mut my_string = String::from("Hello, world!");
my_string.push_str(" How are you?");
my_string = my_string.replace("world", "Rust");

println!("{}", my_string.to_uppercase()); 
// Prints: HELLO, RUST! HOW ARE YOU?
```

**str Methods:**
- `len`: Returns the length of the string.
- `is_empty`: Returns true if the string is empty.
- `starts_with`/`ends_with`: Checks if the string starts or ends with a given substring.

> **Note:** This methods are also available for `String` type.

Example:
```rust
let my_str: &str = "Hello, Rust!";
let my_string: String = String::from("Hello, Rust!");

println!("str Length: {}", my_str.len());
println!("String Length: {}", my_string.len());

println!("str Is empty: {}", my_str.is_empty());
println!("String Is empty: {}", my_string.is_empty());

println!("str Starts with 'Hello': {}", my_str.starts_with("Hello"));
println!("String Starts with 'Hello': {}", my_string.starts_with("Hello"));
```

Output:
```
str Length: 12
String Length: 12
str Is empty: false
String Is empty: false
str Starts with 'Hello': true
String Starts with 'Hello': true
```

While some methods are shared, you need to be aware of the differences between `str` and `String` when choosing and using methods, especially those that involve modifications or ownership changes.

In summary, remember that `String` is mutable and can be modified, while `str` is immutable. Always be aware of ownership and borrowing rules in Rust to avoid ownership issues and lifetime errors.

---

For more information read [Rust Book](https://doc.rust-lang.org/book/ch08-02-strings.html).

