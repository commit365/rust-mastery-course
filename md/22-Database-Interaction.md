# Database Interaction

In this lesson, we will explore how to interact with databases in Rust using two popular libraries: Diesel and SQLx. We will learn how to perform CRUD (Create, Read, Update, Delete) operations and manage database connections effectively. Understanding how to work with databases is essential for building data-driven applications.

## 1. Using Diesel for Database Interaction

Diesel is a powerful and safe ORM (Object Relational Mapping) library for Rust that allows you to interact with databases using Rust types and syntax.

### 1.1 Setting Up a Diesel Project

#### Step 1: Create a New Project

Open your terminal and create a new Rust project:

```bash
cargo new diesel_app
```

Navigate to your project directory:

```bash
cd diesel_app
```

#### Step 2: Add Dependencies

Open the `Cargo.toml` file and add Diesel and the necessary database driver as dependencies. For this example, we will use PostgreSQL:

```toml
[dependencies]
diesel = { version = "2.0", features = ["r2d2", "postgres"] }
dotenv = "0.15"  # For managing environment variables
```

Make sure you have the PostgreSQL database installed and running.

#### Step 3: Set Up Diesel CLI

Install the Diesel CLI tool, which helps manage database migrations and setup:

```bash
cargo install diesel_cli --no-default-features --features postgres
```

#### Step 4: Set Up the Database

Create a `.env` file in the root of your project to specify your database URL:

```plaintext
DATABASE_URL=postgres://username:password@localhost/db_name
```

Make sure to replace `username`, `password`, and `db_name` with your PostgreSQL credentials.

### 1.2 Running Diesel Setup

Run the following command to set up the Diesel project:

```bash
diesel setup
```

This command will create the necessary database and migrations folder.

### 1.3 Creating a Migration

Create a migration to define a table. For example, let’s create a `users` table:

```bash
diesel migration generate create_users
```

This command will create a new migration file in the `migrations` directory. Open the migration file and define the table schema:

```rust
// migrations/xxxxxx_create_users/up.sql

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    email VARCHAR NOT NULL UNIQUE
);
```

Run the migration to create the table:

```bash
diesel migration run
```

### 1.4 Defining the User Model

Create a new file named `models.rs` in the `src` directory and define the `User` struct:

```rust
// src/models.rs

use diesel::prelude::*;
use serde::{Serialize, Deserialize};

#[derive(Queryable, Serialize, Deserialize)]
pub struct User {
    pub id: i32,
    pub name: String,
    pub email: String,
}
```

### 1.5 Performing CRUD Operations

Now, let’s implement the CRUD operations in the `main.rs` file.

#### Example: Implementing CRUD Operations

```rust
// src/main.rs

#[macro_use]
extern crate diesel;

pub mod models;
pub mod schema;

use diesel::prelude::*;
use dotenv::dotenv;
use std::env;

use models::User;

fn establish_connection() -> PgConnection {
    dotenv().ok();
    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    PgConnection::establish(&database_url).expect(&format!("Error connecting to {}", database_url))
}

fn create_user(conn: &PgConnection, name: &str, email: &str) -> User {
    use schema::users;

    let new_user = NewUser {
        name,
        email,
    };

    diesel::insert_into(users::table)
        .values(&new_user)
        .get_result(conn)
        .expect("Error saving new user")
}

#[derive(Insertable)]
#[table_name = "users"]
struct NewUser<'a> {
    name: &'a str,
    email: &'a str,
}

fn main() {
    let connection = establish_connection();

    // Create a new user
    let user = create_user(&connection, "Alice", "alice@example.com");
    println!("Created user: {} with email: {}", user.name, user.email);
}
```

### 1.6 Updating and Deleting Users

You can implement update and delete operations similarly by using Diesel's query builder.

#### Example: Updating a User

```rust
fn update_user_email(conn: &PgConnection, user_id: i32, new_email: &str) {
    use schema::users::dsl::*;

    diesel::update(users.find(user_id))
        .set(email.eq(new_email))
        .execute(conn)
        .expect("Error updating user email");
}
```

#### Example: Deleting a User

```rust
fn delete_user(conn: &PgConnection, user_id: i32) {
    use schema::users::dsl::*;

    diesel::delete(users.find(user_id))
        .execute(conn)
        .expect("Error deleting user");
}
```

## 2. Using SQLx for Database Interaction

SQLx is an async, pure Rust SQL crate that provides compile-time checked queries. It supports multiple databases, including PostgreSQL, MySQL, and SQLite.

### 2.1 Setting Up a SQLx Project

#### Step 1: Create a New Project

Create a new Rust project for SQLx:

```bash
cargo new sqlx_app
```

Navigate to your project directory:

```bash
cd sqlx_app
```

#### Step 2: Add Dependencies

Open the `Cargo.toml` file and add SQLx and the necessary database driver:

```toml
[dependencies]
sqlx = { version = "0.5", features = ["postgres", "runtime-async-std"] }
dotenv = "0.15"
async-std = "1.10"  # For async runtime
```

### 2.2 Setting Up the Database Connection

Create a `.env` file in the root of your project to specify your database URL:

```plaintext
DATABASE_URL=postgres://username:password@localhost/db_name
```

### 2.3 Writing a Simple SQLx Application

Open the `src/main.rs` file and write the following code:

```rust
use sqlx::postgres::PgPoolOptions;
use sqlx::Row;
use serde::{Serialize, Deserialize};
use dotenv::dotenv;
use std::env;

#[derive(Serialize, Deserialize)]
struct User {
    id: i32,
    name: String,
    email: String,
}

async fn create_user(pool: &sqlx::PgPool, name: &str, email: &str) -> Result<User, sqlx::Error> {
    let row = sqlx::query("INSERT INTO users (name, email) VALUES ($1, $2) RETURNING id, name, email")
        .bind(name)
        .bind(email)
        .fetch_one(pool)
        .await?;

    Ok(User {
        id: row.get("id"),
        name: row.get("name"),
        email: row.get("email"),
    })
}

#[async_std::main]
async fn main() -> Result<(), sqlx::Error> {
    dotenv().ok();
    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    let pool = PgPoolOptions::new()
        .connect(&database_url)
        .await?;

    // Create a new user
    let user = create_user(&pool, "Alice", "alice@example.com").await?;
    println!("Created user: {} with email: {}", user.name, user.email);

    Ok(())
}
```

### 2.4 Explanation of the Code

- We use `dotenv` to load environment variables from the `.env` file.
- We establish a connection pool to the PostgreSQL database using `PgPoolOptions`.
- We define an asynchronous function `create_user` that inserts a new user into the database and returns the created user.

### 2.5 Running the SQLx Application

To run your SQLx application, execute the following command in your terminal:

```bash
cargo run
```

Ensure that the `users` table exists in your database. You can create it using the following SQL command:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR NOT NULL,
    email VARCHAR NOT NULL UNIQUE
);
```

## 3. Conclusion

In this lesson, we explored how to interact with databases in Rust using Diesel and SQLx. We learned how to perform CRUD operations and manage database connections effectively. Both Diesel and SQLx provide powerful tools for building data-driven applications in Rust, allowing you to leverage Rust's performance and safety features while working with databases. Understanding how to interact with databases is essential for developing robust applications that require persistent data storage.