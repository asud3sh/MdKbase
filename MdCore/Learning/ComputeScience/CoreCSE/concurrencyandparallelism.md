
# 11 · CONCURRENCY & PARALLELISM

## 11.1 Foundations

### Concurrency vs Parallelism

**Concurrency:** Dealing with multiple tasks at once (structure).
**Parallelism:** Doing multiple tasks simultaneously (execution).

**Analogy:** Concurrency = juggling; parallelism = multiple jugglers.

**Recall:** *Concurrent = structure; Parallel = execution.*

### Threads

**Shared:** Heap, globals, file descriptors.
**Private:** Stack, registers, PC.

**Creation:** pthread_create, std::thread, goroutines, async tasks.

**Recall:** *Lightweight execution unit.*

### Locks

**Mutex:** Binary lock.
**Spinlock:** Busy-wait; good for short critical sections.
**RW lock:** Multiple readers or one writer.
**Semaphore:** Counting.

**Contention:** Many threads waiting → overhead.
**Fairness:** FIFO vs priority.

**Recall:** *Mutual exclusion primitive.*

### Atomic Operations

**CAS (Compare-and-Swap):** `if (*ptr == old) { *ptr = new; return true; } else return false;`

**Fetch-and-add:** Atomic increment.

**Memory ordering:** Acquire, release, relaxed, sequentially consistent.

**Recall:** *Indivisible operations.*

### Memory Models

**Sequential consistency:** All operations in program order; global order.

**TSO (Total Store Order):** x86; writes buffered; reads see own writes.

**Relaxed:** ARM, RISC-V; reordering allowed.

**Happens-before:** If A happens-before B, A's effects visible to B.

**Recall:** *Rules for memory visibility.*

### Race Conditions

**Definition:** Unsynchronized access to shared data → unpredictable result.

**Example:** Two threads increment counter; lost update.

**Detection:** ThreadSanitizer (TSan), Helgrind.

**Prevention:** Locks, atomics, immutability.

**Recall:** *Order matters.*

---

## 11.2 Advanced Concurrency

### Lock-Free Data Structures

**Definition:** No locks; uses atomic operations.

**Progress guarantees:**
- **Wait-free:** Every thread finishes in bounded steps
- **Lock-free:** Some thread finishes
- **Obstruction-free:** Thread finishes if alone

**Examples:** Michael-Scott queue, Treiber stack.

**ABA problem:** Value changes A→B→A; CAS succeeds incorrectly. Solution: tagged pointers, hazard pointers.

**Recall:** *No locks, still correct.*

### Actors

**Definition:** Isolated state + message passing.

**Properties:**
- Each actor has mailbox
- Processes messages one at a time
- Can create new actors
- Can send messages

**Examples:** Erlang, Akka, Elixir.

**Benefits:** No shared state; location transparency; fault tolerance (supervision trees).

**Recall:** *Message-passing concurrency.*

### CSP (Communicating Sequential Processes)

**Definition:** Processes communicate via channels.

**Example (Go):**
```go
ch := make(chan int)
go func() { ch <- 42 }()
x := <-ch
```

**vs Actors:** CSP channels are first-class; actors have implicit mailboxes.

**Recall:** *Channels between processes.*

### Futures & Promises

**Future:** Placeholder for async result.
**Promise:** Writable future.

**Composition:** then, map, flatMap, await.

**Example:**
```python
future = executor.submit(task)
result = future.result()
```

**Recall:** *Async result container.*

### Async/Await

**Definition:** Syntactic sugar over futures.

**Example:**
```python
async def fetch():
    data = await http.get(url)
    return data
```

**Event loop:** Runs tasks; switches on await.

**Recall:** *Write async like sync.*

---

## 11.3 Parallel Computing

### Parallel Algorithms

**Work-depth model:** Work = total operations; depth = longest dependency chain.

**Examples:**
- **Parallel prefix:** O(n) work, O(log n) depth
- **Parallel merge:** O(n) work, O(log n) depth
- **Parallel sort:** O(n log n) work, O(log² n) depth

**Recall:** *Divide + run in parallel.*

### MapReduce

**Model:**
1. **Map:** (k1, v1) → list(k2, v2)
2. **Shuffle:** Group by key
3. **Reduce:** (k2, list(v2)) → list(v3)

**Example:** Word count.
```
map: (doc, "hello world") → [(hello, 1), (world, 1)]
reduce: (hello, [1,1,1]) → (hello, 3)
```

**Implementations:** Hadoop, Spark.

**Recall:** *Parallel data processing.*

### GPU Computing

**SIMT:** Single Instruction, Multiple Threads.

**Thread hierarchy:** Thread → warp (32) → block → grid.

**Memory:** Registers, shared memory (per block), global memory.

**Example (CUDA):**
```cuda
__global__ void add(int *a, int *b, int *c) {
    int i = threadIdx.x;
    c[i] = a[i] + b[i];
}
```

**Recall:** *Thousands of threads.*

**SOTA:** Structured concurrency, async runtimes (Tokio, async-std), software transactional memory, actor frameworks.

---
