# Ownership and Borrowing

In this lesson, we will explore one of the most fundamental concepts in Rust: ownership. Understanding ownership, borrowing, and lifetimes is crucial for writing safe and efficient Rust programs. These concepts help prevent common programming errors such as memory leaks and data races.

## 1. The Ownership Model

Rust uses a unique ownership model to manage memory. The key principles of ownership are:

- **Each value in Rust has a single owner**: This means that each piece of data is owned by one variable at a time. When the owner goes out of scope, the value is automatically dropped (deallocated).

- **Ownership can be transferred**: When you assign a value from one variable to another, the ownership of that value is transferred to the new variable. The original variable can no longer be used.

### 1.1 Example of Ownership

```rust
fn main() {
    let s1 = String::from("Hello"); // s1 owns the String
    let s2 = s1; // Ownership of the String is transferred to s2

    // println!("{}", s1); // This will cause a compile-time error
    println!("{}", s2); // Output: Hello
}
```

In this example, `s1` initially owns the `String`. When we assign `s1` to `s2`, ownership is transferred, and `s1` can no longer be used.

### 1.2 Copy Types

Some types in Rust implement the `Copy` trait, which allows for the value to be duplicated instead of transferred. These types include simple scalar values like integers and booleans.

#### Example of Copy Types

```rust
fn main() {
    let x = 5; // x owns the integer
    let y = x; // y makes a copy of x

    println!("x: {}, y: {}", x, y); // Output: x: 5, y: 5
}
```

In this case, both `x` and `y` can be used independently because `i32` implements the `Copy` trait.

## 2. Borrowing

Borrowing allows you to use a value without taking ownership of it. In Rust, you can borrow a value either immutably or mutably.

### 2.1 Immutable Borrowing

You can create a reference to a value without changing it using an immutable borrow. This is done using the `&` symbol.

#### Example of Immutable Borrowing

```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = &s1; // s2 borrows s1 immutably

    println!("s1: {}, s2: {}", s1, s2); // Output: s1: Hello, s2: Hello
}
```

In this example, `s2` borrows `s1` immutably, allowing both to be used without transferring ownership.

### 2.2 Mutable Borrowing

To modify a borrowed value, you must create a mutable reference using `&mut`. However, you can only have one mutable reference to a value in a particular scope to prevent data races.

#### Example of Mutable Borrowing

```rust
fn main() {
    let mut s1 = String::from("Hello");
    let s2 = &mut s1; // s2 borrows s1 mutably

    s2.push_str(", world!"); // Modify the borrowed value
    println!("{}", s2); // Output: Hello, world!
}
```

In this case, `s2` is a mutable reference to `s1`, allowing us to modify the original string.

### 2.3 Rules of Borrowing

- You can have either one mutable reference or any number of immutable references to a value at a time, but not both.
- References must always be valid. This means that the value being referenced must not go out of scope while it is being borrowed.

## 3. Lifetimes

Lifetimes are a way for Rust to track how long references are valid. They help ensure that references do not outlive the data they point to, preventing dangling references.

### 3.1 Basic Lifetime Annotation

When you define a function that takes references as parameters, you may need to specify lifetimes to indicate how the lifetimes of the parameters relate to each other.

#### Example of Lifetime Annotation

```rust
fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len() {
        s1
    } else {
        s2
    }
}

fn main() {
    let str1 = String::from("Hello");
    let str2 = String::from("World!");
    
    let result = longest(&str1, &str2);
    println!("The longest string is: {}", result); // Output: The longest string is: World!
}
```

In this example, the function `longest` takes two string slices and returns the longest one. The lifetime annotation `'a` indicates that the returned reference will have the same lifetime as the references passed in.

### 3.2 Lifetime Elision

Rust has lifetime elision rules that allow you to omit explicit lifetime annotations in certain situations, making the syntax cleaner and easier to read.

#### Example of Lifetime Elision

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i]; // Return the first word
        }
    }

    &s[..] // Return the whole string if no space is found
}
```

In this example, the function `first_word` does not require explicit lifetime annotations because it follows the elision rules.

### Key Points:
- Ownership ensures that each value has a single owner, preventing memory issues.
- Borrowing allows you to reference values without taking ownership.
- Mutable and immutable borrowing have specific rules to ensure safety.
- Lifetimes help manage the validity of references, preventing dangling references.

## Conclusion

In this lesson, we explored Rust's ownership model, the rules of borrowing, and the concept of lifetimes. Understanding these concepts is crucial for writing safe and efficient Rust programs. By mastering ownership and borrowing, you can effectively manage memory and prevent common programming errors, ensuring that your code is both robust and performant.