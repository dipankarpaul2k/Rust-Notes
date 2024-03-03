# Shared-State Concurrency in Threads

Concurrency allows multiple threads to execute code simultaneously. Shared state concurrency involves multiple threads accessing and potentially modifying the same data concurrently, which can lead to data races and bugs. Rust's ownership system and type system ensure memory safety and prevent data races through various mechanisms.

## Accessing Data Using Mutexes

**Mutexes** (short for *mutual exclusion*) are a synchronization primitive that allows only one thread to access a piece of data at a time. The `Mutex<T>` type in Rust wraps around the data and provides access through the `lock` method, which blocks the current thread until it can acquire the lock.

### API of `Mutex<T>`

The `Mutex<T>` type is part of the standard library's `std::sync` module. It is generic over the type `T`, which is the type of the data it protects. Here's a basic example of using a `Mutex<i32>`:

```rust
use std::sync::Mutex;

fn main() {
    let data = Mutex::new(0);
    {
        let mut guard = data.lock().unwrap();
        *guard += 1;
    }
    println!("Data: {:?}", data.lock().unwrap());
}
```

## Sharing a `Mutex<T>` Between Multiple Threads

To share a `Mutex<T>` between multiple threads, you can use the `Arc<T>` (*Atomic Reference Counting*) type to create a reference-counted pointer to the `Mutex<T>`. This allows multiple threads to have ownership of the `Mutex<T>` and safely access it concurrently.

## Multiple Ownership with Multiple Threads

**Example 1:**

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let data = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let data = Arc::clone(&data);
        let handle = thread::spawn(move || {
            let mut guard = data.lock().unwrap();
            *guard += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Data: {:?}", data.lock().unwrap());
}
// Output: Data: 10
```

In the example code, we're demonstrating how to share a `Mutex<i32>` between multiple threads using `Arc<T>` to safely update a shared counter value from each thread.

1. We start by importing necessary modules.

2. We create an `Arc<Mutex<i32>>` named `data` to hold the shared counter value initialized to `0`. `Arc` is used to share ownership of `Mutex<i32>` between multiple threads safely.

3. We create a mutable vector `handles` to store thread handles.

4. Next, we enter a loop to spawn 10 threads, each incrementing the counter value inside the `Mutex`.
   
5. We clone the `Arc` reference to `data` for each thread using `Arc::clone(&data)` to increment the reference count, allowing multiple threads to own a reference to `Mutex<i32>`.

6. We then use `thread::spawn(move || { ... })` to spawn a new thread that takes ownership of the cloned `Arc<Mutex<i32>>` and increments the counter value inside the `Mutex` by locking it using `lock()`.

7. After spawning all threads, we wait for each thread to finish execution. We use `join()` on each thread handle to wait for the corresponding thread to finish executing.

8. Finally, we print the updated counter value after all threads have finished executing. We lock the `Mutex` and print the counter value inside it. Since the `Mutex` ensures only one thread can access the counter at a time, we get the correct updated value even though multiple threads were incrementing it concurrently.

Summary:

- `Arc` is used for reference counting, allowing multiple threads to share ownership of data.
- `Mutex` ensures exclusive access to the shared data, preventing data races.
- The combination of `Arc` and `Mutex` provides a safe way to share and modify data between multiple threads in Rust.

**Example 2:**

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let data = Arc::new(Mutex::new(Vec::<String>::new()));

    let mut handles = vec![];
    for i in 0..5 {
        let data = Arc::clone(&data);
        let handle = thread::spawn(move || {
            let mut guard = data.lock().unwrap();
            guard.push(format!("Thread {}", i));
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    let final_data = data.lock().unwrap();
    println!("Data: {:?}", *final_data);
}
```

Output:

```bash
Data: [
    "Thread 0",
    "Thread 1",
    "Thread 2",
    "Thread 3",
    "Thread 4",
]
```


## Atomic Reference Counting with `Arc<T>`

`Arc<T>` is used to share ownership of data between multiple threads, ensuring that the data is only deallocated when all threads that have a reference to it are done. It uses atomic operations to manage the reference count, making it thread-safe.

## Similarities Between `RefCell<T>`/`Rc<T>` and `Mutex<T>`/`Arc<T>`

`RefCell<T>`/`Rc<T>` and `Mutex<T>`/`Arc<T>` are both used for interior mutability and sharing data between multiple owners. However, `RefCell<T>`/`Rc<T>` are for single-threaded scenarios and perform runtime checks, while `Mutex<T>`/`Arc<T>` are for multithreaded scenarios and perform synchronization using locking mechanisms.