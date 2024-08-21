# Unit Testing Best Practices

In this lesson, we will explore best practices for writing effective unit tests in Rust. Unit tests are essential for ensuring that your code works as intended and for preventing regressions as your codebase evolves. We will also learn how to use mocks and stubs in testing to isolate the functionality being tested.

## 1. Understanding Unit Testing in Rust

Rust has built-in support for unit testing through the `#[cfg(test)]` module. Unit tests are typically placed in the same file as the code they test, but they are organized under a separate module marked with the `#[cfg(test)]` attribute. This ensures that the test code is only compiled and run during testing.

### 1.1 Writing a Simple Unit Test

Here’s a simple example of a function and its corresponding unit test:

```rust
// src/lib.rs

/// Adds two numbers together.
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*; // Import the outer scope

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5); // Check if 2 + 3 equals 5
    }
}
```

In this example, we define a function `add` and a test function `test_add` that checks if the `add` function works correctly. The `assert_eq!` macro is used to assert that the expected value matches the actual value.

## 2. Best Practices for Writing Effective Unit Tests

### 2.1 Keep Tests Isolated

Each test should be independent of others. Avoid sharing state between tests to prevent side effects. This isolation ensures that tests can be run in any order and still produce the same results.

#### Example: Isolated Tests

```rust
#[test]
fn test_add_positive_numbers() {
    assert_eq!(add(2, 3), 5);
}

#[test]
fn test_add_negative_numbers() {
    assert_eq!(add(-2, -3), -5);
}
```

### 2.2 Use Descriptive Test Names

Use clear and descriptive names for your test functions to indicate what functionality is being tested. This helps improve readability and makes it easier to identify failing tests.

#### Example: Descriptive Test Names

```rust
#[test]
fn adding_two_positive_numbers_should_return_sum() {
    assert_eq!(add(2, 3), 5);
}
```

### 2.3 Test Edge Cases

Make sure to test edge cases and boundary conditions. This helps ensure that your code handles unexpected or extreme inputs correctly.

#### Example: Testing Edge Cases

```rust
#[test]
fn adding_zero_should_return_other_number() {
    assert_eq!(add(0, 5), 5);
    assert_eq!(add(5, 0), 5);
}
```

### 2.4 Use Assertions Effectively

Use assertions to check for expected outcomes. Rust provides several assertion macros, including `assert!`, `assert_eq!`, and `assert_ne!`. Choose the appropriate assertion based on your testing needs.

- **`assert!`**: Checks if a condition is true.
- **`assert_eq!`**: Checks if two values are equal.
- **`assert_ne!`**: Checks if two values are not equal.

### 2.5 Organize Tests Logically

Organize your tests logically, grouping related tests together. You can use submodules to organize tests for different components or functionalities.

#### Example: Organizing Tests

```rust
#[cfg(test)]
mod tests {
    use super::*;

    mod addition_tests {
        use super::*;

        #[test]
        fn test_add() {
            assert_eq!(add(2, 3), 5);
        }

        #[test]
        fn test_add_negative() {
            assert_eq!(add(-1, 1), 0);
        }
    }
}
```

## 3. Using Mocks and Stubs in Testing

Mocks and stubs are techniques used to isolate the functionality being tested by replacing dependencies with controlled substitutes.

### 3.1 Stubs

A stub is a simple implementation of a dependency that returns predefined values. Stubs are useful for isolating the code under test from its dependencies.

#### Example: Using Stubs

```rust
struct DatabaseStub;

impl Database for DatabaseStub {
    fn get_user(&self, _id: i32) -> User {
        User { id: 1, name: String::from("Alice") } // Return a stubbed user
    }
}

#[test]
fn test_get_user() {
    let db = DatabaseStub;
    let user = db.get_user(1);
    assert_eq!(user.name, "Alice");
}
```

### 3.2 Mocks

Mocks are more sophisticated than stubs and can track how they are used. They can verify that certain methods were called with expected arguments.

#### Example: Using Mocks with `mockito`

You can use the `mockito` crate to create HTTP mocks for testing.

1. Add `mockito` to your `Cargo.toml`:

```toml
[dev-dependencies]
mockito = "0.31"
```

2. Use `mockito` in your tests:

```rust
#[cfg(test)]
mod tests {
    use super::*;
    use mockito::{mock, Matcher};

    #[test]
    fn test_api_call() {
        let _m = mock("GET", "/user/1")
            .with_status(200)
            .with_body(r#"{"id": 1, "name": "Alice"}"#)
            .create();

        // Call the API and assert the response
        let response = reqwest::blocking::get(&format!("{}/user/1", mockito::server_url())).unwrap();
        assert_eq!(response.status(), 200);
    }
}
```

In this example, we use `mockito` to create a mock HTTP server that responds to a specific GET request. We can then test our code against this mock server.

## 4. Conclusion

In this lesson, we explored best practices for writing effective unit tests in Rust. We learned how to keep tests isolated, use descriptive names, test edge cases, and organize tests logically. Additionally, we discussed how to use mocks and stubs to isolate functionality and control dependencies during testing. By following these best practices, you can create a robust suite of unit tests that ensure your code behaves as expected and helps maintain quality as your codebase evolves.