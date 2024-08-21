# Error Handling

In this lesson, we will explore error handling in Rust, which is a crucial aspect of writing robust applications. Rust provides two powerful types, `Result` and `Option`, for handling errors and absence of values. We will also learn how to implement error handling using the `?` operator and how to create custom error types for more complex scenarios.

## 1. The `Result` Type

The `Result` type is used for functions that can return an error. It is an enum that has two variants:

- `Ok(T)`: Represents a successful outcome and contains a value of type `T`.
- `Err(E)`: Represents an error and contains an error value of type `E`.

### 1.1 Defining a Function that Returns a `Result`

Here’s an example of a function that returns a `Result` type:

```rust
fn divide(dividend: f64, divisor: f64) -> Result<f64, String> {
    if divisor == 0.0 {
        Err(String::from("Cannot divide by zero")) // Return an error
    } else {
        Ok(dividend / divisor) // Return the result
    }
}
```

In this example, the `divide` function attempts to divide two numbers. If the divisor is zero, it returns an error; otherwise, it returns the result wrapped in `Ok`.

### 1.2 Using the `Result` Type

You can handle the `Result` type using pattern matching or methods like `unwrap`, `expect`, or `map`.

#### Example: Handling a `Result`

```rust
fn main() {
    let result = divide(10.0, 2.0);

    match result {
        Ok(value) => println!("Result: {}", value), // Handle success
        Err(err) => println!("Error: {}", err),     // Handle error
    }
}
```

In this example, we use pattern matching to handle both the success and error cases of the `divide` function.

## 2. The `Option` Type

The `Option` type is used for values that may or may not be present. It is also an enum with two variants:

- `Some(T)`: Represents a value of type `T`.
- `None`: Represents the absence of a value.

### 2.1 Defining a Function that Returns an `Option`

Here’s an example of a function that returns an `Option` type:

```rust
fn find_item(index: usize) -> Option<&'static str> {
    let items = ["apple", "banana", "cherry"];

    if index < items.len() {
        Some(items[index]) // Return the item if index is valid
    } else {
        None // Return None if index is out of bounds
    }
}
```

In this example, the `find_item` function returns an `Option` type, providing a way to handle cases where an item may not exist.

### 2.2 Using the `Option` Type

You can handle the `Option` type similarly to the `Result` type, using pattern matching or methods like `unwrap`, `expect`, and `map`.

#### Example: Handling an `Option`

```rust
fn main() {
    let item = find_item(1);

    match item {
        Some(value) => println!("Found: {}", value), // Handle found item
        None => println!("Item not found"),            // Handle absence of item
    }
}
```

In this example, we use pattern matching to handle both the presence and absence of an item.

## 3. The `?` Operator

The `?` operator is a convenient way to propagate errors in functions that return a `Result` or `Option`. It allows you to write cleaner code by automatically returning the error if it occurs.

### 3.1 Using the `?` Operator

Here’s how you can use the `?` operator in the `divide` function:

```rust
fn safe_divide(dividend: f64, divisor: f64) -> Result<f64, String> {
    let result = divide(dividend, divisor)?; // Propagate error if divisor is zero
    Ok(result)
}
```

In this example, if `divide` returns an `Err`, the `?` operator will return that error from `safe_divide` immediately.

### 3.2 Example of Using the `?` Operator

```rust
fn main() {
    match safe_divide(10.0, 0.0) {
        Ok(value) => println!("Result: {}", value),
        Err(err) => println!("Error: {}", err),
    }
}
```

In this example, the `safe_divide` function uses the `?` operator to handle potential errors gracefully.

## 4. Custom Error Types

For more complex applications, you may want to define your own error types. This allows you to provide more context about the errors that can occur.

### 4.1 Defining a Custom Error Type

You can define a custom error type by creating an enum that represents different kinds of errors.

#### Example: Custom Error Type

```rust
#[derive(Debug)]
enum MathError {
    DivisionByZero,
    NegativeSqrt,
}

fn safe_divide_custom(dividend: f64, divisor: f64) -> Result<f64, MathError> {
    if divisor == 0.0 {
        Err(MathError::DivisionByZero) // Use custom error
    } else {
        Ok(dividend / divisor)
    }
}
```

In this example, we define a `MathError` enum with variants for different types of mathematical errors.

### 4.2 Using Custom Error Types

You can use your custom error type in the same way as the built-in error types.

#### Example: Handling Custom Errors

```rust
fn main() {
    match safe_divide_custom(10.0, 0.0) {
        Ok(value) => println!("Result: {}", value),
        Err(err) => println!("Error: {:?}", err), // Print the custom error
    }
}
```

In this example, we handle the custom error type and print it using the `Debug` trait.

### Key Points:
- Use the `Result` type for functions that can return an error.
- Use the `Option` type for values that may or may not be present.
- The `?` operator simplifies error propagation in functions.
- Custom error types provide more context and flexibility in error handling.

## Conclusion

In this lesson, we explored error handling in Rust using the `Result` and `Option` types. We learned how to implement error handling with the `?` operator and how to define custom error types for more complex scenarios. Understanding error handling is crucial for writing robust and reliable Rust applications, allowing you to manage failures gracefully and provide meaningful feedback to users.