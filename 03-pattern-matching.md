# Pattern Matching: Rust's Swiss Army Knife 🔪

Pattern matching in Rust is like having X-ray vision for your data. It lets you peek inside, extract what you need, and handle every possibility.

## 🎯 The Big Idea

Think of pattern matching like a sorting machine at a recycling center - it looks at each item, identifies what it is, and routes it to the right place.

## 🎰 The `match` Statement

The basic form:
```rust
match value {
    pattern1 => result1,
    pattern2 => result2,
    _ => default_result,  // Catch-all
}
```

### Simple Example: Matching Numbers
```rust
let number = 3;
match number {
    1 => println!("One"),
    2 => println!("Two"),
    3 => println!("Three"),    // This matches!
    _ => println!("Something else"),
}
```

## 📦 Destructuring: Unpacking Data

### Unpacking Tuples
```rust
let point = (3, 5);
match point {
    (0, 0) => println!("Origin"),
    (x, 0) => println!("On X axis at {}", x),
    (0, y) => println!("On Y axis at {}", y),
    (x, y) => println!("At ({}, {})", x, y),  // Captures both values
}
```

### Unpacking Structs
```rust
struct User {
    name: String,
    age: u32,
}

let user = User { name: "Alice".into(), age: 30 };

match user {
    User { age: 0..=17, name } => println!("{} is a minor", name),
    User { age: 18, name } => println!("{} just became an adult!", name),
    User { age, name } => println!("{} is {} years old", name, age),
}
```

## 🎲 Matching Enums (The Killer Feature)

This is where pattern matching shines:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(u8, u8, u8),
}

let msg = Message::Move { x: 10, y: 20 };

match msg {
    Message::Quit => println!("Quitting"),
    Message::Move { x, y } => println!("Moving to ({}, {})", x, y),
    Message::Write(text) => println!("Text: {}", text),
    Message::ChangeColor(r, g, b) => println!("RGB: {}/{}/{}", r, g, b),
}
```

## 🔧 Common Patterns You'll See

### Pattern 1: Option Matching
```rust
let maybe_number = Some(42);

match maybe_number {
    Some(n) => println!("Got number: {}", n),
    None => println!("Got nothing"),
}

// Shorthand with if let
if let Some(n) = maybe_number {
    println!("Got number: {}", n);
}
```

### Pattern 2: Result Matching
```rust
let result: Result<i32, String> = Ok(42);

match result {
    Ok(value) => println!("Success: {}", value),
    Err(error) => println!("Error: {}", error),
}
```

### Pattern 3: Guards (Extra Conditions)
```rust
let num = 4;

match num {
    n if n < 0 => println!("Negative"),
    n if n == 0 => println!("Zero"),
    n if n > 0 && n < 10 => println!("Small positive"),
    _ => println!("Large positive"),
}
```

### Pattern 4: Multiple Patterns
```rust
let x = 2;

match x {
    1 | 2 | 3 => println!("One, two, or three"),  // Multiple options
    4..=10 => println!("Four through ten"),        // Range
    _ => println!("Something else"),
}
```

## 🎪 Advanced Patterns

### Ignoring Values
```rust
let point = (3, 5, 7);

match point {
    (x, _, z) => println!("x={}, z={}", x, z),  // Ignore middle value
}

// Ignoring multiple with ..
let numbers = (1, 2, 3, 4, 5);
match numbers {
    (first, .., last) => println!("First: {}, Last: {}", first, last),
}
```

### Reference Patterns
```rust
let reference = &42;

match reference {
    &val => println!("Got value: {}", val),  // Dereference pattern
}

// Or match the reference itself
match reference {
    val => println!("Got reference to: {}", val),
}
```

### @ Bindings (Capture and Test)
```rust
let age = 25;

match age {
    n @ 13..=19 => println!("Teen of age {}", n),  // Test range AND capture
    n @ 20..=29 => println!("Twenties: {}", n),
    n => println!("Age: {}", n),
}
```

## 🚀 `if let` and `while let`

Sometimes a full `match` is overkill:

### `if let` - Single Pattern
```rust
let some_value = Some(3);

// Instead of:
match some_value {
    Some(x) => println!("Got {}", x),
    None => {},
}

// Use:
if let Some(x) = some_value {
    println!("Got {}", x);
}
```

### `while let` - Loop Until Pattern Fails
```rust
let mut stack = vec![1, 2, 3];

while let Some(top) = stack.pop() {
    println!("Popped: {}", top);
}
// Continues until pop() returns None
```

## 🔍 Quick Recognition Guide

| Pattern | What It Matches | Example |
|---------|-----------------|---------|
| `_` | Anything (ignore) | `_ => "default"` |
| `x` | Anything (capture) | `x => println!("{}", x)` |
| `42` | Exact value | `42 => "the answer"` |
| `1..=5` | Range inclusive | `1..=5 => "one to five"` |
| `Some(x)` | Some with value | `Some(n) => n * 2` |
| `Ok(x)` | Success with value | `Ok(data) => process(data)` |
| `(x, y)` | Tuple elements | `(a, b) => a + b` |
| `Struct { field }` | Struct fields | `User { name } => name` |

## 🚩 Red Flags & Gotchas

### Must Be Exhaustive
```rust
enum Color { Red, Green, Blue }

let color = Color::Red;
match color {
    Color::Red => "red",
    Color::Green => "green",
    // ❌ ERROR: Non-exhaustive patterns: Blue not covered
}
```

### Pattern Matching Moves Values
```rust
let maybe_string = Some(String::from("Hello"));

match maybe_string {
    Some(s) => println!("{}", s),  // s is moved here
    None => println!("Nothing"),
}
// maybe_string is no longer usable (unless you match on &maybe_string)
```

### Order Matters
```rust
let x = 5;
match x {
    _ => println!("Catch all"),   // ⚠️ This catches everything!
    5 => println!("Five"),        // Unreachable!
}
```

## 💡 Mental Model

Think of pattern matching as a series of questions:
1. **What shape is this data?** (Enum variant, Some/None, Ok/Err)
2. **What values does it contain?** (Extract with patterns)
3. **What conditions must it meet?** (Guards with `if`)
4. **What do I do with it?** (The `=>` expression)

## 🎨 Real-World Example

Here's pattern matching in action:
```rust
fn process_message(msg: &str) -> Result<String, String> {
    match msg.len() {
        0 => Err("Empty message".into()),
        1..=10 => Ok(format!("Short: {}", msg)),
        11..=50 => Ok(format!("Medium: {}", msg)),
        _ => Ok(format!("Long: {}...", &msg[..20])),
    }
}

fn handle_response(response: Result<String, String>) {
    match response {
        Ok(data) if data.starts_with("Short") => {
            println!("Got a short message: {}", data)
        },
        Ok(data) => println!("Success: {}", data),
        Err(e) => eprintln!("Error: {}", e),
    }
}
```

## ✅ Quick Check

Can you understand this?
```rust
let value = Some((3, "hello"));

match value {
    Some((n, s)) if n > 0 => println!("Positive {} with {}", n, s),
    Some((0, s)) => println!("Zero with {}", s),
    Some((n, _)) => println!("Negative {}", n),
    None => println!("Nothing"),
}
```

If you can follow the flow, you've got pattern matching down!

---

**Next: [Error Handling](04-error-handling.md)** - How Rust forces you to handle errors (and why that's good) →