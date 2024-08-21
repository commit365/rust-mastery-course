# Control Flow

In this lesson, we will explore control flow in Rust, which allows you to manage the execution of your code based on certain conditions. Control flow statements include conditional statements such as `if`, `else`, and `match`, as well as looping constructs like `loop`, `while`, and `for`. Understanding these concepts is essential for writing dynamic and responsive programs.

## 1. Conditional Statements

Conditional statements enable you to execute different blocks of code based on specific conditions.

### 1.1 `if` Statements

The `if` statement evaluates a condition and executes a block of code if the condition is true. You can also use `else` to specify an alternative block of code if the condition is false.

#### Example:

```rust
fn main() {
    let number = 10;

    if number > 0 {
        println!("The number is positive.");
    } else if number < 0 {
        println!("The number is negative.");
    } else {
        println!("The number is zero.");
    }
}
```

### 1.2 `match` Statements

The `match` statement is a powerful control flow operator that allows you to compare a value against a series of patterns. It is often used as a more expressive alternative to a series of `if` statements.

#### Example:

```rust
fn main() {
    let number = 3;

    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        _ => println!("Not one, two, or three"), // The underscore represents a catch-all pattern
    }
}
```

### Key Points:
- Use `if` statements for simple conditional checks.
- Use `match` statements for pattern matching and more complex conditions.

## 2. Looping Constructs

Looping constructs allow you to execute a block of code multiple times. Rust provides three main types of loops: `loop`, `while`, and `for`.

### 2.1 `loop`

The `loop` construct creates an infinite loop that continues until explicitly broken out of using the `break` statement.

#### Example:

```rust
fn main() {
    let mut count = 0;

    loop {
        count += 1;
        println!("Count: {}", count);

        if count >= 5 {
            break; // Exit the loop when count reaches 5
        }
    }
}
```

### 2.2 `while`

The `while` loop continues executing as long as a specified condition is true. It checks the condition before each iteration.

#### Example:

```rust
fn main() {
    let mut count = 0;

    while count < 5 {
        count += 1;
        println!("Count: {}", count);
    }
}
```

### 2.3 `for`

The `for` loop is used to iterate over a range or a collection. It is a more idiomatic way to loop in Rust compared to traditional `for` loops found in other languages.

#### Example: Iterating Over a Range

```rust
fn main() {
    for number in 1..6 { // Range from 1 to 5 (exclusive of 6)
        println!("Number: {}", number);
    }
}
```

#### Example: Iterating Over an Array

```rust
fn main() {
    let array = [10, 20, 30, 40, 50];

    for element in &array { // Iterate over references to the array elements
        println!("Element: {}", element);
    }
}
```

### Key Points:
- Use `loop` for infinite loops that require a break condition.
- Use `while` for loops that depend on a condition being true.
- Use `for` for iterating over ranges or collections, providing a concise and readable syntax.

## Conclusion

In this lesson, we covered control flow in Rust, focusing on conditional statements (`if`, `else`, and `match`) and looping constructs (`loop`, `while`, and `for`). Mastering these control flow mechanisms will allow you to write dynamic and responsive Rust programs that can handle various conditions and iterate over data efficiently. Understanding how to control the flow of your program is a fundamental skill for any programmer.