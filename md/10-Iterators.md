# Iterators

In this lesson, we will explore iterators in Rust, which provide a powerful way to process collections. Iterators allow you to traverse through elements in a collection and perform operations on them without needing to manage the underlying indexing manually. We will also look at common iterator methods such as `map`, `filter`, and `fold` that enable you to transform and aggregate data efficiently.

## 1. Understanding Iterators

An iterator in Rust is an object that allows you to traverse through a collection, yielding each item in turn. Most collection types in Rust implement the `IntoIterator` trait, which means you can create an iterator from them.

### 1.1 Creating an Iterator

You can create an iterator by calling the `iter()` method on a collection, such as a vector or an array. This method returns an iterator that borrows the collection.

#### Example: Creating an Iterator from a Vector

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    let mut iter = numbers.iter(); // Create an iterator

    // Use the iterator to access elements
    while let Some(&number) = iter.next() {
        println!("{}", number); // Output: 1 2 3 4 5
    }
}
```

In this example, we create an iterator from a vector of numbers and use a `while let` loop to access each element until the iterator is exhausted.

## 2. Common Iterator Methods

Rust provides several useful methods for iterators that allow you to transform, filter, and aggregate data. Here are some of the most commonly used iterator methods:

### 2.1 `map`

The `map` method creates a new iterator by applying a function to each element of the original iterator. It transforms the elements according to the specified function.

#### Example: Using `map`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Use map to create a new iterator that doubles each number
    let doubled: Vec<i32> = numbers.iter().map(|&x| x * 2).collect();

    println!("{:?}", doubled); // Output: [2, 4, 6, 8, 10]
}
```

In this example, we use `map` to double each number in the vector and collect the results into a new vector.

### 2.2 `filter`

The `filter` method creates a new iterator that only includes elements that satisfy a specified condition. It applies a predicate function to each element and retains those for which the predicate returns `true`.

#### Example: Using `filter`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Use filter to create a new iterator with only even numbers
    let evens: Vec<i32> = numbers.iter().filter(|&&x| x % 2 == 0).collect();

    println!("{:?}", evens); // Output: [2, 4]
}
```

In this example, we use `filter` to retain only the even numbers from the original vector.

### 2.3 `fold`

The `fold` method is used to reduce the elements of an iterator to a single value by repeatedly applying a function. It takes an initial accumulator value and a closure that specifies how to combine the accumulator with each element.

#### Example: Using `fold`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Use fold to calculate the sum of the numbers
    let sum: i32 = numbers.iter().fold(0, |acc, &x| acc + x);

    println!("Sum: {}", sum); // Output: Sum: 15
}
```

In this example, we use `fold` to calculate the sum of all numbers in the vector, starting with an initial value of `0`.

## 3. Chaining Iterator Methods

One of the powerful features of iterators in Rust is that you can chain multiple iterator methods together. This allows for concise and expressive data processing pipelines.

#### Example: Chaining `map` and `filter`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Chain map and filter to double even numbers
    let doubled_evens: Vec<i32> = numbers.iter()
        .filter(|&&x| x % 2 == 0) // Filter even numbers
        .map(|&x| x * 2)          // Double them
        .collect();

    println!("{:?}", doubled_evens); // Output: [4, 8]
}
```

In this example, we first filter the even numbers and then double them, collecting the results into a new vector.

## 4. Consuming Iterators

Iterators in Rust are lazy, meaning they do not perform any computation until they are consumed. Common ways to consume an iterator include using methods like `collect`, `for_each`, and `count`.

### 4.1 Using `for_each`

The `for_each` method applies a closure to each element of the iterator, consuming the iterator in the process.

#### Example: Using `for_each`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Use for_each to print each number
    numbers.iter().for_each(|&x| println!("{}", x)); // Output: 1 2 3 4 5
}
```

In this example, we use `for_each` to print each number in the vector.

### 4.2 Using `count`

The `count` method consumes the iterator and returns the number of elements it contains.

#### Example: Using `count`

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];

    // Count the number of elements
    let count = numbers.iter().count();

    println!("Count: {}", count); // Output: Count: 5
}
```

In this example, we count the number of elements in the vector.

## Key Points:
- Iterators allow you to traverse collections and process elements without manual indexing.
- Common iterator methods include `map`, `filter`, and `fold`, which enable transformation, filtering, and aggregation of data.
- You can chain multiple iterator methods for concise data processing.
- Iterators are lazy and only perform computation when consumed.

## Conclusion

In this lesson, we explored iterators in Rust and learned how to use them for processing collections. We examined common iterator methods such as `map`, `filter`, and `fold`, and practiced chaining methods for efficient data manipulation. Understanding iterators is essential for writing clean and efficient Rust code, allowing you to work with collections in a powerful and expressive way.