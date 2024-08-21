# Concurrency

In this lesson, we will explore concurrency in Rust, which allows you to run multiple tasks simultaneously. Rust's approach to concurrency emphasizes safety, ensuring that data races and other common concurrency issues are minimized. We will cover how to create threads, the Rust model for safe concurrency, and how to use channels for communication between threads.

## 1. Understanding Threads

A thread is a lightweight unit of execution that can run concurrently with other threads. In Rust, you can create threads using the `std::thread` module.

### 1.1 Creating a Thread

You can create a new thread using the `thread::spawn` function, which takes a closure as an argument. The closure contains the code that will run in the new thread.

#### Example: Creating a Thread

```rust
use std::thread;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..5 {
            println!("Thread: {}", i);
        }
    });

    for i in 1..3 {
        println!("Main: {}", i);
    }

    handle.join().unwrap(); // Wait for the thread to finish
}
```

In this example, we create a new thread that prints numbers from 1 to 4. Meanwhile, the main thread prints numbers from 1 to 2. The `join` method is called on the thread handle to wait for the thread to finish before the main thread exits.

### 1.2 Moving Data into Threads

When you create a thread, it cannot directly access variables from the parent thread unless those variables are explicitly passed to it. Rust ensures that data is moved into the thread safely.

#### Example: Moving Data into a Thread

```rust
use std::thread;

fn main() {
    let value = String::from("Hello from the thread!");

    let handle = thread::spawn(move || {
        println!("{}", value); // Move value into the thread
    });

    handle.join().unwrap(); // Wait for the thread to finish
}
```

In this example, we use the `move` keyword to transfer ownership of the `value` variable into the thread. This allows the thread to access the variable safely.

## 2. Safe Concurrency in Rust

Rust's ownership and borrowing rules help prevent data races, which occur when multiple threads access the same data simultaneously. Here are some key concepts that contribute to safe concurrency in Rust:

### 2.1 Ownership and Borrowing

Each piece of data in Rust has a single owner, and when that owner goes out of scope, the data is dropped. This ensures that data cannot be accessed after it has been deallocated.

### 2.2 Mutexes for Shared State

If multiple threads need to access shared data, you can use a `Mutex` (mutual exclusion) to ensure that only one thread can access the data at a time.

#### Example: Using a Mutex

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0)); // Create a Mutex wrapped in an Arc

    let mut handles = vec![];

    for _ in 0..10 {
        let counter_clone = Arc::clone(&counter); // Clone the Arc for each thread

        let handle = thread::spawn(move || {
            let mut num = counter_clone.lock().unwrap(); // Lock the mutex
            *num += 1; // Increment the counter
        });

        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap(); // Wait for all threads to finish
    }

    println!("Result: {}", *counter.lock().unwrap()); // Output the final count
}
```

In this example, we create a `Mutex` to protect access to a shared counter. We use `Arc` (atomic reference counting) to allow multiple threads to own the mutex safely. Each thread locks the mutex, increments the counter, and then releases the lock when it goes out of scope.

## 3. Using Channels for Communication

Channels in Rust provide a way for threads to communicate with each other. Channels allow you to send messages between threads safely, enabling synchronization and coordination.

### 3.1 Creating a Channel

You can create a channel using the `std::sync::mpsc` module, which stands for "multiple producer, single consumer." This allows multiple threads to send messages to a single receiver.

#### Example: Creating a Channel

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel(); // Create a channel

    thread::spawn(move || {
        let message = String::from("Hello from the thread!");
        tx.send(message).unwrap(); // Send a message to the receiver
    });

    let received = rx.recv().unwrap(); // Receive the message
    println!("Received: {}", received); // Output: Received: Hello from the thread!
}
```

In this example, we create a channel and spawn a thread that sends a message to the main thread. The main thread receives the message and prints it.

### 3.2 Sending and Receiving Messages

You can send messages using the `send` method on the transmitter (`tx`) and receive messages using the `recv` method on the receiver (`rx`).

#### Example: Sending Multiple Messages

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    for i in 1..5 {
        let tx_clone = tx.clone(); // Clone the transmitter for each thread

        thread::spawn(move || {
            let message = format!("Message {}", i);
            tx_clone.send(message).unwrap(); // Send a message
        });
    }

    for _ in 1..5 {
        let received = rx.recv().unwrap(); // Receive messages
        println!("Received: {}", received);
    }
}
```

In this example, we spawn multiple threads, each sending a message to the main thread. The main thread receives and prints each message.

## Key Points:
- **Threads** allow you to run multiple tasks concurrently in Rust.
- **Mutexes** provide safe access to shared data between threads.
- **Channels** enable communication between threads, allowing them to send messages to each other.

## Conclusion

In this lesson, we explored concurrency in Rust, focusing on threads and the Rust model for safe concurrency. We learned how to create threads, use mutexes to manage shared state, and communicate between threads using channels. Understanding concurrency is essential for building efficient and responsive applications in Rust, allowing you to take full advantage of modern multi-core processors.