# Web Development with Rust

In this lesson, we will explore web development in Rust using popular web frameworks like Rocket and Actix. We will learn how to build REST APIs and handle HTTP requests effectively. Rust’s performance and safety features make it an excellent choice for building web applications.

## 1. Getting Started with Rocket

Rocket is a web framework for Rust that makes it easy to write fast and secure web applications. It provides a simple and intuitive API for handling HTTP requests and responses.

### 1.1 Setting Up a New Rocket Project

To get started with Rocket, you need to create a new Rust project. Open your terminal and run the following command:

```bash
cargo new rocket_app
```

Navigate to your project directory:

```bash
cd rocket_app
```

### 1.2 Adding Rocket as a Dependency

Open the `Cargo.toml` file and add Rocket as a dependency:

```toml
[dependencies]
rocket = { version = "0.4", features = ["json"] }
```

The `json` feature allows us to easily work with JSON data.

### 1.3 Writing a Simple Rocket Application

Open the `src/main.rs` file and replace its contents with the following code:

```rust
#[macro_use] extern crate rocket;

use rocket::serde::json::Json;
use rocket::serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct Message {
    content: String,
}

#[get("/")]
fn index() -> Json<Message> {
    Json(Message {
        content: String::from("Hello, World!"),
    })
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![index])
}
```

### 1.4 Explanation of the Code

- We import the necessary modules from Rocket.
- We define a `Message` struct that will be serialized to JSON.
- We create a route (`/`) that returns a JSON response containing a greeting message.
- The `#[launch]` attribute indicates the entry point of the application.

### 1.5 Running the Rocket Application

To run your Rocket application, execute the following command in your terminal:

```bash
cargo run
```

You should see output indicating that the server is running. Open your web browser and navigate to `http://localhost:8000`. You should see a JSON response:

```json
{
    "content": "Hello, World!"
}
```

## 2. Getting Started with Actix

Actix is another powerful web framework for Rust, known for its performance and flexibility. It is built on top of the Actix actor framework, which allows for concurrent processing.

### 2.1 Setting Up a New Actix Project

Create a new Rust project for Actix:

```bash
cargo new actix_app
```

Navigate to your project directory:

```bash
cd actix_app
```

### 2.2 Adding Actix as a Dependency

Open the `Cargo.toml` file and add Actix as a dependency:

```toml
[dependencies]
actix-web = "4.0"
```

### 2.3 Writing a Simple Actix Application

Open the `src/main.rs` file and replace its contents with the following code:

```rust
use actix_web::{web, App, HttpServer, HttpResponse, Responder};

async fn index() -> impl Responder {
    HttpResponse::Ok().json("Hello, World!") // Respond with JSON
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().route("/", web::get().to(index)) // Define the route
    })
    .bind("127.0.0.1:8080")? // Bind to the address and port
    .run()
    .await
}
```

### 2.4 Explanation of the Code

- We import the necessary modules from Actix.
- We define an asynchronous handler function `index` that returns a JSON response.
- We set up an `HttpServer` that listens on `127.0.0.1:8080` and routes requests to the `index` function.

### 2.5 Running the Actix Application

To run your Actix application, execute the following command in your terminal:

```bash
cargo run
```

You should see output indicating that the server is running. Open your web browser and navigate to `http://127.0.0.1:8080`. You should see a JSON response:

```json
"Hello, World!"
```

## 3. Building REST APIs

Both Rocket and Actix make it easy to build REST APIs. You can define multiple routes for different HTTP methods (GET, POST, PUT, DELETE) and handle JSON data effectively.

### 3.1 Example: Building a Simple REST API with Rocket

Here’s how to extend the Rocket application to handle a POST request:

```rust
#[post("/", format = "json", data = "<message>")]
fn create_message(message: Json<Message>) -> Json<Message> {
    Json(Message {
        content: format!("Received: {}", message.content),
    })
}

#[launch]
fn rocket() -> _ {
    rocket::build()
        .mount("/", routes![index, create_message]) // Add the new route
}
```

### 3.2 Example: Building a Simple REST API with Actix

Here’s how to extend the Actix application to handle a POST request:

```rust
use actix_web::{web, App, HttpServer, HttpResponse, Responder};

#[derive(serde::Deserialize)]
struct Message {
    content: String,
}

async fn create_message(message: web::Json<Message>) -> impl Responder {
    HttpResponse::Ok().json(format!("Received: {}", message.content))
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .route("/", web::get().to(index))
            .route("/", web::post().to(create_message)) // Add the new route
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

## 4. Testing Your API

You can test your REST API using tools like Postman or cURL.

### 4.1 Testing with cURL

To test the POST endpoint, you can use the following cURL command:

```bash
curl -X POST http://localhost:8000 -H "Content-Type: application/json" -d '{"content": "Hello, Rocket!"}'
```

For Actix, use:

```bash
curl -X POST http://127.0.0.1:8080 -H "Content-Type: application/json" -d '{"content": "Hello, Actix!"}'
```

You should receive a response indicating that the message was received.

## 5. Conclusion

In this lesson, we explored web development with Rust using the Rocket and Actix frameworks. We learned how to create simple web applications, implement REST APIs, and handle HTTP requests. Rust’s performance and safety features make it an excellent choice for building robust web applications. By leveraging these frameworks, you can create efficient and secure web services that meet your application needs.