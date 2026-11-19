# 28 · ADVANCED & CROSS-CUTTING TOPICS

## 28.1 Advanced Complexity Theory

### #P (Sharp-P)

**Definition:** Class of function problems counting the number of accepting paths of an NP machine.

**Complete problem:** #SAT — count satisfying assignments of a Boolean formula.

**Permanent:** Computing the permanent of a matrix is #P-complete (Valiant).

**Why it matters:** Counting is often harder than deciding. Many probabilistic inference problems reduce to #P.

**Recall:** *Counting problems; permanent is #P-complete.*

### PSPACE-Completeness

**Definition:** PSPACE = problems solvable with polynomial space.

**Complete problems:**
- **QBF (Quantified Boolean Formula):** PSPACE-complete.
- **Generalized games:** Many board games (Chess, Go with polynomial board) are PSPACE-hard.

**Relationships:** NP ⊆ PSPACE ⊆ EXP; strictness open.

**Recall:** *QBF, generalized games.*

### Interactive Proofs

**IP:** Class of languages with interactive proof systems (prover + verifier).

**IP = PSPACE:** Shamir's theorem (1992).

**Arthur-Merlin games:** Public-coin interactive proofs.

**Zero-knowledge proofs:** Prover convinces without revealing witness.

**Why it matters:** Foundations of cryptography, blockchain, verifiable computation.

**Recall:** *IP = PSPACE.*

### PCP Theorem

**Statement:** Every NP language has a probabilistically checkable proof verifiable by reading O(1) bits.

**Implications:**
- **Inapproximability:** Max-3SAT cannot be approximated within 7/8 + ε unless P = NP.
- **Hardness of approximation:** Many problems have tight inapproximability bounds.

**Why it matters:** Connects complexity, approximation, and coding theory.

**Recall:** *PCP → inapproximability.*

### Quantum Complexity

**BQP:** Bounded-error quantum polynomial time.

**QMA:** Quantum Merlin-Arthur; quantum analogue of NP.

**Quantum advantage:** Problems where quantum computers outperform classical (Shor, Grover).

**Quantum supremacy:** Demonstrating quantum device solves problem beyond classical.

**Recall:** *BQP, QMA.*

### Parameterized Complexity

**FPT:** Fixed-Parameter Tractable; time O(f(k) · n^c).

**W-hierarchy:** W[1], W[2], ...; classes of parameterized problems.

**Kernelization:** Reduce to smaller instance; kernel size bounds.

**Examples:** Vertex Cover (FPT), Clique (W[1]-hard).

**Recall:** *FPT, W-hierarchy, kernelization.*

### Fine-Grained Complexity

**SETH (Strong Exponential Time Hypothesis):** SAT requires 2^(1−ε)n time.

**3SUM conjecture:** 3SUM requires n² time.

**Conditional lower bounds:** Prove hardness assuming SETH or 3SUM.

**Examples:** Edit distance (SETH), orthogonal vectors (SETH).

**Recall:** *SETH, 3SUM.*

### Meta-Complexity

**MCSP (Minimum Circuit Size Problem):** Given truth table, find smallest circuit.

**Kt complexity:** Time-bounded Kolmogorov complexity.

**Why it matters:** Connects complexity, cryptography, learning theory.

**Recall:** *MCSP, Kt complexity.*

---

## 28.2 Advanced Information Theory

### Channel Coding

**Channel capacity:** Maximum rate of reliable communication.

**Shannon's theorem:** Reliable communication possible below capacity.

**Codes:**
- **Hamming:** Error detection/correction
- **Reed-Solomon:** Burst errors; CDs, QR codes
- **Convolutional:** Viterbi decoding
- **LDPC:** Low-Density Parity-Check; 5G, WiFi
- **Polar:** 5G control channels
- **Turbo:** 3G/4G

**Recall:** *Shannon limit; LDPC, polar codes.*

### Rate-Distortion Theory

**Definition:** Trade-off between compression rate and distortion.

**Rate-distortion function:** R(D) = minimum rate for distortion ≤ D.

**Use:** Lossy compression, quantization.

**Recall:** *Rate vs distortion.*

### Network Information Theory

**Definition:** Information theory for networks.

**Problems:** Multiple access, broadcast, interference, relay.

**Network coding:** Combine packets; improve throughput.

**Recall:** *Network coding.*

### Coding Theory

**Linear codes:** Codewords form vector space.

**Properties:** Minimum distance determines error correction.

**Bounds:** Hamming, Singleton, Gilbert-Varshamov.

**Applications:** Storage, communication, cryptography.

**Recall:** *Error-correcting codes.*

---

## 28.3 Cryptanalysis

### Cryptanalysis

**Definition:** Study of breaking cryptographic systems.

**Attacks:**
- **Brute force:** Try all keys
- **Frequency analysis:** Classical ciphers
- **Differential:** Block ciphers
- **Linear:** Block ciphers
- **Side-channel:** Timing, power, cache
- **Fault:** Induce errors; analyze
- **Meet-in-the-middle:** Double encryption

**Recall:** *Attacks on crypto.*

### Side-Channel Analysis

**Types:**
- **Timing:** Measure execution time
- **Power:** Measure power consumption
- **Electromagnetic:** Measure EM emissions
- **Cache:** Cache timing
- **Acoustic:** Sound emissions

**Countermeasures:** Constant-time, masking, hiding.

**Recall:** *Timing, power, cache.*

### Post-Quantum Cryptanalysis

**Threat:** Quantum computers break RSA, ECC.

**Quantum algorithms:** Shor (factoring), Grover (search).

**Post-quantum crypto:** Lattice-based, hash-based, code-based.

**Migration:** Hybrid schemes, crypto agility.

**Recall:** *Quantum-safe crypto.*

---

## 28.4 Program Analysis

### Abstract Interpretation

**Definition:** Approximate program semantics with abstract domain.

**Example:** Interval domain [a, b] for integers.

**Use:** Static analysis, verification.

**Tools:** Astrée, Polyspace.

**Recall:** *Approximate semantics.*

### Dataflow Analysis

**Definition:** Propagate facts through control-flow graph.

**Examples:**
- **Reaching definitions:** Which defs reach each point
- **Live variables:** Which vars used later
- **Available expressions:** Which exprs computed

**Framework:** Lattice, transfer functions, fixed point.

**Recall:** *Propagate facts.*

### Symbolic Execution

**Definition:** Execute with symbolic values; explore paths.

**Tools:** KLEE, angr, S2E.

**Use:** Bug finding, test generation, exploit development.

**Challenges:** Path explosion, constraint solving.

**Recall:** *Symbolic values, path exploration.*

### Model Checking

**Definition:** Exhaustively explore state space; verify properties.

**Temporal logics:** LTL, CTL.

**Tools:** SPIN, NuSMV, CBMC.

**Use:** Hardware, protocols, concurrent systems.

**Recall:** *Exhaustive state exploration.*

---

## 28.5 Runtime Systems

### Garbage Collection

**Algorithms:**
- **Mark-sweep:** Mark reachable; sweep unreachable
- **Mark-compact:** Mark; compact to eliminate fragmentation
- **Copying:** Copy live objects to new space
- **Generational:** Young/old generations

**Concurrent:** Collector runs alongside mutator.
**Incremental:** Small pauses.

**Trade-offs:** Throughput vs pause time vs memory.

**Recall:** *Automatic memory reclamation.*

### JIT Compilation

**Techniques:**
- **Hot path detection:** Compile frequently executed code
- **Tiered compilation:** Interpret → baseline JIT → optimizing JIT
- **Deoptimization:** Fall back to interpreter
- **Speculative optimization:** Assume common cases

**Examples:** V8, HotSpot, PyPy.

**Recall:** *Compile while running.*

### Profiling

**Types:**
- **Sampling:** Periodic snapshots
- **Instrumentation:** Insert counters
- **Hardware:** Performance counters

**Tools:** perf, VTune, gprof.

**Use:** Performance optimization.

**Recall:** *Measure performance.*

### Debugging

**Techniques:**
- **Breakpoints:** Pause execution
- **Watchpoints:** Monitor variables
- **Step:** Line-by-line
- **Reverse debugging:** Step backward

**Tools:** GDB, LLDB, WinDbg, rr.

**Recall:** *Find and fix bugs.*

---

## 28.6 Hardware Acceleration

### TPU (Tensor Processing Unit)

**Definition:** Google's ASIC for ML.

**Design:** Systolic array for matrix multiply.

**Use:** TensorFlow inference/training.

**Recall:** *ML accelerator.*

### NPU (Neural Processing Unit)

**Definition:** Dedicated ML accelerator.

**Use:** On-device ML (phones, edge).

**Examples:** Apple Neural Engine, Qualcomm Hexagon.

**Recall:** *On-device ML.*

### FPGA (Field-Programmable Gate Array)

**Definition:** Reconfigurable hardware.

**Use:** Prototyping, networking, ML, signal processing.

**Languages:** Verilog, VHDL, HLS.

**Recall:** *Reconfigurable hardware.*

### ASIC (Application-Specific Integrated Circuit)

**Definition:** Custom chip for specific function.

**Advantages:** Performance, power, cost at scale.
**Disadvantages:** High NRE, inflexible.

**Examples:** Bitcoin miners, TPU.

**Recall:** *Custom chip.*

---

## 28.7 Advanced Graphics

### Global Illumination

**Definition:** Simulate all light bounces.

**Methods:**
- **Path tracing:** Monte Carlo
- **Photon mapping:** Two-pass
- **Radiosity:** Finite elements
- **Real-time GI:** Lumen, voxel GI, screen-space GI

**Recall:** *All light bounces.*

### BRDF (Bidirectional Reflectance Distribution Function)

**Definition:** Ratio of outgoing radiance to incoming irradiance.

**Properties:** Energy conservation, reciprocity, Helmholtz.

**Models:** Lambertian, Phong, Cook-Torrance, Disney BRDF.

**Recall:** *Surface reflectance.*

### Volumetric Rendering

**Definition:** Render participating media (fog, smoke, clouds).

**Equation:** Radiative transfer equation.

**Methods:** Ray marching, delta tracking.

**Recall:** *Rendering media.*

### Differentiable Rendering

**Definition:** Rendering with gradients; enables inverse graphics.

**Use:** 3D reconstruction, material estimation, ML.

**Tools:** PyTorch3D, Mitsuba, TensorFlow Graphics.

**Recall:** *Gradients through rendering.*

---

## 28.8 Audio & Speech Processing

### Psychoacoustics

**Definition:** Study of sound perception.

**Concepts:** Masking, critical bands, absolute threshold.

**Use:** Audio compression (MP3, AAC).

**Recall:** *Sound perception.*

### Speech Enhancement

**Definition:** Improve speech quality; remove noise.

**Methods:** Spectral subtraction, Wiener filter, deep learning.

**Use:** Hearing aids, VoIP, ASR preprocessing.

**Recall:** *Remove noise.*

### Spatial Audio

**Definition:** 3D audio; directional sound.

**Techniques:** HRTF, Ambisonics, binaural.

**Use:** VR/AR, gaming, cinema.

**Recall:** *3D audio.*

### Beamforming

**Definition:** Directional signal processing with microphone array.

**Use:** Smart speakers, hearing aids, radar.

**Recall:** *Directional audio.*

---

## 28.9 Video Processing

### Video Codecs

**Standards:**
- **H.264/AVC:** Widely used
- **H.265/HEVC:** Better compression
- **AV1:** Royalty-free
- **VP9:** Google

**Techniques:** Motion compensation, transform coding, entropy coding.

**Recall:** *Video compression.*

### Streaming

**Protocols:** HLS, DASH, WebRTC.

**Techniques:** Adaptive bitrate, CDN, buffering.

**Recall:** *Video streaming.*

### Temporal Analysis

**Tasks:** Action recognition, tracking, video captioning.

**Models:** 3D CNNs, Two-stream, Transformers (ViViT, TimeSformer).

**Recall:** *Video understanding.*

---

## 28.10 Deep Recommender Systems

### Neural Collaborative Filtering

**Definition:** Neural networks for recommendation.

**Models:** NCF, Neural MF, AutoRec.

**Recall:** *Neural recommenders.*

### Sequential Recommendation

**Definition:** Model user behavior sequences.

**Models:** GRU4Rec, SASRec, BERT4Rec.

**Recall:** *Sequential recommendation.*

### Ranking Metrics

**Metrics:** Precision@k, Recall@k, NDCG, MAP, MRR.

**Recall:** *Ranking metrics.*

---

## 28.11 Search Engines

### Crawling

**Definition:** Discover and fetch web pages.

**Challenges:** Scale, politeness, duplicates, traps.

**Recall:** *Web crawling.*

### Indexing

**Definition:** Build inverted index for fast retrieval.

**Structures:** Inverted index, forward index, positional index.

**Recall:** *Indexing.*

### Ranking

**Definition:** Order results by relevance.

**Signals:** TF-IDF, BM25, PageRank, click-through, freshness.

**Learning to rank:** Pointwise, pairwise, listwise.

**Recall:** *Ranking.*

### Query Understanding

**Definition:** Interpret user query.

**Tasks:** Spell correction, query expansion, intent classification.

**Recall:** *Query understanding.*

---

## 28.12 Adversarial ML

### Adversarial Examples

**Definition:** Inputs crafted to fool ML models.

**Attacks:** FGSM, PGD, C&W.

**Defenses:** Adversarial training, certified defenses, input validation.

**Recall:** *Fooling models.*

### Data Poisoning

**Definition:** Corrupt training data to degrade model.

**Attacks:** Label flipping, backdoor.

**Defenses:** Data sanitization, robust training.

**Recall:** *Poisoning data.*

### Model Extraction

**Definition:** Steal model via queries.

**Attacks:** Query-based extraction.

**Defenses:** Rate limiting, watermarking.

**Recall:** *Stealing models.*

### Model Inversion

**Definition:** Reconstruct training data from model.

**Defenses:** Differential privacy, regularization.

**Recall:** *Inverting models.*

---

## 28.13 Federated Learning

### Federated Learning

**Definition:** Train models across devices without sharing raw data.

**Types:**
- **Horizontal:** Same features, different samples
- **Vertical:** Different features, same samples
- **Cross-silo:** Organizations
- **Cross-device:** Phones

**Challenges:** Heterogeneity, communication, security.

**Recall:** *Train locally, aggregate globally.*

### Secure Aggregation

**Definition:** Aggregate updates without revealing individual contributions.

**Techniques:** Cryptographic, differential privacy.

**Recall:** *Private aggregation.*

---

## 28.14 Edge AI

### Model Compression

**Techniques:**
- **Quantization:** Reduce precision
- **Pruning:** Remove weights
- **Knowledge distillation:** Train small model
- **Neural architecture search:** Find efficient architectures

**Recall:** *Compress models.*

### On-Device Training

**Definition:** Train on device.

**Challenges:** Compute, memory, power.

**Techniques:** Federated learning, transfer learning.

**Recall:** *Train on device.*

### TinyML

**Definition:** ML on microcontrollers.

**Frameworks:** TensorFlow Lite Micro, Edge Impulse.

**Use:** Wake word, anomaly detection.

**Recall:** *ML on MCUs.*

---

## 28.15 Neuromorphic Computing

### Neuromorphic Computing

**Definition:** Hardware mimicking brain.

**Spiking neural networks (SNN):** Event-driven; temporal coding.

**Hardware:** Intel Loihi, IBM TrueNorth, SpiNNaker.

**Use:** Low-power, real-time.

**Recall:** *Brain-inspired hardware.*

### Event-Based Vision

**Definition:** Cameras that output events on brightness change.

**Benefits:** Low latency, high dynamic range, low power.

**Use:** Robotics, autonomous vehicles.

**Recall:** *Event cameras.*

---

## 28.16 Emerging Computing Paradigms

### DNA Computing

**Definition:** Computation using DNA molecules.

**Operations:** Hybridization, ligation, PCR.

**Use:** Massive parallelism, storage.

**Recall:** *Molecular computation.*

### Optical Computing

**Definition:** Computation using light.

**Benefits:** Speed, bandwidth, low power.

**Use:** Interconnects, matrix multiply.

**Recall:** *Photonic chips.*

### Biological Computing

**Definition:** Computation using biological systems.

**Synthetic biology:** Engineer genetic circuits.

**Use:** Biosensors, therapeutics.

**Recall:** *Genetic circuits.*

### Quantum ML

**Definition:** Quantum algorithms for ML.

**Algorithms:** Quantum kernels, variational classifiers, QGANs.

**Challenges:** NISQ era, noise, qubit count.

**Recall:** *Quantum ML.*

---
