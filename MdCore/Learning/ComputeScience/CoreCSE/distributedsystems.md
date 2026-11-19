# 13 · DISTRIBUTED SYSTEMS

## 13.1 Foundations

### Distributed System Basics

**Definition:** A distributed system is a collection of independent computers that appears to users as a single coherent system.

**Why distribute:**
- **Scalability:** Handle more load than single machine
- **Fault tolerance:** Survive node failures
- **Latency:** Serve users geographically
- **Cost:** Commodity hardware vs mainframe

**Challenges:**
- No shared memory or clock
- Partial failures
- Network partitions
- Concurrency across nodes
- Coordination overhead

**Fallacies of distributed computing:**
1. The network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. The network is secure
5. Topology doesn't change
6. There is one administrator
7. Transport cost is zero
8. The network is homogeneous

**Recall:** *Many computers, one system.*

### CAP Theorem

**Statement:** In a distributed system, you can guarantee at most two of:
- **Consistency:** All nodes see the same data
- **Availability:** Every request gets a response
- **Partition tolerance:** System continues despite dropped messages

**Reality:** Partitions happen; so you choose C or A during partition.

**Classifications:**
- **CA:** Single-node; not partition-tolerant
- **CP:** MongoDB, HBase, Spanner; consistent, may be unavailable
- **AP:** Cassandra, DynamoDB, Riak; available, may be inconsistent

**PACELC extension:** If Partition, choose A or C; Else (normal), choose Latency or Consistency.

**Recall:** *Partition forces C or A.*

### Consistency Models

**Strong consistency:** Reads see latest write.
**Linearizable:** Operations appear atomic; real-time order.
**Sequential:** Operations in program order; global order.
**Causal:** Causally related operations ordered.
**Eventual:** Replicas converge eventually.

**Trade-off:** Stronger consistency = higher latency.

**Recall:** *How fresh is data?*

---

## 13.2 Data Management

### Replication

**Definition:** Maintain copies of data on multiple nodes.

**Topologies:**
- **Leader-follower (primary-backup):** One leader accepts writes; followers replicate
- **Multi-leader:** Multiple leaders accept writes; conflict resolution needed
- **Leaderless:** Any node accepts writes; quorum reads/writes (Dynamo)

**Sync vs async:** Sync waits for replica ACK; async doesn't.

**Benefits:** Durability, availability, read scaling.
**Costs:** Consistency challenges, write amplification.

**Recall:** *Copy data for durability.*

### Sharding (Partitioning)

**Definition:** Split data across nodes by key.

**Strategies:**
- **Range:** Contiguous key ranges (e.g., A–F, G–M)
- **Hash:** hash(key) mod N
- **Directory:** Lookup service maps keys to nodes
- **Consistent hashing:** Minimal reshuffling on node changes

**Challenges:** Hotspots, rebalancing, cross-shard queries.

**Recall:** *Split data across nodes.*

### Consensus

**Definition:** Agreement among distributed nodes on a single value.

**Properties:**
- **Agreement:** All correct nodes decide same value
- **Validity:** Value proposed by some node
- **Termination:** All correct nodes eventually decide

**FLP impossibility:** In async system with one faulty process, deterministic consensus impossible.

**Paxos:** Classic consensus; proposers, acceptors, learners.
**Raft:** Understandable consensus; leader election, log replication, safety.

**Raft roles:**
- **Leader:** Handles client requests
- **Follower:** Passive
- **Candidate:** Running for election

**Raft terms:** Logical time; each term has at most one leader.

**Recall:** *Agree on one value.*

### Leader Election

**Bully algorithm:** Highest ID wins.

**Ring algorithm:** Token passes; node with token is leader.

**Raft election:** Followers timeout → candidate → request votes → leader.

**Recall:** *Pick a coordinator.*

### Clocks & Ordering

**Physical clocks:** NTP-synchronized; can drift.

**Logical clocks (Lamport):** Each event gets timestamp; if A → B, then L(A) < L(B).

**Vector clocks:** Each node maintains vector; detects concurrency.

**Example (vector clocks):** Node A: [1,0,0]; Node B: [1,1,0]; A and B concurrent if neither vector dominates.

**Recall:** *Time is unreliable.*

### Vector Clocks

**Definition:** Vector of logical timestamps, one per node.

**Rules:**
- Increment own counter on event
- Attach vector to messages
- On receive, take element-wise max + increment

**Comparison:**
- V1 < V2 if all elements ≤ and some <
- V1 || V2 if neither dominates (concurrent)

**Use:** Detect causality, resolve conflicts.

**Recall:** *Causality vectors.*

### CRDTs (Conflict-Free Replicated Data Types)

**Definition:** Data types that can be replicated and merged without conflicts.

**Types:**
- **G-Counter:** Grow-only counter
- **PN-Counter:** Positive-negative counter
- **G-Set:** Grow-only set
- **OR-Set:** Observed-remove set
- **LWW-Register:** Last-writer-wins register

**Properties:** Commutative, associative, idempotent merge.

**Use:** Collaborative editing, offline-first apps, distributed databases.

**Recall:** *Merge without conflict.*

---

## 13.3 Reliability & Transactions

### Fault Tolerance

**Failure models:**
- **Crash-stop:** Node halts
- **Crash-recovery:** Node halts then restarts
- **Omission:** Messages lost
- **Byzantine:** Arbitrary behavior

**Techniques:**
- **Replication:** Multiple copies
- **Checkpointing:** Save state periodically
- **Redundancy:** Duplicate components
- **Retry:** Resend on failure

**Fail-stop vs Byzantine:** Crash-stop simpler; Byzantine needs 3f+1 nodes for f faults.

**Recall:** *Survive failures.*

### Byzantine Fault Tolerance

**Definition:** Tolerate malicious or arbitrary node behavior.

**Requirement:** 3f+1 nodes to tolerate f Byzantine faults.

**Algorithms:** PBFT, Tendermint, HotStuff.

**Use:** Blockchain, aerospace, critical infrastructure.

**Recall:** *Tolerate liars.*

### Distributed Transactions

**ACID across nodes:** Atomicity, consistency, isolation, durability.

**Protocols:**
- **2PC (Two-Phase Commit):** Prepare → commit/abort
- **3PC:** Adds pre-commit; less blocking
- **Saga:** Sequence of local transactions with compensations

**2PC phases:**
1. **Prepare:** Coordinator asks all to prepare; participants vote
2. **Commit:** If all yes, commit; else abort

**Problem:** Coordinator failure blocks participants.

**Recall:** *All-or-nothing across nodes.*

### Two-Phase Commit

**Steps:**
1. Coordinator sends prepare to all participants
2. Participants lock resources and vote yes/no
3. Coordinator collects votes
4. If all yes, send commit; else send abort
5. Participants commit/abort and ACK

**Blocking:** If coordinator fails after prepare, participants wait.

**Recall:** *Vote then commit.*

---

## 13.4 Architecture

### Microservices

**Definition:** Application as suite of small, independent services.

**Characteristics:**
- Single responsibility
- Independently deployable
- Communicate via APIs (REST, gRPC, messaging)
- Own their data

**Benefits:** Scalability, flexibility, technology diversity.
**Challenges:** Distributed complexity, data consistency, testing.

**Recall:** *Many small services.*

### Service Mesh

**Definition:** Infrastructure layer for service-to-service communication.

**Components:**
- **Sidecar proxy:** Per-service proxy (Envoy)
- **Control plane:** Configuration, policy (Istio, Linkerd)

**Features:** Traffic management, security, observability.

**Recall:** *Networking layer for microservices.*

### Message Queues

**Definition:** Asynchronous communication via queues.

**Models:**
- **Point-to-point:** One consumer per message
- **Pub/sub:** Many subscribers per message

**Examples:** Kafka, RabbitMQ, SQS, NATS.

**Benefits:** Decoupling, buffering, retries.

**Recall:** *Async communication.*

### Event-Driven Architecture

**Definition:** Events as first-class; components react to events.

**Patterns:**
- **Event sourcing:** Store all events; state derived
- **CQRS:** Separate read/write models
- **Saga:** Distributed transactions via events

**Benefits:** Loose coupling, auditability, scalability.

**Recall:** *React to events.*

**SOTA:** Raft variants, CRDTs, serverless, edge computing, blockchain consensus, geo-distributed databases.

---
