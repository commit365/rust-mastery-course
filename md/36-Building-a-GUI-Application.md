# Building a GUI Application

In this lesson, we will get started with GUI programming in Rust using the `gtk-rs` library. We will create a simple graphical application to understand the basics of event-driven programming in Rust. GUI programming allows you to build interactive applications with graphical interfaces, enhancing user experience.

## 1. Setting Up Your Environment

### 1.1 Installing Dependencies

Before we start, you need to install the necessary dependencies for `gtk-rs`. Make sure you have the GTK development libraries installed on your system.

#### On Ubuntu:

```bash
sudo apt install libgtk-3-dev
```

#### On macOS:

You can install GTK using Homebrew:

```bash
brew install gtk+3
```

#### On Windows:

You can use the MSYS2 package manager to install GTK. Follow the instructions on the [gtk-rs website](https://gtk-rs.org/docs-src/installation).

### 1.2 Creating a New Rust Project

Open your terminal and create a new Rust project:

```bash
cargo new gtk_app
```

Navigate to your project directory:

```bash
cd gtk_app
```

### 1.3 Adding `gtk-rs` as a Dependency

Open the `Cargo.toml` file and add `gtk` as a dependency:

```toml
[dependencies]
gtk = "0.9"  # Check for the latest version on crates.io
```

## 2. Creating a Simple GUI Application

### 2.1 Writing the Application Code

Open the `src/main.rs` file and replace its contents with the following code:

```rust
use gtk::prelude::*;
use gtk::{Button, Label, Window, WindowType};

fn main() {
    // Initialize GTK
    gtk::init().expect("Failed to initialize GTK.");

    // Create a new window
    let window = Window::new(WindowType::Toplevel);
    window.set_title("Simple GTK Application");
    window.set_default_size(300, 200);

    // Create a label
    let label = Label::new(Some("Hello, GTK!"));

    // Create a button
    let button = Button::with_label("Click me!");

    // Connect the button's click event
    button.connect_clicked(move |_| {
        label.set_text("Button clicked!");
    });

    // Create a vertical box to hold the label and button
    let vbox = gtk::Box::new(gtk::Orientation::Vertical, 5);
    vbox.pack_start(&label, true, true, 0);
    vbox.pack_start(&button, true, true, 0);

    // Add the box to the window
    window.add(&vbox);

    // Connect the window's delete event to exit the application
    window.connect_delete_event(|_, _| {
        gtk::main_quit();
        Inhibit(false)
    });

    // Show all widgets
    window.show_all();

    // Start the GTK main loop
    gtk::main();
}
```

### 2.2 Explanation of the Code

- **Initialization**: We start by initializing GTK using `gtk::init()`.
- **Creating the Window**: We create a new window of type `Toplevel`, set its title, and define its default size.
- **Creating Widgets**: We create a `Label` and a `Button`. The label displays text, and the button is clickable.
- **Connecting Events**: We connect the button's click event to a closure that changes the label's text when the button is clicked.
- **Layout**: We use a vertical box (`gtk::Box`) to arrange the label and button vertically.
- **Window Management**: We connect the window's delete event to the `gtk::main_quit()` function, which exits the application when the window is closed.
- **Main Loop**: Finally, we call `gtk::main()` to start the GTK main loop, which handles events and updates the GUI.

## 3. Running the Application

To run your GTK application, execute the following command in your terminal:

```bash
cargo run
```

You should see a window with a label that says "Hello, GTK!" and a button labeled "Click me!". When you click the button, the label will change to "Button clicked!".

## 4. Understanding Event-Driven Programming

Event-driven programming is a programming paradigm where the flow of the program is determined by events, such as user actions (mouse clicks, key presses) or messages from other programs. In GTK, you define how the application responds to events using signal handlers.

### 4.1 Signals and Callbacks

In GTK, widgets emit signals when certain events occur. You can connect these signals to callback functions to define the behavior of your application.

#### Example: Connecting Signals

In our application, we connected the `clicked` signal of the button to a closure that updates the label. This allows the application to respond dynamically to user input.

## 5. Conclusion

In this lesson, we explored how to build a simple GUI application in Rust using the `gtk-rs` library. We learned about the structure of a GTK application, how to create and manage widgets, and how to handle events through signal connections. Understanding GUI programming and event-driven paradigms is essential for developing interactive applications in Rust. By leveraging libraries like `gtk-rs`, you can create rich user interfaces that enhance the user experience in your applications.