# File Handling in Rust

File handling is commonly performed by many applications including databases, web servers. It is an important example of how I/O (Input/Output) operations work.

File handling is also generally known as File I/O.

In Rust, file handling involves interacting with files on the file system, including opening, reading from, writing to, removing, and appending to files. Rust's standard library provides the `std::fs` module for performing file operations.

## File Struct

In Rust, the `std::fs::File` struct represents a file. It allows us to perform read/write operations on a file.

The file I/O is performed through the `std::fs` module which provides functions for working with the file system.

All methods in the `File` struct return a variant of the `std:io::Result` or simply the `Result` enum.

## Opening a File

To open a file in Rust, you can use the `std::fs::File::open` method. This method takes a file path as an argument and returns a `Result` containing a `File` if successful or an error(`Err`) if the file could not be opened.

```rust
use std::fs::File;
use std::io::{self, Read};

fn main() -> io::Result<()> {
    let mut file = File::open("example.txt")?;
    Ok(())
}
```

## Reading from a File

Once you have opened a file, you can read from it using the `std::io::Read` trait. You can use methods like `read_to_string`, `read`, or `BufReader` for efficient reading.

```rust
use std::fs::File;
use std::io::{self, Read};

fn main() -> io::Result<()> {
    let mut file = File::open("example.txt")?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    println!("File contents: {}", contents);
    Ok(())
}
```

Here, once we open the file, we use `read_to_string()` method which takes an empty mutable string `contents` as an argument and copies the content of the file `example.txt` to `contents`.

## Writing to a File

To write to a file, you can use the `std::io::Write` trait. You first need to open the file in write mode and then use methods like `write`, `write_all` or `writeln` to write data to the file.

```rust
use std::fs::File;
use std::io::{self, Write};

fn main() -> io::Result<()> {
    let mut file = File::create("output.txt")?;
    file.write_all("Hello, world!".as_bytes())?;
    Ok(())
}
```

## Removing a File

To remove a file from the file system, you can use the `std::fs::remove_file` function. This function takes a file path as an argument and returns a `Result` indicating whether the file was successfully removed or not.

```rust
use std::fs;

fn main() -> std::io::Result<()> {
    fs::remove_file("example.txt")?;
    Ok(())
}
```

## Appending to a File

To append data to an existing file, you can open the file in append mode using the `std::fs::OpenOptions` struct and the `append` method. Then, you can use the `write_all` method in `std::io::Write` trait to append data to the file.

```rust
use std::fs::{self, OpenOptions};
use std::io::Write;

fn main() -> std::io::Result<()> {
    let mut file = OpenOptions::new().append(true).open("example.txt")?;
    file.write_all(b"Appending text to file")?;
    Ok(())
}
```

Here, `OpenOptions::new()` and the `append(true)` method opens the file `example.txt` for appending.

## Practical Project: Reading and Writing a Configuration File

Now, let's create a small project to demonstrate file handling in a practical scenario. We will write a program that reads a configuration file, modifies it, and then writes the updated configuration back to the file.

Create a `config.txt` file with the following content:

```
key1=value1
key2=value2
```

Then, create a Rust program to read and update this configuration file:

```rust
use std::collections::HashMap;
use std::fs::{self, File};
use std::io::{self, BufRead, BufReader, Write};

fn read_config_file(file_path: &str) -> io::Result<HashMap<String, String>> {
    let file = File::open(file_path)?;
    let reader = BufReader::new(file);
    let mut config_map = HashMap::new();

    for line in reader.lines() {
        let line = line?;
        let parts: Vec<&str> = line.split('=').collect();
        if parts.len() == 2 {
            config_map.insert(parts[0].trim().to_string(), parts[1].trim().to_string());
        }
    }

    Ok(config_map)
}

fn write_config_file(file_path: &str, config_map: &HashMap<String, String>) -> io::Result<()> {
    let mut file = File::create(file_path)?;
    for (key, value) in config_map.iter() {
        writeln!(&mut file, "{}={}", key, value)?;
    }
    Ok(())
}

fn main() -> io::Result<()> {
    let file_path = "config.txt";
    let mut config_map = read_config_file(file_path)?;

    // Modify the configuration
    config_map.insert("key3".to_string(), "value3".to_string());

    // Write the updated configuration back to the file
    write_config_file(file_path, &config_map)?;

    Ok(())
}
```

This program reads the `config.txt` file, adds a new key-value pair to the configuration, and then writes the updated configuration back to the file.