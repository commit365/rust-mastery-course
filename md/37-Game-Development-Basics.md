# Game Development Basics

In this lesson, we will explore the basics of game development using Rust, focusing on popular game engines like Bevy and Amethyst. We will cover fundamental game development concepts, including the game loop and basic game mechanics. Understanding these concepts will provide a solid foundation for building your own games in Rust.

## 1. Getting Started with Game Development in Rust

### 1.1 Choosing a Game Engine

Rust has several game engines that make it easier to develop games. Two popular options are:

- **Bevy**: An open-source game engine that emphasizes simplicity and performance. It uses an entity-component-system (ECS) architecture, making it easy to manage game entities and their behaviors.
- **Amethyst**: A data-driven game engine that also uses an ECS architecture. It is designed for flexibility and scalability, suitable for larger projects.

For this lesson, we will focus on Bevy due to its simplicity and active community.

### 1.2 Setting Up a Bevy Project

#### Step 1: Create a New Rust Project

Open your terminal and create a new Rust project:

```bash
cargo new bevy_game
```

Navigate to your project directory:

```bash
cd bevy_game
```

#### Step 2: Add Bevy as a Dependency

Open the `Cargo.toml` file and add Bevy as a dependency:

```toml
[dependencies]
bevy = "0.10" # Check for the latest version on crates.io
```

## 2. Building a Simple Game

### 2.1 Writing the Game Code

Open the `src/main.rs` file and replace its contents with the following code:

```rust
use bevy::prelude::*;

struct Player;

fn main() {
    App::build()
        .add_plugins(DefaultPlugins) // Add default plugins
        .insert_resource(WindowDescriptor {
            title: "Bevy Game".to_string(),
            width: 800,
            height: 600,
            ..Default::default()
        })
        .add_startup_system(setup.system()) // Setup system
        .add_system(movement_system.system()) // Movement system
        .run(); // Run the app
}

fn setup(mut commands: Commands) {
    commands.spawn_bundle(OrthographicCameraBundle::new_2d()); // Create a 2D camera
    commands.spawn_bundle(SpriteBundle {
        material: Color::rgb(0.0, 0.5, 0.8).into(), // Set the color for the player
        sprite: Sprite::new(Vec2::new(50.0, 50.0)), // Set the size of the player
        ..Default::default()
    })
    .insert(Player); // Add the Player component
}

fn movement_system(
    keyboard_input: Res<Input<KeyCode>>, // Read keyboard input
    mut query: Query<(&Player, &mut Transform)>, // Query for Player entities
) {
    for (_, mut transform) in query.iter_mut() {
        if keyboard_input.pressed(KeyCode::Left) {
            transform.translation.x -= 2.0; // Move left
        }
        if keyboard_input.pressed(KeyCode::Right) {
            transform.translation.x += 2.0; // Move right
        }
        if keyboard_input.pressed(KeyCode::Up) {
            transform.translation.y += 2.0; // Move up
        }
        if keyboard_input.pressed(KeyCode::Down) {
            transform.translation.y -= 2.0; // Move down
        }
    }
}
```

### 2.2 Explanation of the Code

- **App::build()**: This initializes a new Bevy application.
- **add_plugins(DefaultPlugins)**: This adds default plugins, including window management and rendering.
- **insert_resource(WindowDescriptor)**: This sets the window title and dimensions.
- **add_startup_system(setup.system())**: This adds a system that runs once at startup to set up the game.
- **add_system(movement_system.system())**: This adds a system that runs every frame to handle player movement.
- **setup function**: This function creates a camera and a player entity with a sprite.
- **movement_system function**: This function checks for keyboard input and moves the player entity accordingly.

### 2.3 Running the Game

To run your Bevy game, execute the following command in your terminal:

```bash
cargo run
```

You should see a window with a colored square representing the player. You can move the square using the arrow keys.

## 3. Understanding the Game Loop

The game loop is a fundamental concept in game development. It is a continuous loop that updates the game state and renders the game to the screen. The loop typically consists of three main phases:

1. **Processing Input**: Capture user input (keyboard, mouse, etc.) and update the game state accordingly.
2. **Updating the Game State**: Update the positions, animations, and other properties of game entities based on the input and game logic.
3. **Rendering**: Draw the updated game state to the screen.

### 3.1 Example of a Basic Game Loop

In Bevy, the game loop is managed by the engine, and systems are called automatically. The `movement_system` function we defined earlier is called every frame, allowing us to handle input and update the player’s position.

## 4. Basic Game Mechanics

### 4.1 Adding More Game Elements

You can expand your game by adding more elements, such as enemies, collectibles, or obstacles. Each element can be represented as an entity with its own components and systems.

#### Example: Adding an Enemy

You can modify the `setup` function to spawn an enemy entity:

```rust
fn setup(mut commands: Commands) {
    commands.spawn_bundle(OrthographicCameraBundle::new_2d());
    
    // Spawn player
    commands.spawn_bundle(SpriteBundle {
        material: Color::rgb(0.0, 0.5, 0.8).into(),
        sprite: Sprite::new(Vec2::new(50.0, 50.0)),
        ..Default::default()
    })
    .insert(Player);
    
    // Spawn enemy
    commands.spawn_bundle(SpriteBundle {
        material: Color::rgb(1.0, 0.0, 0.0).into(), // Red color for the enemy
        sprite: Sprite::new(Vec2::new(50.0, 50.0)),
        ..Default::default()
    });
}
```

### 4.2 Implementing Collision Detection

You can implement collision detection by checking the positions of entities and responding when they overlap. Bevy provides various tools and plugins for handling physics and collisions.

## 5. Conclusion

In this lesson, we explored the basics of game development using Rust and the Bevy game engine. We learned how to set up a simple graphical application, understand the game loop, and implement basic game mechanics. Game development in Rust can be both fun and rewarding, and by leveraging libraries like Bevy, you can create engaging and interactive experiences. As you continue to learn, consider exploring more advanced topics such as state management, animations, and networking to enhance your game development skills.