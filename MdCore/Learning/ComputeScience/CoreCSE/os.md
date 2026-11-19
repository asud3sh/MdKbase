
# 10 · OPERATING SYSTEMS

## 10.1 Processes & Threads

### OS Basics

**Definition:** An operating system is a resource manager and abstraction layer between hardware and applications.

**Responsibilities:**
- **Process management:** Create, schedule, terminate
- **Memory management:** Allocate, protect, virtualize
- **File systems:** Organize persistent data
- **I/O management:** Coordinate devices
- **Security:** Isolation, access control
- **Networking:** Protocol stack, sockets

**Kernel:** Core of OS; runs in privileged mode (ring 0).
**User space:** Applications; restricted mode (ring 3).

**System call:** Interface from user to kernel (fork, read, write, open).

**Recall:** *Manage hardware for programs.*

### Kernel Types

| Type | Description | Examples |
|------|-------------|----------|
| Monolithic | All services in kernel | Linux, traditional Unix |
| Micro | Minimal kernel; services in user space | Mach, QNX, seL4 |
| Hybrid | Mix of both | Windows NT, macOS |
| Exokernel | Minimal; libraries provide abstractions | MIT Exokernel |

**Trade-offs:** Monolithic faster but less isolated; micro more reliable but IPC overhead.

**Recall:** *Core of OS.*

### Processes

**Definition:** A program in execution with its own address space.

**Process Control Block (PCB):** Contains:
- Process ID (PID)
- Program counter
- Register state
- Memory maps
- Open file descriptors
- Scheduling info
- Accounting info

**Process states:**
```
New → Ready → Running → Waiting → Terminated
         ↑        ↓
         └────────┘
```

**Context switch:** Save current process state; load next. Overhead: ~1–10 μs.

**Recall:** *Running program + state.*

### Threads

**Definition:** Lightweight unit of execution within a process; shares address space.

**Shared:** Code, data, heap, file descriptors.
**Private:** Stack, registers, program counter, thread-local storage.

**User threads:** Managed by library; fast but blocking syscalls block all.
**Kernel threads:** Managed by OS; true parallelism.

**Models:**
- **1:1:** One user thread per kernel thread (Linux, Windows)
- **N:1:** Many user threads per kernel thread
- **M:N:** Many user to many kernel (Go goroutines, Erlang processes)

**Recall:** *Unit of execution.*

### Scheduling

**Definition:** Decide which process/thread runs next.

**Metrics:**
- **CPU utilization:** Keep CPU busy
- **Throughput:** Jobs completed per time
- **Turnaround time:** Submission to completion
- **Waiting time:** Time in ready queue
- **Response time:** Submission to first response

**Algorithms:**

| Algorithm | Preemptive | Description | Starvation |
|-----------|------------|-------------|------------|
| FCFS | No | First come, first served | Convoy effect |
| SJF | No | Shortest job first | Possible |
| SRTF | Yes | Preemptive SJF | Possible |
| RR | Yes | Round robin; time quantum | No |
| Priority | Both | Highest priority | Possible |
| MLFQ | Yes | Multi-level feedback queue | No (with aging) |
| CFS | Yes | Completely fair scheduler | No |

**Linux CFS:** Uses red-black tree; virtual runtime; fair share.

**Recall:** *Who runs next?*

### Context Switching

**Steps:**
1. Save current process registers to PCB
2. Update process state
3. Select next process
4. Load next process registers from PCB
5. Update memory maps (TLB flush)

**Cost:** 1–10 μs; more with cache pollution.

**Recall:** *Swap out, swap in.*

---

## 10.2 Synchronization

### Synchronization

**Critical section:** Code accessing shared resource.

**Requirements:**
- **Mutual exclusion:** Only one thread in CS
- **Progress:** If no thread in CS, one can enter
- **Bounded waiting:** No starvation

**Primitives:**
- **Lock/Mutex:** Binary; acquire/release
- **Semaphore:** Counting; P (wait), V (signal)
- **Monitor:** High-level; condition variables
- **Barrier:** All threads wait until all arrive

**Recall:** *Coordinate threads.*

### Mutexes & Semaphores

**Mutex:** Binary lock; owner must release.
```
lock(mutex)
critical_section()
unlock(mutex)
```

**Semaphore:** Integer counter; P decrements (blocks if 0), V increments.
```
P(sem)  # wait
critical_section()
V(sem)  # signal
```

**Binary semaphore:** Acts like mutex.
**Counting semaphore:** Controls access to N resources.

**Problems:** Deadlock, livelock, priority inversion.

**Recall:** *Lock vs counter.*

### Deadlocks

**Four necessary conditions:**
1. **Mutual exclusion:** Resource held exclusively
2. **Hold and wait:** Hold one, wait for another
3. **No preemption:** Cannot forcibly take resource
4. **Circular wait:** Cycle of waiting

**Handling:**
- **Prevention:** Break one condition
- **Avoidance:** Banker's algorithm; safe state
- **Detection:** Build wait-for graph; find cycles
- **Recovery:** Kill process, rollback

**Banker's algorithm:** Check if granting request leaves system in safe state.

**Recall:** *4 conditions → break one.*

### Inter-Process Communication (IPC)

**Mechanisms:**
| Mechanism | Direction | Speed | Use |
|-----------|-----------|-------|-----|
| Pipe | Unidirectional | Fast | Parent-child |
| Named pipe (FIFO) | Unidirectional | Fast | Unrelated processes |
| Message queue | Bidirectional | Medium | Async |
| Shared memory | Bidirectional | Fastest | Large data |
| Socket | Bidirectional | Medium | Network |
| Signal | Async notification | Fast | Events |

**Shared memory:** Fastest but needs synchronization.

**Recall:** *How processes talk.*

---

## 10.3 Memory & Storage

### Memory Management

**Contiguous allocation:** Each process in contiguous block.
- **First fit:** First hole big enough
- **Best fit:** Smallest hole big enough
- **Worst fit:** Largest hole
- **Fragmentation:** Internal (wasted inside block), external (small holes)

**Paging:** Divide memory into fixed-size pages; virtual → physical via page table.

**Segmentation:** Variable-size segments (code, data, stack).

**Swapping:** Move process to disk when memory tight.

**Recall:** *Allocate + protect memory.*

### Virtual Memory

**Page table:** Maps virtual page → physical frame.

**Multi-level page table:** Hierarchical; saves space.

**TLB (Translation Lookaside Buffer):** Cache for recent translations.
- Hit: 1 cycle
- Miss: page table walk (10–100 cycles)

**Page fault:** Page not in memory; OS loads it.
- Minor: page in memory but not mapped
- Major: page on disk

**Demand paging:** Load pages on demand.

**Thrashing:** Excessive paging; CPU spends time swapping. Solution: working set model, reduce multiprogramming.

**Recall:** *Illusion of large memory.*

### File Systems

**File:** Named collection of data.

**Directory:** Organizes files.

**Inode:** Metadata (size, permissions, timestamps, block pointers).

**Allocation methods:**
- **Contiguous:** Simple; fragmentation
- **Linked:** No fragmentation; slow random access
- **Indexed (inode):** Fast random access; overhead

**Journaling:** Log changes before applying; crash recovery.

**Examples:**
| FS | Features |
|----|----------|
| FAT | Simple; USB drives |
| ext4 | Linux; journaling; extents |
| NTFS | Windows; journaling; ACLs |
| ZFS | Copy-on-write; snapshots; checksums |
| Btrfs | Copy-on-write; snapshots; RAID |

**Recall:** *Organize persistent data.*

### I/O Management

**Buffering:** Temporary storage to smooth speed mismatch.

**Caching:** Keep frequently used data in fast memory.

**Spooling:** Queue jobs for device (print spooler).

**Block devices:** Fixed-size blocks (disk).
**Character devices:** Byte stream (keyboard, serial).

**Recall:** *Manage device I/O.*

### Device Drivers

**Definition:** Kernel modules for specific hardware.

**Responsibilities:**
- Initialize device
- Handle interrupts
- Provide I/O interface
- Manage power

**Interrupt handler:** Runs on interrupt; must be fast.

**Recall:** *Software for hardware.*

### System Calls

**Definition:** Interface from user to kernel.

**Common syscalls:**
| Category | Examples |
|----------|----------|
| Process | fork, exec, exit, wait |
| File | open, read, write, close, stat |
| Memory | mmap, brk, munmap |
| Network | socket, bind, listen, accept |
| IPC | pipe, shmget, msgget |

**Mechanism:** Trap instruction; switch to kernel mode; execute; return.

**Recall:** *Program asks OS for help.*

---

## 10.4 Virtualization

### Virtualization

**Definition:** Run multiple OS instances on one physical machine.

**Hypervisor (VMM):** Manages VMs.
- **Type 1 (bare-metal):** Runs directly on hardware (Xen, ESXi, KVM)
- **Type 2 (hosted):** Runs on host OS (VirtualBox, VMware Workstation)

**Full virtualization:** Guest OS unmodified.
**Para-virtualization:** Guest OS modified for performance.

**Recall:** *Fake hardware.*

### Containers

**Definition:** OS-level virtualization; isolated processes share kernel.

**Mechanisms:**
- **Namespaces:** Isolate PID, network, mount, UTS, IPC, user
- **Cgroups:** Limit CPU, memory, I/O

**Examples:** Docker, LXC, Podman, containerd.

**vs VMs:** Containers lighter (MB vs GB), faster startup (ms vs s), less isolation.

**Recall:** *Isolated processes, shared kernel.*

### Hypervisors

**Type 1:** Xen, ESXi, Hyper-V, KVM (Linux kernel module).
**Type 2:** VirtualBox, VMware Workstation, Parallels.

**Hardware support:** Intel VT-x, AMD-V.

**Recall:** *Runs VMs.*

### Real-Time OS (RTOS)

**Hard real-time:** Missed deadline = failure (pacemaker, flight control).
**Soft real-time:** Missed deadline = degraded (video streaming).

**Scheduling:** Rate-monotonic, earliest deadline first (EDF).

**Examples:** FreeRTOS, VxWorks, QNX, Zephyr.

**Recall:** *Meet deadlines.*

### Distributed OS

**Definition:** Multiple machines appear as one.

**Goals:** Transparency, scalability, fault tolerance.

**Examples:** Plan 9, Amoeba, Google Fuchsia.

**Recall:** *OS across nodes.*

### eBPF

**Definition:** Extended Berkeley Packet Filter; run sandboxed programs in kernel.

**Uses:** Tracing, networking, security, observability.

**Benefits:** Safe kernel programming; no kernel module needed.

**Tools:** bpftrace, Cilium, Falco.

### io_uring

**Definition:** Async I/O interface; submission and completion queues.

**Benefits:** Fewer syscalls; high performance.

**Use:** Databases, web servers, storage.

### Unikernels

**Definition:** Single-address-space OS; library OS compiled with app.

**Benefits:** Small, fast, secure.
**Drawbacks:** Limited compatibility.

**Examples:** MirageOS, IncludeOS.

### Kernel Bypass

**DPDK:** Data Plane Development Kit; user-space networking.
**RDMA:** Remote Direct Memory Access; zero-copy network.

**SOTA:** Rust in kernel, confidential computing, Cognitive OS Agents embedding autonomous planning into OS layer, Agentized OS with system-level AI Runtime.

---
