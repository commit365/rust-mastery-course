# Testing

In this lesson, we will explore how to write tests in Rust using the built-in test framework. We will cover both unit tests and integration tests, as well as best practices for testing and the principles of test-driven development (TDD). Understanding how to effectively test your code is crucial for ensuring its correctness and reliability.

## 1. Writing Unit Tests

Unit tests are designed to test individual functions or methods in isolation. They help verify that each part of your code behaves as expected.

### 1.1 Creating Unit Tests

In Rust, you can write unit tests in the same file as your code, typically in a module annotated with `#[cfg(test)]`. This module will only be compiled when you run the tests.

#### Example: Writing a Unit Test

```rust
// src/lib.rs

pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*; // Import everything from the parent module

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5); // Check if add(2, 3) equals 5
        assert_eq!(add(-1, 1), 0); // Check if add(-1, 1) equals 0
    }
}
```

In this example, we define a simple `add` function and a test module. The `#[test]` attribute marks the `test_add` function as a test case. We use the `assert_eq!` macro to check if the expected value matches the actual value returned by the `add` function.

### 1.2 Running Unit Tests

To run your unit tests, use the following command in your terminal:

```bash
cargo test
```

This command will compile your code and execute all tests, providing a summary of the results.

## 2. Writing Integration Tests

Integration tests are used to test the functionality of your entire crate or module. They are placed in a separate directory called `tests`, and each file in this directory is treated as a separate crate.

### 2.1 Creating Integration Tests

To create integration tests, follow these steps:

1. Create a `tests` directory at the root of your project.
2. Add a new Rust file (e.g., `integration_test.rs`) inside the `tests` directory.

#### Example: Writing an Integration Test

```rust
// tests/integration_test.rs

use my_crate::add; // Import the function from your crate

#[test]
fn test_add_integration() {
    assert_eq!(add(5, 7), 12); // Check if add(5, 7) equals 12
}
```

In this example, we import the `add` function from our crate and write an integration test to verify its behavior.

### 2.2 Running Integration Tests

You can run your integration tests using the same `cargo test` command. Rust will automatically detect and execute tests in the `tests` directory.

## 3. Best Practices for Testing

To ensure your tests are effective and maintainable, consider the following best practices:

### 3.1 Write Clear and Concise Tests

- Each test should focus on a single behavior or functionality. This makes it easier to understand what is being tested and why it might fail.

### 3.2 Use Descriptive Test Names

- Use descriptive names for your test functions to convey the purpose of the test. This helps in identifying failing tests quickly.

### 3.3 Keep Tests Independent

- Ensure that tests do not depend on each other. Each test should be able to run in isolation without relying on the state set by other tests.

### 3.4 Test Edge Cases

- Consider edge cases and boundary conditions when writing tests. This helps ensure that your code handles unexpected inputs gracefully.

### 3.5 Use Assertions Effectively

- Use assertions like `assert_eq!`, `assert_ne!`, `assert!(condition)`, and others to verify expected outcomes clearly.

## 4. Test-Driven Development (TDD)

Test-driven development (TDD) is a software development approach where you write tests before writing the corresponding code. This process typically follows these steps:

1. **Write a Test**: Start by writing a test for a new feature or functionality. The test should initially fail since the feature is not yet implemented.
  
2. **Run the Test**: Execute the test to confirm that it fails. This ensures that the test is valid and correctly identifies the absence of the feature.

3. **Implement the Feature**: Write the minimum code necessary to make the test pass.

4. **Run the Test Again**: Execute the test again to verify that it now passes.

5. **Refactor**: Clean up the code while ensuring that all tests still pass. This step helps improve code quality without changing its behavior.

### Example of TDD

1. **Write a Test**:

```rust
#[test]
fn test_subtract() {
    assert_eq!(subtract(5, 3), 2); // This will fail initially
}
```

2. **Run the Test**: The test fails because the `subtract` function does not exist.

3. **Implement the Feature**:

```rust
pub fn subtract(a: i32, b: i32) -> i32 {
    a - b
}
```

4. **Run the Test Again**: The test now passes.

5. **Refactor**: If necessary, clean up the `subtract` function or related code while ensuring the test still passes.

## Conclusion

In this lesson, we covered how to write unit tests and integration tests using Rust’s built-in test framework. We explored best practices for writing effective tests and introduced the principles of test-driven development (TDD). Mastering testing in Rust is essential for building reliable and maintainable applications, helping you catch errors early and ensure that your code behaves as expected.