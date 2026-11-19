# 1. META & LEARNING

## 1.1 What is Computer Science

**Definition:** Computer Science is the study of computation — what can be computed, how efficiently it can be computed, and how to express computation in forms that machines can execute. It is not the study of computers per se; computers are the tool, but the subject is computation itself.

**The Three Pillars:**

| Pillar | Question | Example |
|--------|----------|---------|
| **Theory** | What is computable? What are the limits? | Halting problem, NP-completeness |
| **Abstraction** | How do we model and express computation? | Programming languages, type systems, protocols |
| **Design** | How do we build real systems? | Operating systems, databases, distributed systems |

**Why this matters:** Understanding the three pillars prevents the common mistake of treating CS as "just programming." Programming is the craft; CS is the science and engineering discipline underneath. A programmer who only knows syntax will hit a ceiling; one who understands theory can reason about what is possible, abstraction can design clean systems, and design can build them at scale.

**Recall:** *CS = Theory + Abstraction + Design. Not "computers" — computation.*

---

## 1.2 CS vs Software Engineering vs IT

**Computer Science** asks: *What can be computed? How efficiently? What are the fundamental limits?* It is concerned with algorithms, complexity, computability, and the mathematical foundations of computation.

**Software Engineering** asks: *How do we build and maintain software at scale?* It is concerned with process, requirements, design patterns, testing, version control, and team collaboration. A software engineer applies CS knowledge but focuses on delivering reliable, maintainable systems.

**Information Technology** asks: *How do we deploy, operate, and support systems?* It is concerned with infrastructure, networking, databases, security operations, and user support. IT keeps systems running; CS and SE build them.

**Overlap:** In practice, these roles blur. A startup engineer may do all three. But the distinction matters for career direction: CS → research, algorithms, systems; SE → product engineering, architecture; IT → operations, DevOps, support.

**Recall:** *CS asks what & why; SE asks how to build; IT asks how to run.*

---

## 1.3 How to Learn CS

**The Four-Phase Method:**

1. **Map** — Build a mental map of the field. Know the major subfields and how they relate. Use this knowledge base as your map.
2. **Build** — Code forces understanding. Reading about a B-tree is not the same as implementing one. Build projects, solve problems, break things.
3. **Read** — Primary sources beat summaries. Read papers, specifications, and source code. The three-pass method (below) makes this tractable.
4. **Teach** — The Feynman technique: explain a concept in simple terms as if teaching a beginner. Gaps in your explanation reveal gaps in your understanding. Spaced repetition locks it in.

**Why this order:** Map first so you know where you're going. Build second so you have intuition. Read third so you understand deeply. Teach fourth so you retain.

**Recall:** *Map → Build → Read → Teach.*

---

## 1.4 Reading Research Papers — The Three-Pass Method

**Pass 1 (5–10 minutes):** Skim the title, abstract, introduction, section headings, conclusions, and references. Goal: understand *what* the paper is about. Answer: What problem does it solve? What is the claimed contribution?

**Pass 2 (1 hour):** Read the paper carefully, ignoring proofs and details. Look at figures, diagrams, and examples. Goal: understand *how* the method works. Answer: What are the key ideas? What assumptions are made? What evidence supports the claims?

**Pass 3 (several hours):** Read deeply, including proofs and appendices. Try to reproduce the results mentally or actually. Goal: *reproduce* the work. Answer: Can I re-derive the results? What are the weaknesses? What would I do differently?

**When to stop:** Most papers only need Pass 1. Some need Pass 2. Only a few need Pass 3.

**Recall:** *Pass 1 = what; Pass 2 = how; Pass 3 = reproduce.*

---

## 1.5 CS Subfields Map

**Six Buckets:**

| Bucket | Focus | Examples |
|--------|-------|----------|
| **Theory** | Foundations, limits, algorithms | Complexity, automata, cryptography |
| **Systems** | Hardware + software infrastructure | OS, compilers, networks, distributed systems |
| **AI** | Intelligence, learning, perception | ML, NLP, vision, robotics |
| **HCI** | Human-computer interaction | UX, accessibility, interfaces |
| **Applied** | Domain-specific computing | Fintech, bioinformatics, graphics |
| **Society** | Ethics, policy, impact | Privacy, fairness, sustainability |

**Why six buckets:** Every CS topic fits into one or more. This map helps you navigate the field and see connections.

**Recall:** *6 buckets — everything fits.*

---

## 1.6 Notation & Conventions

**Big-O:** Asymptotic upper bound. `f(n) = O(g(n))` means f grows no faster than g.

**Σ (Sigma):** Summation. `Σ_{i=1}^{n} i` = 1 + 2 + ... + n.

**Π (Pi):** Product. `Π_{i=1}^{n} i` = n!.

**∈:** Element of. `x ∈ S` means x is in set S.

**∀:** For all. `∀x P(x)` means P holds for every x.

**∃:** There exists. `∃x P(x)` means there is at least one x for which P holds.

**⇒:** Implies. `P ⇒ Q` means if P then Q.

**⇔:** If and only if. `P ⇔ Q` means P and Q are equivalent.

**Why consistency matters:** Mathematical notation is a language. Inconsistent notation causes confusion. Pick a convention and stick to it.

**Recall:** *Consistency > cleverness.*

---

## 1.7 SOTA — AI-Assisted Learning

**Current tools:**
- **Semantic Scholar** — AI-powered academic search; citation graphs, influential citations.
- **Papers with Code** — links papers to implementations; benchmarks.
- **Connected Papers** — visual graph of related papers.
- **Zotero** — reference management with PDF annotation.
- **LLM-based knowledge bases** — wiki-linked notes with verified references; agent-first retrieval.

**Best practice:** Use AI for discovery and summarization, but always verify primary sources. AI can hallucinate citations; the three-pass method still applies.

