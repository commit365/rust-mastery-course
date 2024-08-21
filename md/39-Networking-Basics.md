# Networking Basics

In this lesson, we will explore basic networking concepts and how to implement them in Rust. Networking is a fundamental aspect of modern applications, allowing them to communicate over the internet or local networks. We will also learn how to use the `tokio` library for asynchronous networking, enabling efficient handling of multiple connections.

## 1. Understanding Basic Networking Concepts

### 1.1 Key Networking Concepts

- **Client-Server Model**: This is a common architecture where the client requests services, and the server provides those services. For example, a web browser (client) requests a webpage from a web server.
- **IP Address**: Every device on a network has a unique IP address that identifies it.
- **Ports**: Ports are used to distinguish different services running on a device. For example, HTTP typically uses port 80, while HTTPS uses port 443.
- **Protocols**: Protocols define the rules for communication between devices. Common protocols include TCP (Transmission Control Protocol) and UDP (User Datagram Protocol).

### 1.2 TCP vs. UDP

- **TCP**: A connection-oriented protocol that ensures reliable communication. It guarantees that packets are delivered in order and without errors. This is suitable for applications where data integrity is crucial (e.g., web pages, file transfers).
- **UDP**: A connectionless protocol that does not guarantee delivery or order. It is faster and has lower overhead, making it suitable for applications like video streaming or online gaming where speed is more critical than reliability.

## 2. Setting Up a Rust Project for Networking

### 2.1 Creating a New Rust Project

Open your terminal and create a new Rust project:

```bash
cargo new async_networking
```

Navigate to your project directory:

```bash
cd async_networking
```

### 2.2 Adding Dependencies

Open the `Cargo.toml` file and add `tokio` as a dependency:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

The `full` feature enables all components of the `tokio` runtime, including TCP and UDP support.

## 3. Implementing a Simple TCP Server and Client

### 3.1 Creating a TCP Server

Open the `src/main.rs` file and add the following code to create a simple TCP server:

```rust
use tokio::net::{TcpListener, TcpStream};
use tokio::prelude::*;

#[tokio::main]
async fn main() {
    // Bind the server to an address
    let listener = TcpListener::bind("127.0.0.1:8080").await.unwrap();
    println!("Server listening on 127.0.0.1:8080");

    loop {
        // Accept incoming connections
        let (socket, _) = listener.accept().await.unwrap();
        tokio::spawn(handle_connection(socket)); // Spawn a new task to handle the connection
    }
}

async fn handle_connection(mut socket: TcpStream) {
    let mut buffer = [0; 1024]; // Buffer to hold incoming data

    // Read data from the socket
    match socket.read(&mut buffer).await {
        Ok(0) => return, // Connection closed
        Ok(n) => {
            // Echo the received data back to the client
            if let Err(e) = socket.write_all(&buffer[0..n]).await {
                eprintln!("Failed to write to socket; err = {:?}", e);
            }
        }
        Err(e) => eprintln!("Failed to read from socket; err = {:?}", e),
    }
}
```

### 3.2 Explanation of the TCP Server Code

- **TcpListener**: This struct is used to listen for incoming TCP connections on a specified address.
- **listener.accept()**: This method waits for an incoming connection and returns a `TcpStream` representing the connection.
- **tokio::spawn**: This function spawns a new asynchronous task to handle each incoming connection concurrently.
- **handle_connection**: This function reads data from the socket and echoes it back to the client.

### 3.3 Creating a TCP Client

Now, let’s create a simple TCP client that connects to our server. Add the following code to a new file named `client.rs` in the `src` directory:

```rust
use tokio::net::TcpStream;
use tokio::prelude::*;
use std::io::{self, Write};

#[tokio::main]
async fn main() {
    // Connect to the server
    let mut stream = TcpStream::connect("127.0.0.1:8080").await.unwrap();
    println!("Connected to the server");

    // Read input from the user
    let mut input = String::new();
    println!("Enter a message to send to the server:");

    io::stdin().read_line(&mut input).unwrap();

    // Send the message to the server
    stream.write_all(input.as_bytes()).await.unwrap();
    println!("Message sent!");
}
```

### 3.4 Explanation of the TCP Client Code

- **TcpStream**: This struct represents a TCP connection to a server.
- **TcpStream::connect()**: This method establishes a connection to the specified address.
- **write_all()**: This method sends the data from the input string to the server.

### 3.5 Running the Server and Client

1. Open two terminal windows.
2. In the first terminal, run the TCP server:

   ```bash
   cargo run
   ```

3. In the second terminal, run the TCP client:

   ```bash
   cargo run --bin client
   ```

4. Enter a message in the client terminal. The server should echo the message back.

## 4. Understanding Asynchronous Networking with Tokio

### 4.1 Asynchronous Programming

Asynchronous programming allows your application to handle multiple tasks concurrently without blocking. In the context of networking, this means that your application can handle multiple connections at the same time without waiting for each connection to complete.

### 4.2 Benefits of Using Tokio

- **Concurrency**: Tokio allows you to handle many connections simultaneously without the overhead of threads.
- **Performance**: Asynchronous I/O operations are more efficient, especially for I/O-bound applications.
- **Scalability**: Applications built with Tokio can scale to handle a large number of concurrent connections.

## 5. Conclusion

In this lesson, we explored basic networking concepts and learned how to implement them in Rust using the `tokio` library for asynchronous networking. We created a simple TCP server and client, demonstrating how to handle connections and communicate over the network. Understanding these networking basics and asynchronous programming will enable you to build robust and scalable network applications in Rust. As you continue to learn, consider exploring additional features of `tokio`, such as working with UDP, handling timeouts, and integrating with other asynchronous libraries.