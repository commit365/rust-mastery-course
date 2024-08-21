# Structs and Enums

In this lesson, we will explore two fundamental data structures in Rust: structs and enums. These constructs allow you to create custom data types that can model complex data more effectively. Understanding how to use structs and enums is essential for building robust Rust applications.

## 1. Defining and Using Structs

Structs are used to create custom data types that group related data together. A struct can contain multiple fields, each with its own type, allowing you to model real-world entities more accurately.

### 1.1 Defining a Struct

You define a struct using the `struct` keyword, followed by the struct name and its fields enclosed in curly braces.

#### Example: Defining a Struct

```rust
struct Person {
    name: String,
    age: u32,
}
```

In this example, we define a `Person` struct with two fields: `name`, which is a `String`, and `age`, which is a `u32`.

### 1.2 Creating an Instance of a Struct

To create an instance of a struct, you use the struct name followed by curly braces, specifying values for each field.

#### Example: Creating a Struct Instance

```rust
fn main() {
    let person = Person {
        name: String::from("Alice"),
        age: 30,
    };

    println!("Name: {}, Age: {}", person.name, person.age);
}
```

In this example, we create an instance of `Person` called `person` and print its fields.

### 1.3 Accessing Struct Fields

You can access the fields of a struct using dot notation.

#### Example: Accessing Fields

```rust
fn main() {
    let person = Person {
        name: String::from("Bob"),
        age: 25,
    };

    println!("Name: {}", person.name); // Output: Name: Bob
    println!("Age: {}", person.age);   // Output: Age: 25
}
```

### 1.4 Structs with Methods

You can also define methods for structs using `impl` blocks. This allows you to associate functions with your struct.

#### Example: Defining Methods

```rust
impl Person {
    fn greet(&self) {
        println!("Hello, my name is {} and I am {} years old.", self.name, self.age);
    }
}

fn main() {
    let person = Person {
        name: String::from("Charlie"),
        age: 28,
    };

    person.greet(); // Output: Hello, my name is Charlie and I am 28 years old.
}
```

In this example, we define a method `greet` for the `Person` struct that prints a greeting message.

## 2. Using Enums

Enums (short for "enumerations") allow you to define a type that can have multiple variants. This is useful for representing values that can take on one of several different forms.

### 2.1 Defining an Enum

You define an enum using the `enum` keyword, followed by the enum name and its variants.

#### Example: Defining an Enum

```rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}
```

In this example, we define an enum called `Direction` with four possible variants: `Up`, `Down`, `Left`, and `Right`.

### 2.2 Using Enums

You can create instances of an enum by specifying the variant you want to use.

#### Example: Creating Enum Instances

```rust
fn main() {
    let direction = Direction::Up;

    match direction {
        Direction::Up => println!("Moving up!"),
        Direction::Down => println!("Moving down!"),
        Direction::Left => println!("Moving left!"),
        Direction::Right => println!("Moving right!"),
    }
}
```

In this example, we create a variable `direction` of type `Direction` and use a `match` statement to handle each variant.

### 2.3 Enums with Data

Enums can also hold associated data for each variant. This allows you to store additional information alongside the variant.

#### Example: Enum with Data

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

fn main() {
    let msg1 = Message::Quit;
    let msg2 = Message::Move { x: 10, y: 20 };
    let msg3 = Message::Write(String::from("Hello, World!"));

    match msg2 {
        Message::Quit => println!("Quit message"),
        Message::Move { x, y } => println!("Move to x: {}, y: {}", x, y), // Accessing data
        Message::Write(text) => println!("Write message: {}", text),
    }
}
```

In this example, the `Message` enum has three variants: `Quit`, `Move`, and `Write`. The `Move` variant holds additional data in the form of an `x` and `y` coordinate.

### Key Points:
- Structs are used to create custom data types that group related data.
- You can define methods for structs using `impl` blocks.
- Enums allow you to define a type that can have multiple variants, potentially with associated data.
- Use `match` statements to handle different enum variants effectively.

## Conclusion

In this lesson, we covered how to define and use structs to create custom data types and how to use enums to represent types that can have multiple variants. Structs and enums are powerful tools in Rust that enable you to model complex data structures and improve your code's clarity and maintainability. Mastering these concepts is essential for building robust Rust applications that effectively represent real-world entities and behaviors.