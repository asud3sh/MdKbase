- Fundamentals
  collapsed:: true
	- **Environment Setup**
	  collapsed:: true
		- Installation `rustup`
		  collapsed:: true
			- `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
			  id:: 67ba0740-fe96-4f90-a710-1d44d387566d
			- verify : `rustc --version` and `cargo --version`.
			- update : `rustup update`
			- uninstall : `rustup self uninstall`
		- Build and Package (`crate`) Management : Cargo
		  collapsed:: true
			- It is Rust's build system and package manager.  basic `cargo` cmds:
			- `cargo [--version | build | run | test | doc | publish]`
			- `cargo new project_name` (creates a new project)
			- `cargo build` (compiles project)
			- `cargo run` (builds and runs your project)
			- `cargo check` (checks for errors without building)
			- `cargo test` (runs tests)
			- `cargo doc --open` (builds and opens documentation for your project and dependencies)
			- Build and Run :
				- `cargo check` : w/o producing binary
				- `cargo run`: build and run
				- `cargo build --release`: compile with optimization
				  id:: 642c0ca4-7df7-4fac-8b51-3f410eebbfe7
	- Rust's Core Philosophy
	  collapsed:: true
		- Rust is designed to empower everyone to build reliable and efficient software. It achieves this through:
		  collapsed:: true
			- **Memory Safety without Garbage Collection:**  Guaranteed memory safety at compile time, preventing common bugs like dangling pointers and buffer overflows, without the runtime overhead of garbage collection.
			- **High Performance:**  Rust is designed to be as fast as C and C++, with fine-grained control over memory and system resources.
			- **Concurrency without Fear:** Makes concurrent programming safer and more approachable through its ownership system, preventing data races at compile time.
		- **Core of Rust**
		  collapsed:: true
			- Ownership: *The* fundamental concept.  Governs memory management.  Every value has an owner, there's only one owner at a time, and ownership is transferred when values are assigned or passed. When the owner goes out of scope, the value is dropped (memory freed).
			  collapsed:: true
				- **Why:** Ensures memory safety and prevents dangling pointers.
			- Borrowing:  Allows temporary access to data owned by another variable, without transferring ownership.  References (`&`, `&mut`) are how borrowing is achieved.  Rust enforces borrowing rules at compile time:
			  collapsed:: true
				- **Immutable Borrows:** Multiple readers allowed (shared).
				- **Mutable Borrows:** Only one writer allowed (exclusive).
				- **No Dangling References:** Borrow checker ensures references are always valid.
				- **Why:** Enables safe data sharing and mutation without data races or memory corruption.
			- Types:  Rust is statically and strongly typed.  Types are inferred where possible, but explicit annotations are encouraged for clarity.
			  collapsed:: true
				- **Scalar Types:**  Integers, floats, booleans, characters.
				- **Compound Types:** Tuples, Arrays, Structs, Enums.
				- **Why:** Type system provides compile-time safety, catches type-related errors early, and improves code clarity.
			- Error Handling :  Rust favors explicit error handling.
			  collapsed:: true
				- **`Result<T, E>`:** For recoverable errors.  Represents either success (`Ok(T)`) or failure (`Err(E)`).  Forces you to handle errors.
				- **`panic!`:** For unrecoverable errors.  Causes program termination.  Used for truly exceptional situations.
				- **Why:** Promotes robust and reliable software by requiring error handling and differentiating between recoverable and unrecoverable errors.
			- Traits and Generics:  Enable code reuse and polymorphism.
			  collapsed:: true
				- **Traits:** Define shared behavior that types can implement. Similar to interfaces but more powerful. Allow for generic programming and code extension.
				- **Generics:** Write code that works with multiple types without knowing the specific types at compile time.  Achieves compile-time polymorphism and avoids code duplication.
				- **Why:**  Promote code reusability, abstraction, and writing flexible and efficient code.
	- Basic Program Structure
	  collapsed:: true
		- **`main` Function:** Every Rust executable program starts with a `main` function:
		  collapsed:: true
			- ```rust
			  fn main() {
			    println!("Hello, world!");
			  }
			  ```
			- `fn` keyword defines a function.
			- `main` is the special name for the entry point.
			- `()` indicates it takes no arguments.
			- `{}` curly braces enclose the function body (code block).
			- `println!` is a macro (note the `!`). Macros are like functions but operate at compile time for code generation.
		- **Statements and Expressions:** Rust is primarily an expression-based language.
		  collapsed:: true
			- **Statements** are instructions that perform actions and don't return a value. They usually end with a semicolon `;`.
			- **Expressions** evaluate to a value.  They can be part of statements, or stand alone as the last part of a block to implicitly return a value (without a semicolon).
			- ```rust
			  fn main() {
			   let x = 5; // Statement: variable binding
			   let y = { // Expression block (evaluates to a value)
			       let temp = x + 2;
			       temp * 3 // Last expression in block, no semicolon, implicit return
			   }; // Statement: variable binding using expression's result
			  
			   println!("y is: {}", y); // Another statement
			  }
			  ```
		- **Comments**
		  collapsed:: true
			- Single-line comments: `// This is a comment until the end of the line`
			- Multi-line comments: `/* This is a multi-line comment */`
	- Variables & Mutability
	  collapsed:: true
		- `let var_name = value;`  (Immutable by default)
		- `let mut mutable_var = value;` (For mutable variables)
		- Variables are **immutable** by default in Rust. Once bound to a value, you cannot reassign it.
		  collapsed:: true
			- ```rust
			  fn main() {
			   let x = 10; // x is immutable
			   // x = 20; // Error! Cannot reassign to immutable variable
			   println!("x is: {}", x);
			  }
			  ```
		- **Mutability:** To make a variable mutable (reassignable), use the `mut` keyword:
		  collapsed:: true
			- ```rust
			  fn main() {
			    let mut x = 10; // x is mutable
			    println!("x is initially: {}", x);
			    x = 20;      // Okay, reassignment allowed because x is mutable
			    println!("x is now: {}", x);
			  }
			  ```
		- Immutability as default promotes safer code. `mut` signals intent to modify.
	- Data Types
	  collapsed:: true
		- **Scalar Types:** Represent single values.
		  collapsed:: true
			- **Integers:** `i8`, `i16`, `i32`, `i64`, `i128`, `u8`, `u16`, `u32`, `u64`, `u128`, `isize`, `usize`. (Signed `i` and unsigned `u`, size in bits. `isize`/`usize` depend on architecture).  Examples: `10`, `-5`, `0u32`, `100_000` (underscores for readability).
			- **Floating-point:** `f32`, `f64`. Examples: `3.14`, `-2.5`, `1.0e6`.
			- **Booleans:** `bool`. Values: `true`, `false`.
			- **Characters:** `char`. Unicode scalar values, 4 bytes wide. Examples: `'a'`, `'Z'`, `'あ'`, `'🦀'`. Enclosed in single quotes.
		- **Compound**
			- `tuples = (1, "hello", true);` (fixed-size, ordered, mixed types)
				- Fixed-size ordered lists of values of potentially different types.
				- ```rust
				  fn main() {
				   let person = ("Alice", 30, true); // Tuple of (&str, i32, bool)
				   let name = person.0; // Access tuple elements by index (starting from 0)
				   let age = person.1;
				   let is_student = person.2;
				  
				   println!("Name: {}, Age: {}, Student: {}", name, age, is_student);
				  
				   let (name, age, student_status) = person; // Destructuring a tuple
				   println!("Name (destructured): {}", name);
				  }
				  ```
			- `arrays = [1, 2, 3];` (fixed-size, same type, `[i32; 3]`)
			  collapsed:: true
				- Fixed-size lists of elements of the **same** type.
				- ```rust
				  fn main() {
				   let numbers = [1, 2, 3, 4, 5]; // Array of 5 i32s, type inferred as [i32; 5]
				   let first_number = numbers[0]; // Access array elements by index
				   println!("First number: {}", first_number);
				  
				   // Initialize array with same value
				   let all_zeros = [0; 10]; // Array of 10 i32s, all initialized to 0
				  
				   // Arrays are fixed-size and allocated on the stack by default.
				   // For dynamic size, use Vectors (Vec<T>).
				  }
				  ```
			- `struct StructName { field: Type, ... }` (custom data structures)
			- `enum EnumName { Variant1, Variant2, ... }` (types with variants)
		- Rich type system for safety and clarity.
		- **Type Inference and Type Annotations:** Rust often infers the type of variables. You can optionally provide explicit type annotations:
		  collapsed:: true
			- ```rust
			  fn main() {
			    let age = 30;       // Type inferred as i32 (integer)
			    let price: f64 = 99.99; // Explicit type annotation: f64 (64-bit float)
			    let name: &str = "Alice"; // String slice (more on strings later)
			  
			    println!("Age: {}, Price: {}, Name: {}", age, price, name);
			  }
			  ```
	- Functions
	  collapsed:: true
		- `fn function_name(param: Type, ...) -> ReturnType { body }`
		- Implicit return for last expression.
		- Functions are fundamental building blocks, explicit type signatures for parameters and return values.
	- Control Flow
	  collapsed:: true
		- `if condition { ... } else if condition { ... } else { ... }` (conditional execution)
		  collapsed:: true
			- Conditions in `if` must be of type `bool`. No automatic type conversion from integers to boolean like in some languages.
			- ```rust
			  fn main() {
			    let number = 7;
			  
			    if number < 5 {
			        println!("Condition was true");
			    } else {
			        println!("Condition was false");
			    }
			  
			    // `else if` for multiple conditions
			    if number < 5 {
			        println!("Less than 5");
			    } else if number == 7 {
			        println!("It's seven!");
			    } else {
			        println!("Greater than or equal to 5 and not seven");
			    }
			  
			    // `if` is an expression! It can return a value.
			    let result = if number > 5 { "Greater" } else { "Less or equal" };
			    println!("Result: {}", result);
			  }
			  ```
		- `loop { ... if condition { break; } ... }` (infinite loop with `break`)
		  collapsed:: true
			- ```rust
			  fn main() {
			   let mut counter = 0;
			   loop {
			       counter += 1;
			       println!("Counter: {}", counter);
			       if counter == 5 {
			           break; // Exit the loop
			       }
			   }
			  }
			  ```
		- `while condition { ... }` (loop while condition is true)
		  collapsed:: true
			- ```rust
			  fn main() {
			   let mut number = 3;
			   while number != 0 {
			       println!("Number: {}", number);
			       number -= 1;
			   }
			   println!("Lift off!");
			  }
			  ```
		- `for item in iterable { ... }` Iterate over a sequence (iterators, ranges, collections).
		  collapsed:: true
			- ```rust
			  fn main() {
			   // Iterate over a range (0 to 4 inclusive)
			   for i in 0..5 { // 0..5 is a range, exclusive of 5. 0..=5 is inclusive.
			       println!("i: {}", i);
			   }
			  
			   let array = [10, 20, 30, 40, 50];
			   // Iterate over elements of an array
			   for element in array.iter() { // .iter() creates an iterator over the array
			       println!("Element: {}", element);
			   }
			  }
			  ```
		- **break and continue:** Control loop flow.
		  collapsed:: true
			- `break`: Exits the loop immediately.
			- `continue`: Skips the rest of the current iteration and goes to the next iteration.
	- Data Structures
	  collapsed:: true
		- `struct`: Group related data, methods can be implemented using `impl`.
		  collapsed:: true
			- User-defined data types to group related data.
			- `struct StructName { field_name: Type, ... }`
			- ```rust
			  struct Rectangle {
			    width: u32,
			    height: u32,
			  }
			  
			  fn main() {
			    let rect1 = Rectangle { width: 30, height: 50 };
			    println!("Rectangle width: {}, height: {}", rect1.width, rect1.height);
			  }
			  ```
		- `enum`: Define types that can be one of several possible variants.
		  collapsed:: true
			- ```rust
			  enum Direction {
			    North,
			    South,
			    East,
			    West,
			  }
			  
			  fn main() {
			    let direction = Direction::East; // Creating an enum variant
			  
			    match direction { // `match` is powerful for enum handling
			        Direction::North => println!("Going North"),
			        Direction::South => println!("Going South"),
			        Direction::East => println!("Going East"),
			        Direction::West => println!("Going West"),
			    }
			  }
			  ```
			- `match` expressions are often used to handle different enum variants.
			- `enum EnumName { Variant1, Variant2, Variant3, ... }`
		- **Vectors (`Vec<T>`):** Dynamically sized arrays (growable lists).
		- **Strings (`String`, `&str`):**  Owned strings (`String`) and string slices (`&str`).
		- **HashMap (`HashMap<K, V>`):** Key-value pairs.
	- Modules & Crates
	  collapsed:: true
		- `mod module_name { ... }` (organize code into modules)
		  collapsed:: true
			- ```rust
			  mod my_module {
			    pub fn hello() { // `pub` keyword makes it public
			        println!("Hello from my_module!");
			    }
			  
			    fn internal_function() { // Private by default (module scope)
			        println!("This is internal to my_module");
			    }
			  }
			  
			  fn main() {
			    my_module::hello(); // Call public function from module
			    // my_module::internal_function(); // Error! private function
			  }
			  ```
			- Items within a module are private by default. Use `pub` keyword to make them public (visible outside the module).
		- `pub` (keyword for public visibility within/outside modules)
		- `use module::item;` (bring items into scope)
		- `crate` (compilation unit - binary or library)
		- Modules for code organization and namespace management, crates for project structure and compilation units.
	- Traits & Generics
	  collapsed:: true
		- **Traits:** Define shared behavior that types can implement (like interfaces or protocols).
		  collapsed:: true
			- `trait TraitName { fn method_signature(&self, ...); ... }`
			- `impl TraitName for Type { fn method_signature(&self, ...) { ... } ... }`
			- ```rust
			  trait Summary {
			    fn summarize(&self) -> String; // Trait method signature
			  }
			  
			  struct NewsArticle {
			    headline: String,
			    author: String,
			    content: String,
			  }
			  
			  impl Summary for NewsArticle { // Implement the Summary trait for NewsArticle
			    fn summarize(&self) -> String {
			        format!("{}, by {}", self.headline, self.author)
			    }
			  }
			  
			  fn main() {
			    let article = NewsArticle {
			        headline: String::from("Rust is awesome!"),
			        author: String::from("Rustacean"),
			        content: String::from("... content ..."),
			    };
			  
			    println!("Summary: {}", article.summarize()); // Call the summarize method
			  }
			  ```
		- **Generics:** Write code that can work with different types.
		  collapsed:: true
			- `fn generic_function<T: TraitBound>(param: T) { ... }` (generic functions)
			- `struct GenericStruct<T> { field: T }` (generic structs)
			- ```rust
			  fn largest<T: PartialOrd>(list: &[T]) -> &T { // Generic function, T must implement PartialOrd for comparison
			    let mut largest_val = &list[0];
			    for item in list.iter() {
			        if item > largest_val {
			            largest_val = item;
			        }
			    }
			    largest_val
			  }
			  
			  fn main() {
			    let numbers = [1, 5, 2, 8, 3];
			    let largest_number = largest(&numbers);
			    println!("Largest number: {}", largest_number);
			  
			    let chars = ['a', 'z', 'c'];
			    let largest_char = largest(&chars);
			    println!("Largest char: {}", largest_char);
			  }
			  ```
			- `<T>` in function or struct definition introduces a generic type parameter `T`.
			- Trait bounds (`T: PartialOrd`) can constrain the types that generics can be used with.
		- Polymorphism and code reusability through shared behavior (traits) and type parameters (generics).
	- Error Handling
	  collapsed:: true
		- `Result<T, E>` (enum for recoverable errors)
		  collapsed:: true
			- Represents either success (`Ok(T)`) or failure (`Err(E)`).
				- ```rust
				  use std::fs::File;
				  use std::io::Error;
				  
				  fn open_file(filename: &str) -> Result<File, Error> {
				    File::open(filename) // File::open returns Result<File, Error>
				  }
				  
				  fn main() {
				    match open_file("hello.txt") {
				        Ok(file) => println!("File opened successfully! {:?}", file),
				        Err(error) => println!("Error opening file: {:?}", error),
				    }
				  }
				  ```
				- `Result<T, E>` is an enum with two variants: `Ok(T)` (success, holds value of type `T`) and `Err(E)` (error, holds error value of type `E`).
				- `match` is used to handle both success and error cases.
		- `panic!("Error message");` (unrecoverable errors - program crash)
		- `?` operator (propagate errors concisely)
		- `match result { Ok(val) => ..., Err(err) => ... }` (handle `Result` variants)
		- Explicit error handling, `Result` for robustness, `panic!` for exceptional cases.
	- Ownership & Borrowing (References)
	  collapsed:: true
		- **References (& and &mut):**  Borrowing values without taking ownership
		  collapsed:: true
			- `&`: Immutable reference (shared borrow). Allows reading but not modifying. Multiple immutable references are allowed at the same time.
			- `&mut`: Mutable reference (exclusive borrow). Allows modification. Only one mutable reference to a value is allowed at any given time.
			- ```rust
			  fn calculate_length(s: &String) -> usize { // s is a reference to a String, not ownership
			    s.len() // Can access String data through reference, but not own it
			  } // s goes out of scope here, but the String it refers to is NOT dropped because ownership was not transferred
			  
			  fn change_string(s: &mut String) { // Mutable reference (&mut) allows modification
			    s.push_str(", world!");
			  }
			  
			  fn main() {
			    let mut my_string = String::from("hello"); // my_string owns the String data
			  
			    let len = calculate_length(&my_string); // &my_string creates an immutable reference
			    println!("Length: {}", len);
			  
			    change_string(&mut my_string); // &mut my_string creates a mutable reference
			    println!("Modified string: {}", my_string);
			  } // my_string goes out of scope here, and the String data IS dropped (because my_string is the owner)
			  ```
		- Lifetimes (implicit in most cases, explicit `'a` in complex scenarios - ensure reference validity)
		- Core memory safety mechanism, compile-time borrow checker enforces rules.
	- **Resources**
	  collapsed:: true
		- [Rust-lang-Book](https://doc.rust-lang.org/stable/book/)
- Rust Hands-on
  collapsed:: true
	- Concepts involved
	- `guesscli` - Number Guessing Game
	  collapsed:: true
		- Variables and Mutability
		  collapsed:: true
			- ```rust
			  let mut attempts = 0;
			  ```
			  
			  In Rust, variables are immutable by default. This means once you assign a value, you can't change it. To make a variable mutable, you use `mut`. This design choice prevents accidental changes and makes code safer. The compiler forces you to be explicit about what can change.
		- String vs &str
		  collapsed:: true
			- ```rust
			  let input: String = String::new();  // Owned string, can grow/shrink
			  let msg: &str = "hello";             // Borrowed string slice, fixed
			  ```
			  
			  `String` is an owned, heap-allocated string that you can modify. `&str` is a borrowed reference to a string - it's just a view into existing data. Understanding this distinction is fundamental to Rust's ownership model. When you call `read_line()`, you need a `String` because the function needs to append to it.
		- Type Inference
		  collapsed:: true
			- ```rust
			  let guess = input.parse::<u32>()?;  // Explicit type annotation
			  let count = 5;                       // Inferred as i32
			  ```
			  
			  Rust can often figure out types from context, but sometimes you need to be explicit. The turbofish syntax `::<u32>` tells the compiler exactly what type to parse. This is needed when the type can't be inferred from how the value is used.
		- Standard Input/Output
		  collapsed:: true
			- ```rust
			  use std::io::{self, Write};
			  
			  fn prompt(msg: &str) -> String {
			    print!("{msg}");
			    io::stdout().flush().expect("Failed to flush stdout");
			    let mut input = String::new();
			    io::stdin().read_line(&mut input).expect("Failed to read line");
			    input.trim().to_lowercase()
			  }
			  ```
			  
			  **Buffering explained:** When you print to stdout, Rust doesn't immediately write to the terminal. It stores output in a buffer (a temporary memory area) and writes it all at once for efficiency. The `flush()` call forces the buffer to be written immediately. Without it, the prompt might not appear before the user needs to type.
			  
			  **Reading input:** `read_line()` appends to the string rather than replacing it. This is why we create a new empty string each time. The function returns a `Result` because reading might fail - the user might close stdin, or there might be an I/O error.
		- Error Handling Basics
		  collapsed:: true
			- ```rust
			  let Ok(guess) = input.parse::<u32>() else {
			    println!("Please enter a valid number!");
			    continue;
			  };
			  ```
			  
			  This is the `let...else` pattern, introduced in Rust 1.65. It's a concise way to handle errors when you want to do something else if the operation fails. The `else` block must diverge (return, break, continue, or panic) because there's no valid value to continue with.
		- Range Syntax
		  collapsed:: true
			- ```rust
			  1..=100    // Inclusive range: 1, 2, ..., 100
			  1..100     // Exclusive range: 1, 2, ..., 99
			  ```
			  
			  The `..=` operator creates an inclusive range. This is used for both random generation and validation. The `contains()` method checks if a value is within the range without iterating through all values.
		- Pattern Matching
		  collapsed:: true
			- ```rust
			  match guess.cmp(&secret_number) {
			    Ordering::Less => println!("Try Higher!"),
			    Ordering::Greater => println!("Try Lower!"),
			    Ordering::Equal => {
			        println!("Correct!");
			        break;
			    }
			  }
			  ```
			  
			  Pattern matching is one of Rust's most powerful features. Unlike switch statements in other languages, Rust's `match` is exhaustive - the compiler will error if you forget a case. Each arm can contain a block of code, not just a single expression.
		- Signal Handling
		  collapsed:: true
			- ```rust
			  ctrlc::set_handler(move || {
			    print_exit_banner();
			    process::exit(0);
			  }).expect("Error setting Ctrl+C handler");
			  ```
			  
			  This uses a callback function (closure) that runs when Ctrl+C is pressed. The handler runs in a separate thread managed by the ctrlc crate. We use `process::exit()` because the handler can't return a value to the main thread - it just terminates the program.
	- `isthere` - TCP Ping Tool
	  collapsed:: true
		- Networking Fundamentals
		- TCP Protocol
		  collapsed:: true
			- TCP (Transmission Control Protocol) is a connection-oriented protocol. It's like a phone call:
			  1. **Dial** - Client sends SYN packet
			  2. **Answer** - Server responds with SYN-ACK
			  3. **Talk** - Data flows both ways
			  4. **Hang up** - Connection closes
			  
			  ```rust
			  TcpStream::connect_timeout(&addr, Duration::from_secs(2))
			  ```
			  
			  This performs the TCP handshake. If the server responds, the connection succeeds. If not, it times out after 2 seconds.
		- DNS Resolution
		  collapsed:: true
			- ```	rust
			  let target = format!("{host}:{port}");
			  let Ok(mut address) = target.to_socket_addrs() else {
			    println!("Failed to resolve host: {host}");
			    return;
			  };
			  ```
			  
			  **How DNS works:**
			  1. You provide "google.com:443"
			  2. `to_socket_addrs()` queries DNS
			  3. Returns an iterator of possible IP addresses
			  4. We take the first one
			  
			  This is an iterator because domains can have multiple IP addresses (for load balancing). The `ToSocketAddrs` trait is implemented for strings, tuples, and other types.
		- Advanced Struct Patterns
		  collapsed:: true
			- ```rust
			  struct PingStatus {
			    successful: u32,
			    failed: u32,
			    total_time: Duration,
			    min_time: Duration,
			    max_time: Duration,
			  }
			  
			  impl PingStatus {
			    const fn new() -> Self {
			        Self {
			            successful: 0,
			            failed: 0,
			            total_time: Duration::ZERO,
			            min_time: Duration::MAX,
			            max_time: Duration::ZERO,
			        }
			    }
			  }
			  ```
			  
			  **The constructor pattern:** `new()` is a convention, not a language feature. It's just a function that returns `Self`. The `const fn` allows this to be evaluated at compile time if called with constant arguments.
			  
			  **Duration::MAX trick:** We initialize `min_time` to the largest possible duration. Any real ping time will be smaller, so the comparison `if elapsed < self.min_time` will always be true for the first ping. This avoids needing an `Option<Duration>` for the min/max.
		- Ownership and Borrowing
		  collapsed:: true
			- ```rust
			  fn record_success(&mut self, elapsed: Duration) {
			    self.successful += 1;
			    self.total_time += elapsed;
			  }
			  ```
			- The `&mut self` means this method borrows the struct mutably. This prevents:
			- Multiple mutable references (data races)
			- Mutable and immutable references simultaneously
			- Use after free
			- The borrow checker enforces these rules at compile time.
		- Thread Safety with Arc and AtomicBool
		  collapsed:: true
			- ```rust
			  let running = Arc::new(AtomicBool::new(true));
			  let r = Arc::clone(&running);
			  
			  ctrlc::set_handler(move || {
			    r.store(false, Ordering::SeqCst);
			  }).expect("Error setting Ctrl+C handler");
			  ```
			- **Why this is needed:**
				- Ctrl+C handler runs in a different thread
				- Normal `bool` can't be shared safely between threads
				- `Arc` (Atomic Reference Counting) allows sharing
				- `AtomicBool` provides thread-safe read/write
			- **Arc explained:** When you clone an `Arc`, you don't copy the data - you increment a reference count. When all references are dropped, the data is freed. This is Rust's way of doing shared ownership.
			- **AtomicBool explained:** Regular `bool` operations aren't atomic - another thread might see a half-updated value. `AtomicBool` guarantees that reads and writes are complete operations that can't be interrupted.
		- Command Line Argument Parsing
		  collapsed:: true
			- ```rust
			  let args: Vec<String> = env::args().collect();
			  let Some(raw_target) = args.get(1) else {
			    print_usage();
			    return;
			  };
			  ```
			  
			  **Manual parsing:** We're manually handling arguments here. This is error-prone but educational. We need to:
			  1. Check if arguments exist
			  2. Parse them to the right types
			  3. Handle invalid input
			  
			  ```rust
			  let (host, inline_port) = if let Some((h, p)) = raw_target.rsplit_once(':') {
			    (h, p.parse::<u16>().ok())
			  } else {
			    (raw_target.as_str(), None)
			  };
			  ```
			  
			  **`rsplit_once(':'):`** Splits from the right at the first colon. This handles IPv6 addresses correctly (which contain colons). For "google.com:80", it gives ("google.com", "80"). For "[::1]:8080", it gives ("[::1]", "8080").
		- Time Measurement
		  collapsed:: true
			- ```rust
			  let start: Instant = Instant::now();
			  if let Ok(_stream) = TcpStream::connect_timeout(&addr, Duration::from_secs(2)) {
			    let elapsed = start.elapsed();
			    stats.record_success(elapsed);
			  }
			  ```
		- **Instant vs SystemTime:**
		  collapsed:: true
			- `Instant` is for measuring elapsed time (monotonic clock)
			- `SystemTime` is for wall-clock time (can go backwards)
			- For performance measurement, always use `Instant`. It's not affected by system clock changes.
			  
			  ---
	- `restcli` - REST Client
	  collapsed:: true
		- Async Programming
		  collapsed:: true
			- Why Async?
			  collapsed:: true
				- Traditional synchronous I/O blocks the thread. If you're downloading a file, you can't do anything else. Async allows multiple operations to progress without blocking.
				  
				  ```rust
				  #[tokio::main]
				  async fn main() -> Result<(), Box<dyn Error>> {
				    let response = request.send().await?;
				  }
				  ```
				- **How async works:**
				  1. `async fn` returns a `Future`
				  2. A `Future` is a value that might not be ready yet
				  3. `await` polls the future until it's complete
				  4. While waiting, other tasks can run
				- **The tokio runtime:** `#[tokio::main]` sets up an async runtime that manages all async tasks. It's like an event loop that continuously checks which tasks are ready.
		- CLI Parsing with clap
		  collapsed:: true
			- ```rust
			  #[derive(Parser, Debug)]
			  struct Cli {
			    /// The URL to make a request to
			    url: String,
			  
			    /// HTTP method to use
			    #[arg(short, long, default_value = "GET", value_parser = parse_method)]
			    method: Method,
			  }
			  ```
			- **Derive macros:** The `#[derive(Parser)]` generates code at compile time that parses command line arguments into this struct. This is metaprogramming - code that writes code.
			- **Attributes:**
				- `#[arg(short, long)]` - Creates `-m` and `--method` flags
				- `default_value` - Default if not specified
				- `value_parser` - Custom parsing function
		- The Builder Pattern
		  collapsed:: true
			- ```rust
			  let client = Client::builder()
			    .user_agent("restcli/0.1.0")
			    .timeout(Duration::from_secs(30))
			    .build()?;
			  ```
			- **Why builder pattern?**
				- Many optional parameters
				- Readable configuration
				- Can validate before building
				- Immutable after construction
			- **How it works:**
			  1. `builder()` returns a `ClientBuilder`
			  2. Each method modifies the builder
			  3. `build()` creates the final `Client`
			  4. The builder is consumed
		- Error Handling with Result
		  collapsed:: true
			- ```rust
			  fn create_client() -> Result<Client, Box<dyn Error>> {
			    Ok(Client::builder()
			        .timeout(Duration::from_secs(30))
			        .build()?)
			  }
			  ```
			  
			  **The `?` operator:**
			  ```rust
			  // Without ?
			  let result = something();
			  let value = match result {
			    Ok(v) => v,
			    Err(e) => return Err(e),
			  };
			  
			  // With ?
			  let value = something()?;
			  ```
			  
			  **Box<dyn Error>:** This is a trait object. It can hold any error type. The `Box` puts it on the heap because we don't know the size at compile time.
		- Option and Result Composition
		  collapsed:: true
			- ```rust
			  fn get_body_data(cli: &Cli) -> Result<Option<String>, Box<dyn Error>> {
			    if let Some(data) = &cli.data {
			        return Ok(Some(data.clone()));
			    }
			    Ok(None)
			  }
			  ```
			- **Why Result<Option<String>>?**
				- Outer `Result`: Operation might fail (file I/O error)
				- Inner `Option`: Data might not exist (user didn't provide any)
				- This is different from just `Result<String, Error>` because "no data" is not an error - it's a valid state.
		- Request Building
		  collapsed:: true
			- ```rust
			  let mut request = client.request(method, url);
			  request = request.header(key, value);
			  request = request.body(data);
			  ```
			- **Ownership in builders:** Each method consumes `self` and returns a new builder. The old builder is dropped. This prevents using a builder after it's been modified.
		- Response Processing
		  collapsed:: true
			- ```rust
			  let response = request.send().await?;
			  let status = response.status();
			  let body = response.text().await?;
			  
			  match serde_json::from_str::<serde_json::Value>(&body) {
			    Ok(parsed) => {
			        let pretty = serde_json::to_string_pretty(&parsed)?;
			        println!("{pretty}");
			    }
			    Err(_) => {
			        println!("{body}");
			    }
			  }
			  ```
			  
			  **Serde JSON:** The `serde_json::Value` is an enum that can represent any JSON value:
			  ```rust
			  enum Value {
			    Null,
			    Bool(bool),
			    Number(Number),
			    String(String),
			    Array(Vec<Value>),
			    Object(Map<String, Value>),
			  }
			  ```
			  
			  We try to parse the response as JSON. If it works, we pretty-print it. If not, we show the raw text.
		- Status Code Handling
		  collapsed:: true
			- ```rust
			  let status_colored = match status.as_u16() {
			    200..=299 => status_str.green(),
			    400..=499 => status_str.yellow(),
			    500..=599 => status_str.red(),
			    _ => status_str.normal(),
			  };
			  ```
			  
			  This uses range patterns in a match statement. HTTP status codes are grouped:
				- 2xx: Success
				- 4xx: Client errors
				- 5xx: Server errors
-