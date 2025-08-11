# Traits & Generics: Rust's Way of Sharing Behavior 🧬

Traits are like contracts that types can sign. Generics let you write code that works with any type that signs the right contract.

## 🎯 The Big Ideas

**Traits** = "I promise I can do these things"  
**Generics** = "I work with any type that keeps its promises"

## 📝 Traits: Shared Behavior

Think of traits like job requirements. If you meet the requirements, you can do the job.

### Basic Trait Example
```rust
trait Greet {
    fn say_hello(&self);
}

struct Person { name: String }
struct Dog { name: String }

impl Greet for Person {
    fn say_hello(&self) {
        println!("Hello, I'm {}", self.name);
    }
}

impl Greet for Dog {
    fn say_hello(&self) {
        println!("Woof! I'm {}", self.name);
    }
}
```

### Real-World Analogy
Traits are like interfaces or contracts:
- **Driver's License** = You can drive any car
- **Pilot's License** = You can fly planes
- The license (trait) defines what you must be able to do

## 🔤 Common Built-in Traits

| Trait | What It Means | How You See It |
|-------|--------------|----------------|
| `Clone` | Can be duplicated | `.clone()` method |
| `Copy` | Cheap to duplicate | Automatic copying |
| `Debug` | Can be printed for debugging | `{:?}` in println! |
| `Display` | Can be pretty-printed | `{}` in println! |
| `Default` | Has a default value | `Default::default()` |
| `PartialEq` | Can be compared with == | `a == b` |
| `Iterator` | Can be iterated over | `for item in collection` |
| `From/Into` | Can convert between types | `.into()`, `Type::from()` |

### Derive Macro: Auto-Implementation
```rust
#[derive(Debug, Clone, PartialEq)]  // Compiler implements these for you
struct Point {
    x: i32,
    y: i32,
}

let p1 = Point { x: 1, y: 2 };
let p2 = p1.clone();  // Clone trait
println!("{:?}", p1);  // Debug trait
assert_eq!(p1, p2);    // PartialEq trait
```

## 🎁 Generics: Write Once, Use with Many Types

### Generic Functions
```rust
// Works with any type T
fn first<T>(list: &[T]) -> Option<&T> {
    list.get(0)
}

// Usage
let numbers = vec![1, 2, 3];
let strings = vec!["a", "b", "c"];
first(&numbers);  // Works with i32
first(&strings);  // Works with &str
```

### Generic Structs
```rust
// A box that can hold any type
struct Container<T> {
    value: T,
}

let int_container = Container { value: 42 };        // Container<i32>
let str_container = Container { value: "hello" };   // Container<&str>
```

### Think of Generics Like...
- **Tupperware** 🥡: Same container, different contents
- **USB Port** 🔌: Same port, different devices
- The container/port doesn't care what's inside/connected

## 🔗 Trait Bounds: Generics with Requirements

Sometimes you need a generic type that can do specific things:

```rust
// T must implement Display trait
fn print_it<T: Display>(value: T) {
    println!("{}", value);
}

// Multiple bounds with +
fn process<T: Clone + Debug>(value: T) {
    let copy = value.clone();
    println!("{:?}", copy);
}

// Where clause for complex bounds
fn complex<T, U>(t: T, u: U) -> String 
where
    T: Display + Clone,
    U: Debug,
{
    format!("{} {:?}", t, u)
}
```

## 🎨 Common Patterns You'll See

### Pattern 1: impl Trait in Arguments
```rust
// Instead of generics, use impl
fn print_anything(value: impl Display) {
    println!("{}", value);
}

// Equivalent to:
fn print_anything<T: Display>(value: T) {
    println!("{}", value);
}
```

### Pattern 2: Trait Objects (Dynamic Dispatch)
```rust
// Box<dyn Trait> for runtime polymorphism
fn make_sound(animal: Box<dyn MakeNoise>) {
    animal.noise();
}

// Can pass any type that implements MakeNoise
let dog: Box<dyn MakeNoise> = Box::new(Dog {});
let cat: Box<dyn MakeNoise> = Box::new(Cat {});
make_sound(dog);
make_sound(cat);
```

### Pattern 3: Associated Types
```rust
trait Container {
    type Item;  // Associated type
    fn get(&self) -> Option<&Self::Item>;
}

struct StringBox {
    value: String,
}

impl Container for StringBox {
    type Item = String;  // Specify the associated type
    fn get(&self) -> Option<&String> {
        Some(&self.value)
    }
}
```

### Pattern 4: Default Implementations
```rust
trait Describable {
    fn description(&self) -> String {
        String::from("No description")  // Default implementation
    }
}

struct Thing;
impl Describable for Thing {}  // Uses default

struct DetailedThing;
impl Describable for DetailedThing {
    fn description(&self) -> String {
        String::from("Detailed description")  // Override default
    }
}
```

## 🔍 Quick Recognition Guide

| What You See | What It Means |
|-------------|---------------|
| `<T>` | Generic type parameter |
| `impl Trait` | Implements a trait |
| `dyn Trait` | Trait object (dynamic) |
| `T: Trait` | T must implement Trait |
| `where T: Trait` | Trait bound in where clause |
| `#[derive(Trait)]` | Auto-implement trait |
| `::method()` | Calling trait method explicitly |
| `<Type as Trait>::method()` | Disambiguating trait method |

## 🚩 Red Flags & Common Issues

### The "Trait Not Implemented" Error
```rust
struct MyType;
println!("{}", MyType);  // ❌ ERROR: Display not implemented

// Fix: Implement Display
impl Display for MyType {
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        write!(f, "MyType")
    }
}
```

### The "Size Not Known at Compile Time" Error
```rust
// ❌ ERROR: Size of dyn Trait not known
fn take_trait(t: dyn MyTrait) { }

// ✅ Fix: Use reference or Box
fn take_trait(t: &dyn MyTrait) { }
fn take_trait(t: Box<dyn MyTrait>) { }
```

### Orphan Rule
```rust
// ❌ Can't implement external trait for external type
impl Display for Vec<String> { }  // Both Display and Vec are external

// ✅ OK: Your trait for external type
trait MyTrait { }
impl MyTrait for Vec<String> { }  // MyTrait is yours

// ✅ OK: External trait for your type
struct MyType;
impl Display for MyType { }  // MyType is yours
```

## 💡 Mental Models

### Traits = Job Requirements
- **Job Posting**: "Must be able to: drive, type, speak English"
- **Trait**: "Must be able to: clone(), display(), compare()"
- Types "apply" for the job by implementing the trait

### Generics = Universal Adapters
- Like a universal phone charger that works with any phone
- The adapter (generic function) works with anything that fits the spec

### impl Trait = "Trust Me, I Can Do This"
- Instead of showing ID, you just say "I can do the job"
- The compiler verifies you're not lying

## 🎯 Reading Generic Code

When you see:
```rust
fn process<T, U>(data: T, processor: U) -> Result<String, Error>
where
    T: AsRef<str> + Send,
    U: Fn(&str) -> String,
{
    // ...
}
```

Read it as:
1. **`process`** is a function that works with any two types (T and U)
2. **`T`** must be convertible to a string reference and thread-safe
3. **`U`** must be a function that takes a string slice and returns a String
4. The function returns a Result

## ✅ Quick Check

Can you understand this code?
```rust
fn longest<'a, T>(x: &'a T, y: &'a T) -> &'a T 
where 
    T: PartialOrd 
{
    if x > y { x } else { y }
}
```

Breaking it down:
- Takes two references of the same type T
- T must be comparable (PartialOrd)
- Returns a reference to the larger one
- `'a` is a lifetime (covered in next chapter)

If you can follow this, you understand traits and generics!

---

**Next: [Lifetimes](06-lifetimes.md)** - Those mysterious `'a` annotations explained →