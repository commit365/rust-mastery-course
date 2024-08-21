# WebAssembly with Rust

In this lesson, we will learn how to compile Rust code to WebAssembly (Wasm) and explore the use cases for WebAssembly in web development. WebAssembly is a binary instruction format that allows code written in languages like Rust to run in web browsers at near-native speed. This enables developers to create high-performance web applications with Rust.

## 1. Setting Up Your Environment for WebAssembly

### 1.1 Installing the Required Tools

To compile Rust to WebAssembly, you need to have the following tools installed:

1. **Rust**: Ensure you have Rust installed. You can install Rust using `rustup`:

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **wasm-pack**: This is a tool that helps build Rust-generated WebAssembly packages. Install it using:

   ```bash
   cargo install wasm-pack
   ```

3. **Node.js** (optional): If you want to serve your WebAssembly module locally, you can install Node.js. You can download it from [nodejs.org](https://nodejs.org/).

### 1.2 Creating a New Rust Project

Open your terminal and create a new Rust project:

```bash
cargo new wasm_example --lib
```

Navigate to your project directory:

```bash
cd wasm_example
```

### 1.3 Configuring the Project for WebAssembly

Open the `Cargo.toml` file and add the following configuration:

```toml
[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2" # Check for the latest version on crates.io
```

- The `crate-type = ["cdylib"]` line tells Cargo to compile the library as a dynamic library suitable for use with WebAssembly.
- The `wasm-bindgen` crate allows you to interact with JavaScript from Rust.

## 2. Writing Rust Code for WebAssembly

### 2.1 Writing a Simple Function

Open the `src/lib.rs` file and write a simple Rust function that we will expose to JavaScript:

```rust
use wasm_bindgen::prelude::*;

// This function will be accessible from JavaScript
#[wasm_bindgen]
pub fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}
```

In this example, we define a function `greet` that takes a string and returns a greeting.

### 2.2 Compiling to WebAssembly

To compile your Rust code to WebAssembly, run the following command:

```bash
wasm-pack build --target web
```

This command compiles the Rust code into WebAssembly and generates the necessary JavaScript bindings for use in web applications. The output will be in the `pkg` directory.

## 3. Using WebAssembly in a Web Application

### 3.1 Setting Up a Simple Web Project

Create a new directory for your web project:

```bash
mkdir web_app
cd web_app
```

### 3.2 Creating an HTML File

Create an `index.html` file in the `web_app` directory with the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wasm Example</title>
    <script type="module">
        import init, { greet } from '../wasm_example/pkg/wasm_example.js';

        async function run() {
            await init(); // Initialize the WebAssembly module
            const name = "World";
            const greeting = greet(name);
            document.getElementById("greeting").innerText = greeting;
        }

        run();
    </script>
</head>
<body>
    <h1 id="greeting">Loading...</h1>
</body>
</html>
```

### 3.3 Serving the Web Application

You can use a simple HTTP server to serve your web application. If you have Node.js installed, you can use the `http-server` package:

1. Install `http-server` globally:

   ```bash
   npm install -g http-server
   ```

2. Run the server in the `web_app` directory:

   ```bash
   http-server .
   ```

3. Open your web browser and navigate to `http://localhost:8080`. You should see a message that says "Hello, World!" displayed on the page.

## 4. Use Cases for WebAssembly in Web Development

WebAssembly opens up many possibilities in web development. Here are some common use cases:

### 4.1 Performance-Critical Applications

WebAssembly is ideal for applications that require high performance, such as:

- **Games**: WebAssembly can run complex game logic and graphics rendering efficiently in the browser.
- **Image and Video Processing**: Applications that manipulate images or videos can benefit from the speed of WebAssembly.

### 4.2 Porting Existing Applications

You can port existing applications written in languages like C, C++, or Rust to the web using WebAssembly. This allows you to leverage existing codebases and libraries in web applications.

### 4.3 Computationally Intensive Tasks

WebAssembly is suitable for tasks that involve heavy computation, such as:

- **Scientific Simulations**: Running simulations that require significant processing power.
- **Data Analysis**: Performing complex data analysis directly in the browser.

### 4.4 Enhancing JavaScript Applications

WebAssembly can be used alongside JavaScript to enhance existing applications. You can offload performance-critical tasks to WebAssembly while using JavaScript for the user interface and other less intensive tasks.

## 5. Conclusion

In this lesson, we learned how to compile Rust code to WebAssembly and explored the use cases for WebAssembly in web development. We set up a simple web application that uses a Rust function compiled to WebAssembly, demonstrating how to interact with JavaScript. WebAssembly is a powerful tool that allows developers to build high-performance web applications using Rust and other languages, expanding the possibilities of what can be achieved in the browser. By understanding and leveraging WebAssembly, you can create more efficient and capable web applications.