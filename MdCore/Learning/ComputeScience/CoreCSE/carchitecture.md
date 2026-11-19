# 9 · COMPUTER ARCHITECTURE

## 9.1 Digital Logic

### Digital Logic

**Definition:** Circuits built from logic gates processing binary signals.

**Gates:**
| Gate | Symbol | Output |
|------|--------|--------|
| AND | A·B | 1 if both 1 |
| OR | A+B | 1 if either 1 |
| NOT | ¬A | Inverts |
| NAND | ¬(A·B) | 0 if both 1 |
| NOR | ¬(A+B) | 0 if either 1 |
| XOR | A⊕B | 1 if different |

**Boolean algebra:** Laws for simplifying expressions (De Morgan, distributivity).

**Recall:** *Gates = logic.*

### Boolean Circuits

**Combinational:** Output depends only on current inputs.
- Adders, multiplexers, decoders, ALU

**Sequential:** Output depends on inputs + state.
- Flip-flops, latches, registers

**Truth tables:** List outputs for all input combinations.
**K-maps:** Visual simplification of Boolean expressions.

**Recall:** *Gates wired together.*

### Combinational Circuits

**Half adder:** Sum + carry of two bits.
**Full adder:** Sum + carry with carry-in.
**Ripple-carry adder:** Chain full adders.
**Carry-lookahead adder:** Faster; computes carries in parallel.
**Multiplexer (MUX):** Select one of many inputs.
**Decoder:** n inputs → 2ⁿ outputs.
**ALU:** Arithmetic Logic Unit; performs operations.

**Recall:** *No memory.*

### Sequential Circuits

**SR latch:** Set/Reset; basic memory.
**D flip-flop:** Captures input on clock edge.
**Register:** Multiple flip-flops; stores word.
**Counter:** Counts clock pulses.

**Clock:** Synchronizes state changes.

**Recall:** *Has memory.*

### Finite State Machines (FSM)

**Definition:** Model with states, transitions, outputs.

**Mealy:** Output depends on state + input.
**Moore:** Output depends only on state.

**Example:** Traffic light controller.

**Recall:** *State machine = FSM.*

---

## 9.2 Processor Design

### CPU Basics

**Fetch-Decode-Execute cycle:**
1. **Fetch:** Get instruction from memory (PC → IR)
2. **Decode:** Determine operation
3. **Execute:** Perform operation (ALU, memory, branch)
4. **Memory:** Access data if needed
5. **Writeback:** Store result

**Components:**
- **Control Unit:** Orchestrates
- **ALU:** Arithmetic/logic
- **Registers:** Fast storage (PC, IR, SP, general-purpose)
- **Cache:** Fast memory

**Recall:** *FDEW cycle.*

### Instruction Set Architecture (ISA)

**Definition:** Contract between hardware and software.

**Components:**
- **Instructions:** Operations (add, load, branch)
- **Registers:** Named storage
- **Addressing modes:** How to compute addresses (immediate, direct, indirect, indexed)
- **Data types:** Sizes, formats

**RISC vs CISC:**
| Aspect | RISC | CISC |
|--------|------|------|
| Instructions | Simple, fixed-length | Complex, variable-length |
| Execution | 1 cycle typical | Multi-cycle |
| Load/store | Separate | Combined |
| Examples | ARM, RISC-V, MIPS | x86, VAX |

**Recall:** *Contract between HW and SW.*

### Pipelining

**Definition:** Overlap execution of multiple instructions.

**Stages:** IF, ID, EX, MEM, WB.

**Hazards:**
- **Data:** Instruction needs result of previous
- **Control:** Branch target unknown
- **Structural:** Resource conflict

**Solutions:**
- **Forwarding:** Bypass result to next instruction
- **Stalling:** Insert bubbles
- **Branch prediction:** Guess branch direction
- **Speculation:** Execute guessed path

**Recall:** *Assembly line for instructions.*

### Superscalar

**Definition:** Issue multiple instructions per cycle.

**Requirements:** Multiple execution units; dependency analysis.

**Limits:** Instruction-level parallelism (ILP) available; register file ports.

**Recall:** *Parallel pipelines.*

### Out-of-Order Execution

**Definition:** Execute instructions as operands become ready; commit in order.

**Components:**
- **Reorder buffer:** Track in-flight instructions
- **Reservation stations:** Wait for operands
- **Register renaming:** Eliminate false dependencies

**Benefits:** Hides latencies; improves ILP.

**Recall:** *Execute when ready, commit in order.*

### Branch Prediction

**Static:** Always taken/not taken; backward taken, forward not.

**Dynamic:**
- **Two-bit:** 4 states; tolerates occasional mispredictions
- **Tournament:** Combine predictors
- **TAGE:** Tagged geometric history; state-of-art

**Misprediction cost:** Pipeline flush (10–20 cycles).

**Recall:** *Guess branch direction.*

---

## 9.3 Memory & I/O

### Cache Memory

**Definition:** Small, fast memory between CPU and main memory.

**Levels:** L1 (per-core, smallest), L2 (per-core), L3 (shared).

**Mapping:**
- **Direct-mapped:** Each block → one cache line
- **Set-associative:** Each block → one of k lines in set
- **Fully associative:** Any block → any line

**Replacement:** LRU, random, FIFO.

**Write policies:**
- **Write-through:** Write to cache + memory
- **Write-back:** Write to cache; write to memory on eviction

**Performance:** Hit rate, miss penalty, AMAT = hit_time + miss_rate × miss_penalty.

**Recall:** *Small fast memory near CPU.*

### Memory Hierarchy

**Pyramid:**
```
Registers  | ~1 cycle   | bytes
L1 Cache   | ~4 cycles  | KB
L2 Cache   | ~10 cycles | MB
L3 Cache   | ~40 cycles | MB
RAM        | ~100 cycles| GB
SSD        | ~100μs     | TB
HDD        | ~10ms      | TB
```

**Principle:** Locality (temporal, spatial) makes hierarchy effective.

**Recall:** *Pyramid of speed vs size.*

### Virtual Memory

**Definition:** Illusion of large, contiguous memory per process.

**Mechanism:**
- **Page tables:** Map virtual → physical pages
- **TLB:** Cache for page table entries
- **Page fault:** Page not in memory; load from disk

**Demand paging:** Load pages on demand.

**Recall:** *Fake contiguous memory.*

### Paging & Segmentation

**Paging:** Fixed-size pages; avoids external fragmentation.
**Segmentation:** Variable-size segments; logical units (code, data, stack).
**Combined:** Segment + page.

**Page replacement:** LRU, FIFO, Clock, Optimal.

**Recall:** *Pages = fixed; segments = logical.*

### Interrupts

**Definition:** Signal to CPU to handle event.

**Types:**
- **Hardware:** Device signals (I/O complete)
- **Software:** Trap, exception, syscall

**ISR (Interrupt Service Routine):** Handler.

**Vector table:** Maps interrupt number to handler.

**Exceptions vs interrupts:** Exceptions are synchronous (divide by zero); interrupts are asynchronous.

**Recall:** *Signal to CPU: handle this.*

### I/O Systems

**Polling:** CPU repeatedly checks device status; simple but wasteful.

**Interrupts:** Device signals CPU; efficient.

**DMA (Direct Memory Access):** Device transfers data to memory without CPU; CPU notified on completion.

**Memory-mapped I/O:** Device registers mapped to memory addresses.

**Recall:** *CPU ↔ devices.*

### Buses

**Definition:** Shared communication path.

**Types:**
- **Address bus:** Addresses
- **Data bus:** Data
- **Control bus:** Control signals

**Examples:** PCIe (peripherals), USB (external), memory bus (CPU-RAM).

**Recall:** *Shared communication path.*

---

## 9.4 Parallel Hardware

### Multicore

**Definition:** Multiple cores on one chip.

**Cache coherence:** Ensure all cores see consistent memory.
- **MESI:** Modified, Exclusive, Shared, Invalid
- **Snooping:** Cores monitor bus
- **Directory:** Central directory tracks sharing

**NUMA:** Non-Uniform Memory Access; memory latency depends on location.

**Recall:** *Many cores, shared memory.*

### SIMD

**Definition:** Single Instruction, Multiple Data; one instruction operates on vector.

**Examples:** SSE, AVX (x86), NEON (ARM).

**Uses:** Graphics, ML, scientific computing.

**Recall:** *Vector parallelism.*

### GPU Architecture

**Definition:** Thousands of cores optimized for parallel throughput.

**SIMT:** Single Instruction, Multiple Threads; warps of 32 threads.

**Memory hierarchy:** Registers, shared memory, global memory.

**Programming:** CUDA, OpenCL, HIP.

**Recall:** *Massive parallelism.*

### Accelerators

**TPU:** Tensor Processing Unit; Google; matrix multiply.

**NPU:** Neural Processing Unit; on-device ML.

**FPGA:** Field-Programmable Gate Array; reconfigurable.

**ASIC:** Application-Specific Integrated Circuit; custom.

**Recall:** *Specialized hardware for specific workloads.*

### RISC-V

**Definition:** Open-source ISA; modular extensions.

**Base ISA:** RV32I, RV64I.
**Extensions:** M (multiply), A (atomic), F (float), D (double), C (compressed), V (vector).

**Ecosystem:** Growing; used in embedded, education, research.

**Recall:** *Open-source ISA.*

### x86

**Definition:** Dominant desktop/server ISA; CISC.

**Backward compatible:** Runs decades-old code.

**Microarchitecture:** Modern x86 decodes to RISC-like micro-ops.

**Recall:** *Intel/AMD.*

### ARM

**Definition:** RISC ISA; dominant mobile/embedded.

**Licensing:** ARM designs cores; companies license.

**Examples:** Apple Silicon (M1/M2/M3), Qualcomm Snapdragon, AWS Graviton.

**Recall:** *Efficient RISC.*

**SOTA:** Chiplets, heterogeneous computing, CXL, in-memory computing, near-memory computing.

---