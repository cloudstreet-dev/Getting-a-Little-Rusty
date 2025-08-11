# Common Patterns: Idiomatic Rust You'll See Everywhere 🎨

These are the patterns that make Rust code "Rusty." Recognizing them will help you read real-world Rust code quickly.

## 🔄 Iterator Patterns

Rust loves iterators. They're everywhere.

### The Iterator Chain Pattern
```rust
let result: Vec<i32> = numbers
    .iter()                    // Create iterator
    .filter(|n| n % 2 == 0)    // Keep even numbers
    .map(|n| n * 2)            // Double them
    .take(5)                   // Take first 5
    .collect();                // Gather into collection
```

**Think of it as:** A data pipeline where each step transforms the flow.

### Common Iterator Methods

| Method | What It Does | Example |
|--------|-------------|---------|
| `.iter()` | Borrow elements | `vec.iter()` |
| `.into_iter()` | Take ownership | `vec.into_iter()` |
| `.iter_mut()` | Mutably borrow | `vec.iter_mut()` |
| `.map()` | Transform each | `.map(\|x\| x * 2)` |
| `.filter()` | Keep matching | `.filter(\|x\| x > 0)` |
| `.fold()` | Reduce to single value | `.fold(0, \|acc, x\| acc + x)` |
| `.collect()` | Gather into collection | `.collect::<Vec<_>>()` |
| `.zip()` | Pair up two iterators | `a.iter().zip(b.iter())` |
| `.enumerate()` | Add index | `.enumerate()` |
| `.flatten()` | Flatten nested | `[[1],[2]].flatten()` |

### The Question Mark Pattern with Iterators
```rust
fn process_all(items: Vec<String>) -> Result<Vec<i32>, ParseError> {
    items.iter()
        .map(|s| s.parse::<i32>())  // Returns Result<i32, ParseError>
        .collect()                   // collect() handles the Results!
}
```

## 🏗️ Builder Pattern

Used for constructing complex objects step by step:

```rust
let client = Client::builder()
    .timeout(Duration::from_secs(30))
    .max_retries(3)
    .user_agent("my-app/1.0")
    .build()?;

// You'll see this with:
// - HTTP clients
// - Database connections  
// - Complex configurations
```

**Recognition:** Chains of method calls ending with `.build()` or `.finish()`

## 🎁 NewType Pattern

Wrapping a type to give it new meaning:

```rust
struct UserId(u64);  // Not just any u64, it's a UserId
struct Meters(f64);  // Not just any f64, it's meters

impl UserId {
    fn new(id: u64) -> Self {
        UserId(id)
    }
}
```

**Why:** Type safety without runtime cost.

## 🔒 Interior Mutability Pattern

Mutating data through immutable references using `Cell`, `RefCell`, or `Mutex`:

```rust
use std::cell::RefCell;

struct Counter {
    count: RefCell<i32>,  // Can mutate even through & reference
}

impl Counter {
    fn increment(&self) {  // Note: &self, not &mut self
        *self.count.borrow_mut() += 1;
    }
}
```

**When you see:** `RefCell`, `Cell`, `Mutex`, `RwLock`  
**It means:** "I need to mutate but can't get a mutable reference"

## 🎯 Type State Pattern

Using types to encode state:

```rust
struct Door<State> {
    phantom: PhantomData<State>,
}

struct Open;
struct Closed;

impl Door<Closed> {
    fn open(self) -> Door<Open> {  // Can only open closed doors
        Door { phantom: PhantomData }
    }
}

impl Door<Open> {
    fn close(self) -> Door<Closed> {  // Can only close open doors
        Door { phantom: PhantomData }
    }
}
```

**Recognition:** Types that change based on operations.

## 🔄 From/Into Pattern

Converting between types:

```rust
impl From<String> for MyError {
    fn from(s: String) -> Self {
        MyError::Message(s)
    }
}

// Now you can:
let error: MyError = "Something went wrong".to_string().into();
// Or with ?:
let result = something_that_returns_string_error()?;  // Auto-converts!
```

**Rule:** Implement `From`, get `Into` for free.

## 📝 Default Implementation Pattern

```rust
#[derive(Default)]
struct Config {
    timeout: u64,      // Will be 0
    retries: u32,      // Will be 0
    verbose: bool,     // Will be false
}

// Usage:
let config = Config {
    timeout: 30,
    ..Default::default()  // Fill rest with defaults
};
```

**Recognition:** `..Default::default()` fills in the blanks.

## 🎨 Method Chaining Pattern

Returning `self` for fluent interfaces:

```rust
impl MyStruct {
    fn set_name(mut self, name: &str) -> Self {
        self.name = name.to_string();
        self  // Return self for chaining
    }
    
    fn set_age(mut self, age: u32) -> Self {
        self.age = age;
        self
    }
}

// Usage:
let person = MyStruct::new()
    .set_name("Alice")
    .set_age(30);
```

## 🚫 RAII Pattern (Resource Acquisition Is Initialization)

Resources cleaned up automatically when dropped:

```rust
struct TempFile {
    path: PathBuf,
}

impl Drop for TempFile {
    fn drop(&mut self) {
        fs::remove_file(&self.path).ok();  // Cleanup on drop
    }
}

// File automatically deleted when temp_file goes out of scope
{
    let temp_file = TempFile::new();
    // Use temp_file
}  // Automatically cleaned up here
```

**Recognition:** Types implementing `Drop` for cleanup.

## 🔍 Common Code Smells vs Idiomatic Patterns

### Anti-Pattern: Unnecessary Clone
```rust
// 🚩 BAD: Cloning when borrowing would work
fn print_vec(v: Vec<String>) {  // Takes ownership
    println!("{:?}", v);
}
let data = vec!["a".to_string()];
print_vec(data.clone());  // Unnecessary clone

// ✅ GOOD: Borrow instead
fn print_vec(v: &[String]) {  // Borrows
    println!("{:?}", v);
}
print_vec(&data);  // Just borrow
```

### Anti-Pattern: Stringly Typed APIs
```rust
// 🚩 BAD: Using strings for everything
fn process(action: &str) {
    match action {
        "start" => {},
        "stop" => {},
        _ => panic!("Unknown action"),
    }
}

// ✅ GOOD: Use enums
enum Action { Start, Stop }
fn process(action: Action) {
    match action {
        Action::Start => {},
        Action::Stop => {},
    }
}
```

### Anti-Pattern: Unwrap in Libraries
```rust
// 🚩 BAD: Library code that panics
pub fn parse_config(s: &str) -> Config {
    let value = s.parse().unwrap();  // Could panic!
    Config { value }
}

// ✅ GOOD: Return Result
pub fn parse_config(s: &str) -> Result<Config, ParseError> {
    let value = s.parse()?;
    Ok(Config { value })
}
```

## 🎯 Quick Recognition Cheat Sheet

| When You See | It Usually Means |
|--------------|------------------|
| `.iter().map().collect()` | Transform a collection |
| `Box<dyn Trait>` | Runtime polymorphism |
| `#[derive(...)]` | Auto-implement traits |
| `impl From<X> for Y` | Type conversion |
| `.clone()` | Avoiding borrow checker (sometimes necessary) |
| `Rc<RefCell<T>>` | Shared mutable ownership |
| `Arc<Mutex<T>>` | Thread-safe shared mutable ownership |
| `PhantomData<T>` | Type-level state machine |
| `..Default::default()` | Partial initialization |
| `pub(crate)` | Crate-level visibility |
| `.and_then()` | Chaining operations that might fail |
| `todo!()` | Not implemented yet |
| `unreachable!()` | This code should never run |

## 🎨 Async Patterns

When you see async code:

```rust
async fn fetch_data() -> Result<String, Error> {
    let response = client.get(url).await?;  // .await suspends here
    let text = response.text().await?;
    Ok(text)
}

// Using it:
let future = fetch_data();  // Doesn't run yet!
let result = future.await;   // NOW it runs
```

**Key insights:**
- `async fn` returns a Future
- Nothing happens until `.await`
- `?` works in async functions
- Often see `tokio::spawn()` or `async_std::task::spawn()`

## 🏁 Summary of Patterns

The most important patterns to recognize:
1. **Iterator chains** - Data transformation pipelines
2. **Builder pattern** - Step-by-step construction
3. **? operator** - Error propagation
4. **Match expressions** - Exhaustive handling
5. **From/Into** - Type conversions
6. **Drop trait** - Automatic cleanup

These patterns make Rust code:
- **Safe** - Compiler enforces correctness
- **Expressive** - Clear intent
- **Efficient** - Zero-cost abstractions

## ✅ Quick Check

Can you identify the patterns in this code?
```rust
let result: Result<Vec<u32>, _> = input
    .lines()                           // Iterator
    .filter(|line| !line.is_empty())  // Iterator chain
    .map(|line| line.parse::<u32>())  // Transform to Results
    .collect();                        // Collect handles Results

match result {                        // Pattern matching
    Ok(numbers) => process(numbers),
    Err(e) => eprintln!("Error: {}", e),
}
```

If you can spot the iterator chain, Result handling, and pattern matching, you're reading Rust like a pro!

---

**Next: [Resources](08-resources.md)** - Where to go from here to deepen your Rust knowledge →