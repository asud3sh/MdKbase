
# 23 · QUANTUM COMPUTING

## 23.1 Quantum Foundations

### Qubits

**Definition:** Quantum bit; superposition of |0⟩ and |1⟩.

**State:** |ψ⟩ = α|0⟩ + β|1⟩, where |α|² + |β|² = 1.

**Bloch sphere:** Visualize qubit state; north pole = |0⟩, south = |1⟩.

**Measurement:** Collapses state to |0⟩ (probability |α|²) or |1⟩ (probability |β|²).

**vs classical bit:** Classical bit is 0 or 1; qubit is superposition until measured.

**Recall:** *Quantum bit.*

### Quantum Gates

**Single-qubit gates:**
- **X:** Bit flip (NOT)
- **Y:** Bit + phase flip
- **Z:** Phase flip
- **H (Hadamard):** Creates superposition
- **S, T:** Phase gates

**Multi-qubit gates:**
- **CNOT:** Controlled-NOT; entangles
- **Toffoli:** Controlled-controlled-NOT
- **SWAP:** Exchanges qubits

**Properties:** Unitary (reversible); represented by matrices.

**Recall:** *Reversible operations.*

### Superposition

**Definition:** Linear combination of basis states.

**Example:** H|0⟩ = (|0⟩ + |1⟩)/√2.

**Why it matters:** Quantum algorithms exploit superposition to explore many states simultaneously.

**Recall:** *Both at once.*

### Entanglement

**Definition:** Correlated quantum states; measuring one affects the other.

**Bell states:**
```
|Φ⁺⟩ = (|00⟩ + |11⟩)/√2
|Φ⁻⟩ = (|00⟩ − |11⟩)/√2
|Ψ⁺⟩ = (|01⟩ + |10⟩)/√2
|Ψ⁻⟩ = (|01⟩ − |10⟩)/√2
```

**EPR paradox:** Einstein, Podolsky, Rosen; "spooky action at a distance."

**Bell's theorem:** Local hidden variables cannot explain entanglement.

**Use:** Quantum teleportation, superdense coding, QKD.

**Recall:** *Spooky correlation.*

### Quantum Circuits

**Definition:** Sequence of quantum gates + measurements.

**Model:** Qubits initialized to |0⟩; gates applied; measurement at end.

**Example (Bell state):**
```
q0: ─H──●──
q1: ────X──
```

**Recall:** *Quantum computation model.*

---

## 23.2 Quantum Algorithms

### Shor's Algorithm

**Purpose:** Factor integers in polynomial time.

**Impact:** Breaks RSA, ECC.

**Steps:**
1. Reduce factoring to period finding
2. Use quantum Fourier transform to find period
3. Classical post-processing to get factors

**Complexity:** O((log N)³) vs classical sub-exponential.

**Recall:** *Quantum factoring.*

### Grover's Algorithm

**Purpose:** Search unsorted database in O(√N).

**Steps:**
1. Initialize superposition of all states
2. Apply oracle (marks target)
3. Apply diffusion (amplifies target amplitude)
4. Repeat ~√N times
5. Measure

**Speedup:** Quadratic.

**Use:** SAT, cryptanalysis, optimization.

**Recall:** *Quantum search.*

### Quantum Fourier Transform (QFT)

**Definition:** Quantum analogue of DFT.

**Use:** Shor's algorithm, phase estimation.

**Recall:** *Quantum DFT.*

### Phase Estimation

**Definition:** Estimate eigenvalue of unitary operator.

**Use:** Shor's, quantum simulation.

**Recall:** *Estimate phase.*

### HHL Algorithm

**Purpose:** Solve linear systems Ax = b in O(log N).

**Caveats:** Requires sparse, well-conditioned A; output is quantum state.

**Recall:** *Quantum linear solver.*

### VQE (Variational Quantum Eigensolver)

**Purpose:** Find ground state energy of molecule.

**Hybrid:** Quantum + classical optimization.

**Use:** Quantum chemistry, materials.

**Recall:** *Quantum chemistry.*

### QAOA (Quantum Approximate Optimization Algorithm)

**Purpose:** Solve combinatorial optimization.

**Hybrid:** Quantum + classical.

**Use:** Max-Cut, scheduling.

**Recall:** *Quantum optimization.*

---

## 23.3 Quantum Error Correction

### Quantum Error Correction

**Challenge:** Qubits decohere; errors accumulate.

**Approaches:**
- **Surface codes:** 2D lattice; high threshold
- **Color codes:** Topological
- **LDPC codes:** Low-density parity-check

**Logical qubits:** Encoded using many physical qubits.

**Threshold theorem:** If error rate below threshold, can correct.

**Recall:** *Protect quantum info.*

### Surface Codes

**Definition:** Topological error-correcting code on 2D lattice.

**Properties:** High threshold (~1%), local operations.

**Use:** Leading approach for fault-tolerant quantum computing.

**Recall:** *Topological QEC.*

---

## 23.4 Quantum Hardware

### Quantum Hardware

**Types:**
- **Superconducting:** IBM, Google; transmon qubits; ~millikelvin
- **Trapped ion:** IonQ, Quantinuum; high fidelity; slower gates
- **Photonic:** Xanadu, PsiQuantum; room temperature; entanglement via photons
- **Topological:** Microsoft; Majorana fermions; theoretical

**Challenges:** Decoherence, gate fidelity, scalability, cryogenics.

**Recall:** *Superconducting, trapped ion, photonic.*

### Quantum Networking

**QKD (Quantum Key Distribution):** Secure key exchange using quantum states.

**Entanglement distribution:** Share entangled qubits over distance.

**Quantum repeaters:** Extend range.

**Recall:** *QKD, entanglement distribution.*

### Quantum Programming

**Languages:** Qiskit (IBM), Cirq (Google), Q# (Microsoft), Pennylane.

**Example (Qiskit):**
```python
from qiskit import QuantumCircuit
qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure([0,1], [0,1])
```

**Recall:** *Program quantum computers.*

**SOTA:** NISQ algorithms, quantum ML, error-corrected qubits, quantum advantage demonstrations, AI-discovered proofs in quantum complexity theory (exponential parallel repetition theorem for entangled games).

---
