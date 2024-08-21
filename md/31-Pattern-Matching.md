# Pattern Matching

In this lesson, we will explore advanced pattern matching techniques in Rust. Pattern matching is a powerful feature that allows you to compare a value against a series of patterns and execute code based on which pattern matches. We will learn how to use pattern matching with enums and destructuring, enhancing our ability to write expressive and concise Rust code.

## 1. Basic Pattern Matching

Pattern matching in Rust is primarily done using the `match` statement, which allows you to compare a value against different patterns.

### Example: Basic Match Statement

```rust
fn main() {
    let number = 3;

    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        _ => println!("Not one, two, or three"), // Catch-all pattern
    }
}
```

In this example, the `match` statement checks the value of `number` and executes the corresponding block of code based on the matched pattern.

## 2. Pattern Matching with Enums

Enums are a powerful feature in Rust that allow you to define a type that can be one of several variants. Pattern matching is particularly useful when working with enums.

### 2.1 Defining an Enum

Let’s define an enum to represent different shapes:

```rust
enum Shape {
    Circle(f64),          // Circle with a radius
    Rectangle(f64, f64),  // Rectangle with width and height
    Triangle(f64, f64, f64), // Triangle with three sides
}
```

### 2.2 Matching on Enums

You can use pattern matching to handle different variants of the enum:

```rust
fn describe_shape(shape: Shape) {
    match shape {
        Shape::Circle(radius) => {
            println!("Circle with radius: {}", radius);
        }
        Shape::Rectangle(width, height) => {
            println!("Rectangle with width: {} and height: {}", width, height);
        }
        Shape::Triangle(a, b, c) => {
            println!("Triangle with sides: {}, {}, {}", a, b, c);
        }
    }
}

fn main() {
    let my_shape = Shape::Rectangle(10.0, 5.0);
    describe_shape(my_shape);
}
```

In this example, the `describe_shape` function uses pattern matching to determine the type of shape and print its properties.

## 3. Destructuring

Destructuring allows you to break down complex data structures into their components. This is particularly useful when working with tuples, structs, and enums.

### 3.1 Destructuring Tuples

You can destructure tuples directly in a `match` statement or using a `let` binding.

#### Example: Destructuring a Tuple

```rust
fn main() {
    let point = (3, 4);

    match point {
        (0, 0) => println!("Origin"),
        (x, 0) => println!("On the x-axis at {}", x),
        (0, y) => println!("On the y-axis at {}", y),
        (x, y) => println!("Point at ({}, {})", x, y),
    }
}
```

In this example, we match on the `point` tuple and destructure its components to print different messages based on its coordinates.

### 3.2 Destructuring Structs

You can also destructure structs in a similar manner.

#### Example: Destructuring a Struct

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 10, y: 20 };

    match point {
        Point { x, y: 0 } => println!("On the x-axis at {}", x),
        Point { x: 0, y } => println!("On the y-axis at {}", y),
        Point { x, y } => println!("Point at ({}, {})", x, y),
    }
}
```

In this example, we define a `Point` struct and use pattern matching to destructure its fields while matching.

## 4. Combining Patterns

You can combine multiple patterns in a single match arm using the `|` operator.

### Example: Combining Patterns

```rust
fn main() {
    let number = 2;

    match number {
        1 | 2 => println!("One or Two"),
        3 => println!("Three"),
        _ => println!("Not one, two, or three"),
    }
}
```

In this example, both `1` and `2` will match the same arm, allowing you to handle multiple cases in a concise way.

## 5. Guard Clauses

You can add guard clauses to match arms to provide additional conditions for a match.

### Example: Using Guard Clauses

```rust
fn main() {
    let number = 7;

    match number {
        n if n < 0 => println!("Negative number"),
        n if n % 2 == 0 => println!("Even number: {}", n),
        _ => println!("Odd number: {}", number),
    }
}
```

In this example, we use guard clauses to add conditions to the match arms, allowing us to handle additional cases based on the value of `number`.

## 6. Conclusion

In this lesson, we explored advanced pattern matching techniques in Rust. We learned how to use pattern matching with enums, destructuring, combining patterns, and guard clauses. Pattern matching is a powerful feature that allows you to write expressive and concise code, making it easier to handle complex data structures and control flow in your applications. By mastering pattern matching, you can improve the readability and maintainability of your Rust code.