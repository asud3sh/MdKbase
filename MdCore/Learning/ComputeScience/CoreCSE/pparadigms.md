# 7 · PROGRAMMING PARADIGMS

## 7.1 Imperative Programming

**Definition:** Imperative programming describes computation as a sequence of statements that change program state. The programmer specifies *how* to compute, step by step.

**Core characteristics:**
- Variables hold mutable state
- Statements execute in order
- Control flow via loops, conditionals, jumps
- Assignment changes state

**Example (C):**
```c
int sum = 0;
for (int i = 1; i <= n; i++) {
    sum += i;
}
```

**Strengths:** Close to machine model; efficient; intuitive for many problems.
**Weaknesses:** State management complexity; harder to reason about; side effects.

**Languages:** C, Pascal, Assembly, Fortran (early).

**Recall:** *Step-by-step state changes.*

---

## 7.2 Procedural Programming

**Definition:** Imperative programming organized around procedures (functions) that encapsulate reusable logic.

**Core characteristics:**
- Procedures/functions as primary abstraction
- Parameters and return values
- Modular decomposition
- Top-down design

**Example (C):**
```c
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

**Strengths:** Reusability, modularity, easier testing.
**Weaknesses:** Global state still problematic; doesn't scale to large systems without discipline.

**Languages:** C, Fortran, Pascal, COBOL.

**Recall:** *Imperative + functions.*

---

## 7.3 Object-Oriented Programming (OOP)

**Definition:** Programs are organized around objects that encapsulate state (fields) and behavior (methods). Objects interact via message passing.

**Core concepts:**
| Concept | Meaning |
|---------|---------|
| Class | Blueprint for objects |
| Object | Instance of a class |
| Encapsulation | Hide internals; expose interface |
| Inheritance | Derive class from parent |
| Polymorphism | Same interface, different behavior |
| Abstraction | Model essential features |

**Example (Java):**
```java
class Animal {
    void speak() { System.out.println("..."); }
}
class Dog extends Animal {
    @Override
    void speak() { System.out.println("Woof"); }
}
```

**Design principles:**
- **SOLID:** SRP, OCP, LSP, ISP, DIP
- **Composition over inheritance:** More flexible
- **Program to interface, not implementation**

**Strengths:** Models real-world domains; reusable; extensible.
**Weaknesses:** Over-engineering; inheritance hierarchies can become fragile.

**Languages:** Java, C++, Python, Smalltalk, C#, Ruby.

**Recall:** *Objects + messages.*

---

## 7.4 Functional Programming (FP)

**Definition:** Computation as evaluation of mathematical functions; avoids mutable state and side effects.

**Core concepts:**
- **Pure functions:** Same input → same output; no side effects
- **Immutability:** Data cannot change after creation
- **First-class functions:** Functions as values
- **Higher-order functions:** Take/return functions
- **Recursion:** Primary control structure
- **Lazy evaluation:** Evaluate only when needed

**Example (Haskell):**
```haskell
factorial :: Int -> Int
factorial 0 = 1
factorial n = n * factorial (n - 1)

-- Higher-order
map (*2) [1,2,3]  -- [2,4,6]
filter even [1..10]  -- [2,4,6,8,10]
foldr (+) 0 [1..5]  -- 15
```

**Key abstractions:**
- **Functor:** Mappable structure
- **Monad:** Sequenced computations with context (Maybe, Either, IO, State)
- **Applicative:** Function application in context

**Strengths:** Easier reasoning; concurrency-safe; composable.
**Weaknesses:** Performance overhead; steep learning curve; not all problems fit.

**Languages:** Haskell, Lisp, Erlang, Clojure, F#, OCaml, Elixir.

**Recall:** *No side effects.*

---

## 7.5 Logic Programming

**Definition:** Programs consist of facts and rules; computation is querying the knowledge base.

**Core concepts:**
- **Facts:** Ground truths (e.g., `parent(alice, bob)`)
- **Rules:** Implications (e.g., `ancestor(X,Y) :- parent(X,Y)`)
- **Query:** Ask if something is true; find bindings
- **Unification:** Match terms with variables
- **Backtracking:** Try alternatives on failure

**Example (Prolog):**
```prolog
parent(alice, bob).
parent(bob, charlie).
ancestor(X, Y) :- parent(X, Y).
ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y).

?- ancestor(alice, charlie).
true.
```

**Strengths:** Declarative; good for rule-based systems, NLP, databases.
**Weaknesses:** Performance; hard to debug; limited ecosystem.

**Languages:** Prolog, Datalog, Mercury.

**Recall:** *Declare facts, ask questions.*

---

## 7.6 Declarative Programming

**Definition:** Describe *what* the result should be, not *how* to compute it.

**Examples:**
- **SQL:** `SELECT * FROM users WHERE age > 18`
- **HTML:** `<div class="container">...</div>`
- **Regex:** `\d{3}-\d{4}`
- **Prolog:** Rules and queries

**Strengths:** Concise; easier to optimize; less error-prone.
**Weaknesses:** Limited expressiveness; performance opaque.

**Recall:** *What, not how.*

---

## 7.7 Concurrent Programming

**Definition:** Multiple computations execute concurrently (interleaved or parallel).

**Models:**
- **Threads + locks:** Shared memory; mutex, semaphore
- **Actors:** Isolated state; message passing (Erlang, Akka)
- **CSP:** Channels between processes (Go, Occam)
- **STM:** Transactional memory (Haskell, Clojure)

**Example (Go):**
```go
ch := make(chan int)
go func() { ch <- 42 }()
x := <-ch
```

**Strengths:** Utilizes multicore; responsive systems.
**Weaknesses:** Race conditions, deadlocks, complexity.

**Languages:** Erlang, Go, Rust, Java (threads), Clojure (STM).

**Recall:** *Parallel execution model.*

---

## 7.8 Reactive Programming

**Definition:** Programs react to data streams and change propagation.

**Core concepts:**
- **Observable:** Stream of events
- **Observer:** Subscriber to events
- **Operators:** map, filter, merge, debounce
- **Backpressure:** Handle fast producers

**Example (RxJS):**
```javascript
observable
  .filter(x => x > 10)
  .map(x => x * 2)
  .subscribe(x => console.log(x));
```

**Strengths:** Elegant for async, UI, streams.
**Weaknesses:** Hard to debug; complex operator semantics.

**Languages/Frameworks:** RxJS, React, Elm, Vue, Svelte.

**Recall:** *React to changes.*

---

## 7.9 Dataflow Programming

**Definition:** Computation as a directed graph of nodes; data flows through edges.

**Core concepts:**
- **Nodes:** Operations
- **Edges:** Data dependencies
- **Data-driven:** Nodes fire when inputs ready

**Examples:** TensorFlow, LabVIEW, Apache NiFi, Simulink.

**Strengths:** Parallelism implicit; visual; good for signal processing, ML.
**Weaknesses:** Limited control flow; overhead.

**Recall:** *Data flows through graph.*

---

## 7.10 Aspect-Oriented Programming

**Definition:** Modularize cross-cutting concerns (logging, security, transactions) as aspects.

**Core concepts:**
- **Aspect:** Module encapsulating cross-cutting concern
- **Join point:** Point in execution (method call, field access)
- **Pointcut:** Expression selecting join points
- **Advice:** Code to run at join point
- **Weaving:** Integrate aspects into code

**Example (AspectJ):**
```java
aspect Logging {
    before(): call(* *.save(..)) {
        System.out.println("Saving...");
    }
}
```

**Strengths:** Separation of concerns; reduces code duplication.
**Weaknesses:** Complexity; debugging; tooling.

**Languages:** AspectJ, Spring AOP.

**Recall:** *Separate cross-cutting concerns.*

---

## 7.11 Array Programming

**Definition:** Operations apply to entire arrays, not individual elements.

**Examples:**
- **NumPy:** `a + b` adds arrays element-wise
- **APL:** `+/1 2 3` sums array
- **MATLAB:** `A * B` matrix multiply

**Strengths:** Concise; vectorized performance; good for numeric computing.
**Weaknesses:** Limited to array-shaped problems.

**Recall:** *Vectorized operations.*

---

## 7.12 Probabilistic Programming

**Definition:** Programs encode probabilistic models; inference is automatic.

**Core concepts:**
- **Random variables:** Distributions
- **Conditioning:** Observe data
- **Inference:** Compute posterior

**Example (Stan):**
```stan
data { int N; real y[N]; }
parameters { real mu; real<lower=0> sigma; }
model { y ~ normal(mu, sigma); }
```

**Strengths:** Declarative Bayesian modeling; uncertainty quantification.
**Weaknesses:** Computational cost; inference complexity.

**Languages:** Stan, PyMC, Edward, WebPPL.

**SOTA:** Multi-paradigm languages, effect handlers, dependent types, differentiable programming.

**Recall:** *Models as programs.*

---
