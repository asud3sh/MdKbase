
# 24 · GAME THEORY & OPERATIONS RESEARCH

## 24.1 Game Theory

### Game Theory Fundamentals

**Definition:** Study of strategic interaction among rational agents.

**Components:**
- **Players:** Decision-makers
- **Strategies:** Actions available
- **Payoffs:** Outcomes for each strategy profile
- **Information:** What players know

**Types:**
- **Cooperative vs non-cooperative:** Binding agreements or not
- **Zero-sum vs non-zero-sum:** Total payoff constant or not
- **Simultaneous vs sequential:** Move timing
- **Perfect vs imperfect information:** Observe moves or not

**Recall:** *Nash equilibrium, mechanism design, auction theory.*

### Nash Equilibrium

**Definition:** Strategy profile where no player can improve by unilateral deviation.

**Formally:** For all players i and strategies s'_i: u_i(s_i, s_{−i}) ≥ u_i(s'_i, s_{−i}).

**Existence:** Every finite game has at least one mixed-strategy Nash equilibrium (Nash 1950).

**Examples:**
- **Prisoner's Dilemma:** (Defect, Defect) is Nash, but (Cooperate, Cooperate) is better
- **Battle of Sexes:** Two equilibria
- **Matching Pennies:** Mixed equilibrium

**Recall:** *No unilateral improvement.*

### Mechanism Design

**Definition:** Design rules of game to achieve desired outcome.

**Goal:** Incentive compatibility (truthful reporting is optimal).

**Examples:**
- **VCG (Vickrey-Clarke-Groves):** Truthful auctions
- **Auction design:** First-price, second-price, English, Dutch
- **Matching markets:** Stable matching (Gale-Shapley)

**Recall:** *Design the game.*

### Auction Theory

**Types:**
| Type | Description |
|------|-------------|
| First-price sealed-bid | Highest bid wins, pays bid |
| Second-price sealed-bid (Vickrey) | Highest wins, pays second-highest |
| English | Ascending open outcry |
| Dutch | Descending open outcry |

**Revenue equivalence theorem:** Under certain conditions, all auction types yield same expected revenue.

**Winner's curse:** Overpay due to uncertainty.

**Recall:** *Auctions.*

### Algorithmic Game Theory

**Definition:** Computational aspects of game theory.

**Price of anarchy:** Ratio of worst Nash equilibrium to optimal.

**Selfish routing:** Braess's paradox; adding road can increase congestion.

**Truthful mechanisms:** Incentive-compatible algorithms.

**Recall:** *Price of anarchy, truthful mechanisms.*

---

## 24.2 Optimization

### Linear Programming (LP)

**Definition:** Optimize linear objective subject to linear constraints.

**Standard form:**
```
Minimize cᵀx
Subject to Ax ≤ b, x ≥ 0
```

**Simplex:** Vertex-hopping; exponential worst case, fast in practice.

**Interior point:** Polynomial; barrier methods.

**Duality:** Every LP has dual; strong duality for optimal.

**Recall:** *Simplex, interior point.*

### Integer Programming (IP)

**Definition:** LP with integer variables.

**Complexity:** NP-hard.

**Techniques:**
- **Branch and bound:** Branch on fractional variables
- **Cutting planes:** Add valid inequalities
- **Branch and cut:** Combine

**Recall:** *Branch & bound, cutting planes.*

### Scheduling

**Types:**
- **Job shop:** Jobs with operations on machines
- **Flow shop:** Jobs in fixed order
- **RCPSP:** Resource-constrained project scheduling

**Algorithms:** List scheduling, priority rules, metaheuristics.

**Recall:** *Job shop, flow shop, RCPSP.*

---

## 24.3 Stochastic Models

### Queueing Theory

**Definition:** Mathematical study of waiting lines.

**Components:**
- **Arrival process:** Poisson (M), general (G), deterministic (D)
- **Service process:** Exponential (M), general (G)
- **Number of servers:** 1, c
- **Queue capacity:** Finite, infinite
- **Discipline:** FIFO, LIFO, priority

**Notation:** Kendall's notation: A/S/c/K/N/D.

**M/M/1:**
- Utilization ρ = λ/μ
- Average queue length L = ρ/(1−ρ)
- Average wait time W = L/λ

**Little's law:** L = λW.

**Recall:** *M/M/1, M/M/c, Little's law.*

### Simulation

**Definition:** Model system; run experiments.

**Types:**
- **Discrete event:** Events at points in time
- **Monte Carlo:** Random sampling

**Use:** Queueing, finance, physics, logistics.

**Recall:** *Discrete event, Monte Carlo.*

### Markov Chains

**Definition:** State transitions depend only on current state.

**Stationary distribution:** πP = π.

**Use:** PageRank, queueing, RL, finance.

**Recall:** *Memoryless transitions.*

### Stochastic Optimization

**Definition:** Optimize under uncertainty.

**Methods:**
- **Stochastic gradient descent:** Sample gradients
- **Sample average approximation:** Monte Carlo + deterministic solver
- **Robust optimization:** Worst-case over uncertainty set

**Recall:** *Optimize under uncertainty.*

**SOTA:** Market design, matching markets, algorithmic mechanism design.

---
