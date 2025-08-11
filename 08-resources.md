# Resources: Your Next Steps in Rust 🚀

Now that you can read Rust code, here's where to go next based on your goals.

## 📚 The Essential Resource

### **The Rust Programming Language Book** ("The Book")
- **Link:** [doc.rust-lang.org/book](https://doc.rust-lang.org/book/)
- **What:** The official, comprehensive guide to Rust
- **Best for:** Anyone ready to write Rust, not just read it
- **Why it's special:** Free, well-written, constantly updated, includes exercises
- **Pro tip:** Chapters 4 (Ownership), 10 (Generics/Traits/Lifetimes), and 9 (Error Handling) reinforce what you've learned here

## 🎯 Based on Your Goals

### "I Need to Review Rust Code at Work"

1. **Rust API Guidelines** - [rust-lang.github.io/api-guidelines](https://rust-lang.github.io/api-guidelines/)
   - Learn what idiomatic Rust APIs look like
   - Great checklist for code reviews

2. **Clippy Lints** - [rust-lang.github.io/rust-clippy](https://rust-lang.github.io/rust-clippy/)
   - Understand what the linter is complaining about
   - Each lint has explanations and examples

3. **Rust Patterns** - [rust-unofficial.github.io/patterns](https://rust-unofficial.github.io/patterns/)
   - Recognize design patterns and anti-patterns
   - Useful for architectural reviews

### "I Want to Understand Rust Projects"

1. **Cargo Book** - [doc.rust-lang.org/cargo](https://doc.rust-lang.org/cargo/)
   - Understand `Cargo.toml` files
   - Learn about dependencies, features, workspaces

2. **crates.io** - [crates.io](https://crates.io/)
   - The Rust package registry
   - See documentation for any crate
   - Check download stats and dependencies

3. **docs.rs** - [docs.rs](https://docs.rs/)
   - Auto-generated documentation for all crates
   - Great for understanding library APIs

### "I Want to Start Writing Rust"

1. **Rust By Example** - [doc.rust-lang.org/rust-by-example](https://doc.rust-lang.org/rust-by-example/)
   - Learn by doing
   - Runnable examples for every concept
   - Less reading, more coding

2. **Rustlings** - [github.com/rust-lang/rustlings](https://github.com/rust-lang/rustlings)
   - Small exercises to get you coding
   - Fix broken code to learn concepts
   - Great for hands-on learners

3. **Exercism Rust Track** - [exercism.org/tracks/rust](https://exercism.org/tracks/rust)
   - Coding exercises with mentorship
   - Real feedback from experienced Rustaceans
   - Progressive difficulty

## 🛠️ Interactive Tools

### Online Playgrounds
- **Rust Playground** - [play.rust-lang.org](https://play.rust-lang.org/)
  - Run Rust in your browser
  - Share code snippets
  - Test ideas quickly

- **Godbolt Compiler Explorer** - [godbolt.org](https://godbolt.org/)
  - See generated assembly
  - Understand performance implications
  - Compare with other languages

### Learning Platforms
- **Tour of Rust** - [tourofrust.com](https://tourofrust.com/)
  - Interactive tour through Rust
  - Available in many languages
  - Bite-sized lessons

- **Learn Rust With Entirely Too Many Linked Lists** - [rust-unofficial.github.io/too-many-lists](https://rust-unofficial.github.io/too-many-lists/)
  - Deep dive into Rust through implementing linked lists
  - Surprisingly comprehensive
  - Shows why Rust is different

## 📖 Specific Topics

### Async Rust
- **Async Book** - [rust-lang.github.io/async-book](https://rust-lang.github.io/async-book/)
- **Tokio Tutorial** - [tokio.rs/tokio/tutorial](https://tokio.rs/tokio/tutorial)

### Web Development
- **Are we web yet?** - [arewewebyet.org](https://www.arewewebyet.org/)
- **Rocket Guide** - [rocket.rs/guide](https://rocket.rs/guide/)
- **Actix Web** - [actix.rs](https://actix.rs/)

### Systems Programming
- **Writing an OS in Rust** - [os.phil-opp.com](https://os.phil-opp.com/)
- **The Rustonomicon** - [doc.rust-lang.org/nomicon](https://doc.rust-lang.org/nomicon/) (Advanced/Unsafe Rust)

### Embedded Rust
- **The Embedded Rust Book** - [docs.rust-embedded.org/book](https://docs.rust-embedded.org/book/)
- **Discovery** - [docs.rust-embedded.org/discovery](https://docs.rust-embedded.org/discovery/)

## 🎥 Video Resources

### YouTube Channels
- **Jon Gjengset** - Deep technical Rust streams
- **Let's Get Rusty** - Beginner-friendly tutorials
- **Ryan Levick** - Microsoft's Rust videos
- **FasterthanlLime** - Deep dives and explanations

### Conference Talks
- **RustConf** - Annual conference recordings
- **Rust Belt Rust** - Regional conference talks
- Search for "RustConf [Year]" on YouTube

## 💬 Community Resources

### Getting Help
- **Official Rust Forum** - [users.rust-lang.org](https://users.rust-lang.org/)
  - Friendly, beginner-welcoming
  - Searchable archive of questions

- **Rust Discord** - [discord.gg/rust-lang](https://discord.gg/rust-lang)
  - Real-time chat
  - Channels for beginners

- **r/rust** - [reddit.com/r/rust](https://www.reddit.com/r/rust/)
  - News, discussions, questions
  - Weekly "easy questions" thread

### Stay Updated
- **This Week in Rust** - [this-week-in-rust.org](https://this-week-in-rust.org/)
  - Weekly newsletter
  - New crates, blog posts, events

- **Rust Blog** - [blog.rust-lang.org](https://blog.rust-lang.org/)
  - Official announcements
  - New releases and features

## 🎮 Fun Projects to Read

These are well-documented Rust projects great for learning:

1. **ripgrep** - [github.com/BurntSushi/ripgrep](https://github.com/BurntSushi/ripgrep)
   - Fast grep replacement
   - Excellent code quality

2. **bat** - [github.com/sharkdp/bat](https://github.com/sharkdp/bat)
   - Cat clone with syntax highlighting
   - Good CLI app example

3. **exa** - [github.com/ogham/exa](https://github.com/ogham/exa)
   - Modern ls replacement
   - Clean, readable code

4. **mdBook** - [github.com/rust-lang/mdBook](https://github.com/rust-lang/mdBook)
   - Tool that created The Rust Book
   - Good example of a larger application

## 🔍 Quick Reference

### Cheat Sheets
- **Rust Cheat Sheet** - [cheats.rs](https://cheats.rs/)
  - Comprehensive syntax reference
  - Great for quick lookups

- **Rust Language Cheat Sheet** - [github.com/ralfbiedert/cheats.rs](https://github.com/ralfbiedert/cheats.rs)
  - PDF version available
  - Comprehensive and visual

### When You're Stuck
1. **Error Messages** - Read them! Rust's are excellent
2. **Rust Analyzer** - IDE support that explains code
3. **Compiler Help** - `rustc --explain E0308` for any error code
4. **Search** - "rust [your error]" usually finds answers

## 🎯 Learning Path Recommendations

### Minimum Viable Rust Developer
1. Read this guide (✅ Done!)
2. Complete Rustlings exercises
3. Read The Book chapters 1-10
4. Build a CLI tool
5. Contribute to an existing project

### From Reader to Writer
1. **Week 1-2:** Rustlings + Rust By Example
2. **Week 3-4:** The Book (with exercises)
3. **Week 5-6:** Small project (CLI tool, web scraper)
4. **Week 7-8:** Contribute to open source

### Going Deep
1. The Book (complete)
2. Rust for Rustaceans (advanced book)
3. The Rustonomicon (unsafe Rust)
4. Pick a domain (web, embedded, games)
5. Build something substantial

## 🏁 Final Advice

1. **Don't fight the borrow checker** - It's teaching you something
2. **Embrace the compiler errors** - They're better than runtime crashes
3. **Start small** - CLI tools are perfect first projects
4. **Read other people's code** - crates.io has thousands of examples
5. **Ask questions** - The Rust community is famously helpful

## 🦀 You're Ready!

You now have:
- ✅ The ability to read and understand Rust code
- ✅ Mental models for Rust's unique concepts
- ✅ Recognition patterns for idiomatic Rust
- ✅ Resources for going deeper

Whether you're reviewing PRs, debugging services, or ready to write Rust yourself, you have the foundation you need.

**Remember:** The goal wasn't to make you a Rust expert - it was to make you comfortable reading Rust. Mission accomplished! 

The rest is just practice and curiosity. Welcome to the Rust community! 🦀

---

*Want to write Rust? Your next stop: [The Rust Programming Language Book](https://doc.rust-lang.org/book/)*