# Start Here: Your Rust Reading Journey 🗺️

Welcome! If you're reading this, you probably need to understand some Rust code but don't have time to become a Rust expert. Perfect - you're in the right place.

## 🎯 Your Goal

By the end of these guides, you'll be able to look at Rust code like this:

```rust
fn process_data(input: &str) -> Result<Vec<u32>, ParseError> {
    input.lines()
        .filter(|line| !line.is_empty())
        .map(|line| line.parse::<u32>())
        .collect()
}
```

And understand:
- What `&str` means (it's borrowing a string)
- Why there's a `Result` (Rust makes you handle errors)
- What that `?` operator does (propagates errors upward)
- How the chain of methods works (iterator pattern)

## 🗺️ Your Learning Path

### Step 1: Understand the Big Difference (Required)
Start with **[Ownership & Borrowing](01-ownership-borrowing.md)** - This is Rust's most unique concept. Without understanding this, Rust code will seem mysterious. It's like trying to read Japanese without knowing it's read right-to-left.

### Step 2: Core Concepts (Read in Order)
1. **[Types & Memory](02-types-and-memory.md)** - How Rust organizes data
2. **[Error Handling](04-error-handling.md)** - The Result/Option pattern
3. **[Pattern Matching](03-pattern-matching.md)** - Rust's powerful `match` statement

### Step 3: Deeper Patterns (Read as Needed)
- **[Traits & Generics](05-traits-generics.md)** - When you see `impl Trait` or `<T>`
- **[Lifetimes](06-lifetimes.md)** - When you see `'a` or `'static`
- **[Common Patterns](07-common-patterns.md)** - Idiomatic Rust you'll see everywhere

### Step 4: Going Further
- **[Resources](08-resources.md)** - Where to learn more

## 🔑 Key Things That Make Rust Different

Before diving in, here are the big ideas that make Rust unique:

### 1. **Ownership is Explicit**
In Python, you might write:
```python
data = [1, 2, 3]
process(data)  # Can I still use data? Who knows!
```

In Rust, it's clear:
```rust
let data = vec![1, 2, 3];
process(data);  // data is MOVED - can't use it anymore
// OR
process(&data); // data is BORROWED - can still use it
```

### 2. **No Null by Default**
In Java:
```java
String name = getUserName();  // Could be null!
name.length();  // Might crash!
```

In Rust:
```rust
let name: Option<String> = get_user_name();  // Explicitly might not exist
match name {
    Some(n) => n.len(),  // Must handle both cases
    None => 0,
}
```

### 3. **Errors are Values, Not Exceptions**
Instead of try/catch, Rust uses Result types that you must handle.

## 📊 Quick Recognition Patterns

When scanning Rust code, look for these common patterns:

| Pattern | What it Means | Example |
|---------|--------------|---------|
| `&` | Borrowing (read-only) | `fn read(data: &String)` |
| `&mut` | Mutable borrow (can modify) | `fn modify(data: &mut Vec<i32>)` |
| `Option<T>` | Might have a value | `Option<User>` |
| `Result<T, E>` | Might succeed or fail | `Result<File, Error>` |
| `?` | "If error, return it" | `file.read_to_string(&mut contents)?` |
| `impl` | Implementation block | `impl MyStruct { ... }` |
| `match` | Pattern matching | `match value { ... }` |
| `::` | Path separator | `std::fs::read_file` |

## 🚩 Red Flags When Reading Rust

Watch out for these - they often indicate important logic:

1. **`unsafe`** - Raw memory manipulation, be extra careful
2. **`.unwrap()`** - Could panic/crash if None or Err
3. **`.clone()`** - Potentially expensive copy operation
4. **`Box<>`, `Rc<>`, `Arc<>`** - Heap allocation and reference counting
5. **`'static`** - Data that lives for entire program

## 💭 Mental Models That Help

### Rust is like a Library 📚
- You can **own** a book (you bought it)
- You can **borrow** a book (temporary access)
- You can **mutably borrow** (borrow with a pencil to make notes)
- Only one person can mutably borrow at a time
- When you're done borrowing, you give it back

### Rust is like a Careful Chef 👨‍🍳
- Every ingredient is accounted for
- You know exactly who's using what
- No ingredient gets used after it's consumed
- Recipes (functions) declare what they need upfront

## 🎪 What Makes Rust Code "Rusty"?

Idiomatic Rust code tends to:
- Use iterators instead of loops when possible
- Return `Result` or `Option` instead of panicking
- Prefer borrowing over cloning
- Use pattern matching for control flow
- Keep mutations localized and explicit

## ✅ Quick Confidence Check

Can you guess what this does?
```rust
let numbers = vec![1, 2, 3, 4, 5];
let doubled: Vec<i32> = numbers.iter().map(|n| n * 2).collect();
```

If you guessed "creates a new vector with each number doubled" - you're ready to continue!

## 🚀 Next Steps

1. **Must Read**: [Ownership & Borrowing](01-ownership-borrowing.md) - This is non-negotiable for understanding Rust
2. **Then**: Work through the core concepts in order
3. **Finally**: Reference the other guides as you encounter those patterns

Remember: **You don't need to write Rust to read Rust.** Focus on recognition, not memorization.

---

*Ready? Let's tackle [Ownership & Borrowing](01-ownership-borrowing.md), Rust's signature concept!* →