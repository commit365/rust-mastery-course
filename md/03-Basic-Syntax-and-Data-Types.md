# Basic Syntax and Data Types

In this lesson, we will explore the basic syntax of Rust, including variable declaration, mutability, and fundamental data types. Additionally, we will look at compound data types such as tuples and arrays. Understanding these concepts is essential for writing effective Rust programs.

## 1. Variable Declaration and Mutability

In Rust, variables are declared using the `let` keyword. By default, variables are immutable, meaning their values cannot be changed once assigned. If you want a variable to be mutable, you must explicitly declare it as mutable using the `mut` keyword.

### Example: Variable Declaration

```rust
fn main() {
    let x = 5; // Immutable variable
    println!("The value of x is: {}", x);
    
    // x = 10; // This will cause a compile-time error

    let mut y = 10; // Mutable variable
    println!("The value of y is: {}", y);
    
    y = 15; // Now we can change the value of y
    println!("The value of y is now: {}", y);
}
```

### Key Points:
- Use `let` to declare variables.
- Variables are immutable by default.
- Use `mut` to make a variable mutable.

## 2. Basic Data Types

Rust has several built-in data types. Here are some of the most commonly used basic data types:

### 2.1 Integers

Rust provides several integer types with varying sizes. The most common integer types are:

- `i32`: A signed 32-bit integer (default type).
- `u32`: An unsigned 32-bit integer.
- `i64`: A signed 64-bit integer.
- `u64`: An unsigned 64-bit integer.

You can specify the type when declaring a variable:

```rust
fn main() {
    let a: i32 = 10; // Signed integer
    let b: u32 = 20; // Unsigned integer
    println!("a: {}, b: {}", a, b);
}
```

### 2.2 Floats

Rust supports floating-point numbers, which are used for representing decimal values. The two main floating-point types are:

- `f32`: A 32-bit floating-point number.
- `f64`: A 64-bit floating-point number (default type).

Example:

```rust
fn main() {
    let pi: f64 = 3.14159; // 64-bit floating-point
    let e: f32 = 2.71828; // 32-bit floating-point
    println!("Pi: {}, e: {}", pi, e);
}
```

### 2.3 Booleans

The boolean type in Rust is represented by the `bool` type, which can hold one of two values: `true` or `false`.

Example:

```rust
fn main() {
    let is_rust_fun: bool = true;
    println!("Is Rust fun? {}", is_rust_fun);
}
```

## 3. Compound Data Types

Rust also provides compound data types, which can group multiple values together. The two primary compound data types are tuples and arrays.

### 3.1 Tuples

A tuple is a collection of values of different types grouped together. Tuples are created by placing values inside parentheses, separated by commas. You can access tuple elements using a dot notation with the index.

Example:

```rust
fn main() {
    let tuple: (i32, f64, &str) = (42, 3.14, "Hello");
    
    // Accessing tuple elements
    let (x, y, z) = tuple; // Destructuring
    println!("x: {}, y: {}, z: {}", x, y, z);
    
    // Accessing using index
    println!("First element: {}", tuple.0);
}
```

### 3.2 Arrays

An array is a collection of elements of the same type. Arrays have a fixed size, which must be known at compile time. You can create an array by using square brackets.

Example:

```rust
fn main() {
    let array: [i32; 5] = [1, 2, 3, 4, 5]; // An array of 5 integers
    
    // Accessing array elements
    println!("First element: {}", array[0]);
    println!("Second element: {}", array[1]);
    
    // Iterating over an array
    for element in &array {
        println!("{}", element);
    }
}
```

### Key Points:
- Tuples can hold values of different types and have a fixed size.
- Arrays hold values of the same type and also have a fixed size.
- Access tuple elements using indexing or destructuring.
- Access array elements using indexing.

## Conclusion

In this lesson, we covered the basics of variable declaration and mutability in Rust, along with fundamental data types such as integers, floats, and booleans. We also explored compound data types, including tuples and arrays. Understanding these concepts is crucial for writing effective Rust programs, as they form the foundation for managing data in your applications.