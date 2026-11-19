
# 8 · COMPILERS & INTERPRETERS

## 8.1 Frontend

### Lexing (Lexical Analysis)

**Definition:** Convert source character stream into tokens.

**Token types:** Keywords, identifiers, literals, operators, punctuation.

**Techniques:**
- **Regular expressions:** Pattern for each token type
- **DFA:** Finite automaton recognizing tokens
- **Lex/Flex:** Generator tools

**Example:**
```
Input:  "int x = 42;"
Tokens: [KEYWORD:int, IDENT:x, ASSIGN:=, NUM:42, SEMI:;]
```

**Challenges:** Longest match, lookahead, error recovery.

**Recall:** *Chars → tokens.*

### Parsing (Syntax Analysis)

**Definition:** Build parse tree from tokens according to grammar.

**Grammar:** Context-free grammar (CFG) with terminals and nonterminals.

**Parsing strategies:**
| Strategy | Direction | Type | Examples |
|----------|-----------|------|----------|
| Recursive descent | Top-down | LL | Hand-written |
| LL(k) | Top-down | Predictive | ANTLR |
| LR(k) | Bottom-up | Shift-reduce | Yacc, Bison |
| LALR | Bottom-up | LR variant | Yacc |
| PEG | Top-down | Parsing expression | Pest |

**Parse tree vs AST:** Parse tree includes all grammar details; AST is simplified.

**Error recovery:** Panic mode, phrase-level, error productions.

**Recall:** *Tokens → tree.*

### Abstract Syntax Trees (AST)

**Definition:** Simplified tree representation of program structure.

**Nodes:** Expressions, statements, declarations.

**Uses:**
- Semantic analysis
- Optimization
- Code generation
- Static analysis
- Refactoring tools

**Example:**
```
x = 1 + 2 * 3
```
AST:
```
Assign
├── Var(x)
└── Add
    ├── Num(1)
    └── Mul
        ├── Num(2)
        └── Num(3)
```

**Recall:** *Tree of syntax.*

### Semantic Analysis

**Definition:** Check meaning and consistency of program.

**Tasks:**
- **Type checking:** Ensure operations are type-safe
- **Scope resolution:** Bind names to declarations
- **Symbol tables:** Track declarations
- **Definite assignment:** Variable used before assigned
- **Control flow analysis:** Reachability, loops

**Example:** Type error: `int x = "hello";`

**Recall:** *Meaning + types.*

---

## 8.2 Middle-End

### Intermediate Representation (IR)

**Definition:** Machine-independent representation between source and target.

**Forms:**
- **Three-address code:** `t1 = a + b`
- **SSA (Static Single Assignment):** Each variable assigned once
- **LLVM IR:** Rich, typed, SSA-based
- **Bytecode:** Stack-based or register-based

**Example (three-address):**
```
t1 = b * c
t2 = a + t1
x = t2
```

**SSA form:**
```
t1 = b * c
t2 = a + t1
x = t2
```

**Benefits:** Optimization-friendly; portable; analyzable.

**Recall:** *Middle layer for optimization.*

### Optimization

**Definition:** Transform IR to improve performance without changing behavior.

**Local optimizations:**
- **Constant folding:** `2 + 3` → `5`
- **Constant propagation:** Replace variable with known constant
- **Dead code elimination:** Remove unreachable/unused code
- **Common subexpression elimination:** Reuse computed values

**Global optimizations:**
- **Loop invariant code motion:** Move code out of loops
- **Loop unrolling:** Duplicate loop body
- **Inlining:** Replace call with body
- **Vectorization:** Use SIMD instructions

**Interprocedural:**
- **Whole-program analysis**
- **Link-time optimization (LTO)**

**Trade-offs:** Compile time vs runtime; code size vs speed.

**Recall:** *Make code faster, same behavior.*

---

## 8.3 Backend

### Code Generation

**Definition:** Translate IR to target assembly/machine code.

**Tasks:**
- **Instruction selection:** Choose instructions for IR ops
- **Register allocation:** Map variables to registers
- **Instruction scheduling:** Order for pipeline efficiency
- **Peephole optimization:** Local pattern-based improvements

**Challenges:** Target-specific; register pressure; instruction latencies.

**Recall:** *IR → machine code.*

### Register Allocation

**Definition:** Assign variables to CPU registers; spill to memory when insufficient.

**Graph coloring approach:**
1. Build interference graph (edge if variables live simultaneously)
2. Color graph with k colors (k = number of registers)
3. Spill variables that can't be colored

**Linear scan:** Faster, less optimal; used in JITs.

**Spilling:** Store variable in memory; load/store around uses.

**Recall:** *Map variables → registers.*

### Linkers & Loaders

**Linker:** Combine object files into executable.
- **Symbol resolution:** Match references to definitions
- **Relocation:** Adjust addresses

**Loader:** Load executable into memory; start execution.

**Static linking:** Libraries copied into executable at link time.
**Dynamic linking:** Libraries loaded at runtime (DLLs, .so).

**Recall:** *Combine objects → executable.*

---

## 8.4 Runtimes & VMs

### Virtual Machines

**Definition:** Abstract machine executing bytecode.

**Types:**
- **Stack-based:** JVM, CPython; operands on stack
- **Register-based:** Dalvik, Lua; operands in registers

**Example (JVM bytecode):**
```
iload_1    // push local 1
iload_2    // push local 2
iadd       // add
istore_3   // store to local 3
```

**Benefits:** Portability, safety, managed memory.

**Recall:** *Abstract machine for portability.*

### Bytecode

**Definition:** Portable intermediate representation executed by VM.

**Examples:**
- **JVM bytecode:** Java, Kotlin, Scala
- **CPython bytecode:** Python
- **WASM:** WebAssembly; portable, sandboxed
- **CLR bytecode:** C#, F#

**Benefits:** Platform-independent; can be JIT-compiled.

**Recall:** *Compiled IR for VM.*

### LLVM

**Definition:** Modular compiler infrastructure; frontend → LLVM IR → backend.

**Components:**
- **Clang:** C/C++ frontend
- **LLVM IR:** SSA-based intermediate
- **Optimization passes:** Hundreds available
- **Backends:** x86, ARM, RISC-V, etc.

**Strengths:** Reusable; extensible; high-quality codegen.

**Recall:** *Compiler as library.*

### WebAssembly (WASM)

**Definition:** Portable binary instruction format for web and beyond.

**Properties:**
- Sandboxed
- Near-native speed
- Language-agnostic (C, C++, Rust, Go, etc.)
- Small binary size

**Uses:** Web apps, edge computing, serverless, blockchain.

**Recall:** *Bytecode for the web.*

### Garbage Collection Implementation

**Algorithms:**
- **Mark-sweep:** Mark reachable; sweep unreachable
- **Mark-compact:** Mark; compact to eliminate fragmentation
- **Copying:** Copy live objects to new space
- **Generational:** Young/old generations; collect young often

**Concurrent:** Collector runs alongside mutator.
**Incremental:** Small pauses.

**Trade-offs:** Throughput vs pause time vs memory.

### JIT Compilation

**Definition:** Compile bytecode to machine code at runtime.

**Techniques:**
- **Hot path detection:** Compile frequently executed code
- **Tiered compilation:** Interpret → baseline JIT → optimizing JIT
- **Deoptimization:** Fall back to interpreter on wrong assumptions
- **Speculative optimization:** Assume common cases; guard

**Examples:** V8 (JavaScript), HotSpot (Java), PyPy (Python).

**Recall:** *Compile while running.*

### Interpreter Design

**Types:**
- **AST walker:** Recursively evaluate AST
- **Bytecode interpreter:** Loop over bytecode instructions
- **Threaded code:** Each instruction points to handler

**Trade-offs:** Startup vs peak performance.

### Domain-Specific Compilers

- **SQL query compilers:** Optimize relational queries
- **Regex compilers:** NFA/DFA construction
- **Tensor compilers:** XLA, TVM; optimize ML graphs
- **Shader compilers:** GLSL, HLSL → GPU code

### Verified Compilation

**CompCert:** Formally verified C compiler; proves correctness in Coq.

**Translation validation:** Verify each compilation step.

**SOTA:** MLIR, LLM-based compiler hint synthesis (HintPilot achieving up to 6.88× speedup over -Ofast), neural-symbolic RTL optimization (SymRTLO).

---

