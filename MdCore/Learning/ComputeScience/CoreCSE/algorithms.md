
# 5 · ALGORITHMS

## 5.1 Paradigms

### Design Paradigms Overview

| Paradigm | Strategy | Example |
|----------|----------|---------|
| Brute force | Try all possibilities | Password cracking |
| Divide & conquer | Split, solve, combine | Merge sort |
| Greedy | Local optimal | Huffman coding |
| Dynamic programming | Cache subproblems | Knapsack |
| Backtracking | Explore + prune | N-Queens |
| Branch & bound | Prune with bounds | TSP |
| Randomized | Use randomness | Quicksort |
| Approximation | Near-optimal | TSP 2-approx |

**Recall:** *7 paradigms cover 95%.*

### Recursion

**Definition:** A function that calls itself with smaller inputs.

**Components:**
- **Base case:** Terminates recursion
- **Recursive case:** Reduces problem size

**Recursion tree:** Visualizes recursive calls; total cost = sum over levels.

**Tail recursion:** Recursive call is last operation; can be optimized to iteration.

**Memoization:** Cache results of subproblems; converts exponential to polynomial.

**Example:** Fibonacci.
- Naive: O(2ⁿ)
- Memoized: O(n)
- Bottom-up DP: O(n)

**Recall:** *Trust the recursive call.*

### Divide & Conquer

**Strategy:** Divide problem into subproblems, solve recursively, combine results.

**Master Theorem:** For T(n) = aT(n/b) + f(n):
- If f(n) = O(n^(log_b a − ε)) → T(n) = Θ(n^(log_b a))
- If f(n) = Θ(n^(log_b a)) → T(n) = Θ(n^(log_b a) log n)
- If f(n) = Ω(n^(log_b a + ε)) and regularity → T(n) = Θ(f(n))

**Examples:**
- Merge sort: T(n) = 2T(n/2) + O(n) → O(n log n)
- Binary search: T(n) = T(n/2) + O(1) → O(log n)
- Strassen: T(n) = 7T(n/2) + O(n²) → O(n^2.807)
- FFT: T(n) = 2T(n/2) + O(n) → O(n log n)

**Recall:** *Divide + combine; Master theorem solves.*

### Greedy

**Strategy:** Make locally optimal choice at each step; hope for global optimum.

**When it works:** Exchange argument or matroid structure.

**Examples:**
- Activity selection: earliest finish time
- Huffman coding: merge two smallest frequencies
- Kruskal's MST: add smallest edge that doesn't create cycle
- Prim's MST: add smallest edge connecting tree to non-tree
- Dijkstra: extract min-distance vertex

**Exchange argument:** Show that any optimal solution can be transformed to include the greedy choice without worsening.

**Recall:** *Greedy works when exchange argument holds.*

### Dynamic Programming

**Definition:** Solve problems by combining solutions to overlapping subproblems.

**Requirements:**
1. **Optimal substructure:** Optimal solution contains optimal solutions to subproblems
2. **Overlapping subproblems:** Same subproblems recur

**Approaches:**
- **Top-down (memoization):** Recursive + cache
- **Bottom-up (tabulation):** Iterative; fill table

**Classic problems:**
| Problem | Recurrence | Complexity |
|---------|------------|------------|
| Fibonacci | F(n) = F(n−1) + F(n−2) | O(n) |
| LCS | LCS(i,j) = ... | O(mn) |
| Edit distance | ED(i,j) = ... | O(mn) |
| Knapsack | K(i,w) = ... | O(nW) |
| Matrix chain | M(i,j) = ... | O(n³) |
| Floyd-Warshall | D(i,j) = ... | O(n³) |

**Recall:** *DP = recursion + memoization.*

### Backtracking

**Strategy:** Explore candidates incrementally; abandon (backtrack) when constraint violated.

**Template:**
```
def backtrack(state):
    if solution(state): record(state); return
    for choice in choices(state):
        if valid(choice):
            apply(choice)
            backtrack(new_state)
            undo(choice)
```

**Examples:** N-Queens, sudoku, subset-sum, graph coloring, Hamiltonian path.

**Pruning:** Reduce search space with bounds or constraints.

**Recall:** *Try, fail, undo.*

### Online Algorithms

**Definition:** Algorithm must make decisions without knowing future input.

**Competitive ratio:** Ratio of online cost to optimal offline cost.

**Examples:**
- Ski rental: 2-competitive
- Paging (LRU): k-competitive
- K-server: k-competitive for some cases

**Recall:** *Decide without future knowledge.*

---

## 5.2 Classic Algorithms

### Sorting

**Comparison sorts:**
| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | No |

**Non-comparison sorts:**
| Algorithm | Complexity | Constraint |
|-----------|------------|------------|
| Counting | O(n + k) | Integer keys in [0, k] |
| Radix | O(d(n + k)) | d digits, base k |
| Bucket | O(n + k) average | Uniform distribution |

**Lower bound:** Ω(n log n) for comparison sorts (decision tree argument).

**Recall:** *Merge = stable O(n log n); Quick = fast average; Radix = O(n) non-compare.*

### Searching

**Linear search:** O(n); works on unsorted.

**Binary search:** O(log n); requires sorted.

**Interpolation search:** O(log log n) average for uniform data.

**Exponential search:** O(log n) for unbounded arrays.

**Ternary search:** For unimodal functions.

**Recall:** *Sorted → binary search.*

### Graph Algorithms

**BFS:** Level-by-level; O(V+E); finds shortest path in unweighted graph.

**DFS:** Depth-first; O(V+E); used for topological sort, SCC, cycle detection.

**Topological sort:** Linear ordering of DAG vertices; used for scheduling, build systems.

**SCC (Tarjan/Kosaraju):** Find strongly connected components; O(V+E).

**MST (Kruskal/Prim):** Minimum spanning tree; O(E log E) or O(E + V log V).

**Shortest paths:**
- Dijkstra: O(E + V log V) with heap; non-negative weights
- Bellman-Ford: O(VE); handles negative weights
- Floyd-Warshall: O(V³); all-pairs
- A*: Heuristic-guided; optimal with admissible heuristic

**Max flow:**
- Ford-Fulkerson: O(E × max_flow)
- Edmonds-Karp: O(VE²)
- Dinic: O(V²E)

**Matching:**
- Hungarian: O(n³) for assignment
- Hopcroft-Karp: O(E√V) for bipartite

**Recall:** *Traverse → sort → shortest → flow.*

### String Algorithms

**KMP:** Pattern matching in O(n+m); failure function.

**Rabin-Karp:** Rolling hash; O(n+m) average.

**Z-algorithm:** Z-array; pattern matching.

**Suffix arrays/trees:** All substrings; O(m) pattern matching.

**Aho-Corasick:** Multiple pattern matching; O(n + m + z).

**Edit distance:** Levenshtein; DP; O(mn).

**LCS:** Longest common subsequence; DP; O(mn).

**Recall:** *KMP = failure function; suffix tree = all substrings.*

### Computational Geometry

**Convex hull:** Graham scan O(n log n), Jarvis march O(nh).

**Sweep line:** Process events in order; used for intersection, closest pair.

**Closest pair:** Divide & conquer; O(n log n).

**Voronoi diagram:** Partition plane by nearest site.

**Delaunay triangulation:** Dual of Voronoi; maximizes minimum angle.

**Recall:** *Sweep + hull.*

---

## 5.3 Analysis

### Complexity Analysis

**Time complexity:** Number of operations as function of input size.

**Space complexity:** Memory used as function of input size.

**Worst/average/best case:** Different input distributions.

**Recurrences:** T(n) = aT(n/b) + f(n); solve with Master theorem.

**Akra-Bazzi:** Generalization for unequal splits.

**Amortized analysis:** Average over sequence of operations.

**Recall:** *Count operations, solve recurrence.*

### Streaming Algorithms

**Definition:** Process data in one pass with limited memory.

**Sketches:**
- **Count-Min Sketch:** Frequency estimation
- **HyperLogLog:** Cardinality estimation
- **Bloom filter:** Set membership
- **Reservoir sampling:** Uniform sample from stream

**Applications:** Network monitoring, database query optimization, ML feature engineering.

### Sublinear Algorithms

**Property testing:** Test if object has property with few queries.

**Sublinear time:** Approximate answers without reading all input.

**Examples:** Test if graph is bipartite, if list is sorted.

**SOTA:** Quantum algorithms, learned algorithms, LLM-assisted algorithm discovery.

---
