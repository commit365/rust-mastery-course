# Memory Management

In this lesson, we will explore Rust’s memory management model, which is designed to ensure memory safety without the need for a garbage collector. We will discuss the `Drop` trait, which allows for custom cleanup of resources, and we will examine `Rc` (Reference Counted) and `Arc` (Atomic Reference Counted) types for managing shared ownership and reference counting in Rust.

## 1. Rust’s Memory Management Model

Rust uses a unique memory management model based on ownership, borrowing, and lifetimes. This model helps prevent common memory-related issues, such as dangling pointers, memory leaks, and data races.

### 1.1 Ownership

In Rust, every value has a single owner, which is a variable responsible for managing the value's memory. When the owner goes out of scope, the memory for that value is automatically deallocated. This ensures that memory is managed safely and efficiently.

#### Example: Ownership

```rust
fn main() {
    let s = String::from("Hello, Rust!"); // s owns the String
    println!("{}", s); // Use the value
} // s goes out of scope and memory is freed here
```

In this example, the string is deallocated automatically when `s` goes out of scope.

### 1.2 Borrowing

Rust allows you to borrow values instead of transferring ownership. Borrowing can be immutable or mutable, and Rust enforces rules to ensure that data is accessed safely.

#### Example: Borrowing

```rust
fn main() {
    let s = String::from("Hello, Rust!");
    let len = calculate_length(&s); // Borrowing s immutably
    println!("Length: {}", len);
}

fn calculate_length(s: &String) -> usize {
    s.len() // Use the borrowed value
}
```

In this example, we borrow `s` immutably when passing it to the `calculate_length` function.

### 1.3 The `Drop` Trait

The `Drop` trait allows you to specify custom cleanup logic for a type when it goes out of scope. You can implement the `Drop` trait for your struct to define what should happen when an instance of that struct is dropped.

#### Example: Implementing the `Drop` Trait

```rust
struct Resource {
    name: String,
}

impl Drop for Resource {
    fn drop(&mut self) {
        println!("Dropping resource: {}", self.name);
    }
}

fn main() {
    let resource = Resource {
        name: String::from("MyResource"),
    };
    // The resource will be dropped at the end of this scope
} // Output: Dropping resource: MyResource
```

In this example, we define a struct `Resource` and implement the `Drop` trait to print a message when the resource is dropped.

## 2. Shared Ownership with `Rc` and `Arc`

In some cases, you may want multiple parts of your program to share ownership of a value. Rust provides `Rc` and `Arc` types to facilitate shared ownership through reference counting.

### 2.1 `Rc` (Reference Counted)

`Rc` is a type that enables multiple ownership of a value in a single-threaded context. It keeps track of the number of references to a value, allowing the value to be deallocated when there are no more references.

#### Example: Using `Rc`

```rust
use std::rc::Rc;

fn main() {
    let value = Rc::new(String::from("Shared Value")); // Create an Rc

    let value1 = Rc::clone(&value); // Clone the Rc to create a new reference
    let value2 = Rc::clone(&value); // Clone again

    println!("Value: {}", value); // Output: Value: Shared Value
    println!("Count: {}", Rc::strong_count(&value)); // Output: Count: 3
}
```

In this example, we create an `Rc` instance and clone it to create multiple references. The `strong_count` method returns the number of references to the value.

### 2.2 `Arc` (Atomic Reference Counted)

`Arc` is similar to `Rc`, but it is thread-safe and can be used in concurrent contexts. `Arc` uses atomic operations to manage the reference count, allowing safe sharing of data across threads.

#### Example: Using `Arc`

```rust
use std::sync::Arc;
use std::thread;

fn main() {
    let value = Arc::new(String::from("Shared Value")); // Create an Arc

    let value1 = Arc::clone(&value); // Clone the Arc for the first thread
    let value2 = Arc::clone(&value); // Clone for the second thread

    let handle1 = thread::spawn(move || {
        println!("Thread 1: {}", value1);
    });

    let handle2 = thread::spawn(move || {
        println!("Thread 2: {}", value2);
    });

    handle1.join().unwrap(); // Wait for the first thread to finish
    handle2.join().unwrap(); // Wait for the second thread to finish
}
```

In this example, we create an `Arc` instance and clone it for two threads. Each thread can safely access the shared value.

### 2.3 Key Differences Between `Rc` and `Arc`

- **`Rc`**: Not thread-safe; suitable for single-threaded scenarios.
- **`Arc`**: Thread-safe; allows sharing data across multiple threads.

## Key Points:
- Rust’s memory management model is based on ownership, borrowing, and the `Drop` trait, ensuring memory safety without a garbage collector.
- The `Drop` trait allows for custom cleanup logic when an instance goes out of scope.
- `Rc` and `Arc` enable shared ownership of values, with `Rc` being suitable for single-threaded contexts and `Arc` for multi-threaded contexts.

## Conclusion

In this lesson, we explored Rust’s memory management model, including ownership, borrowing, and the `Drop` trait. We also learned about `Rc` and `Arc` for managing shared ownership and reference counting. Understanding memory management in Rust is crucial for writing safe and efficient applications, allowing you to manage resources effectively while preventing common memory-related issues.