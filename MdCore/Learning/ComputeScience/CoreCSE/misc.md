
# 29 · MISCELLANEOUS

## 29.1 Human-Computer Interaction (HCI)

### Interaction Design

**Definition:** Design interactive products.

**Goals:** Usability, user experience.

**Principles:** Affordances, feedback, constraints, mapping.

**Recall:** *Design interactions.*

### Usability

**Definition:** Ease of use.

**Nielsen's heuristics:**
1. Visibility of system status
2. Match between system and real world
3. User control and freedom
4. Consistency and standards
5. Error prevention
6. Recognition over recall
7. Flexibility and efficiency
8. Aesthetic and minimalist design
9. Help users recognize and recover from errors
10. Help and documentation

**Recall:** *Easy to use.*

### Accessibility

**Definition:** Usable by people with disabilities.

**Standards:** WCAG (POUR: Perceivable, Operable, Understandable, Robust).

**Techniques:** Screen readers, keyboard navigation, alt text, contrast.

**Recall:** *Usable by all.*

### UI Design

**Elements:** Layout, color, typography, icons, spacing.

**Principles:** Hierarchy, contrast, alignment, proximity, repetition.

**Recall:** *Visual interface.*

### UX Research

**Methods:** Interviews, surveys, usability tests, A/B tests.

**Artifacts:** Personas, journey maps, wireframes.

**Recall:** *Understand users.*

### Prototyping

**Levels:** Low-fi (paper), mid-fi (wireframe), hi-fi (Figma).

**Purpose:** Test before building.

**Recall:** *Test before building.*

### Evaluation Methods

**Types:** Heuristic, cognitive walkthrough, usability test, A/B.

**Recall:** *Measure usability.*

### Human Factors

**Topics:** Cognition, perception, motor limits, ergonomics.

**Recall:** *Human capabilities.*

**SOTA:** AR/VR interfaces, voice/gesture, AI-assisted UX, brain-computer interfaces.

---

## 29.2 Scientific Computing

### Numerical Methods

**Topics:** Root finding, integration, ODE/PDE solvers.

**Properties:** Stability, convergence, accuracy.

**Recall:** *Approximate math.*

### Floating Point

**Standard:** IEEE 754.

**Formats:** Single (32-bit), double (64-bit), half (16-bit).

**Issues:** Rounding, cancellation, NaN, infinity.

**Recall:** *Finite precision.*

### Linear Solvers

**Direct:** LU, QR, Cholesky.

**Iterative:** CG, GMRES, BiCGSTAB.

**Sparse:** Specialized algorithms.

**Recall:** *Solve Ax = b.*

### Differential Equations

**ODE:** Euler, Runge-Kutta, Adams-Bashforth.

**PDE:** Finite difference, finite element, finite volume.

**Recall:** *Solve dynamics.*

### Simulation

**Types:** Monte Carlo, molecular dynamics, CFD.

**Use:** Physics, chemistry, engineering.

**Recall:** *Model reality.*

### HPC (High-Performance Computing)

**Clusters:** Many nodes; MPI.

**Shared memory:** OpenMP.

**GPUs:** CUDA, OpenCL.

**Recall:** *Supercomputing.*

### Parallel Computing

**Data parallelism:** Same op on many data.

**Task parallelism:** Different ops on same data.

**Scaling:** Strong (fixed problem), weak (growing problem).

**Recall:** *Many cores at once.*

**SOTA:** Differentiable programming, GPU-accelerated HPC, AI for science, exascale computing.

---

## 29.3 Formal Methods & Verification

### Model Checking

**Definition:** Exhaustively explore state space; verify properties.

**Temporal logics:** LTL, CTL.

**Tools:** SPIN, NuSMV, CBMC.

**Recall:** *Exhaustive state exploration.*

### Theorem Proving

**Definition:** Prove theorems in formal logic.

**Proof assistants:** Coq, Lean, Isabelle, Agda.

**Use:** Mathematics, verified software.

**Recall:** *Prove in logic.*

### Program Verification

**Definition:** Prove program correctness.

**Methods:** Hoare logic, separation logic, refinement.

**Tools:** Dafny, Frama-C, VeriFast.

**Recall:** *Prove program correct.*

### Specification Languages

**TLA+:** Temporal logic of actions; distributed systems.

**Alloy:** Relational logic; bounded verification.

**Z:** Set theory; state-based.

**Recall:** *Specify behavior.*

### Static Analysis

**Definition:** Analyze without running.

**Techniques:** Abstract interpretation, dataflow, type checking.

**Tools:** ESLint, Pylint, SonarQube, Coverity.

**Recall:** *Analyze without running.*

### SMT Solvers

**Definition:** Satisfiability Modulo Theories.

**Theories:** Arithmetic, arrays, bit-vectors, uninterpreted functions.

**Tools:** Z3, CVC5, Yices.

**Use:** Verification, synthesis, planning.

**Recall:** *Solve logical formulas.*

**SOTA:** Verified compilers (CompCert), verified OS (seL4), AI-assisted proofs, formal verification of AI-generated mathematical proofs (Lean 4 formalization).

---

## 29.4 Applied Domains

### Digital Humanities & Computational Social Science

**Text analysis:** Corpus linguistics, stylometry.

**Network analysis:** Social networks, graph metrics.

**Social simulation:** Agent-based modeling.

**Cultural analytics:** Visualization, distant reading.

**SOTA:** LLMs for humanities.

**Recall:** *CS + humanities.*

### Computational Finance

**Risk modeling:** VaR, CVaR, stress testing.

**Derivatives pricing:** Black-Scholes, Monte Carlo, binomial trees.

**Algorithmic trading:** Market microstructure, execution algorithms.

**Portfolio optimization:** Markowitz, Black-Litterman.

**SOTA:** ML for finance, deep hedging, quantum finance.

**Recall:** *CS + finance.*

### Health Informatics

**EHR:** Electronic Health Records; HL7, FHIR.

**Medical imaging:** DICOM, radiology AI.

**Clinical decision support:** CDS, rules engines.

**Telemedicine:** Remote care.

**HIPAA:** Privacy, security.

**SOTA:** AI diagnostics, federated learning for health, wearables.

**Recall:** *CS + health.*

### Legal Tech & RegTech

**Compliance automation:** AML, KYC, sanctions.

**Contract analysis:** NLP for legal documents.

**E-discovery:** Document review, privilege detection.

**Regulatory reporting:** MiFID, Dodd-Frank.

**SOTA:** LLMs for legal, smart contracts, RegTech platforms.

**Recall:** *CS + law.*

### Education Technology

**Intelligent tutoring:** Adaptive learning, knowledge tracing.

**Learning analytics:** Student modeling, dropout prediction.

**Spaced repetition:** Anki, SuperMemo.

**MOOC platforms:** Coursera, edX.

**SOTA:** AI tutors, LLM-based assessment, personalized learning.

**Recall:** *CS + education.*

---

## 29.5 Cross-Cutting Concerns

### Sustainability in Computing

**Topics:** Energy-efficient computing, carbon-aware scheduling, green software, e-waste.

**SOTA:** Carbon-aware AI, sustainable ML, energy-efficient models.

**Recall:** *Green computing.*

### Privacy-Enhancing Technologies

**Topics:** Homomorphic encryption, secure multi-party computation, federated learning, differential privacy, TEEs (SGX, TrustZone).

**SOTA:** FHE acceleration, confidential AI.

**Recall:** *Privacy tech.*

### Algorithmic Fairness

**Topics:** Bias detection, fairness metrics, mitigation strategies, accountability.

**Metrics:** Demographic parity, equalized odds, disparate impact.

**SOTA:** Fairness in LLMs, causal fairness, EU AI Act.

**Recall:** *Fair algorithms.*

---
