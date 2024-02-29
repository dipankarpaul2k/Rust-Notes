# Threads in Rust

## What are Threads?

Threads are lightweight units of execution that enable concurrent programming. They allow a program to perform multiple tasks concurrently, potentially improving performance and responsiveness. However, it can add complexity.

## Why use them?

Threads are useful for tasks that can be executed independently and simultaneously, such as handling multiple clients in a server, performing background tasks, or parallelizing computation.

## Creating a New Thread

In Rust, you can create a native operating system thread using the `std::thread::spawn` function, which takes a closure containing the code to be executed in the new thread.

```rust
use std::thread;
use std::time::Duration;

fn main() {
    // create a thread
    thread::spawn(|| {
        // everything in here runs in a separate thread
        for i in 0..10 {
            println!("{} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(2));
        }
    });

    // main thread
    for i in 0..5 {
        println!("{} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
}
```
**Output:**
```
0 from the main thread!
0 from the spawned thread!
1 from the main thread!
1 from the spawned thread!
2 from the main thread!
3 from the main thread!
2 from the spawned thread!
4 from the main thread!
```

In the above example, notice that we sleep **2** milliseconds in the spawned thread and **1** millisecond in the main thread using the `std::thread::sleep` function.

The output from this program might be a little different every time. But the important thing to remember here is that **if the main thread completes, all other threads are shut down whether or not they have finished running**. So, even though the spawned thread should print until i is 9, it only reaches to 2 because the main thread shut down.

## Join Handles

A spawned thread always returns a join handle. If we want the spawned thread to complete execution, we can save the return value of `thread::spawn` in a variable and then call the `join()` method on it.

The `join()` method of `JoinHandle` (return type of` thread::spawn`) is used to wait for the spawned thread to finish its execution. It returns a `Result` containing the result of the thread's execution, if any.

```rust
use std::thread;
use std::time::Duration;

fn main() {
    // create a thread and save the handle to a variable
    let handle = thread::spawn(|| {
        // everything in here runs in a separate thread
        for i in 0..10 {
            println!("{} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(2));
        }
    });

    // main thread
    for i in 0..5 {
        println!("{} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
    
    // wait for the separate thread to complete
    handle.join().unwrap();
}
```

**Output:**
```
0 from the main thread!
0 from the spawned thread!
1 from the main thread!
2 from the main thread!
1 from the spawned thread!
3 from the main thread!
2 from the spawned thread!
4 from the main thread!
3 from the spawned thread!
4 from the spawned thread!
5 from the spawned thread!
6 from the spawned thread!
7 from the spawned thread!
8 from the spawned thread!
9 from the spawned thread!
```

Here, we bind the result of the `thread::spawn()` function to a variable called `handle`. Then, we call the `join()` method on the `handle` to block the thread until the thread terminates.

The two threads (main and spawned thread) continue alternating for some time, but the main thread waits because of handle.`join()` and does not end until the spawned thread is finished.

> **Note:** If we move the `handle.join()` before the main thread loop, the output will change and the print statements won't be interleaved. Thus, it is important to know where `join()` is called. I will dictate whether threads run at the same time or not.

## Passing data into Threads with the `move` Keyword

To pass data into a thread, you can use the `move` keyword to move ownership of the data into the closure.

```rust
use std::thread;

fn main() {
    let data = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("Data: {:?}", data);
    });

    handle.join().unwrap();
}
```

When a value is moved into a thread, ownership of the value is transferred to the thread, and the main thread can no longer access the value. This means that the closure can use the `data` variable even after the main thread has completed.

If we don't use the move keyword in front of the closure.

**output:**

```bash
error[E0373]: closure may outlive the current function, but it borrows `data`, which is owned by the current function
 --> src/main.rs:6:33
  |
6 |     let handle = thread::spawn( || {
  |                                 ^^ may outlive borrowed value `data`
7 |         println!("Data: {:?}", data);
  |                                ---- `data` is borrowed here
  |
help: to force the closure to take ownership of `data` (and any other referenced variables), use the `move` keyword
  |
6 |     let handle = thread::spawn( move || {
  |    
```

The program in this case fails to compile. Because, Rust doesn't know how long the spawned thread will run. Thus it can't tell if the reference to the `data` variable will always be valid.

By adding the `move` keyword before the closure, we force the closure to take ownership of the `data` variable or any variable used inside closure.

## Using Multiple Threads

You can create multiple threads to perform tasks concurrently.

```rust
use std::thread;

fn main() {
    let mut handles = vec![];

    for i in 0..5 {
        let handle = thread::spawn(move || {
            println!("Thread {} is running.", i);
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

## Shared Mutable State with Mutexes

When multiple threads need to access shared data, you can use a `Mutex` to synchronize access to the data.

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..5 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

## Sending Messages between Threads

You can use channels (`std::sync::mpsc`) to send messages between threads.

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (sender, receiver) = mpsc::channel();

    let sender_clone = sender.clone();
    thread::spawn(move || {
        sender_clone.send("Message from thread 1").unwrap();
    });

    thread::spawn(move || {
        sender.send("Message from thread 2").unwrap();
    });

    for received in receiver {
        println!("Received: {}", received);
    }
}
```

## Project: Multi-threaded Summation

Here's a small project that demonstrates using threads to calculate the sum of numbers in parallel.

```rust
use std::thread;

fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let mut handles = vec![];
    let mut sums = vec![0; 4];

    for i in 0..4 {
        let start = i * 3;
        let end = start + 3;
        let numbers_slice = &numbers[start..end];
        let handle = thread::spawn(move || {
            let sum: i32 = numbers_slice.iter().sum();
            sums[i] = sum;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    let total_sum: i32 = sums.iter().sum();
    println!("Total sum: {}", total_sum);
}
```

This project divides the array of numbers into four slices and calculates the sum of each slice in parallel using four threads. Finally, it calculates the total sum by summing the individual slice sums.

Feel free to run and modify this project to experiment with threads in Rust!