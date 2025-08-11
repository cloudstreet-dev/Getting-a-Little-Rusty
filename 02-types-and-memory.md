# Types & Memory: How Rust Thinks About Data 🧱

Rust is obsessed with knowing exactly how big things are and where they live in memory. This obsession is what makes Rust fast and safe.

## 🏗️ The Two Places Data Lives

Think of memory like a restaurant:
- **Stack** = The counter (fast, organized, fixed-size orders)
- **Heap** = The dining room (slower, flexible, any-size parties)

### Stack Data (Fast & Simple)
```rust
let x: i32 = 42;        // 4 bytes, lives on stack
let y: bool = true;     // 1 byte, lives on stack
let point = (10, 20);   // 8 bytes, lives on stack
```

### Heap Data (Flexible but Slower)
```rust
let s = String::from("hello");  // Data on heap, pointer on stack
let v = vec![1, 2, 3];         // Data on heap, pointer on stack
let b = Box::new(42);          // 42 on heap, Box pointer on stack
```

## 📊 Common Types Quick Reference

| Type | Size | Stack/Heap | Example | What It's For |
|------|------|------------|---------|---------------|
| `i32`, `u32` | 4 bytes | Stack | `42` | Integers |
| `i64`, `u64` | 8 bytes | Stack | `1_000_000` | Large integers |
| `f32`, `f64` | 4/8 bytes | Stack | `3.14` | Decimals |
| `bool` | 1 byte | Stack | `true` | True/false |
| `char` | 4 bytes | Stack | `'a'` | Single Unicode character |
| `&str` | Pointer | Stack | `"hello"` | String slice (borrowed) |
| `String` | Pointer | Heap | `String::from("hi")` | Owned string |
| `Vec<T>` | Pointer | Heap | `vec![1,2,3]` | Dynamic array |
| `[T; N]` | N × size of T | Stack | `[1, 2, 3]` | Fixed array |
| `Option<T>` | T + 1 byte | Depends on T | `Some(42)` | Maybe value |
| `Result<T,E>` | Larger of T/E + tag | Depends | `Ok(42)` | Success or error |

## 🔍 Recognizing Type Patterns

### Pattern 1: Number Types
```rust
let a = 42;         // i32 by default
let b = 42i64;      // Explicitly i64
let c = 42_u8;      // Explicitly u8 (unsigned, 0-255)
let d = 3.14;       // f64 by default
let e = 3.14f32;    // Explicitly f32
```
**Quick Rule:** `i` = signed integer, `u` = unsigned, `f` = float

### Pattern 2: Strings - The Two Kinds
```rust
let s1: &str = "I'm borrowed";           // String slice, fixed
let s2: String = String::from("I'm owned"); // Owned, can grow

// Converting between them
let owned = s1.to_string();              // &str -> String
let borrowed = &s2;                      // String -> &str (auto)
let borrowed2 = s2.as_str();            // String -> &str (explicit)
```

**Think of it as:**
- `&str` = Looking at someone else's text
- `String` = Having your own notebook

### Pattern 3: Collections
```rust
// Arrays - fixed size, stack
let arr: [i32; 3] = [1, 2, 3];           // Exactly 3 elements

// Vectors - dynamic size, heap
let mut vec: Vec<i32> = vec![1, 2, 3];   // Can grow
vec.push(4);                             // Now has 4 elements

// Slices - borrowed view
let slice: &[i32] = &vec[1..3];          // Borrows elements 1 and 2
```

## 🎯 Type Annotations: When Rust Needs Help

Usually Rust figures out types, but sometimes you need to help:

```rust
// Rust can't guess
let numbers: Vec<i32> = Vec::new();  // What type of Vec?

// Rust can guess
let numbers = vec![1, 2, 3];         // Obviously Vec<i32>

// Collecting needs a hint
let parsed: Vec<i32> = "1,2,3"
    .split(',')
    .map(|s| s.parse().unwrap())
    .collect();  // Need to specify Vec<i32>
```

## 🎁 Box, Rc, Arc: Smart Pointers

When you see these, think "special heap storage":

| Type | What It Means | Use Case |
|------|--------------|----------|
| `Box<T>` | Single owner on heap | Large data, recursive types |
| `Rc<T>` | Reference counted | Multiple owners, single thread |
| `Arc<T>` | Atomic ref counted | Multiple owners, multiple threads |
| `RefCell<T>` | Interior mutability | Mutate through immutable reference |

```rust
// Box - simple heap allocation
let b = Box::new(5);

// Rc - multiple owners (single thread)
use std::rc::Rc;
let shared = Rc::new(vec![1, 2, 3]);
let clone1 = Rc::clone(&shared);  // Not a deep copy!

// Arc - thread-safe multiple owners
use std::sync::Arc;
let shared = Arc::new(data);
```

## 🔄 Type Conversions

### Common Patterns You'll See:

```rust
// String conversions
let s = 42.to_string();              // Anything to String
let n: i32 = "42".parse().unwrap();  // String to number

// as for numeric casts
let a = 42i32;
let b = a as i64;                    // Widen
let c = 300u16 as u8;                // Narrow (truncates to 44!)

// Into/From traits
let s: String = "hello".into();      // &str into String
let s = String::from("hello");       // Same thing

// Dereferencing
let x = Box::new(5);
let y = *x;                          // Dereference to get value
```

## 🚩 Red Flags in Type Usage

### The `.unwrap()` Bomb
```rust
let n: i32 = user_input.parse().unwrap();  // 💣 Panics on bad input!
```
**Better:**
```rust
let n: i32 = match user_input.parse() {
    Ok(num) => num,
    Err(_) => 0,  // Default value
};
```

### Integer Overflow
```rust
let x: u8 = 255;
let y = x + 1;  // ⚠️ Wraps to 0 in release mode!
```

### String Slicing Panics
```rust
let s = "hello";
let slice = &s[0..10];  // 💣 Panics! String is only 5 bytes
```

## 💡 Memory Patterns to Recognize

### Pattern: Builder Pattern
```rust
let s = String::new()
    .push_str("Hello")    // Won't work! push_str returns ()
    
let mut s = String::new();
s.push_str("Hello");      // Correct: mutate in place
s.push_str(" World");
```

### Pattern: Capacity Hints
```rust
let mut vec = Vec::with_capacity(100);  // Pre-allocate space
// Avoids reallocation as you add elements
```

### Pattern: Slice Arguments
```rust
fn process(data: &[u8]) { }  // Takes any slice

let arr = [1, 2, 3];
let vec = vec![1, 2, 3];
process(&arr);               // Array to slice
process(&vec);               // Vec to slice
```

## 📏 Size Matters

Rust needs to know sizes at compile time for stack data:

```rust
// This won't compile:
// let arr = [0; n];  // ❌ n must be known at compile time

// Use Vec for runtime-sized collections:
let vec = vec![0; n];  // ✅ n can be a variable
```

## 🎯 Quick Recognition: Stack vs Heap

| See This | It Means |
|----------|----------|
| `String`, `Vec<T>`, `HashMap<K,V>` | Heap allocated |
| `i32`, `f64`, `bool`, `[T; N]` | Stack allocated |
| `Box<T>`, `Rc<T>`, `Arc<T>` | Heap with smart pointer |
| `&T`, `&mut T` | Borrowing (wherever T lives) |

## ✅ Quick Check

Can you spot the difference?
```rust
let a = [1, 2, 3];        // Array: fixed size, stack
let b = vec![1, 2, 3];    // Vector: dynamic, heap
let c = &[1, 2, 3];       // Slice: borrowed view
let d = "hello";          // &str: string slice
let e = String::from("hello"); // String: owned, heap
```

If yes, you understand Rust's type system!

---

**Next: [Pattern Matching](03-pattern-matching.md)** - Rust's Swiss Army knife for control flow →