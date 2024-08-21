# Collections

In this lesson, we will explore collections in Rust, which are essential data structures for storing and managing multiple values. We will focus on three primary collection types: vectors, hash maps, and sets. Additionally, we will learn how to manipulate and iterate over these collections effectively.

## 1. Vectors

Vectors are dynamic arrays that can grow and shrink in size. They allow you to store multiple values of the same type in a contiguous block of memory.

### 1.1 Creating a Vector

You can create a vector using the `Vec` type. Here’s how to create a new vector:

#### Example: Creating a Vector

```rust
fn main() {
    let mut numbers: Vec<i32> = Vec::new(); // Create an empty vector

    numbers.push(1); // Add elements to the vector
    numbers.push(2);
    numbers.push(3);

    println!("{:?}", numbers); // Output: [1, 2, 3]
}
```

In this example, we create an empty vector of integers and use the `push` method to add elements to it.

### 1.2 Accessing Elements

You can access elements in a vector using indexing. Rust will panic if you try to access an index that is out of bounds, so ensure the index is valid.

#### Example: Accessing Vector Elements

```rust
fn main() {
    let numbers = vec![10, 20, 30, 40, 50];

    println!("First element: {}", numbers[0]); // Output: First element: 10
    println!("Second element: {}", numbers[1]); // Output: Second element: 20
}
```

### 1.3 Iterating Over a Vector

You can iterate over the elements of a vector using a `for` loop.

#### Example: Iterating Over a Vector

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    for number in &numbers { // Use a reference to avoid moving the vector
        println!("{}", number);
    }
}
```

In this example, we iterate over the vector and print each number.

## 2. Hash Maps

Hash maps are collections that store key-value pairs. They allow you to associate values with unique keys, making it easy to retrieve values based on their keys.

### 2.1 Creating a Hash Map

You can create a hash map using the `HashMap` type from the `std::collections` module.

#### Example: Creating a Hash Map

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new(); // Create a new hash map

    scores.insert(String::from("Alice"), 10); // Insert key-value pairs
    scores.insert(String::from("Bob"), 20);
    scores.insert(String::from("Charlie"), 15);

    println!("{:?}", scores); // Output: {"Alice": 10, "Bob": 20, "Charlie": 15}
}
```

### 2.2 Accessing Values

You can access values in a hash map using the `get` method. This method returns an `Option` type, so you need to handle the possibility of a missing key.

#### Example: Accessing Hash Map Values

```rust
fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Alice"), 10);
    
    match scores.get("Alice") {
        Some(&score) => println!("Alice's score: {}", score), // Output: Alice's score: 10
        None => println!("Alice not found"),
    }
}
```

### 2.3 Iterating Over a Hash Map

You can iterate over the key-value pairs in a hash map using a `for` loop.

#### Example: Iterating Over a Hash Map

```rust
fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Alice"), 10);
    scores.insert(String::from("Bob"), 20);

    for (key, value) in &scores { // Use references to avoid moving the hash map
        println!("{}: {}", key, value);
    }
}
```

## 3. Sets

Sets are collections that store unique values without any particular order. Rust provides the `HashSet` type for this purpose.

### 3.1 Creating a Set

You can create a set using the `HashSet` type from the `std::collections` module.

#### Example: Creating a HashSet

```rust
use std::collections::HashSet;

fn main() {
    let mut unique_numbers = HashSet::new(); // Create a new hash set

    unique_numbers.insert(1);
    unique_numbers.insert(2);
    unique_numbers.insert(2); // Duplicate value, will not be added

    println!("{:?}", unique_numbers); // Output: {1, 2}
}
```

### 3.2 Checking for Existence

You can check if a value exists in a set using the `contains` method.

#### Example: Checking for Existence

```rust
fn main() {
    let mut unique_numbers = HashSet::new();
    unique_numbers.insert(1);
    unique_numbers.insert(2);

    if unique_numbers.contains(&1) {
        println!("Set contains 1"); // Output: Set contains 1
    } else {
        println!("Set does not contain 1");
    }
}
```

### 3.3 Iterating Over a Set

You can iterate over the elements of a set using a `for` loop.

#### Example: Iterating Over a Set

```rust
fn main() {
    let mut unique_numbers = HashSet::new();
    unique_numbers.insert(1);
    unique_numbers.insert(2);
    
    for number in &unique_numbers {
        println!("{}", number);
    }
}
```

## Key Points
- **Vectors** are dynamic arrays that allow you to store multiple values of the same type and provide methods for adding, accessing, and iterating over elements.
- **Hash Maps** store key-value pairs, allowing for efficient retrieval of values based on their keys.
- **Sets** store unique values and provide methods for checking existence and iterating over elements.

## Conclusion

In this lesson, we explored the essential collection types in Rust: vectors, hash maps, and sets. We learned how to create, manipulate, and iterate over these collections. Mastering collections is crucial for managing and organizing data effectively in your Rust programs, enabling you to build more complex and efficient applications.