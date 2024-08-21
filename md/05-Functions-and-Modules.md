# Functions and Modules

In this lesson, we will explore how to define and call functions in Rust, including how to use parameters and return values. Additionally, we will learn how to organize our code using modules, which helps manage complexity and improve code readability. Understanding functions and modules is essential for writing clean and maintainable Rust programs.

## 1. Defining and Calling Functions

Functions are a fundamental building block in Rust. They allow you to encapsulate code that performs a specific task, making your code more modular and reusable.

### 1.1 Defining Functions

You can define a function using the `fn` keyword, followed by the function name, parameters, and the return type (if any). Here’s the basic syntax:

```rust
fn function_name(parameter1: Type1, parameter2: Type2) -> ReturnType {
    // Function body
}
```

#### Example: A Simple Function

```rust
fn greet(name: &str) {
    println!("Hello, {}!", name);
}
```

### 1.2 Calling Functions

To call a function, simply use its name followed by parentheses, including any required arguments.

#### Example: Calling the `greet` Function

```rust
fn main() {
    greet("Alice"); // Output: Hello, Alice!
}
```

### 1.3 Functions with Parameters and Return Values

Functions can take parameters and return values. To specify the return type, use the `->` operator followed by the type.

#### Example: A Function with Return Value

```rust
fn add(x: i32, y: i32) -> i32 {
    x + y // The last expression is the return value
}

fn main() {
    let sum = add(5, 10);
    println!("The sum is: {}", sum); // Output: The sum is: 15
}
```

### Key Points:
- Use the `fn` keyword to define a function.
- Functions can take parameters and return values.
- Call functions by using their name followed by parentheses.

## 2. Organizing Code Using Modules

Modules in Rust are used to organize code into separate namespaces, making it easier to manage larger codebases. A module can contain functions, structs, enums, and other modules.

### 2.1 Defining a Module

You can define a module using the `mod` keyword. The contents of a module can be defined inline or in a separate file.

#### Example: Defining a Module Inline

```rust
mod math {
    pub fn multiply(x: i32, y: i32) -> i32 {
        x * y
    }
}
```

### 2.2 Using a Module

To use a function from a module, you need to specify the module name followed by `::` and the function name.

#### Example: Using the `multiply` Function

```rust
fn main() {
    let result = math::multiply(4, 5);
    println!("The result is: {}", result); // Output: The result is: 20
}
```

### 2.3 Making Functions Public

By default, functions and items in a module are private. To make them accessible outside the module, you need to use the `pub` keyword.

#### Example: Public Function

```rust
mod utils {
    pub fn square(x: i32) -> i32 {
        x * x
    }
}

fn main() {
    let num = 3;
    let squared = utils::square(num);
    println!("The square of {} is: {}", num, squared); // Output: The square of 3 is: 9
}
```

### 2.4 Organizing Modules in Separate Files

For larger projects, you can organize modules in separate files. Create a directory with the same name as the module, and inside that directory, create a `mod.rs` file or individual files for each submodule.

#### Example: Directory Structure

```
src/
├── main.rs
└── utils/
    ├── mod.rs
    └── math.rs
```

In `mod.rs`, you can declare submodules:

```rust
pub mod math; // Declares the math module
```

In `math.rs`, define the functions:

```rust
pub fn add(x: i32, y: i32) -> i32 {
    x + y
}
```

### Key Points:
- Use the `mod` keyword to define a module.
- Access module functions using the `::` syntax.
- Use `pub` to make functions and items public.
- Organize larger modules in separate files for better structure.

## Conclusion

In this lesson, we covered how to define and call functions in Rust, including the use of parameters and return values. We also explored how to organize code using modules, which helps improve code readability and maintainability. Mastering functions and modules is essential for writing structured and efficient Rust programs, allowing you to create reusable components and manage complexity effectively.