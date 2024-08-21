# Macros

In this lesson, we will explore macros in Rust, which are powerful tools for code generation and metaprogramming. Macros allow you to write more concise and reusable code by enabling you to define patterns that can be expanded into Rust code. We will focus on declarative macros and procedural macros, understanding how to write and use them effectively.

## 1. Declarative Macros

Declarative macros, also known as "macro_rules!" macros, allow you to define macros using pattern matching. They are typically used for code generation and can help reduce boilerplate code.

### 1.1 Defining a Declarative Macro

You define a declarative macro using the `macro_rules!` keyword, followed by the macro name and the patterns it should match.

#### Example: Defining a Simple Macro

```rust
macro_rules! say_hello {
    () => {
        println!("Hello, world!");
    };
}

fn main() {
    say_hello!(); // Call the macro
}
```

In this example, we define a macro named `say_hello` that prints "Hello, world!" when called. The macro takes no arguments.

### 1.2 Using Macros with Arguments

You can also define macros that accept arguments, allowing for more flexible code generation.

#### Example: Macro with Arguments

```rust
macro_rules! greet {
    ($name:expr) => {
        println!("Hello, {}!", $name);
    };
}

fn main() {
    greet!("Alice"); // Output: Hello, Alice!
    greet!("Bob");   // Output: Hello, Bob!
}
```

In this example, the `greet` macro takes a single argument `$name` and uses it to print a personalized greeting.

### 1.3 Pattern Matching in Macros

Declarative macros can match multiple patterns, allowing you to define different behaviors based on the input.

#### Example: Matching Multiple Patterns

```rust
macro_rules! calculate {
    ($a:expr, $b:expr, add) => {
        $a + $b
    };
    ($a:expr, $b:expr, subtract) => {
        $a - $b
    };
}

fn main() {
    let sum = calculate!(5, 3, add);
    let difference = calculate!(5, 3, subtract);
    
    println!("Sum: {}", sum);          // Output: Sum: 8
    println!("Difference: {}", difference); // Output: Difference: 2
}
```

In this example, the `calculate` macro can perform addition or subtraction based on the third argument.

## 2. Procedural Macros

Procedural macros are more advanced and allow you to write custom macros that operate on the syntax tree of Rust code. They enable you to generate code at compile time based on input code.

### 2.1 Types of Procedural Macros

There are three types of procedural macros:

1. **Custom Derive Macros**: Allow you to add custom behavior to structs and enums using the `#[derive]` attribute.
2. **Attribute-like Macros**: Allow you to create custom attributes that can be applied to functions, structs, or other items.
3. **Function-like Macros**: Allow you to define macros that look like function calls.

### 2.2 Creating a Custom Derive Macro

To create a custom derive macro, you need to create a new library crate. Here’s how to create a simple custom derive macro that implements the `Debug` trait.

#### Step 1: Create a New Library Crate

Run the following command to create a new library crate:

```bash
cargo new my_macro --lib
```

#### Step 2: Add Dependencies

In the `Cargo.toml` file of your new crate, add the following dependencies:

```toml
[dependencies]
quote = "1.0"    # For generating code
syn = "1.0"      # For parsing Rust code
proc-macro2 = "1.0" # For working with procedural macros

[lib]
proc-macro = true
```

#### Step 3: Implement the Macro

In `src/lib.rs`, implement the custom derive macro:

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput};

#[proc_macro_derive(Debuggable)]
pub fn debuggable(input: TokenStream) -> TokenStream {
    let input = parse_macro_input!(input as DeriveInput);
    let name = input.ident;

    let expanded = quote! {
        impl std::fmt::Debug for #name {
            fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
                f.write_str(stringify!(#name))
            }
        }
    };

    TokenStream::from(expanded)
}
```

In this example, we define a custom derive macro `Debuggable` that implements the `Debug` trait for any struct it is applied to.

#### Step 4: Using the Custom Derive Macro

Now, you can use the custom derive macro in another crate.

1. Add the new crate as a dependency in your main project’s `Cargo.toml`:

```toml
[dependencies]
my_macro = { path = "../my_macro" }  # Adjust the path as necessary
```

2. Use the macro in your code:

```rust
use my_macro::Debuggable;

#[derive(Debuggable)]
struct Person {
    name: String,
    age: u32,
}

fn main() {
    let person = Person {
        name: String::from("Alice"),
        age: 30,
    };

    println!("{:?}", person); // Output: Person
}
```

In this example, we apply the `Debuggable` macro to the `Person` struct, allowing it to implement the `Debug` trait.

## 3. Summary of Procedural Macros

- **Custom Derive Macros**: Automatically implement traits for structs and enums.
- **Attribute-like Macros**: Create custom attributes for functions and structs.
- **Function-like Macros**: Define macros that look like function calls.

## Key Points:
- **Declarative macros** allow you to define reusable code patterns using `macro_rules!`.
- **Procedural macros** enable you to write custom macros that operate on Rust syntax and generate code.
- Macros can help reduce boilerplate code and improve code readability and maintainability.

## Conclusion

In this lesson, we explored macros in Rust, focusing on declarative macros and procedural macros. We learned how to define and use macros for code generation and how to create custom derive macros to extend the functionality of structs and enums. Understanding macros is essential for writing more concise and flexible Rust code, allowing you to automate repetitive tasks and enhance your codebase's expressiveness.