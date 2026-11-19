# 4 · DATA STRUCTURES

## 4.1 Linear Structures

### Arrays

**Definition:** An array is a contiguous block of memory storing elements of the same type, indexed by integer position.

**Key Properties:**
- **Random access:** O(1) — address = base + index × element_size
- **Insert/delete at end:** O(1) if space available
- **Insert/delete in middle:** O(n) — must shift elements
- **Search (unsorted):** O(n) linear scan
- **Search (sorted):** O(log n) binary search

**Dynamic Arrays:** Grow automatically when full.
- **Growth strategy:** Double capacity (amortized O(1) append)
- **Why doubling:** Total cost of n appends = 1 + 2 + 4 + ... + n = O(n); amortized O(1) per append
- **Trade-off:** Wasted space up to 50%

**Example:** Python list, C++ vector, Java ArrayList.

**Why it matters:** Arrays are the foundation of nearly every other data structure. Cache-friendly due to contiguity.

**Recall:** *Fast access, slow insert.*

### Linked Lists

**Definition:** A linked list is a sequence of nodes, each containing data and a pointer to the next node.

**Types:**
- **Singly linked:** Each node points to next
- **Doubly linked:** Each node points to next and previous
- **Circular:** Last node points back to first

**Operations:**
| Operation | Singly | Doubly |
|-----------|--------|--------|
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n) or O(1) with tail ptr | O(1) |
| Delete given node | O(n) | O(1) |
| Search | O(n) | O(n) |
| Access by index | O(n) | O(n) |

**Advantages:** Dynamic size, O(1) insert/delete given node.
**Disadvantages:** No random access, poor cache locality, extra memory for pointers.

**Example:** LRU cache (doubly linked list + hash map).

**Recall:** *Pointers, no random access.*

### Stacks

**Definition:** A stack is a LIFO (Last In, First Out) data structure.

**Operations:**
- **push(x):** Add x to top — O(1)
- **pop():** Remove and return top — O(1)
- **peek():** Return top without removing — O(1)
- **isEmpty():** Check if empty — O(1)

**Implementations:** Array-backed (fixed or dynamic), linked list-backed.

**Applications:**
- Function call stack (recursion)
- Expression evaluation (infix → postfix)
- Undo/redo
- Depth-first search
- Balanced parentheses checking

**Example:** Check if parentheses are balanced: push on open, pop on close; if pop fails or stack non-empty at end, unbalanced.

**Recall:** *LIFO.*

### Queues

**Definition:** A queue is a FIFO (First In, First Out) data structure.

**Operations:**
- **enqueue(x):** Add x to back — O(1)
- **dequeue():** Remove and return front — O(1)
- **peek():** Return front without removing — O(1)

**Variants:**
- **Circular queue:** Reuses array space; O(1) operations
- **Deque (double-ended queue):** Insert/delete at both ends
- **Priority queue:** Elements have priorities; dequeue returns highest priority (usually heap-backed)

**Applications:**
- BFS
- Task scheduling
- Message queues
- Print spooling
- Rate limiting

**Recall:** *FIFO.*

---

## 4.2 Hashing & Sets

### Hash Tables

**Definition:** A hash table maps keys to values using a hash function to compute an index into an array of buckets.

**Components:**
- **Hash function:** h(key) → index
- **Buckets:** Array slots
- **Load factor:** α = n/m (n = entries, m = buckets)

**Collision Resolution:**
- **Chaining:** Each bucket holds a linked list of entries. Simple, handles high load factors.
- **Open addressing:** Probe for next empty slot. Types: linear probing, quadratic probing, double hashing. Better cache locality, but clustering.

**Operations (average):**
- Insert: O(1)
- Delete: O(1)
- Search: O(1)

**Worst case:** O(n) — all keys hash to same bucket.

**Rehashing:** When load factor exceeds threshold (e.g., 0.75), create larger table and reinsert all entries. Amortized O(1).

**Good hash functions:** Uniform distribution, fast to compute, avalanche effect. Examples: MurmurHash, CityHash, SipHash.

**Applications:** Dictionaries, caches, sets, database indexes.

**Recall:** *Hash → bucket → collision handling.*

### Bloom Filters

**Definition:** A probabilistic data structure for set membership. Can have false positives but no false negatives.

**Structure:** Bit array of m bits, k hash functions.

**Operations:**
- **Insert(x):** Set bits at h₁(x), h₂(x), ..., h_k(x)
- **Query(x):** Check if all bits are set. If any 0 → definitely not in set. If all 1 → possibly in set.

**False positive rate:** (1 − e^(−kn/m))^k

**Trade-offs:** More bits → fewer false positives. More hash functions → lower FPR up to a point.

**Applications:** Caches (avoid disk lookups), databases (avoid SSTable lookups), network routers, cryptocurrency (SPV nodes).

**Variants:** Counting Bloom filter (supports deletion), Cuckoo filter (better FPR), Quotient filter.

**Recall:** *Maybe-contains set.*

### Union-Find (Disjoint Set Union)

**Definition:** Data structure for maintaining a partition of a set into disjoint subsets.

**Operations:**
- **makeSet(x):** Create singleton set {x}
- **find(x):** Return representative of set containing x
- **union(x, y):** Merge sets containing x and y

**Optimizations:**
- **Union by rank:** Attach smaller tree under larger tree
- **Path compression:** During find, point all nodes directly to root

**Complexity:** With both optimizations, amortized O(α(n)) ≈ O(1), where α is inverse Ackermann function.

**Applications:** Kruskal's MST, connected components, cycle detection, image processing.

**Recall:** *Find + union, α(n).*

---

## 4.3 Trees & Heaps

### Trees

**Definition:** A tree is a connected acyclic graph. A rooted tree has a designated root node.

**Terminology:**
- **Root:** Top node
- **Parent/Child:** Direct relationships
- **Leaf:** Node with no children
- **Height:** Longest path from root to leaf
- **Depth:** Distance from root
- **Subtree:** Node and all descendants

**Binary Tree:** Each node has at most 2 children.

**Traversals:**
- **Preorder:** Root → Left → Right
- **Inorder:** Left → Root → Right (gives sorted order for BST)
- **Postorder:** Left → Right → Root
- **Level-order:** BFS by level

**Applications:** File systems, DOM, expression trees, decision trees, B-trees.

**Recall:** *Hierarchy + traversal.*

### Binary Search Trees (BST)

**Definition:** A binary tree where for every node, all keys in left subtree < node key < all keys in right subtree.

**Operations:**
| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

**Worst case:** Degenerate tree (linked list) when inserting sorted keys.

**Solution:** Self-balancing trees (AVL, Red-Black).

**Deletion cases:**
1. Leaf: remove directly
2. One child: replace with child
3. Two children: replace with inorder successor (min of right subtree), then delete successor

**Recall:** *Ordered tree.*

### Heaps

**Definition:** A heap is a complete binary tree satisfying the heap property:
- **Min-heap:** Parent ≤ children
- **Max-heap:** Parent ≥ children

**Array representation:** For node at index i:
- Left child: 2i + 1
- Right child: 2i + 2
- Parent: (i − 1) / 2

**Operations:**
| Operation | Complexity |
|-----------|------------|
| Insert (push) | O(log n) |
| Extract min/max | O(log n) |
| Peek | O(1) |
| Build heap | O(n) |
| Heapify | O(log n) |

**Build heap:** Start from last non-leaf, sift down. Total cost O(n) (not O(n log n)).

**Applications:** Priority queue, heapsort, Dijkstra, scheduling, median maintenance (two heaps).

**Variants:** Binary heap, Fibonacci heap (O(1) amortized insert, decrease-key), binomial heap, pairing heap.

**Recall:** *Partial order, fast min/max.*

### Tries

**Definition:** A trie (prefix tree) is a tree where each node represents a character and paths from root represent strings.

**Properties:**
- Lookup: O(m) where m = string length
- Insert: O(m)
- Delete: O(m)
- Space: O(alphabet_size × total_chars)

**Variants:**
- **Compressed trie (radix tree):** Compress chains of single-child nodes
- **Suffix trie:** All suffixes of a string; O(m) substring search
- **Ternary search tree:** Space-efficient

**Applications:** Autocomplete, spell check, IP routing (longest prefix match), T9 predictive text.

**Recall:** *Character-by-character tree.*

---

## 4.4 Advanced Structures

### Graphs

**Definition:** G = (V, E); V = vertices, E = edges.

**Representations:**
| Representation | Space | Edge lookup | Neighbor iteration |
|----------------|-------|-------------|-------------------|
| Adjacency list | O(V + E) | O(degree) | O(degree) |
| Adjacency matrix | O(V²) | O(1) | O(V) |

**When to use:**
- Sparse graphs → adjacency list
- Dense graphs → adjacency matrix
- Frequent edge lookup → matrix
- Frequent neighbor iteration → list

**Types:** Directed/undirected, weighted/unweighted, bipartite, DAG, planar.

**Recall:** *Vertices + edges; list vs matrix.*

### Segment Trees

**Definition:** A segment tree is a binary tree for range queries and range updates on an array.

**Structure:** Each node represents an interval [l, r]; leaves are single elements; internal nodes store aggregate (sum, min, max).

**Operations:**
- Build: O(n)
- Range query: O(log n)
- Point update: O(log n)
- Range update: O(log n) with lazy propagation

**Lazy propagation:** Defer updates to children until needed.

**Applications:** Range sum/min/max queries, interval scheduling, computational geometry.

**Recall:** *Range queries fast.*

### Fenwick Trees (Binary Indexed Tree)

**Definition:** A Fenwick tree supports prefix sum queries and point updates in O(log n).

**Structure:** Array where each index i stores sum of range (i − lowbit(i), i], where lowbit(i) = i & (−i).

**Operations:**
- Update: Add to indices i, i + lowbit(i), ... — O(log n)
- Query: Sum indices i, i − lowbit(i), ... — O(log n)

**Advantages:** Simpler, less memory than segment tree.
**Limitations:** Only for invertible operations (sum, xor); not min/max without modification.

**Applications:** Prefix sums, counting inversions, 2D BIT.

**Recall:** *BIT for prefix sums.*

### Balanced Trees

**AVL Tree:** Height-balanced BST. Balance factor = height(left) − height(right) ∈ {−1, 0, 1}. Rotations maintain balance.

**Red-Black Tree:** BST with color invariants: root black, red nodes have black children, equal black-height on all paths. Used in Java TreeMap, C++ std::map.

**Splay Tree:** Self-adjusting; recently accessed nodes move to root. Amortized O(log n).

**Treap:** BST + heap property on random priorities. Expected O(log n).

**B-Tree:** Multi-way balanced tree; each node has many keys. Used in databases and file systems.

**Why balanced trees matter:** Guarantee O(log n) operations even in worst case.

**Recall:** *Rotations keep height log n.*

### Skip Lists

**Definition:** A probabilistic data structure with multiple levels of linked lists.

**Structure:** Bottom level is sorted linked list; each higher level is a subset of the level below.

**Operations:**
- Search: O(log n) expected
- Insert: O(log n) expected
- Delete: O(log n) expected

**Advantages:** Simpler than balanced trees, good concurrency.
**Used in:** Redis sorted sets, LevelDB memtable.

**Recall:** *Randomized layered list.*

### Persistent Data Structures

**Definition:** Data structures that preserve previous versions after modification.

**Approaches:**
- **Path copying:** Copy nodes along path from root to modified node
- **Fat nodes:** Store version history in each node
- **Functional:** Immutable; new version shares unchanged parts

**Applications:** Functional programming, version control (git), databases (MVCC), computational geometry.

**Recall:** *Immutable + versioned.*

### Suffix Arrays/Trees

**Suffix Array:** Sorted array of all suffixes of a string. Supports pattern matching in O(m log n).

**Suffix Tree:** Compressed trie of all suffixes. O(m) pattern matching, but O(n) space.

**Applications:** String matching, bioinformatics (genome alignment), data compression.

### External Memory Structures

**B-Tree:** Designed for disk; each node fits in a disk block. Minimizes disk I/O.

**LSM Tree:** Log-structured merge tree; write-optimized. Used in Cassandra, RocksDB, LevelDB.

**Bε-tree:** Write-optimized B-tree; buffers updates in internal nodes.

**Why external memory matters:** Disk access is ~100,000× slower than memory; structure must minimize I/O.

### Concurrent Data Structures

**Lock-free:** Uses atomic operations (CAS) instead of locks. Progress guaranteed.
**Wait-free:** Every thread makes progress in bounded steps.
**Examples:** Concurrent hash map, lock-free queue (Michael-Scott), concurrent skip list.

**Challenges:** ABA problem, memory reclamation, false sharing.

**SOTA:** Succinct data structures, GPU data structures, RDMA-based structures, learned indexes.

---
