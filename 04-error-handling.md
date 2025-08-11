# Error Handling: No More Null Pointer Exceptions 🛡️

Rust doesn't have null. Instead, it has two types that make you explicitly handle the absence of values and errors. This is Rust's secret to reliability.

## 🎯 The Big Idea

Imagine if every function came with a warning label: "This might fail" or "This might not have a value." That's Rust's `Result` and `Option` types.

## 📦 Option: Something or Nothing

`Option<T>` means "I might have a T, or I might have nothing."

```rust
enum Option<T> {
    Some(T),  // We have a value
    None,     // We have nothing
}
```

### Real-World Analogy
Think of `Option` like a gift box:
- `Some(gift)` = There's something inside 🎁
- `None` = The box is empty 📦

### Common Option Patterns

```rust
// Finding something that might not exist
let users = vec!["Alice", "Bob"];
let first = users.get(0);  // Returns Option<&str>

match first {
    Some(name) => println!("First user: {}", name),
    None => println!("No users found"),
}

// Shorter with if let
if let Some(name) = users.get(0) {
    println!("First user: {}", name);
}
```

## 🎲 Result: Success or Failure

`Result<T, E>` means "I'll either succeed with T or fail with error E."

```rust
enum Result<T, E> {
    Ok(T),   // Success with value
    Err(E),  // Failure with error
}
```

### Real-World Analogy
Think of `Result` like ordering food:
- `Ok(meal)` = Your order arrived 🍕
- `Err(problem)` = Something went wrong 🚫

### Common Result Patterns

```rust
// Parsing might fail
let input = "42";
let number: Result<i32, _> = input.parse();

match number {
    Ok(n) => println!("Parsed: {}", n),
    Err(e) => println!("Failed to parse: {}", e),
}
```

## ❓ The ? Operator: Error Propagation

The `?` operator is Rust's error-handling superpower. It means "if this is an error, return it immediately."

```rust
fn read_username() -> Result<String, io::Error> {
    let mut file = File::open("user.txt")?;  // Returns early if error
    let mut username = String::new();
    file.read_to_string(&mut username)?;     // Returns early if error
    Ok(username)                             // Success!
}

// Without ?, you'd write:
fn read_username_verbose() -> Result<String, io::Error> {
    let mut file = match File::open("user.txt") {
        Ok(f) => f,
        Err(e) => return Err(e),  // Manual early return
    };
    // ... and so on
}
```

### Think of ? as "Try This"
- `operation()?` = "Try this operation, bail if it fails"
- Only works in functions that return `Result` or `Option`

## 🛠️ Common Methods on Option and Result

### Option Methods

| Method | What It Does | Example |
|--------|-------------|---------|
| `.unwrap()` | Get value or panic | `Some(5).unwrap() // Returns 5` |
| `.unwrap_or(default)` | Get value or default | `None.unwrap_or(0) // Returns 0` |
| `.unwrap_or_else(\|\| ...)` | Get value or compute default | `None.unwrap_or_else(\|\| expensive())` |
| `.map(\|x\| ...)` | Transform if Some | `Some(5).map(\|x\| x * 2) // Some(10)` |
| `.and_then(\|x\| ...)` | Chain operations | `Some(5).and_then(\|x\| Some(x * 2))` |
| `.is_some()` / `.is_none()` | Check variant | `Some(5).is_some() // true` |
| `.take()` | Take value, leave None | `let val = option.take()` |

### Result Methods

| Method | What It Does | Example |
|--------|-------------|---------|
| `.unwrap()` | Get value or panic | `Ok(5).unwrap() // Returns 5` |
| `.unwrap_or(default)` | Get value or default | `Err("oh no").unwrap_or(0)` |
| `.expect("msg")` | Unwrap with custom panic | `result.expect("Failed to open")` |
| `.map(\|x\| ...)` | Transform if Ok | `Ok(5).map(\|x\| x * 2) // Ok(10)` |
| `.map_err(\|e\| ...)` | Transform error | `Err(5).map_err(\|e\| e.to_string())` |
| `.and_then(\|x\| ...)` | Chain Results | `Ok(5).and_then(\|x\| Ok(x * 2))` |
| `.is_ok()` / `.is_err()` | Check variant | `Ok(5).is_ok() // true` |
| `.ok()` | Convert to Option | `Ok(5).ok() // Some(5)` |

## 🎨 Real-World Patterns

### Pattern 1: Early Returns with ?
```rust
fn process_file(path: &str) -> Result<String, Box<dyn Error>> {
    let contents = fs::read_to_string(path)?;
    let processed = contents.trim().to_uppercase();
    Ok(processed)
}
```

### Pattern 2: Combining Multiple Results
```rust
fn get_two_numbers() -> Result<(i32, i32), String> {
    let first = "10".parse::<i32>().map_err(|_| "First failed")?;
    let second = "20".parse::<i32>().map_err(|_| "Second failed")?;
    Ok((first, second))
}
```

### Pattern 3: Option to Result
```rust
fn find_user(id: u32) -> Result<User, String> {
    users.get(id)
        .ok_or_else(|| format!("User {} not found", id))
}
```

### Pattern 4: Chaining with and_then
```rust
fn parse_and_double(input: &str) -> Option<i32> {
    input.parse::<i32>()
        .ok()                    // Result -> Option
        .and_then(|n| Some(n * 2))  // Transform if Some
}
```

## 🚩 Red Flags & Anti-Patterns

### Overusing unwrap()
```rust
// 🚩 BAD: Will panic if file doesn't exist
let contents = fs::read_to_string("file.txt").unwrap();

// ✅ BETTER: Handle the error
let contents = fs::read_to_string("file.txt")
    .unwrap_or_else(|_| String::from("default content"));

// ✅ OR: Propagate the error
let contents = fs::read_to_string("file.txt")?;
```

### Nested Match Hell
```rust
// 🚩 BAD: Deeply nested matches
match result1 {
    Ok(val1) => {
        match result2 {
            Ok(val2) => {
                // Do something
            },
            Err(e) => // Handle
        }
    },
    Err(e) => // Handle
}

// ✅ BETTER: Use ? operator
let val1 = result1?;
let val2 = result2?;
// Do something
```

### Ignoring Errors
```rust
// 🚩 BAD: Silently ignoring errors
let _ = fs::remove_file("temp.txt");  // Error ignored!

// ✅ BETTER: At least log it
if let Err(e) = fs::remove_file("temp.txt") {
    eprintln!("Failed to remove temp file: {}", e);
}
```

## 💡 Mental Models

### Option = Maybe Box
- Check if empty before using
- Can transform contents while keeping box
- Can provide default if empty

### Result = Delivery Status
- Either package arrived (Ok) or delivery failed (Err)
- Can transform package if arrived
- Can pass along delivery failures with ?

### The ? Operator = Delegation
- "I can't handle this error, my caller should deal with it"
- Like forwarding an email you can't answer

## 🔍 Quick Recognition Guide

| Pattern | Meaning |
|---------|---------|
| `Option<T>` | Might not have a value |
| `Result<T, E>` | Might fail with error |
| `?` | Propagate error up |
| `.unwrap()` | 🚩 Get value or panic |
| `.expect("msg")` | 🚩 Get value or panic with message |
| `.unwrap_or(default)` | Safe default value |
| `if let Some(x) = ...` | Handle only success case |
| `match ... { Ok/Some => ..., Err/None => ... }` | Handle both cases |

## 🎯 When Reading Rust Code

1. **See `Result` or `Option`?** → Something might fail/not exist
2. **See `?`?** → Errors bubble up to caller
3. **See `.unwrap()`?** → Potential panic point (be careful!)
4. **See `match` on Result/Option?** → Explicit error handling
5. **See `.map()` or `.and_then()`?** → Transforming success values

## ✅ Quick Check

Can you follow this error handling?
```rust
fn get_weather(city: &str) -> Result<String, String> {
    let data = fetch_data(city)?;  // Might fail
    let temp = data.get("temperature")
        .ok_or("No temperature found")?;  // Convert Option to Result
    
    Ok(format!("{}°C", temp))
}
```

If you understand how errors flow through this function, you've got Rust error handling!

---

**Next: [Traits & Generics](05-traits-generics.md)** - How Rust shares behavior between types →