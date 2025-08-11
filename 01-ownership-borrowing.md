# Ownership & Borrowing: Rust's Superpower 📚

This is **the** concept that makes Rust different. Once you get this, everything else in Rust makes sense. Skip this, and you'll be confused forever.

## 🎯 The Big Idea

Imagine a world where every piece of data has a clear owner, and that owner is responsible for cleaning up when done. That's Rust's ownership system.

## 📖 The Library Book Analogy

Think of data in Rust like books in a library:

### Ownership = Buying the Book
```rust
let book = String::from("The Rust Book");  // You OWN this book
give_away(book);                           // You gave it away
// Can't use 'book' anymore - you don't own it!
```

### Borrowing = Library Loan
```rust
let book = String::from("The Rust Book");  // You own this
read_book(&book);                           // Someone borrows it to read
println!("{}", book);                       // You still have it!
```

### Mutable Borrowing = Loan with a Pencil
```rust
let mut book = String::from("The Rust Book");  // Mutable ownership
add_notes(&mut book);                           // Loan it with permission to write
println!("{}", book);                           // Got it back, with notes!
```

## 🔑 The Three Rules (That's It!)

1. **Each value has exactly one owner**
2. **You can have EITHER:**
   - One mutable borrow (`&mut T`)
   - OR any number of immutable borrows (`&T`)
   - But not both at the same time!
3. **When the owner goes out of scope, the value is dropped**

## 👀 Quick Recognition Guide

| What You See | What It Means | Can You Still Use Original? |
|-------------|---------------|----------------------------|
| `foo(x)` | Moving ownership | ❌ No, it's gone |
| `foo(&x)` | Immutable borrow | ✅ Yes, unchanged |
| `foo(&mut x)` | Mutable borrow | ✅ Yes, might be changed |
| `let y = x` | Move ownership to y | ❌ No, y owns it now |
| `let y = &x` | y borrows from x | ✅ Yes, both valid |
| `let y = x.clone()` | Make a copy | ✅ Yes, independent copy |

## 🎨 Visual Examples

### Example 1: Moving Ownership
```rust
fn main() {
    let s1 = String::from("hello");  // s1 owns "hello"
    let s2 = s1;                     // Ownership MOVES to s2
    
    // println!("{}", s1);           // ❌ ERROR! s1 no longer owns it
    println!("{}", s2);               // ✅ s2 owns it now
}
```
**Think of it as:** Handing someone your car keys. You can't drive it anymore!

### Example 2: Borrowing
```rust
fn calculate_length(s: &String) -> usize {  // Borrows, doesn't own
    s.len()                                  // Can read it
    // s.push_str("more");                   // ❌ Can't modify!
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);         // Lend it temporarily
    println!("Length of '{}' is {}", s1, len); // ✅ Still have it!
}
```
**Think of it as:** Letting someone look at your phone to check the time.

### Example 3: Mutable Borrowing
```rust
fn add_world(s: &mut String) {
    s.push_str(", world");  // Can modify because it's &mut
}

fn main() {
    let mut s = String::from("Hello");  // Note: mut is required
    add_world(&mut s);                  // Lend with edit permissions
    println!("{}", s);                   // Prints: "Hello, world"
}
```
**Think of it as:** Letting someone borrow your notebook to add their notes.

## 🚩 Red Flags & Common Patterns

### The "Use After Move" Error
```rust
let data = vec![1, 2, 3];
process_data(data);        // Moved!
println!("{:?}", data);    // ❌ ERROR: value used after move
```
**Fix:** Either clone or borrow:
```rust
process_data(data.clone());  // Send a copy
// OR
process_data(&data);         // Lend it instead
```

### The "Cannot Borrow as Mutable" Error
```rust
let x = 5;              // Not mutable
change_value(&mut x);   // ❌ ERROR: cannot borrow as mutable
```
**Fix:** Declare as mutable:
```rust
let mut x = 5;          // Now it's mutable
change_value(&mut x);   // ✅ Works!
```

### The Iterator Pattern
```rust
let numbers = vec![1, 2, 3];
numbers.iter()      // Borrows each element
    .map(|n| n * 2) // Process borrowed values
    .collect();     // Create new collection
// numbers still available here!
```

## 🎯 What to Look For When Reading Code

### 1. Function Signatures Tell You Everything
```rust
fn take_ownership(s: String) { }        // Will consume the input
fn borrow(s: &String) { }               // Just wants to read
fn borrow_mut(s: &mut String) { }       // Wants to modify
fn return_ownership() -> String { }     // Gives you ownership
```

### 2. The `.clone()` Escape Hatch
When you see `.clone()`, someone is making a copy to avoid ownership issues:
```rust
let s1 = String::from("hello");
let s2 = s1.clone();  // Explicit copy
// Both s1 and s2 are usable
```

### 3. Common Borrowing Patterns
```rust
// Iteration borrows by default
for item in &collection { }     // Borrow each item
for item in &mut collection { }  // Mutably borrow each item
for item in collection { }       // Take ownership (consumes collection)

// Method chains often borrow
text.trim().to_lowercase()      // Borrows text, returns new String
```

## 💡 Mental Shortcuts

### When Reading Rust Code, Ask:
1. **Who owns this data?** (Look for the original `let` binding)
2. **Is it being moved or borrowed?** (Look for `&`)
3. **Can it be modified?** (Look for `mut`)

### Quick Rules:
- **No `&` = Moving** (ownership transfer)
- **`&` without `mut` = Reading** (immutable borrow)
- **`&mut` = Modifying** (mutable borrow)
- **`.clone()` = Copying** (avoiding ownership complexity)

## 🎮 Interactive Example

Try to predict what happens:
```rust
fn main() {
    let mut x = 5;           // x owns the value 5
    let y = &x;              // y borrows x
    let z = &x;              // z also borrows x (multiple readers OK!)
    
    // let w = &mut x;       // ❌ ERROR! Can't mutably borrow while borrowed
    
    println!("{} {}", y, z); // Use the borrows
    
    let w = &mut x;          // ✅ NOW we can mutably borrow (y and z done)
    *w += 1;                 // Modify through the mutable borrow
    
    println!("{}", x);       // Prints: 6
}
```

## 🏁 Summary

- **Ownership** = Who's responsible for cleaning up
- **Borrowing** = Temporary access without ownership
- **`&`** = "I just want to look"
- **`&mut`** = "I need to change this"
- **No `&`** = "I'm taking this"

The compiler enforces these rules at compile time, preventing entire classes of bugs that plague other languages.

## ✅ Quick Check

If you understand why this doesn't work:
```rust
let s = String::from("hello");
let r1 = &s;
let r2 = &mut s;  // ❌ Can't have & and &mut at same time
```

But this does:
```rust
let mut s = String::from("hello");
{
    let r1 = &s;
    println!("{}", r1);
}  // r1 goes out of scope
let r2 = &mut s;  // ✅ Now we can mutably borrow
```

Then you understand ownership and borrowing!

---

**Next: [Types & Memory](02-types-and-memory.md)** - Now that you understand ownership, let's see how Rust organizes data →