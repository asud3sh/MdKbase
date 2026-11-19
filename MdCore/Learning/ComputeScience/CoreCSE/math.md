# 2. MATHEMATICAL FOUNDATIONS

## 2.1 Logic & Proof

### Propositional Logic

**Definition:** Propositional logic deals with propositions (statements that are either true or false) and logical connectives.

**Connectives:**
| Symbol | Name | Meaning |
|--------|------|---------|
| ¬ | NOT | Negation |
| ∧ | AND | Conjunction |
| ∨ | OR | Disjunction |
| → | IMPLIES | Implication |
| ↔ | IFF | Biconditional |

**Truth tables:** For each connective, list all possible input combinations and the output.

**Example:** `P → Q` is false only when P is true and Q is false.

**Laws:**
- **De Morgan:** ¬(P ∧ Q) = ¬P ∨ ¬Q; ¬(P ∨ Q) = ¬P ∧ ¬Q
- **Distributivity:** P ∧ (Q ∨ R) = (P ∧ Q) ∨ (P ∧ R)
- **Contrapositive:** P → Q ≡ ¬Q → ¬P
- **Modus ponens:** P, P → Q ⊢ Q
- **Modus tollens:** ¬Q, P → Q ⊢ ¬P

**Why it matters:** Propositional logic is the foundation of digital circuits, type systems, and AI reasoning.

### Predicate Logic

**Definition:** Predicate logic extends propositional logic with predicates (properties of objects) and quantifiers.

**Quantifiers:**
- **∀ (universal):** ∀x P(x) — P holds for all x
- **∃ (existential):** ∃x P(x) — P holds for at least one x

**Example:** ∀x (Human(x) → Mortal(x)) — all humans are mortal.

**Why it matters:** Predicate logic is the foundation of formal specification, database queries, and theorem proving.

### Proof Techniques

| Technique | Method | When to Use |
|-----------|--------|-------------|
| **Direct** | Assume P, derive Q | Straightforward implications |
| **Contradiction** | Assume ¬P, derive ⊥ | When direct proof is hard |
| **Contrapositive** | Prove ¬Q → ¬P | Equivalent to P → Q |
| **Induction** | Base case + inductive step | Statements about natural numbers |
| **Strong Induction** | Assume all smaller cases | When multiple previous cases needed |
| **Structural Induction** | Induct on structure | Recursive data types |
| **Construction** | Exhibit a witness | Existence proofs |
| **Diagonalization** | Construct a counterexample | Uncountability, undecidability |

**Induction example:** Prove Σ_{i=1}^{n} i = n(n+1)/2.
- Base: n=1: 1 = 1(2)/2 = 1. ✓
- Step: Assume true for n. For n+1: Σ_{i=1}^{n+1} i = n(n+1)/2 + (n+1) = (n+1)(n+2)/2. ✓

**Diagonalization example:** Cantor's proof that real numbers are uncountable. Assume a list of all reals; construct a number differing from each listed number at one digit; contradiction.

**Recall:** *Direct, contrapositive, contradiction, induction, construction, diagonal.*

---

## 2.2 Discrete Structures

### Sets & Relations

**Sets:** Collections of distinct objects. Operations: ∪ (union), ∩ (intersection), ∖ (difference), × (Cartesian product), P(S) (powerset).

**Relations:** A relation R on set A is a subset of A × A. Properties:
- **Reflexive:** ∀a (a,a) ∈ R
- **Symmetric:** (a,b) ∈ R → (b,a) ∈ R
- **Transitive:** (a,b) ∈ R ∧ (b,c) ∈ R → (a,c) ∈ R
- **Equivalence:** reflexive + symmetric + transitive
- **Partial order:** reflexive + antisymmetric + transitive

**Functions:** A function f: A → B maps each element of A to exactly one element of B.
- **Injective (one-to-one):** f(a₁) = f(a₂) → a₁ = a₂
- **Surjective (onto):** ∀b ∈ B ∃a ∈ A f(a) = b
- **Bijective:** injective + surjective

**Recall:** *Equivalence = RST; Partial order = RAT.*

### Discrete Math

**Counting:**
- **Permutations:** P(n,k) = n!/(n-k)! — ordered arrangements
- **Combinations:** C(n,k) = n!/(k!(n-k)!) — unordered selections
- **Pigeonhole:** If n items in m containers and n > m, at least one container has ≥2 items
- **Inclusion–Exclusion:** |A ∪ B| = |A| + |B| − |A ∩ B|

**Recurrences:**
- **Master theorem:** Solves T(n) = aT(n/b) + f(n)
- **Generating functions:** Encode sequences as coefficients of power series

**Modular arithmetic:** Arithmetic modulo n. `a ≡ b (mod n)` means n divides (a−b).

**Chinese Remainder Theorem (CRT):** Solves systems of congruences with coprime moduli.

**Recall:** *Count, recurse, mod.*

### Number Theory

**Divisibility:** a | b means b = ak for some integer k.

**Primes:** Numbers with exactly two divisors: 1 and themselves.

**GCD (Euclid's algorithm):** gcd(a,b) = gcd(b, a mod b). Efficient: O(log min(a,b)).

**Modular inverse:** a⁻¹ mod n exists iff gcd(a,n) = 1.

**Fermat's Little Theorem:** If p is prime and p ∤ a, then a^(p−1) ≡ 1 (mod p).

**Euler's Theorem:** If gcd(a,n) = 1, then a^φ(n) ≡ 1 (mod n), where φ is Euler's totient.

**Applications:** RSA encryption, hashing, pseudorandom generation.

**Recall:** *GCD → modular arithmetic → crypto.*

### Graph Theory

**Definition:** A graph G = (V, E) consists of vertices V and edges E.

**Types:**
- **Directed:** edges have direction
- **Undirected:** edges are symmetric
- **Weighted:** edges have weights
- **Bipartite:** vertices can be 2-colored
- **DAG:** directed acyclic graph

**Traversal:**
- **BFS:** level-by-level; uses queue; O(V+E)
- **DFS:** depth-first; uses stack; O(V+E)

**Connectivity:** A graph is connected if there is a path between any two vertices.

**Trees:** Connected acyclic graphs. n vertices → n−1 edges.

**Planarity:** A graph is planar if it can be drawn without edge crossings.

**Coloring:** Assign colors to vertices so no adjacent vertices share a color.

**Matching:** Set of edges with no shared vertices.

**Flows:** Max flow = min cut (Ford-Fulkerson).

**Recall:** *Vertices + edges + traversal.*

### Boolean Algebra

**Operations:** AND, OR, NOT, NAND, NOR, XOR.

**Laws:**
- Identity: A ∧ 1 = A; A ∨ 0 = A
- Idempotent: A ∧ A = A; A ∨ A = A
- Distributive: A ∧ (B ∨ C) = (A ∧ B) ∨ (A ∧ C)
- Absorption: A ∧ (A ∨ B) = A

**Canonical forms:**
- **SOP (Sum of Products):** OR of ANDs
- **POS (Product of Sums):** AND of ORs

**Karnaugh maps:** Visual method for simplifying Boolean expressions.

**Recall:** *Logic → gates → circuits.*

---

## 2.3 Continuous & Applied Math

### Probability

**Axioms:**
1. P(A) ≥ 0
2. P(Ω) = 1
3. P(A ∪ B) = P(A) + P(B) if A ∩ B = ∅

**Conditional probability:** P(A|B) = P(A ∩ B) / P(B)

**Bayes' theorem:** P(A|B) = P(B|A) P(A) / P(B)

**Independence:** P(A ∩ B) = P(A) P(B)

**Random variables:** Functions from outcomes to real numbers.
- **Expectation:** E[X] = Σ x P(X=x)
- **Variance:** Var(X) = E[(X − E[X])²]

**Distributions:**
| Distribution | Use |
|--------------|-----|
| Bernoulli | Single yes/no trial |
| Binomial | Number of successes in n trials |
| Poisson | Rare events over time |
| Normal | Central limit theorem |
| Exponential | Time between events |

**Concentration inequalities:**
- **Markov:** P(X ≥ a) ≤ E[X]/a
- **Chebyshev:** P(|X − E[X]| ≥ a) ≤ Var(X)/a²
- **Chernoff:** Exponential bounds for sums of independent variables

**Recall:** *Bayes + expectation + concentration.*

### Statistics

**Descriptive vs inferential:** Descriptive summarizes data; inferential draws conclusions.

**Estimation:** Point estimates (mean, median) and interval estimates (confidence intervals).

**Hypothesis testing:** Null hypothesis H₀, alternative H₁; p-value; significance level α.

**Regression:** Linear regression fits y = β₀ + β₁x + ε.

**Correlation ≠ causation:** Two variables may be correlated without one causing the other (confounding variables).

**Recall:** *Estimate → test → infer.*

### Linear Algebra

**Vectors:** Ordered lists of numbers. Operations: addition, scalar multiplication, dot product.

**Matrices:** 2D arrays. Operations: addition, multiplication, transpose, inverse.

**Linear independence:** Vectors are independent if no vector is a linear combination of the others.

**Basis:** A set of independent vectors that span the space.

**Rank:** Dimension of the column space.

**Eigenvalues/vectors:** Av = λv; λ is eigenvalue, v is eigenvector.

**SVD:** A = UΣVᵀ; decomposes any matrix into rotation, scaling, rotation.

**PCA:** Principal Component Analysis; dimensionality reduction using SVD.

**Applications:** Graphics, ML, quantum mechanics.

**Recall:** *Ax = b → eigen → SVD.*

### Calculus

**Limits:** lim_{x→a} f(x) = L means f approaches L as x approaches a.

**Derivatives:** f'(x) = lim_{h→0} (f(x+h) − f(x))/h; rate of change.

**Integrals:** ∫ f(x) dx; area under curve; inverse of derivative.

**Multivariable:** Partial derivatives, gradients (∇f), Jacobians, Hessians.

**Optimization:** Find minima/maxima.
- **Gradient descent:** x ← x − η∇f(x)
- **Lagrange multipliers:** Optimize subject to constraints.

**Recall:** *Slope → area → gradient.*

### Optimization

**Convex vs non-convex:** Convex functions have a single global minimum; non-convex may have many local minima.

**Algorithms:**
- **Gradient descent:** First-order; simple, slow
- **SGD:** Stochastic gradient descent; mini-batches
- **Newton's method:** Second-order; uses Hessian; fast but expensive
- **Momentum:** Accumulates velocity; faster convergence
- **Adam:** Adaptive learning rates; combines momentum and RMSProp

**Linear programming (LP):** Minimize cᵀx subject to Ax ≤ b, x ≥ 0.

**Quadratic programming (QP):** Minimize ½xᵀQx + cᵀx subject to constraints.

**Duality:** Every LP has a dual; strong duality holds for convex problems.

**KKT conditions:** Necessary conditions for optimality in constrained optimization.

**Recall:** *Minimize f(x); convex = safe.*

### Information Theory

**Shannon entropy:** H(X) = −Σ p(x) log p(x); measures uncertainty.

**Mutual information:** I(X;Y) = H(X) − H(X|Y); measures dependence.

**KL divergence:** D_KL(P||Q) = Σ p(x) log(p(x)/q(x)); measures difference between distributions.

**Source coding:** Compress data to entropy limit.

**Channel coding:** Transmit reliably over noisy channel; capacity = max mutual information.

**Kolmogorov complexity:** Length of shortest program that produces a string; incomputable but theoretically important.

**Recall:** *Entropy = uncertainty; capacity = limit.*

### Queueing Theory

**M/M/1:** Single server, Poisson arrivals, exponential service.
- Utilization ρ = λ/μ (λ = arrival rate, μ = service rate)
- Average queue length L = ρ/(1−ρ)
- Average wait time W = L/λ

**M/M/c:** Multiple servers.

**Little's law:** L = λW (average number in system = arrival rate × average time in system).

**Applications:** Systems design, capacity planning.

### Control Theory

**Feedback:** Output is fed back to adjust input.

**PID controller:** Proportional + Integral + Derivative; classic control.

**State-space:** ẋ = Ax + Bu; y = Cx + Du.

**Stability:** System is stable if output remains bounded for bounded input.

**Applications:** Robotics, aerospace, process control.

### Stochastic Processes

**Markov chains:** State transitions depend only on current state (memoryless). Stationary distribution π satisfies πP = π.

**Martingales:** Sequences where E[X_{n+1} | X_1,...,X_n] = X_n; fair games.

**Brownian motion:** Continuous-time random walk; foundation of stochastic calculus.

**Applications:** RL, finance, queueing.

### Measure Theory

**Sigma-algebras:** Collections of sets closed under complement and countable union.

**Lebesgue integral:** Generalizes Riemann integral; integrates more functions.

**Why it matters:** Rigorous foundation for probability theory and advanced ML.

### Category Theory

**Objects and morphisms:** Abstract structures and structure-preserving maps.

**Functors:** Maps between categories.

**Natural transformations:** Maps between functors.

**Why it matters:** Functional programming (monads, functors), type theory, semantics.

### Convex Analysis

**Subgradients:** Generalize gradients for non-differentiable convex functions.

**Duality:** Lagrangian dual; strong duality for convex problems.

**KKT conditions:** Generalize Lagrange multipliers to inequality constraints.

**Why it matters:** Optimization theory, ML (SVM, regularization).

### Numerical Linear Algebra

**Conditioning:** Condition number κ(A) = ||A|| ||A⁻¹||; measures sensitivity to perturbations.

**Stability:** Algorithms should be numerically stable; small errors shouldn't grow.

**Matrix decompositions:** LU, QR, Cholesky, SVD.

**Why it matters:** Scientific computing, ML, graphics.

### Combinatorial Optimization

**Matroids:** Abstract structures generalizing linear independence; greedy works.

**Polyhedral combinatorics:** Study of polytopes defined by combinatorial constraints.

**Why it matters:** Scheduling, network design, approximation algorithms.

**SOTA:** Randomized numerical linear algebra, sketching, tensor methods, differential privacy math.

---

