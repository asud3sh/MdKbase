# 27 · EMBEDDED & REAL-TIME SYSTEMS

## 27.1 Fundamentals

### Embedded Systems

**Definition:** Computer system with dedicated function within larger system.

**Characteristics:**

- Constrained resources (CPU, memory, power)
- Real-time constraints
- Reliability requirements
- Often safety-critical

**Examples:** Microcontrollers, IoT devices, automotive ECUs, medical devices, aerospace.

**Design considerations:** Power, cost, size, reliability, real-time.

**Recall:** *Computers in devices.*

### Microcontrollers

**Definition:** Small computer on a single chip; CPU, memory, peripherals.

**Examples:** Arduino (ATmega), STM32, ESP32, Raspberry Pi Pico.

**Peripherals:** GPIO, UART, SPI, I2C, ADC, PWM, timers.

**Recall:** *MCU = CPU + memory + peripherals.*

### RTOS (Real-Time Operating System)

**Definition:** OS designed for real-time applications.

**Properties:**

- Deterministic scheduling
- Bounded latency
- Priority-based preemption

**Components:** Scheduler, tasks, semaphores, queues, timers.

**Examples:** FreeRTOS, Zephyr, VxWorks, QNX, RT-Thread.

**Scheduling algorithms:**

- **Rate-monotonic:** Shorter period = higher priority
- **Earliest deadline first (EDF):** Nearest deadline = highest priority

**Recall:** *Meet deadlines.*

### WCET (Worst-Case Execution Time)

**Definition:** Maximum time a task can take.

**Analysis methods:**

- **Static:** Analyze code and hardware
- **Measurement:** Run many times; take max
- **Hybrid:** Combine

**Challenges:** Caches, pipelines, interrupts.

**Recall:** *Worst-case execution time.*

### Safety-Critical Systems

**Standards:**

| Domain     | Standard  |
| ---------- | --------- |
| Aviation   | DO-178C   |
| Automotive | ISO 26262 |
| Medical    | IEC 62304 |
| Railway    | EN 50128  |
| Industrial | IEC 61508 |

**Levels:** ASIL (automotive), DAL (aviation), SIL (industrial).

**Practices:** Formal methods, testing, traceability, certification.

**Recall:** *DO-178C, ISO 26262, MISRA C.*

---

## 27.2 Connectivity & Power

### IoT (Internet of Things)

**Definition:** Networked sensors and actuators.

**Protocols:**

- **MQTT:** Publish/subscribe; lightweight
- **CoAP:** REST-like; UDP
- **Zigbee:** Mesh; low power
- **LoRaWAN:** Long range; low power
- **BLE:** Bluetooth Low Energy

**Architecture:** Device → Gateway → Cloud.

**Challenges:** Security, interoperability, power.

**Recall:** *Connected things.*

### Firmware

**Definition:** Software embedded in hardware.

**Components:** Bootloader, kernel/RTOS, application.

**Bootloader:** Initializes hardware; loads application.

**OTA (Over-the-Air) updates:** Update firmware remotely.

**Recall:** *Software for hardware.*

### Low-Power Design

**Techniques:**

- **Sleep modes:** Deep sleep, light sleep
- **DVFS:** Dynamic voltage and frequency scaling
- **Clock gating:** Disable unused clocks
- **Power gating:** Disable unused blocks
- **Energy harvesting:** Solar, vibration, thermal

**Metrics:** Energy per operation, battery life.

**Recall:** *Sleep modes, DVFS.*

---

## 27.3 Co-Design & Verification

### Hardware-Software Co-Design

**Definition:** Design hardware and software together.

**Platforms:**

- **FPGA:** Field-Programmable Gate Array; reconfigurable
- **ASIC:** Application-Specific Integrated Circuit; custom
- **SoC:** System on Chip; integrated

**Trade-offs:** Performance, power, flexibility, cost.

**Recall:** *FPGA, ASIC, SoC.*

### Formal Verification

**Definition:** Prove correctness mathematically.

**Methods:**

- **Model checking:** Exhaustive state exploration
- **Theorem proving:** Prove properties in logic
- **Abstract interpretation:** Approximate analysis

**Tools:** SPIN, NuSMV, Coq, Isabelle, CBMC.

**Use:** Safety-critical systems, protocols, hardware.

**Recall:** *Prove correctness.*

### Real-Time Communication

**Protocols:**

- **CAN:** Controller Area Network; automotive
- **FlexRay:** High-speed; automotive
- **TSN:** Time-Sensitive Networking; Ethernet-based
- **ARINC 653:** Avionics partitioning

**Properties:** Deterministic, bounded latency, fault-tolerant.

**Recall:** *CAN, TSN, ARINC 653.*

### Security in Embedded Systems

**Threats:** Physical access, side-channel, firmware tampering.

**Mitigations:**

- **Secure boot:** Verify firmware signature
- **Trusted execution:** TEE, TrustZone
- **Encryption:** At rest, in transit
- **Secure element:** Dedicated security chip

**Recall:** *Secure boot, TEE.*

### Domain-Specific Standards

**Automotive:**

- **AUTOSAR:** Software architecture
- **ISO 26262:** Functional safety
- **MISRA C:** Coding standard

**Aviation:**

- **DO-178C:** Software
- **DO-254:** Hardware
- **ARINC 653:** Partitioning

**Medical:**

- **IEC 62304:** Software lifecycle
- **FDA:** Regulatory approval

**Industrial:**

- **IEC 61508:** Functional safety
- **IEC 61131:** PLC programming

**Recall:** *AUTOSAR, DO-178C, IEC 62304.*

**SOTA:** Rust for embedded, RISC-V MCUs, edge AI, TinyML, Zephyr RTOS.

---