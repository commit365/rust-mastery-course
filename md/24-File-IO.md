# File I/O

In this lesson, we will explore how to perform file input and output (I/O) operations in Rust. We will learn how to read from and write to files, as well as how to handle errors that may occur during these operations. File I/O is essential for many applications, allowing you to store and retrieve data from the filesystem.

## 1. Setting Up the Project

### Step 1: Create a New Project

Open your terminal and create a new Rust project:

```bash
cargo new file_io_app
```

Navigate to your project directory:

```bash
cd file_io_app
```

## 2. Writing to a File

### 2.1 Using the `std::fs` Module

Rust provides the `std::fs` module for file operations. To write to a file, you can use the `File::create` function to create a new file or open an existing file for writing.

#### Example: Writing to a File

Open the `src/main.rs` file and replace its contents with the following code:

```rust
use std::fs::File; // Import the File struct
use std::io::{self, Write}; // Import the io module and the Write trait

fn main() -> io::Result<()> {
    // Create a new file or truncate an existing file
    let mut file = File::create("output.txt")?; // The ? operator handles errors

    // Write data to the file
    file.write_all(b"Hello, Rust!\n")?; // Write a byte string
    file.write_all(b"File I/O is easy with Rust.\n")?;

    println!("Data written to file successfully.");
    Ok(())
}
```

### 2.2 Explanation of the Code

- We use `File::create` to create a new file named `output.txt`. If the file already exists, it will be truncated (emptied).
- We use the `write_all` method to write byte strings to the file. The `?` operator is used to propagate any errors that may occur during file operations.
- The `main` function returns `io::Result<()>`, which allows us to handle errors gracefully.

### 2.3 Running the Application

To run your application, execute the following command in your terminal:

```bash
cargo run
```

After running the application, check the project directory for the `output.txt` file. It should contain the following text:

```
Hello, Rust!
File I/O is easy with Rust.
```

## 3. Reading from a File

### 3.1 Using the `std::fs` Module for Reading

To read from a file, you can use the `File::open` function to open an existing file and then read its contents.

#### Example: Reading from a File

Add the following code to your `src/main.rs` file:

```rust
use std::fs::File; // Import the File struct
use std::io::{self, Read}; // Import the io module and the Read trait

fn main() -> io::Result<()> {
    // Create and write to the file
    let mut file = File::create("output.txt")?;
    file.write_all(b"Hello, Rust!\n")?;
    file.write_all(b"File I/O is easy with Rust.\n")?;

    // Read from the file
    let mut contents = String::new(); // Create a String to hold the file contents
    let mut file = File::open("output.txt")?; // Open the file for reading
    file.read_to_string(&mut contents)?; // Read the file contents into the String

    println!("File contents:\n{}", contents); // Print the contents
    Ok(())
}
```

### 3.2 Explanation of the Code

- We create a `String` to hold the contents of the file.
- We use `File::open` to open the `output.txt` file for reading.
- The `read_to_string` method reads the entire contents of the file into the `contents` string.
- Finally, we print the contents of the file.

### 3.3 Running the Application

Run your application again:

```bash
cargo run
```

You should see the following output:

```
Data written to file successfully.
File contents:
Hello, Rust!
File I/O is easy with Rust.
```

## 4. Error Handling in File Operations

Error handling is crucial when performing file I/O operations. Rust uses the `Result` type to indicate success or failure. When an error occurs, you can use the `?` operator to propagate the error or handle it explicitly.

### 4.1 Handling Errors Explicitly

You can handle errors explicitly using a `match` statement or the `if let` syntax.

#### Example: Handling Errors with `match`

```rust
fn main() {
    match File::open("non_existent_file.txt") {
        Ok(mut file) => {
            let mut contents = String::new();
            file.read_to_string(&mut contents).unwrap();
            println!("File contents:\n{}", contents);
        }
        Err(e) => {
            println!("Error opening file: {}", e);
        }
    }
}
```

In this example, we attempt to open a non-existent file and handle the error by printing a message.

### 4.2 Using `if let` for Error Handling

You can also use `if let` to handle errors in a more concise way:

```rust
fn main() {
    if let Err(e) = File::open("non_existent_file.txt") {
        println!("Error opening file: {}", e);
    }
}
```

## Key Points:
- Use the `std::fs` module for file I/O operations in Rust.
- Use the `File::create` method to write to files and `File::open` to read from files.
- Handle errors using the `Result` type and the `?` operator, or explicitly using `match` or `if let`.

## Conclusion

In this lesson, we explored how to perform file I/O operations in Rust, including reading from and writing to files. We learned how to handle errors effectively during file operations. Understanding file I/O is essential for building applications that need to store and retrieve data from the filesystem, and Rust provides powerful tools to manage these operations safely and efficiently.