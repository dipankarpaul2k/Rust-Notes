# Message Passing Between Threads using Channels

In Rust, message passing between threads is a way for threads to communicate by sending data to each other. This can be useful for coordinating work, sharing data, and synchronizing operations.

## Channels

Channels in Rust are a feature of its standard library's concurrency module (`std::sync::mpsc`) that allows communication between threads. They provide a way to send data from one thread to another in a thread-safe manner. Channels consist of two parts: a transmitter (sender) and a receiver.

### API of Channels

#### `std::sync::mpsc::channel`

This function `channel()` creates a new unbounded channel. It returns a tuple containing the sender and receiver.

```rust
use std::sync::mpsc::channel;

let (sender, receiver) = channel();
```

#### `Sender<T>`
  
The `Sender<T>` type is used to send values of type `T` over the channel.

```rust
use std::sync::mpsc::Sender;

let sender: Sender<i32> = sender;
sender.send(42).unwrap();
```

#### `Receiver<T>`

The `Receiver<T>` type is used to receive values of type `T` from the channel.

```rust
use std::sync::mpsc::Receiver;

let receiver: Receiver<i32> = receiver;
let received_value = receiver.recv().unwrap();
println!("Received: {}", received_value);
```

### How to Use Channels

#### Sending Data

To send data over a channel, use the `send` method on the `Sender`:

```rust
sender.send(data).unwrap();
```

#### Receiving Data

To receive data from a channel, use the `recv` method on the `Receiver`:

```rust
let received_data = receiver.recv().unwrap();
```

#### Example

Here's a simple example demonstrating the use of channels:

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hello");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

In this example, we create a channel using `mpsc::channel()`, which returns a tuple containing a transmitter (`tx`) and a receiver (`rx`). We then spawn a new thread using `thread::spawn()` and move the transmitter into the new thread's closure. Inside the closure, we send a message (`String`) through the transmitter using `send()`.

The main thread then receives the message using `recv()` on the receiver. Since `recv()` blocks until a value is available, the main thread will wait until the new thread sends a message before continuing. Finally, we print the received message.

### Why Do We Need Channels?

We need channels because Rust's ownership system prevents multiple threads from accessing data simultaneously, which helps prevent data races. Channels provide a way to safely pass ownership of data between threads, ensuring that only one thread can access the data at a time.

## Channels and Ownership Transference

When a value is sent through a channel, ownership of the value is transferred to the receiver. This means that the sender can no longer access the value after it has been sent. This is a key aspect of Rust's ownership system that helps prevent data races. For example,

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
        println!("val is {}", val);
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

Output:
```bash
error[E0382]: borrow of moved value: `val`
  --> src/main.rs:10:31
   |
8  |         let val = String::from("hi");
   |             --- move occurs because `val` has type `String`, which does not implement the `Copy` trait
9  |         tx.send(val).unwrap();
   |                 --- value moved here
10 |         println!("val is {}", val);
   |                               ^^^ value borrowed here after move
   |
```

The `send` function takes ownership of its parameter, and when the value is moved, the receiver takes ownership of it.

## Sending Multiple Values and Seeing the Receiver Waiting

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        for i in 1..=5 {
            tx.send(i).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx {
        println!("Got: {}", received);
    }
}
```

In this example, the spawned thread sends values from 1 to 5 through the channel, with a delay of 1 second between each send. The main thread receives these values using a `for` loop, printing each received value. Since `recv()` is not used, the main thread does not block and can continue processing other tasks while waiting for values from the channel.

## Creating Multiple Producers by Cloning the Transmitter

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();
    let tx1 = tx.clone();

    thread::spawn(move || {
        tx.send("hello from thread 1").unwrap();
    });

    thread::spawn(move || {
        tx1.send("hello from thread 2").unwrap();
    });

    for received in rx {
        println!("Got: {}", received);
    }
}
```

In this example, we clone the transmitter (`tx`) to create two transmitters (`tx` and `tx1`), which allows us to have multiple threads sending messages through the channel. Each thread sends a message through its respective transmitter, and the main thread receives and prints the messages as they arrive.