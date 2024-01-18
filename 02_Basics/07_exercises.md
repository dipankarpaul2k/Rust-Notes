# Exercises

Here we will solve some problems with the concepts that we have learned as of now.

## Easy:

<details>
<summary>Write a Rust program that prints "Hello, Rust!" to the console.</summary>

```rust
fn main() {
    println!("Hello, Rust!");
}
```
</details>

<details>
<summary>Create a function that takes an integer as input and prints whether it's even or odd.</summary>

```rust
use std::io;

fn main() {
    println!("Enter a number:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let number: i32 = input.trim().parse().expect("Please enter a number");

    even_or_odd(number);
}

fn even_or_odd(number: i32) {
    if number % 2 == 0 {
        println!("Even");
    } else {
        println!("Odd");
    }
}
```
</details>

<details>
<summary>Write a function that calculates and returns the sum of squares of numbers from 1 to n.</summary>

```rust
use std::io;

fn main() {
    println!("Enter a number:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let n: u32 = input.trim().parse().expect("Please enter a non-negative number");

    let result = sum_of_squares(n);
    println!("Sum of squares: {}", result);
}

fn sum_of_squares(n: u32) -> u32 {
    (1..=n).map(|x| x * x).sum()
}
```
</details>

<details>
<summary>Implement a function to calculate the factorial of a given number.</summary>

```rust
use std::io;

fn main() {
    println!("Enter a non-negative  number:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let n: u32 = input.trim().parse().expect("Please enter a non-negative number");

    let result = factorial(n);
    println!("Factorial: {}", result);
}

fn factorial(n: u32) -> u32 {
    (1..=n).product()
}
```
</details>

<details>
<summary>Create a function that reverses a given string.</summary>

```rust
use std::io;

fn main() {
    println!("Enter a string:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");

    let reversed = reverse_string(input.trim());
    println!("Reversed string: {}", reversed);
}

fn reverse_string(s: &str) -> String {
    s.chars().rev().collect()
}
```
</details>

## Medium:

<details>
<summary>Write a function to check if a given string is a palindrome.</summary>

```rust
use std::io;

fn main() {
    println!("Enter a string:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");

    if is_palindrome(input.trim()) {
        println!("Palindrome");
    } else {
        println!("Not a palindrome");
    }
}

fn is_palindrome(s: &str) -> bool {
    s.chars().eq(s.chars().rev())
}
```
</details>

<details>
<summary>Implement a function to generate the first n numbers in the Fibonacci sequence.</summary>

```rust
use std::io;

fn main() {
    println!("Enter the number of Fibonacci sequence elements:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let n: u32 = input.trim().parse().expect("Please enter a non-negative number");

    let sequence = fibonacci(n);
    println!("Fibonacci sequence: {:?}", sequence);
}

fn fibonacci(n: u32) -> Vec<u32> {
    let mut result = Vec::new();
    let mut a = 0;
    let mut b = 1;

    for _ in 0..n {
        result.push(a);
        let temp = a + b;
        a = b;
        b = temp;
    }

    result
}
```
</details>

<details>
<summary>Write a program that prints all prime numbers up to a given limit.</summary>

```rust
use std::io;

fn main() {
    println!("Enter the upper limit:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let limit: u32 = input.trim().parse().expect("Please enter a non-negative number");

    println!("Prime numbers up to {}: {:?}", limit, find_primes(limit));
}

fn find_primes(limit: u32) -> Vec<u32> {
    let mut primes = Vec::new();
    for num in 2..=limit {
        if is_prime(num) {
            primes.push(num);
        }
    }
    primes
}

fn is_prime(num: u32) -> bool {
    if num < 2 {
        return false;
    }
    for i in 2..=((num as f64).sqrt() as u32) {
        if num % i == 0 {
            return false;
        }
    }
    true
}
```
</details>

<details>
<summary>Convert temperatures between Fahrenheit and Celsius.</summary>

```rust
use std::io;

fn main() {
    println!("Temperature Converter");

    // Get user input for temperature value
    let mut temperature = String::new();
    println!("Enter temperature value:");
    io::stdin().read_line(&mut temperature).expect("Failed to read line");

    // Convert user input to a floating-point number
    let temperature: f64 = match temperature.trim().parse() {
        Ok(num) => num,
        Err(_) => {
            println!("Invalid input. Please enter a valid number.");
            return;
        }
    };

    // Get user input for temperature unit
    let mut unit = String::new();
    println!("Enter temperature unit (F for Fahrenheit, C for Celsius):");
    io::stdin().read_line(&mut unit).expect("Failed to read line");

    // Convert temperature based on user input
    let converted_temperature = match unit.trim().to_ascii_lowercase().as_str() {
        "f" => (temperature - 32.0) * (5.0/9.0), // Fahrenheit to Celsius
        "c" => (temperature * (9.0/5.0)) + 32.0, // Celsius to Fahrenheit
        _ => {
            println!("Invalid unit. Please enter 'F' or 'C'.");
            return;
        }
    };

    // Display the converted temperature
    println!("Converted Temperature: {:.2}", converted_temperature);
}
```
</details>

<details>
<summary>Generate the nth Fibonacci number.</summary>

```rust
use std::io;

fn fibonacci(n: u32) -> u64 {
    if n == 0 {
        return 0;
    } else if n == 1 {
        return 1;
    } else {
        let mut prev = 0;
        let mut current = 1;

        for _ in 2..=n {
            let temp = current;
            current += prev;
            prev = temp;
        }

        return current;
    }
}

fn main() {
    println!("Fibonacci Number Generator");

    // Get user input for the nth Fibonacci number
    println!("Enter the value of n:");
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");

    // Convert user input to a positive integer
    let n: u32 = match input.trim().parse() {
        Ok(num) => num,
        Err(_) => {
            println!("Invalid input. Please enter a valid positive integer.");
            return;
        }
    };

    // Generate the nth Fibonacci number
    let result = fibonacci(n);

    // Display the result
    println!("The {}th Fibonacci number is: {}", n, result);
}
```
</details>
