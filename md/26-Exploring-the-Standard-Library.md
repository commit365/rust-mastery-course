# Exploring the Standard Library

In this lesson, we will familiarize ourselves with the Rust standard library and its various modules. The standard library provides essential utilities and data structures that are fundamental for building Rust applications. Understanding the standard library is crucial for writing efficient and idiomatic Rust code.

## 1. Overview of the Rust Standard Library

The Rust standard library is a collection of modules that provide functionality for tasks such as memory management, data manipulation, and input/output operations. It is automatically included in every Rust project, so you can use its features without needing to add any dependencies.

### Key Features of the Standard Library:

- **Core Types**: Fundamental types like integers, floating-point numbers, booleans, and characters.
- **Collections**: Data structures like vectors, hash maps, and sets for storing and manipulating data.
- **Concurrency**: Tools for working with threads and asynchronous programming.
- **I/O**: Functions for reading from and writing to files, as well as handling input and output streams.
- **Error Handling**: Types like `Result` and `Option` for managing errors and optional values.

## 2. Common Modules in the Standard Library

### 2.1 `std::vec`

The `std::vec` module provides the `Vec<T>` type, a growable array type that is one of the most commonly used collections in Rust.

#### Example: Using `Vec`

```rust
fn main() {
    let mut numbers = Vec::new(); // Create a new vector

    // Add elements to the vector
    numbers.push(1);
    numbers.push(2);
    numbers.push(3);

    // Iterate over the vector
    for number in &numbers {
        println!("{}", number);
    }
}
```

### 2.2 `std::collections`

The `std::collections` module provides various data structures, including `HashMap`, `HashSet`, and `BTreeMap`.

#### Example: Using `HashMap`

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new(); // Create a new HashMap

    // Insert key-value pairs
    scores.insert("Alice", 10);
    scores.insert("Bob", 20);

    // Access a value
    let alice_score = scores.get("Alice").unwrap();
    println!("Alice's score: {}", alice_score);
}
```

### 2.3 `std::fs`

The `std::fs` module provides functions for file system operations, such as reading and writing files.

#### Example: Reading a File

```rust
use std::fs;

fn main() {
    let contents = fs::read_to_string("example.txt").expect("Unable to read file");
    println!("File contents:\n{}", contents);
}
```

### 2.4 `std::io`

The `std::io` module provides traits and functions for input and output operations.

#### Example: Reading from Standard Input

```rust
use std::io;

fn main() {
    let mut input = String::new();
    println!("Enter your name:");

    // Read user input
    io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");

    println!("Hello, {}!", input.trim());
}
```

### 2.5 `std::thread`

The `std::thread` module provides functionality for creating and managing threads.

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

## 3. Common Utilities and Data Structures

### 3.1 `Option<T>`

The `Option` type is used to represent a value that can be either some value or none. It is commonly used for functions that may not return a value.

#### Example: Using `Option`

```rust
fn find_item(index: usize) -> Option<&'static str> {
    let items = ["apple", "banana", "cherry"];
    if index < items.len() {
        Some(items[index]) // Return Some if the index is valid
    } else {
        None // Return None if the index is out of bounds
    }
}

fn main() {
    match find_item(1) {
        Some(item) => println!("Found: {}", item),
        None => println!("Item not found"),
    }
}
```

### 3.2 `Result<T, E>`

The `Result` type is used for functions that can return an error. It can be either `Ok(T)` for a successful result or `Err(E)` for an error.

#### Example: Using `Result`

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Cannot divide by zero")) // Return an error
    } else {
        Ok(a / b) // Return the result
    }
}

fn main() {
    match divide(10.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(err) => println!("Error: {}", err),
    }
}
```

### 3.3 Iterators

The standard library provides powerful iterator functionality, allowing you to work with collections in a functional style.

#### Example: Using Iterators

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Use map to double each number and collect the results
    let doubled: Vec<i32> = numbers.iter().map(|&x| x * 2).collect();
    println!("Doubled: {:?}", doubled); // Output: Doubled: [2, 4, 6, 8, 10]
}
```

## 4. Conclusion

In this lesson, we explored the Rust standard library and its various modules. We learned about common utilities and data structures provided by the standard library, including vectors, hash maps, file I/O, and error handling with `Option` and `Result`. Familiarizing yourself with the standard library is essential for writing efficient and idiomatic Rust code, as it provides the foundational tools you need to build robust applications. By leveraging the standard library, you can focus on implementing your application logic while relying on well-tested and optimized components.