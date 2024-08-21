# Using Rust for Data Science

In this lesson, we will explore how to use Rust for data science, focusing on libraries that facilitate numerical computing and data manipulation. We will specifically look at the `ndarray` library, which provides support for N-dimensional arrays and mathematical operations, making it suitable for numerical analysis and data manipulation tasks.

## 1. Setting Up Your Environment

### 1.1 Installing Rust

If you haven't already installed Rust, you can do so using `rustup`. Open your terminal and run:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 1.2 Creating a New Rust Project

Create a new Rust project for your data science work:

```bash
cargo new rust_data_science
```

Navigate to your project directory:

```bash
cd rust_data_science
```

### 1.3 Adding Dependencies

Open the `Cargo.toml` file and add the `ndarray` crate as a dependency:

```toml
[dependencies]
ndarray = "0.15"  # Check for the latest version on crates.io
```

## 2. Exploring `ndarray`

The `ndarray` library provides a powerful way to work with N-dimensional arrays in Rust. It offers a range of functionalities for numerical computing, including array creation, manipulation, and mathematical operations.

### 2.1 Creating Arrays

You can create arrays using various methods provided by `ndarray`. Here are some examples:

#### Example: Creating a 1D Array

```rust
use ndarray::Array;

fn main() {
    // Create a 1D array
    let array_1d = Array::from_vec(vec![1, 2, 3, 4, 5]);
    println!("1D Array: {:?}", array_1d);
}
```

#### Example: Creating a 2D Array

```rust
use ndarray::Array2;

fn main() {
    // Create a 2D array (matrix)
    let array_2d = Array2::from_shape_vec((2, 3), vec![1, 2, 3, 4, 5, 6]).unwrap();
    println!("2D Array:\n{}", array_2d);
}
```

In these examples, we create both a 1D and a 2D array using `ndarray`. The `from_shape_vec` method allows you to specify the shape of the array and provide the data in a vector.

### 2.2 Accessing and Modifying Elements

You can access and modify elements in an `ndarray` using indexing.

#### Example: Accessing and Modifying Elements

```rust
fn main() {
    let mut array_2d = Array2::from_shape_vec((2, 3), vec![1, 2, 3, 4, 5, 6]).unwrap();

    // Access an element
    let element = array_2d[[0, 1]]; // Access the element at row 0, column 1
    println!("Element at (0, 1): {}", element);

    // Modify an element
    array_2d[[1, 2]] = 10; // Set the element at row 1, column 2 to 10
    println!("Modified 2D Array:\n{}", array_2d);
}
```

In this example, we access an element in the 2D array using indexing and modify another element.

### 2.3 Performing Mathematical Operations

`ndarray` supports a variety of mathematical operations, including element-wise operations, matrix multiplication, and more.

#### Example: Element-wise Operations

```rust
use ndarray::Array;

fn main() {
    let array_a = Array::from_vec(vec![1, 2, 3]);
    let array_b = Array::from_vec(vec![4, 5, 6]);

    // Perform element-wise addition
    let result = &array_a + &array_b;
    println!("Element-wise Addition: {:?}", result);
}
```

#### Example: Matrix Multiplication

```rust
use ndarray::{Array2, Array};

fn main() {
    let a = Array2::from_shape_vec((2, 2), vec![1, 2, 3, 4]).unwrap();
    let b = Array2::from_shape_vec((2, 2), vec![5, 6, 7, 8]).unwrap();

    // Perform matrix multiplication
    let result = a.dot(&b);
    println!("Matrix Multiplication:\n{}", result);
}
```

In these examples, we demonstrate how to perform element-wise addition and matrix multiplication using `ndarray`.

## 3. Data Manipulation and Analysis

### 3.1 Reshaping Arrays

You can reshape arrays to change their dimensions without changing the underlying data.

#### Example: Reshaping an Array

```rust
fn main() {
    let array = Array::from_vec(vec![1, 2, 3, 4, 5, 6]);
    let reshaped = array.into_shape((2, 3)).unwrap(); // Reshape to 2 rows and 3 columns
    println!("Reshaped Array:\n{}", reshaped);
}
```

### 3.2 Slicing and Indexing

You can slice arrays to access subarrays or specific sections of data.

#### Example: Slicing an Array

```rust
fn main() {
    let array = Array::from_vec(vec![1, 2, 3, 4, 5, 6]);
    let slice = array.slice(s![1..4]); // Get elements from index 1 to 3
    println!("Slice of Array: {:?}", slice);
}
```

### 3.3 Statistical Analysis

`ndarray` can be used for basic statistical analysis, such as calculating the mean or standard deviation.

#### Example: Calculating the Mean

```rust
fn main() {
    let array = Array::from_vec(vec![1.0, 2.0, 3.0, 4.0, 5.0]);
    let mean = array.mean().unwrap();
    println!("Mean: {}", mean);
}
```

## 4. Conclusion

In this lesson, we explored how to use Rust for data science, focusing on the `ndarray` library for numerical computing. We learned how to create and manipulate arrays, perform mathematical operations, and conduct data analysis. Rust's performance and safety make it an excellent choice for data science applications, and with libraries like `ndarray`, you can efficiently handle numerical data and perform complex computations. As you continue your journey in data science with Rust, consider exploring additional libraries such as `polars` for data manipulation and `rust-ml` for machine learning tasks.