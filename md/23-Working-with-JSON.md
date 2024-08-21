# Working with JSON

In this lesson, we will explore how to work with JSON data in Rust using the `serde` library. We will learn how to serialize and deserialize JSON data, as well as how to interact with JSON APIs in Rust applications. JSON (JavaScript Object Notation) is a popular data interchange format, and Rust's `serde` library provides powerful tools for working with it.

## 1. Setting Up the Project

### Step 1: Create a New Project

Open your terminal and create a new Rust project:

```bash
cargo new json_app
```

Navigate to your project directory:

```bash
cd json_app
```

### Step 2: Add Dependencies

Open the `Cargo.toml` file and add `serde` and `serde_json` as dependencies:

```toml
[dependencies]
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
```

- `serde` is the core library for serialization and deserialization.
- `serde_json` provides support for working with JSON data.

## 2. Serializing and Deserializing JSON Data

### 2.1 Defining a Struct

To work with JSON data, we first need to define a struct that represents the data structure we want to serialize or deserialize.

#### Example: Defining a Struct

```rust
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize, Debug)]
struct Person {
    name: String,
    age: u32,
}
```

In this example, we define a `Person` struct with two fields: `name` and `age`. We derive the `Serialize` and `Deserialize` traits to enable JSON serialization and deserialization.

### 2.2 Serializing a Struct to JSON

To convert a Rust struct to a JSON string, we can use the `serde_json::to_string` function.

#### Example: Serializing to JSON

```rust
fn main() {
    let person = Person {
        name: String::from("Alice"),
        age: 30,
    };

    // Serialize the struct to a JSON string
    let json_string = serde_json::to_string(&person).unwrap();
    println!("Serialized JSON: {}", json_string);
}
```

In this example, we create an instance of `Person` and serialize it to a JSON string. The `unwrap()` method is used to handle any potential errors during serialization.

### 2.3 Deserializing JSON to a Struct

To convert a JSON string back into a Rust struct, we can use the `serde_json::from_str` function.

#### Example: Deserializing from JSON

```rust
fn main() {
    let json_data = r#"{"name":"Bob","age":25}"#; // JSON string

    // Deserialize the JSON string to a Person struct
    let person: Person = serde_json::from_str(json_data).unwrap();
    println!("Deserialized Person: {:?}", person);
}
```

In this example, we define a JSON string representing a `Person` and deserialize it into a `Person` struct.

## 3. Working with JSON APIs

### 3.1 Making HTTP Requests

To interact with JSON APIs, we can use the `reqwest` library to make HTTP requests. First, add `reqwest` to your dependencies in `Cargo.toml`:

```toml
[dependencies]
reqwest = { version = "0.11", features = ["json"] }
```

### 3.2 Example: Fetching JSON Data from an API

Here’s how to make a GET request to a JSON API and deserialize the response.

#### Example: Fetching JSON Data

```rust
use reqwest::Error;

#[derive(Serialize, Deserialize, Debug)]
struct ApiResponse {
    user_id: u32,
    id: u32,
    title: String,
    completed: bool,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    // Make a GET request to a JSON API
    let response = reqwest::get("https://jsonplaceholder.typicode.com/todos/1")
        .await?
        .json::<ApiResponse>()
        .await?;

    // Print the response
    println!("Response: {:?}", response);
    Ok(())
}
```

In this example, we define a struct `ApiResponse` that matches the structure of the JSON data returned by the API. We use `reqwest` to make an asynchronous GET request to a sample API and deserialize the JSON response into the `ApiResponse` struct.

### 3.3 Running the Application

To run your application, make sure you have the `tokio` runtime set up in your `Cargo.toml`:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

Then, run your application:

```bash
cargo run
```

You should see output similar to the following:

```
Response: ApiResponse { user_id: 1, id: 1, title: "delectus aut autem", completed: false }
```

## 4. Conclusion

In this lesson, we learned how to work with JSON data in Rust using the `serde` library. We covered how to serialize and deserialize JSON data to and from Rust structs. Additionally, we explored how to interact with JSON APIs using the `reqwest` library. Working with JSON is essential for building modern web applications and services, and Rust provides powerful tools to handle it efficiently. By mastering JSON handling in Rust, you can create robust applications that communicate effectively with external services.