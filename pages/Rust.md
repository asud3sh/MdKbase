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