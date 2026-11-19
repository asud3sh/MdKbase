# 3 · THEORY OF COMPUTATION

## 3.1 Automata & Languages

### Formal Languages

**Definition:** An alphabet Σ is a finite set of symbols. A string is a finite sequence of symbols. A language L ⊆ Σ* is a set of strings over Σ.

**Operations:**
- **Union:** L₁ ∪ L₂
- **Concatenation:** L₁L₂ = {xy | x ∈ L₁, y ∈ L₂}
- **Kleene star:** L* = {ε} ∪ L ∪ LL ∪ LLL ∪ ...
- **Complement:** Σ* − L

**Why it matters:** Formal languages are the foundation of compilers, text processing, and protocol specification.

**Recall:** *Language = set of strings.*

### Finite Automata

**DFA (Deterministic Finite Automaton):** A 5-tuple (Q, Σ, δ, q₀, F) where:
- Q = finite set of states
- Σ = alphabet
- δ: Q × Σ → Q = transition function
- q₀ ∈ Q = start state
- F ⊆ Q = accept states

**NFA (Nondeterministic Finite Automaton):** Like DFA but δ: Q × Σ → P(Q); can have ε-transitions.

**Equivalence:** NFA → DFA via subset construction. Every NFA has an equivalent DFA (possibly exponentially larger).

**Regular languages:** Languages accepted by DFAs = NFAs = regular expressions.

**Example:** DFA for strings over {0,1} ending in 1.

**Recall:** *DFA = NFA = regex.*

### Regular Expressions

**Syntax:**
- ∅ (empty set)
- ε (empty string)
- a (single symbol)
- R₁ ∪ R₂ (union)
- R₁R₂ (concatenation)
- R* (Kleene star)

**Kleene's theorem:** Regular expressions and finite automata describe the same class of languages.

**Pumping lemma:** If L is regular, there exists p such that any string s ∈ L with |s| ≥ p can be split s = xyz where:
1. |y| > 0
2. |xy| ≤ p
3. xyⁱz ∈ L for all i ≥ 0

**Use:** Prove a language is not regular by contradiction.

**Example:** L = {0ⁿ1ⁿ | n ≥ 0} is not regular.

**Recall:** *Regex = FA; pumping = proof tool.*

### Context-Free Grammars

**Definition:** A CFG is a 4-tuple (V, Σ, R, S) where:
- V = variables (nonterminals)
- Σ = terminals
- R = production rules
- S = start variable

**Derivation:** Sequence of rule applications from S to a string of terminals.

**Parse tree:** Tree representation of derivation.

**Ambiguity:** A grammar is ambiguous if some string has multiple parse trees.

**Chomsky Normal Form:** All rules are of form A → BC or A → a.

**Example:** Grammar for balanced parentheses: S → (S) | SS | ε.

**Recall:** *CFG → parse tree → PDA.*

### Pushdown Automata

**Definition:** A PDA is an NFA with a stack. It can push and pop symbols.

**Acceptance:** By final state or by empty stack.

**Equivalence:** CFG ↔ PDA. Every CFG has an equivalent PDA and vice versa.

**Why it matters:** PDAs model parsers, nested structures, and recursive protocols.

**Recall:** *Stack = one memory cell of infinite depth.*

---

## 3.2 Computability

### Turing Machines

**Definition:** A TM is a 7-tuple (Q, Σ, Γ, δ, q₀, q_accept, q_reject) where:
- Q = states
- Σ = input alphabet
- Γ = tape alphabet (Σ ⊆ Γ)
- δ: Q × Γ → Q × Γ × {L, R} = transition function
- q₀ = start state
- q_accept, q_reject = halting states

**Operation:** Read symbol, write symbol, move left/right, change state.

**Deterministic vs nondeterministic:** DTM has one transition per (state, symbol); NTM has multiple.

**Universal TM:** A TM that can simulate any other TM given its description. Programmable computer.

**Recall:** *TM = infinite tape + finite control.*

### Church-Turing Thesis

**Statement:** All "effective computations" can be computed by a Turing machine.

**Equivalences:**
- Lambda calculus ≡ TM
- Recursive functions ≡ TM
- Cellular automata ≡ TM
- Quantum computers (probably) ≡ TM (for computability, not efficiency)

**Why it matters:** Defines the limits of computation. If a problem is undecidable for TMs, it's undecidable for any reasonable model.

**Recall:** *Any computation = TM.*

### Decidability

**Decidable (recursive):** A language L is decidable if there exists a TM that halts on all inputs and accepts exactly L.

**Recognizable (recursively enumerable, RE):** A language L is recognizable if there exists a TM that accepts all strings in L and either rejects or loops on strings not in L.

**Co-recognizable:** L is co-recognizable if its complement is recognizable.

**Relationships:**
- Decidable ⊆ RE
- Decidable ⊆ co-RE
- RE ∩ co-RE = Decidable

**Recall:** *Decide = always halt; recognize = accept if yes.*

### Halting Problem

**Statement:** Given a TM M and input w, does M halt on w?

**Proof (Turing 1936):** Assume H(M, w) decides halting. Construct D(M) that loops if H(M, M) accepts, halts if H(M, M) rejects. Then D(D) contradicts. Therefore H cannot exist.

**Implications:**
- Many problems are undecidable
- Rice's theorem: Any non-trivial semantic property of programs is undecidable

**Recall:** *No TM decides if TM halts.*

### Reductions

**Many-one reduction (≤m):** A ≤m B if there exists a computable function f such that x ∈ A ⇔ f(x) ∈ B.

**Turing reduction (≤T):** A ≤T B if A can be decided using an oracle for B.

**Use:** To prove a problem B is undecidable, reduce a known undecidable problem A to B. If A ≤ B and A is undecidable, then B is undecidable.

**Example:** Reduction from halting problem to Post's correspondence problem.

**Recall:** *If A ≤ B and A hard → B hard.*

### Computability

**Recursive:** Decidable.

**RE:** Recognizable.

**co-RE:** Complement is RE.

**Post's correspondence problem:** Given pairs of strings, is there a sequence of pairs where concatenations match? Undecidable.

**Busy beaver:** Maximum number of steps a TM with n states can run before halting. Incomputable but grows faster than any computable function.

**Recall:** *Not everything is computable.*

---

## 3.3 Complexity

### Complexity Classes

**TIME(f(n)):** Languages decidable in time O(f(n)).

**SPACE(f(n)):** Languages decidable in space O(f(n)).

**P:** Polynomial time. Solvable in time O(n^k).

**NP:** Nondeterministic polynomial time. Verifiable in polynomial time.

**co-NP:** Complements of NP languages.

**BPP:** Bounded-error probabilistic polynomial time.

**PSPACE:** Polynomial space.

**EXP:** Exponential time.

**Relationships:**
- P ⊆ NP ⊆ PSPACE ⊆ EXP
- P ⊆ BPP ⊆ PSPACE
- NP ⊆ co-NP? Open.

**Recall:** *P ⊆ NP ⊆ PSPACE ⊆ EXP.*

### P vs NP

**P:** Problems solvable in polynomial time.

**NP:** Problems verifiable in polynomial time.

**Question:** Does P = NP?

**Significance:** If P = NP, many hard problems become easy (cryptography breaks). If P ≠ NP, some problems are inherently hard.

**Millennium Prize:** $1M for resolution.

**Recall:** *Solve vs verify.*

### NP-Completeness

**NP-hard:** A problem is NP-hard if every NP problem reduces to it.

**NP-complete:** NP-hard and in NP.

**Cook-Levin theorem:** SAT is NP-complete.

**Classic NP-complete problems:**
- 3-SAT
- Clique
- Vertex Cover
- Hamiltonian Cycle
- Traveling Salesman (decision)
- Subset Sum

**Proof technique:** Reduce from known NP-complete problem.

**Recall:** *SAT is the mother of NP-complete.*

### Approximation Algorithms

**PTAS:** Polynomial-time approximation scheme. For any ε > 0, (1+ε)-approximation in polynomial time.

**FPTAS:** Fully polynomial-time approximation scheme. Polynomial in both n and 1/ε.

**APX:** Class of problems with constant-factor approximation.

**Techniques:**
- Greedy
- LP rounding
- Primal-dual

**Inapproximability:** PCP theorem implies some problems cannot be approximated within certain factors unless P = NP.

**Recall:** *When exact is hard, approximate.*

### Randomized Algorithms

**Las Vegas:** Always correct, but running time is random. Example: randomized quicksort.

**Monte Carlo:** Running time is deterministic, but may be wrong with small probability. Example: Miller-Rabin primality test.

**Examples:**
- Randomized quicksort: O(n log n) expected
- Miller-Rabin: O(k log³ n) for k rounds
- Karger's min-cut: O(n²) expected

**Recall:** *Flip coins for speed.*

### Big-O Notation

**Definitions:**
- O(g): f(n) ≤ c g(n) for n ≥ n₀
- Ω(g): f(n) ≥ c g(n) for n ≥ n₀
- Θ(g): f(n) = O(g) and f(n) = Ω(g)
- o(g): f(n)/g(n) → 0
- ω(g): f(n)/g(n) → ∞

**Common orders:**
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)

**Recall:** *Growth, not exact cost.*

### Amortized Analysis

**Aggregate method:** Total cost of n operations / n.

**Accounting method:** Assign amortized costs to operations; credit for future expensive operations.

**Potential method:** Define potential function Φ; amortized cost = actual cost + ΔΦ.

**Example:** Dynamic array push. Doubling strategy gives O(1) amortized push.

**Recall:** *Average over worst-case sequence.*

---

## 3.4 Advanced Complexity

### `#P`

**Definition:** Class of counting problems. `#SAT` counts satisfying assignments.

**Permanent:** Computing permanent of a matrix is `#P-complete`.

**Why it matters:** Counting is often harder than deciding.

### PSPACE-Completeness

**QBF (Quantified Boolean Formula):** PSPACE-complete.

**Generalized games:** Many are PSPACE-complete.

**Why it matters:** PSPACE captures problems with polynomial space, including many games and planning problems.

### Interactive Proofs

**IP:** Class of languages with interactive proof systems.

**IP = PSPACE:** Shamir's theorem.

**Arthur-Merlin games:** Public-coin interactive proofs.

**Why it matters:** Foundations of zero-knowledge proofs and cryptography.

### PCP Theorem

**Statement:** Every NP language has a probabilistically checkable proof that can be verified by reading O(1) bits.

**Implications:** Inapproximability results. Example: Max-3SAT cannot be approximated within 7/8 + ε unless P = NP.

**Why it matters:** Connects complexity, approximation, and coding theory.

### Quantum Complexity

**BQP:** Bounded-error quantum polynomial time. Problems solvable efficiently by quantum computers.

**QMA:** Quantum Merlin-Arthur. Quantum analogue of NP.

**Quantum advantage:** Problems where quantum computers outperform classical.

**Why it matters:** Cryptography (Shor breaks RSA), simulation (quantum chemistry).

### Parameterized Complexity

**FPT:** Fixed-parameter tractable. Time `O(f(k) · n^c)` where k is parameter.

**W-hierarchy:** Classes of parameterized problems.

**Kernelization:** Reduce problem to smaller instance.

**Why it matters:** NP-hard problems may be tractable for small parameters.

### Fine-Grained Complexity

**SETH (Strong Exponential Time Hypothesis):** SAT requires `2^(1−ε)n` time.

**3SUM conjecture:** 3SUM requires n² time.

**Conditional lower bounds:** Prove problems are hard assuming SETH or 3SUM.

**Why it matters:** Explains why we can't improve certain algorithms.

### Meta-Complexity

**MCSP (Minimum Circuit Size Problem):** Given truth table, find smallest circuit.

**Kt complexity:** Time-bounded Kolmogorov complexity.

**Why it matters:** Connects complexity, cryptography, and learning theory.

**SOTA:** AI-discovered proofs in theoretical CS (non-sofic groups, circuit complexity lower bounds), exponential parallel repetition theorem for entangled games.

---
