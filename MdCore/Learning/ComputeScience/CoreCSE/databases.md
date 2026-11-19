
# 14 · DATABASES

## 14.1 Relational

### DBMS Basics

**Definition:** Database Management System; software for storing, querying, managing data.

**Types:**
- **Relational (SQL):** Tables, rows, columns
- **NoSQL:** Key-value, document, column, graph
- **NewSQL:** Distributed SQL with ACID

**Components:** Query parser, optimizer, executor, storage engine, transaction manager.

**Recall:** *Manage structured data.*

### Relational Model

**Definition:** Data as relations (tables) with tuples (rows) and attributes (columns).

**Keys:**
- **Primary key:** Unique identifier
- **Foreign key:** Reference to another table
- **Candidate key:** Minimal unique identifier
- **Composite key:** Multiple columns

**Codd's rules:** 12 rules for relational DBMS.

**Recall:** *Data as relations.*

### Relational Algebra

**Operations:**
| Symbol | Name | Meaning |
|--------|------|---------|
| σ | Selection | Filter rows |
| π | Projection | Select columns |
| ∪ | Union | Combine |
| ∩ | Intersection | Common |
| − | Difference | Subtract |
| × | Cartesian product | All pairs |
| ⋈ | Join | Combine on condition |

**Foundation:** SQL is based on relational algebra.

**Recall:** *Ops on tables.*

### SQL

**Categories:**
- **DDL:** CREATE, ALTER, DROP
- **DML:** INSERT, UPDATE, DELETE
- **DQL:** SELECT
- **DCL:** GRANT, REVOKE

**Joins:**
- **INNER:** Matching rows
- **LEFT:** All left + matching right
- **RIGHT:** All right + matching left
- **FULL:** All rows
- **CROSS:** Cartesian product

**Aggregates:** COUNT, SUM, AVG, MIN, MAX.

**Window functions:** ROW_NUMBER, RANK, LAG, LEAD.

**Recall:** *Query language for relations.*

### Normalization

**Goal:** Reduce redundancy; prevent anomalies.

**Normal forms:**
| NF | Condition |
|----|-----------|
| 1NF | Atomic values |
| 2NF | 1NF + no partial dependencies |
| 3NF | 2NF + no transitive dependencies |
| BCNF | Every determinant is candidate key |
| 4NF | BCNF + no multi-valued dependencies |

**Trade-off:** Normalization reduces redundancy but may increase joins.

**Recall:** *Eliminate redundancy.*

---

## 14.2 Transactions & Indexing

### ACID

- **Atomicity:** All or nothing
- **Consistency:** Valid state to valid state
- **Isolation:** Concurrent transactions don't interfere
- **Durability:** Committed changes persist

**Recall:** *Transaction guarantees.*

### Transactions

**Isolation levels:**
| Level | Dirty read | Non-repeatable | Phantom |
|-------|------------|----------------|---------|
| Read uncommitted | Yes | Yes | Yes |
| Read committed | No | Yes | Yes |
| Repeatable read | No | No | Yes |
| Serializable | No | No | No |

**Implementation:** 2PL, MVCC, optimistic.

**Recall:** *All-or-nothing unit.*

### Concurrency Control

**2PL (Two-Phase Locking):** Acquire locks in growing phase; release in shrinking phase.

**MVCC (Multi-Version Concurrency Control):** Keep multiple versions; readers don't block writers.

**Optimistic:** Validate at commit; retry on conflict.

**Recall:** *Manage concurrent access.*

### Recovery

**WAL (Write-Ahead Logging):** Log changes before applying.

**ARIES:** Algorithm for recovery; analysis, redo, undo phases.

**Checkpoint:** Periodic save of state.

**Recall:** *Crash recovery.*

### Indexing

**Definition:** Data structure for fast lookup.

**Types:**
- **B-tree:** Balanced; range queries
- **Hash:** Equality lookups
- **Bitmap:** Low-cardinality columns
- **GiST/GIN:** Full-text, geometric

**Trade-off:** Faster reads, slower writes.

**Recall:** *Speed up lookups.*

### B-Trees

**Definition:** Balanced multi-way tree; each node has many keys.

**Properties:**
- All leaves at same depth
- Each node has between ⌈m/2⌉ and m children
- O(log n) operations

**Use:** Databases, file systems.

**Recall:** *Disk-friendly tree.*

### LSM Trees

**Definition:** Log-Structured Merge tree; write-optimized.

**Structure:** Memtable (in-memory) + SSTables (on-disk).

**Process:** Write to memtable; flush to SSTable; merge SSTables.

**Use:** Cassandra, RocksDB, LevelDB.

**Recall:** *Write-heavy storage.*

### Query Planning

**Steps:**
1. Parse SQL → AST
2. Logical plan (relational algebra)
3. Optimize (rule-based, cost-based)
4. Physical plan (execution)
5. Execute

**Cost model:** Estimate I/O, CPU.

**Recall:** *Choose execution strategy.*

### Query Optimization

**Techniques:**
- **Predicate pushdown:** Filter early
- **Join reordering:** Smaller results first
- **Index selection:** Use indexes
- **Projection pushdown:** Select columns early

**Recall:** *Make query fast.*

### Query Execution

**Iterator model:** Volcano; next() returns one tuple.
**Vectorized:** Process batches; SIMD-friendly.
**Compilation:** Generate code for query (HyPer, Vectorwise).

**Recall:** *Execute plan efficiently.*

---

## 14.3 NoSQL & Distributed

### NoSQL

**Definition:** Non-relational databases; flexible schema.

**Types:**
| Type | Model | Examples |
|------|-------|----------|
| Key-value | Key → value | Redis, DynamoDB |
| Document | JSON-like docs | MongoDB, CouchDB |
| Column | Column families | Cassandra, HBase |
| Graph | Nodes + edges | Neo4j, JanusGraph |

**BASE:** Basically Available, Soft state, Eventual consistency.

**Recall:** *Not only SQL.*

### Key-Value Stores

**Model:** Simple map; key → value.

**Examples:** Redis (in-memory), DynamoDB (managed), Riak (distributed).

**Use:** Caching, sessions, feature flags.

**Recall:** *Dict at scale.*

### Document Stores

**Model:** JSON-like documents; flexible schema.

**Examples:** MongoDB, CouchDB, Firestore.

**Use:** Content management, catalogs, user profiles.

**Recall:** *Store documents.*

### Graph Databases

**Model:** Nodes + edges with properties.

**Examples:** Neo4j (Cypher), JanusGraph (Gremlin).

**Use:** Social networks, recommendations, fraud detection.

**Recall:** *Relationships first.*

### Columnar Databases

**Model:** Column-oriented storage; analytics.

**Examples:** ClickHouse, Parquet, Redshift.

**Benefits:** Compression, column pruning, vectorized execution.

**Recall:** *Column-wise for OLAP.*

### Time-Series Databases

**Model:** Optimized for time-stamped data.

**Examples:** InfluxDB, Prometheus, TimescaleDB.

**Use:** Metrics, IoT, monitoring.

**Recall:** *Metrics at scale.*

### Vector Databases

**Model:** Store embeddings; ANN search.

**Examples:** Pinecone, Weaviate, pgvector, Milvus, FAISS.

**Use:** Semantic search, RAG, recommendations.

**Recall:** *Similarity search.*

### Distributed Databases

**Definition:** Database across multiple nodes.

**Examples:** Spanner (Google), CockroachDB, TiDB, YugabyteDB.

**Features:** Sharding, replication, consensus, global consistency.

**Recall:** *DB across nodes.*

---

## 14.4 Analytics & Engineering

### Data Warehousing

**Definition:** Central repository for analytics.

**Schema:** Star (fact + dimensions), snowflake (normalized dimensions).

**Examples:** Redshift, BigQuery, Snowflake.

**Recall:** *Analytics repository.*

### Data Lakes

**Definition:** Raw storage; schema-on-read.

**Examples:** S3 + Parquet + Iceberg/Delta Lake.

**Benefits:** Flexibility, cost.

**Recall:** *Store everything raw.*

### OLTP vs OLAP

| Aspect | OLTP | OLAP |
|--------|------|------|
| Workload | Transactions | Analytics |
| Queries | Simple, many | Complex, few |
| Storage | Row-oriented | Column-oriented |
| Examples | PostgreSQL, MySQL | Redshift, ClickHouse |

**Recall:** *Transactions vs analytics.*

### HTAP

**Definition:** Hybrid Transactional/Analytical Processing; both in one system.

**Examples:** TiDB, SingleStore, YugabyteDB.

**Recall:** *Both workloads.*

### Sharding

**Definition:** Horizontal partitioning across nodes.

**Strategies:** Range, hash, directory.

**Challenges:** Rebalancing, hotspots, cross-shard queries.

**Recall:** *Split rows across nodes.*

### Replication

**Definition:** Copy data across nodes.

**Topologies:** Master-slave, master-master.

**Sync vs async:** Sync waits; async doesn't.

**Recall:** *Copy data.*

### Data Engineering

**Definition:** Building pipelines for data movement and transformation.

**Components:**
- **ETL/ELT:** Extract, transform, load
- **Orchestration:** Airflow, Dagster, Prefect
- **Transformation:** dbt, Spark
- **Streaming:** Kafka, Flink, Spark Streaming

**Recall:** *Move and transform data.*

### Database Internals

**Components:**
- **Buffer pool:** Cache pages in memory
- **Lock manager:** Track locks
- **Log manager:** WAL
- **Storage engine:** Heap, B-tree, LSM

**Recall:** *Under the hood.*

**SOTA:** Vector databases, lakehouse, serverless databases, AI-driven query optimization, selective state-space models for relational deep learning (Ramba achieving O(L) serialization with schema dynamic gating).

---
