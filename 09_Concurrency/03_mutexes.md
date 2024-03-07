# Shared-State Concurrency in Threads

Concurrency allows multiple threads to execute code simultaneously. Shared state concurrency involves multiple threads accessing and potentially modifying the same data concurrently, which can lead to data races and bugs. Rust's ownership system and type system ensure memory safety and prevent data races through various mechanisms.

## Mutex

Mutex (short for *mutual exclusion*) in Rust is a synchronization primitive that allows multiple threads to access a resource concurrently while ensuring that only one thread can access the resource at a time. This prevents data races and ensures thread safety.

### API of Mutex

#### `std::sync::Mutex`

The `Mutex` type is used to create a mutex-protected value.

```rust
use std::sync::Mutex;

let mutex = Mutex::new(42);
```

#### `std::sync::MutexGuard`

If the lock is successfully acquired, the `lock` method returns a `Result` containing a `MutexGuard`. This guard is a smart pointer that implements the `Deref` and `DerefMut` traits, allowing you to access the protected data. The guard ensures that the `lock` is released when it goes out of scope, ensuring that other threads can acquire the lock.

```rust
let mut data = mutex.lock().unwrap();
*data += 1;
```

#### `lock` method

When a thread calls `lock` on a `Mutex`, it attempts to acquire the lock. It returns a `Result` containing a `MutexGuard` if the lock was successfully acquired. If the lock is currently held by another thread, the calling thread will be blocked until the lock becomes available. This means that the thread will not consume any CPU time while waiting for the lock, which is essential for efficient multithreading.. This blocking behavior is crucial for ensuring mutual exclusion, as only one thread can hold the lock at a time.

```rust
let mut data = mutex.lock().unwrap();
```

>  It's important to note that the `lock` method blocks the current thread until the lock is acquired. This can lead to potential deadlocks if care is not taken. Deadlocks occur when two or more threads are waiting for each other to release locks, causing all threads involved to remain blocked indefinitely.


#### `try_lock` method

To avoid potential deadlocks, you can use the `try_lock` method, which attempts to acquire the lock without blocking. The `try_lock` method is similar to `lock` but does not block the current thread. It returns `Some(MutexGuard)` if the lock is acquired. If the lock is not available, it returns `None` immediately, allowing you to handle the situation accordingly.

```rust
if let Some(mut data) = mutex.try_lock() {
    *data += 1;
}
```

### How to Use Mutex

Here's a simple example demonstrating the use of a mutex:

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

## Sharing a `Mutex<T>` Between Multiple Threads

To share a `Mutex<T>` between multiple threads, you can use the `Arc<T>` (*Atomic Reference Counting*) type to create a reference-counted pointer to the `Mutex<T>`. This allows multiple threads to have ownership of the `Mutex<T>` and safely access it concurrently.

- `Arc` is used for reference counting, allowing multiple threads to share ownership of data.
- `Mutex` ensures exclusive access to the shared data, preventing data races.
- The combination of `Arc` and `Mutex` provides a safe way to share and modify data between multiple threads in Rust.

## Multiple Ownership with Multiple Threads

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

## Similarities Between `RefCell<T>`/`Rc<T>` and `Mutex<T>`/`Arc<T>`

`RefCell<T>`/`Rc<T>` and `Mutex<T>`/`Arc<T>` are both used for interior mutability and sharing data between multiple owners. However, `RefCell<T>`/`Rc<T>` are for single-threaded scenarios and perform runtime checks, while `Mutex<T>`/`Arc<T>` are for multithreaded scenarios and perform synchronization using locking mechanisms.