# HashMaps in Rust

In Rust, you can use the `std::collections::HashMap` module to work with hash maps. The Rust HashMap data structure allows us to store data in **key-value pairs**. Here are some of the features of hashmap:

- Each value is associated with a corresponding key.
- Keys are unique, whereas values can duplicate.
- Values can be accessed using their corresponding keys.

## Creating a HashMap

```rust
use std::collections::HashMap; // This is so we can just write HashMap instead of std::collections::HashMap every time
```
This line imports the `HashMap` data structure from the `std::collections` module, making it available for use in your Rust program.

```rust
let mut my_map = HashMap::new();
```
This line creates a new, mutable empty HashMap called `my_map`. The `mut` keyword is used to allow modifications to the HashMap.

## Adding Elements

```rust
my_map.insert("key1", "value1");
my_map.insert("key2", "value2");
```
These lines insert key-value pairs into the HashMap. The keys are strings ("key1" and "key2"), and the values are also strings ("value1" and "value2").

## Printing HashMap

```rust
println!("{:?}", my_map);
```
The `{:?}` format specifier is used for printing the debug representation of the HashMap.

Notably, a `HashMap` is not in order, so if you print every key in a `HashMap` together it will probably print differently.

Example:
```rust
use std::collections::HashMap;

struct City {
    name: String,
    population: HashMap<u32, u32>, // This will have the year and the population for the year
}

fn main() {
    let mut my_map = City {
        name: "India".to_string(),
        population: HashMap::new(), // empty HashMap
    };

    my_map.population.insert(1372, 3_250);
    my_map.population.insert(1851, 24_000);
    my_map.population.insert(2020, 437_619);


    for (year, population) in my_map.population {
        println!("{} {} {}.", year, my_map.name, population);
    }
}
```
Output (1st run):
```
1851 India 24000.
2020 India 437619.
1372 India 3250.
```
Output (2nd run):
```
2020 India 437619.
1851 India 24000.
1372 India 3250.
```

You can see that it's not in order. If you want a `HashMap` that you can sort, you can use a `BTreeMap`.

Example:
```rust
use std::collections::BTreeMap; // Just change HashMap to BTreeMap

struct City {
    name: String,
    population: BTreeMap<u32, u32>, // Just change HashMap to BTreeMap
}

fn main() {

    let mut my_map = City {
        name: "India".to_string(),
        population: BTreeMap::new(), // Just change HashMap to BTreeMap
    };

    my_map.population.insert(1372, 3_250);
    my_map.population.insert(1851, 24_000);
    my_map.population.insert(2020, 437_619);

    for (year, population) in my_map.population {
        println!("{} {} {}.", year, my_map.name, population);
    }
}
```
Output:
```
1372 India 3250.
1851 India 24000.
2020 India 437619.
```

Now the out will be always in order.

## Accessing Values

You can get a value in a `HashMap` by just putting the key in `[]` square brackets. But be careful, because the program will crash if there is no key. Instead you can use `.get()` which returns an `Option`. If key exists it will be `Some(value)`, and if not you will get `None` instead of crashing the program. That's why `.get()` is the safer way to get a value from a HashMap.

```rust
if let Some(value) = my_map.get("key1") {
    println!("Value for key1: {}", value);
}
```
This code checks if the key "key1" is present in the HashMap and, if so, retrieves the corresponding value and prints it.

## Removing Elements

```rust
my_map.remove("key2");
```
This line removes the key-value pair with the key "key2" from the `HashMap`. The remove method returns an `Option<V>` where `V` is the type of the values in the `HashMap`, representing the removed value if the key was present.

## Changing/Updating Elements

If a HashMap already has a key when you try to put it in, it will overwrite its value.

```rust
my_map.insert("key1", "new_value1");
```

This line updates the value associated with the key "key1" to "new_value1" in the HashMap. If the key already existed, the `insert` method returns the previous value. Otherwise, it returns `None`.

## HashMap Usage

1. **Fast Lookups:**
   - Use hash maps when you need fast and efficient lookups based on a key. The underlying hash function allows for constant-time average-case lookups, making them suitable for scenarios where quick access to values is crucial.

2. **Uniqueness of Keys:**
   - Hash maps are suitable when you need to ensure that each key in your collection is unique. The internal structure of hash maps relies on unique keys, and attempting to insert a duplicate key will either replace the existing value or be rejected, depending on the implementation.

3. **Associative Data:**
   - When working with data that has an associative relationship between keys and values, hash maps provide a natural representation. This is especially useful for modeling relationships where each key corresponds to a specific value, such as a dictionary or a mapping of properties.

4. **Dynamic Size:**
   - Hash maps are dynamic data structures that can grow or shrink based on the number of elements they contain. This makes them suitable for situations where the size of the data set is not known in advance or may change over time. They dynamically manage memory and adapt to the amount of data being stored.

5. **Efficient Inserts and Removes:**
   - If your application involves frequent insertions and removals of key-value pairs, hash maps offer efficient performance. The average-case time complexity for insertions and removals is constant, making hash maps well-suited for scenarios where data is frequently updated or modified.

In summary, use `HashMap` when you need fast and unique key-based lookups, have an associative relationship between keys and values, require a dynamic data structure, and anticipate frequent insertions or removals.

## Panic Situations

While working with `HashMap` in Rust, there are some potential panic situations and pitfalls to be aware of:

1. **Unordered Iteration:**
   - The order of items during iteration in a `HashMap` is not guaranteed to be in any specific order. If you rely on a particular order, you might face unexpected behavior. If order is important, consider using a `BTreeMap` instead.

2. **Borrowing and Ownership:**
   - When iterating over a `HashMap`, be cautious about borrowing values. For example, using `iter()` will borrow references to the key-value pairs, but if you try to modify the `HashMap` while iterating, it may result in borrowing issues. Consider using `iter_mut()` for mutable iteration.

3. **Missing Key Panic:**
   - If you attempt to access a key that does not exist using `get` and unwrap the result, it may panic. It's advisable to use `match` or `unwrap_or` with a default value to handle the absence of a key gracefully.

    ```rust
    let my_map = HashMap::new();
    let value = my_map.get("nonexistent_key").unwrap_or(&default_value);
    ```

4. **Hashing Function Limitations:**
   - If your custom types are used as keys, ensure that the `Hash` trait is properly implemented for them. Failing to do so may result in incorrect behavior or panics. Use `HashMap` with types that have a good hash function.

5. **Collision Handling:**
   - While rare, hash collisions can occur, where different keys produce the same hash value. Modern hash maps handle collisions gracefully, but if you're using a custom hash function, ensure it distributes values evenly to minimize collisions.

Being aware of these points and carefully handling situations involving borrowing, missing keys, custom types, and concurrency will help you avoid pitfalls and ensure more robust usage of HashMaps in Rust.

## HashMap Methods

Some commonly used `HashMap` methods.

### 1. `insert`: 

Inserts a key-value pair into the map. Returns Option<V>, or the previous value associated with the key if it existed. 

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    // Insert key-value pairs
    let previous_value = my_map.insert("key1", "value1");
    println!("Previous value for key1: {:?}", previous_value); // Output: None
}
```

### 2. `get`:

Retrieves the value for a given key. Returns `Option<&V>`, with a reference to the value if the key is found, otherwise `None`. 

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // Get the value for a key
    if let Some(value) = my_map.get("key1") {
        println!("key1: {}", value); 
        // Output: key1: value1
    }
}
```

### 3. `remove`:

Removes a key-value pair from the map. Returns `Option<V>`, representing the value associated with the key if it existed.

Example:
```rust
use std::collections::HashMap;

fn main() {
    // previous code

    // Remove a key-value pair
    let removed_value = my_map.remove("key2");
    println!("Removed value for key2: {:?}", removed_value); // Output: Removed value for key2: Some("value2")
}
```

### 4. `contains_key`:

Checks if a key is present in the map. Returns `bool`, `true` if the key is present, otherwise `false`.

Example:
```rust
use std::collections::HashMap;

fn main() {
    // previous code

    // Check if a key is present
    if my_map.contains_key("key1") {
        println!("Key1 is present!"); // Output: Key1 is present!
    }
}
```

### 5. `len`:

Returns the number of elements in the map. Returns `usize`, the number of key-value pairs in the map.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // Get the number of elements
    let num_elements = my_map.len();
    println!("Number of elements: {}", num_elements); // Output: Number of elements: 2
}
```

### 6. `is_empty`:

Checks if the map is empty. Returns `bool`, `true` if the map is empty, otherwise `false`.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();

    // Check if the map is empty
    if my_map.is_empty() {
        println!("Map is empty!"); // Output: Map is empty!
    }
}
```

### 7. `keys`:

Returns an iterator over the keys. Returns `Keys<'_, K, V>`, an iterator yielding references to the keys.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // Iterate over keys
    for key in my_map.keys() {
        println!("Key: {}", key); // Output: Key: key1, Key: key2
    }
}
```

### 8. `values`:

Returns an iterator over the values. Returns `Values<'_, K, V>`, an iterator yielding references to the values.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // Iterate over values
    for value in my_map.values() {
        println!("Value: {}", value); // Output: Value: value1, Value: value2
    }
}
```

### 9. `iter`:

Returns an iterator over key-value pairs. Returns `Iter<'_, K, V>`, an iterator yielding references to the key-value pairs.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // Iterate over key-value pairs
    for (key, value) in my_map.iter() {
        println!("Key: {}, Value: {}", key, value);
        // Output: Key: key1, Value: value1
        //        Key: key2, Value: value2
    }
}
```

### 10. `entry`:

Provides an API for conditional insertion and updating. The `entry` method returns an `Entry<'_, K, V>`, an `enum`, which represents either an occupied or vacant entry in the `HashMap`.

Here is the basic syntax of the `entry` method:

```rust
fn entry(&mut self, key: K) -> Entry<K, V>
```

- `&mut self`: This indicates that the method modifies the HashMap in place.
- `key: K`: The key for which you want to get the entry.

The `Entry` enum has two variants:

1. **Vacant:**
    - Represents an entry where the key is not present in the HashMap.
    - Provides methods to insert a new value for the specified key.

2. **Occupied:**
    - Represents an entry where the key is already present in the HashMap.
    - Provides methods to access and modify the existing value.

Example:
```rust
use std::collections::HashMap;

fn main() {
    let mut my_map = HashMap::new();
    my_map.insert("key1", "value1");

    // Use entry to conditionally insert or update
    my_map.entry("key2").or_insert("value2"); 
    // This method is called on a "Vacant" entry
    my_map.entry("key1").and_modify(|v| *v = "new_value1");
    // This method is called on an "Occupied" entry

    println!("{:?}", my_map); 
    // Output: {"key1": "new_value1", "key2": "value2"}
}
```

Few methods present in the `Entry` enum:

1. **`or_insert` method:**

    - This method is called on a `Vacant` entry.
    - If the entry is vacant, it inserts a new key-value pair with the specified default value.
    - It returns a mutable reference to the value(`&'a mut V`) associated with the key.

    Example:
    ```rust
    my_map.entry("key2").or_insert("default_value");
    ```

2. **`and_modify` method:**

    - This method is called on an `Occupied` entry.
    - It takes a *closure* that is called with a mutable reference to the existing value(`&mut V`).
    - The closure can modify the value in-place.
    - The modified entry(`Self`) is then returned.

    Example:
    ```rust
    my_map.entry("key1").and_modify(|v| *v = "new_value1");
    ```

3. **`or_default` method:**

    - This method is called on a `Vacant` entry.
    - If the entry is vacant, it inserts a new key-value pair with the default value of the value type (`Default` trait is required for the value type).
    - It returns a mutable reference to the value(`&'a mut V`) associated with the key.

    Example:
    ```rust
    my_map.entry("key3").or_default();
    ```

4. **`key` method:**

    - This method is available on both `Vacant` and `Occupied` entries.
    - It returns a reference to the key(`&K`) of the entry.

    Example:
    ```rust
    let key = my_map.entry("key1").key();
    ```

5. **`remove` method:**

    - This method is called on an `Occupied` entry.
    - It removes the entry from the map and returns the value(`V`).

    Example:
    ```rust
    let removed_value = my_map.entry("key1").remove();
    ```

**Example of modifying the value if key exists or insert a new entry**

```rust
use std::collections::HashMap;

fn main() {
    let data = vec![ // This is the raw data
        ("male", 9),
        ("female", 5),
        ("male", 0),
        ("female", 6),
        ("female", 5),
        ("male", 10),
    ];

    let mut survey_hash = HashMap::new();

    for (gender, number) in data { // This gives a tuple of (&str, i32)
        survey_hash
            .entry(gender)
            .and_modify(|v:&mut Vec<i32>| v.push(number))
            .or_insert_with(|| vec![number]);
    }

    for (gender, numbers) in survey_hash {
        println!("{:?}: {:?}", gender, numbers);
    }
}
```
---

For more information read [Rust Book](https://doc.rust-lang.org/book/ch08-03-hash-maps.html).
