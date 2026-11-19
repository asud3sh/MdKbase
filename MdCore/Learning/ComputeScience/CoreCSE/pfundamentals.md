
# 6 · PROGRAMMING FUNDAMENTALS

## 6.1 Core Concepts

### Variables & Types

**Variable:** Named storage location holding a value.

**Binding:** Association between name and value.

**Scope:** Region where name is visible.
- **Lexical (static):** Determined by program text
- **Dynamic:** Determined by runtime call stack

**Lifetime:** Duration variable exists.
- **Automatic:** Stack-allocated; ends at scope exit
- **Static:** Program duration
- **Dynamic:** Heap-allocated; ends at free

**Mutability:** Whether value can change.

**Type systems:**
| Dimension             | Options              |
| --------------------- | -------------------- |
| Checking              | Static vs dynamic    |
| Strength              | Strong vs weak       |
| Inference             | Explicit vs inferred |
| Nominal vs structural | By name vs by shape  |

**Static typing:** Errors at compile time; safer, faster.
**Dynamic typing:** Errors at runtime; flexible, slower.

**Recall:** *Name → value → type.*

### Control Flow

**Sequence:** Statements execute in order.

**Selection:** Branch based on condition.
- if/else
- switch/match
- Ternary operator

**Iteration:** Repeat block.
- for: counted loop
- while: condition-checked loop
- do-while: condition-checked after body

**Jump statements:**
- break: exit loop
- continue: skip to next iteration
- goto: unconditional jump (avoid)
- return: exit function

**Recall:** *Sequence, branch, loop.*

### Functions

**Definition:** Reusable block of code with parameters and return value.

**Parameters:** Inputs.
- **By value:** Copy passed
- **By reference:** Alias passed
- **By pointer:** Address passed

**Return:** Output value.

**Higher-order functions:** Take or return functions.

**Pure functions:** No side effects; same input → same output.

**Side effects:** Modify state outside function.

**Recall:** *Input → output.*

### Recursion

**Definition:** Function calls itself.

**Base case:** Terminates recursion.

**Recursive case:** Reduces problem.

**Stack frames:** Each call pushes frame with local variables and return address.

**Tail call optimization:** Compiler converts tail recursion to iteration; avoids stack overflow.

**Example:** Factorial.
```
factorial(n):
    if n <= 1: return 1
    return n * factorial(n−1)
```

**Recall:** *Function calls itself.*

### Scope & Lifetime

**Lexical scope:** Name resolution based on program text.
```
x = 1
def f():
    print(x)  # sees outer x
def g():
    x = 2
    f()  # still prints 1
```

**Dynamic scope:** Name resolution based on call stack.
```
x = 1
def f():
    print(x)  # sees caller's x
def g():
    x = 2
    f()  # prints 2
```

**Storage classes:**
- **Stack:** Automatic; fast; limited size
- **Heap:** Dynamic; flexible; slower
- **Static:** Program duration; fixed

**Recall:** *Where names live and die.*

---

## 6.2 Memory & Errors

### Pointers & References

**Pointer:** Variable holding memory address.

**Reference:** Alias for another variable.

**Operations:**
- `&x`: address of x
- `*p`: dereference p
- `p->field`: access field via pointer

**Common bugs:**
- **Dangling pointer:** Points to freed memory
- **Null pointer:** Points to nothing; dereferencing crashes
- **Wild pointer:** Uninitialized; arbitrary address
- **Memory leak:** Allocated but never freed

**Aliasing:** Two pointers to same memory; can cause subtle bugs.

**Recall:** *Pointer = address.*

### Memory Model

**Stack:**
- Automatic allocation
- LIFO
- Fast (pointer increment)
- Limited size (~8 MB typical)
- Stores local variables, function frames

**Heap:**
- Dynamic allocation (malloc/new)
- Manual or GC-managed
- Slower (allocator overhead)
- Large
- Stores objects, dynamic arrays

**Static:**
- Global variables
- Program duration
- Fixed size

**Code:**
- Program instructions
- Read-only

**Alignment:** Data must be at addresses multiple of size (e.g., 4-byte int at multiple of 4).

**Padding:** Inserted to satisfy alignment; wastes space.

**Recall:** *Stack fast, heap flexible.*

### Error Handling

**Exceptions:** Runtime error propagation.
```
try:
    risky()
except Exception as e:
    handle(e)
finally:
    cleanup()
```

**Error codes:** Return integer status.
```
if (result = operation()) != OK:
    handle(result)
```

**Result/Option types:** Explicit error handling.
```
Result<T, E> = Ok(T) | Err(E)
Option<T> = Some(T) | None
```

**RAII (Resource Acquisition Is Initialization):** Resource tied to object lifetime; freed in destructor.

**Recall:** *Detect → propagate → handle.*

---

## 6.3 Abstractions

### Modules & Packages

**Module:** Unit of code organization with public/private interface.

**Namespace:** Scope for names; prevents collisions.

**Import:** Bring names from another module.

**Visibility:** Public, private, protected, internal.

**Semantic versioning:** MAJOR.MINOR.PATCH.
- MAJOR: breaking changes
- MINOR: new features, backward compatible
- PATCH: bug fixes

**Recall:** *Encapsulate + expose.*

### Object-Oriented Programming (OOP)

**Four pillars:**
1. **Encapsulation:** Bundle data + methods; hide internals
2. **Abstraction:** Expose essential, hide details
3. **Inheritance:** Derive class from parent
4. **Polymorphism:** Same interface, different implementations

**Class:** Blueprint for objects.

**Object:** Instance of class.

**Inheritance:** Child inherits parent's members.
- **Single:** One parent
- **Multiple:** Many parents (diamond problem)
- **Composition:** Has-a relationship (preferred over inheritance)

**Polymorphism:**
- **Compile-time:** Overloading, templates
- **Runtime:** Virtual methods, interfaces

**Composition > Inheritance:** More flexible, avoids fragile base class problem.

**Recall:** *4 pillars: encapsulation, abstraction, inheritance, polymorphism.*

### Functional Programming

**Pure functions:** No side effects; referentially transparent.

**Immutability:** Data cannot change after creation.

**Higher-order functions:** Take/return functions.

**Currying:** Transform f(x,y) into f(x)(y).

**Map/filter/reduce:**
- map: transform each element
- filter: keep elements matching predicate
- reduce: combine elements into single value

**Monads:** Abstract pattern for sequencing computations with context.
- Maybe: optional value
- Either: value or error
- IO: side effects
- State: mutable state

**Recall:** *Functions as values, no side effects.*

### Generic Programming

**Definition:** Write code once, use for many types.

**Templates (C++):** Compile-time instantiation.

**Generics (Java, C#):** Type parameters with erasure or reification.

**Traits (Rust):** Define shared behavior.

**Concepts (C++20):** Constraints on template parameters.

**Benefits:** Code reuse, type safety, performance.

**Recall:** *Write once, use for many types.*

### Metaprogramming

**Definition:** Code that writes or manipulates code.

**Macros:** Textual or syntactic transformation at compile time.

**Reflection:** Inspect/modify program structure at runtime.

**Code generation:** Produce code programmatically.

**Compile-time vs runtime:** Compile-time is faster but less flexible.

**Recall:** *Code that writes code.*

### Concurrency Basics

**Thread:** Lightweight process; shares memory.

**Race condition:** Unsynchronized access to shared data.

**Lock:** Mutual exclusion primitive.

**Atomic:** Indivisible operation.

**Async:** Non-blocking execution.

**Future:** Placeholder for async result.

**Actor:** Isolated state + message passing.

**SOTA:** Effect systems, algebraic effects, ownership types, gradual typing.

---