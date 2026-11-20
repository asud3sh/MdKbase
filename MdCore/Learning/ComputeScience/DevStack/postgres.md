# **Next chapter — Part 04: PostgreSQL**

[An In-Depth Look at PostgreSQL Architecture | by Yasemin Büşra Karakaş | Medium](https://images.openai.com/static-rsc-4/Bo8tv1bFGM2Qn__ivZWJHuCSt-zVTBC4-6-9KRW52H6_V-7eIxS3Qvu8i1GqVokpKkwH1XQgCGZtUXq_GioGBxcWXk10oEhygfX_TUvEimOWtGoB-p-5lOZoA5OY-3nwBZZJUPdD6YCNdJPwralJm0U-qt1e7_wlBEySZgXrluQ?purpose=inline)

**PostgreSQL — Relational database management and transaction processing**

Deep technical reference

SQL · Database architecture · ACID · Indexes · Transactions

The PostgreSQL chapter will follow the same deep technical approach.

**Planned coverage:**

1. Relational database foundations and relational algebra.
2. PostgreSQL architecture and internal processes.
3. Client-server architecture and connection lifecycle.
4. SQL fundamentals and advanced SQL.
5. Data types, domains, arrays, JSON and JSONB.
6. Schemas, tables, constraints, and relationships.
7. Primary keys, foreign keys, unique constraints, and normalization.
8. Database design and ER modeling.
9. Transactions and ACID properties.
10. Isolation levels, MVCC, and concurrency control.
11. Locks, deadlocks, and transaction management.
12. Indexes: B-tree, Hash, GIN, GiST, BRIN, and expression indexes.
13. Query planner, optimizer, execution plans, and `EXPLAIN ANALYZE`.
14. Joins, subqueries, CTEs, window functions, and recursive queries.
15. Views, materialized views, functions, procedures, and triggers.
16. Partitioning and large-scale data management.
17. WAL, checkpoints, replication, and recovery.
18. Backup, restore, PITR, and high availability.
19. Security, roles, privileges, row-level security, and auditing.
20. Performance tuning and connection pooling.
21. PostgreSQL extensions and ecosystem.
22. Integration with Rust, Python, Django, and SQLAlchemy.
23. Migrations, testing, production deployment, and monitoring.
24. Practical database architecture for payment and transaction systems.

The emphasis will be on understanding PostgreSQL as an actual database engine, not merely learning SQL syntax or using an ORM.

**Sequence continues:** PostgreSQL → Redis → HTML5 → CSS3 → TypeScript → Git → Linux → the concise ecosystem reference for all remaining technologies.

# **PostgreSQL**

Deep technical reference

Relational databases

Transaction processing

[PostgreSQL Architecture | 3 Wonderful Architecture of PostgreSQL](https://images.openai.com/static-rsc-4/A2zNS1ZZoomQkst7fAt4-vDJ29GFCwnByZ1APtp8NwQq4Dn4Ov_FJkF2z1zcvFnIti7QiEt7Rhez3NxcI-buWW-YxP4V5fpJCFOdstTdj4x4mthBv5pu4FAgonjAxge0DQfCeMh6saTguOzTIpuu9ZzwGeS5PP9cBk5yld2y_V8?purpose=inline)

[educba.com](https://www.educba.com/postgresql-architecture/?utm_source=chatgpt.com)

## **1. Introduction: What is PostgreSQL?**

PostgreSQL is an open-source, object-relational database management system (ORDBMS) designed for reliable data storage, querying, concurrency, transactional integrity, extensibility, and large-scale application workloads.

It is commonly used for:

- Financial transaction systems.
- Payment processing platforms.
- Banking and fintech applications.
- Enterprise resource planning systems.
- Web applications and REST APIs.
- Internet of Things platforms.
- Geospatial applications.
- Data analytics and reporting.
- Event and telemetry storage.
- Multi-tenant SaaS applications.
- Scientific and research systems.
- Distributed application backends.

PostgreSQL is more than a relational database that understands SQL. It provides a sophisticated database engine with transaction management, multi-version concurrency control, multiple index types, query optimization, replication, extensible data types, stored procedures, and recovery facilities.

### **1.1 Why PostgreSQL matters**

PostgreSQL is particularly valuable because it combines strong data integrity with considerable flexibility.

| Capability            | Description                                                                                          |
| --------------------- | ---------------------------------------------------------------------------------------------------- |
| Relational data model | Organizes structured data through tables and relationships.                                          |
| ACID transactions     | Provides transactional consistency and durability guarantees under configured operating assumptions. |
| MVCC                  | Supports concurrent readers and writers through multiple data versions.                              |
| Advanced SQL          | Supports joins, CTEs, recursive queries, window functions, and complex aggregations.                 |
| Referential integrity | Enforces relationships using constraints and foreign keys.                                           |
| Extensibility         | Supports custom types, operators, functions, and extensions.                                         |
| JSON support          | Supports JSON and binary-oriented JSONB data.                                                        |
| Indexing              | Provides several index structures for different access patterns.                                     |
| Replication           | Supports physical and logical replication.                                                           |
| Recovery              | Supports WAL, crash recovery, backup, and point-in-time recovery.                                    |
| Security              | Provides roles, privileges, row-level security, and authentication controls.                         |
| Geospatial support    | PostGIS enables advanced spatial storage and querying.                                               |

### **1.2 PostgreSQL as an object-relational database**

A conventional relational model uses tables, rows, columns, keys, and relationships.

PostgreSQL retains this model but supports additional capabilities such as:

- User-defined types.
- Composite types.
- Arrays.
- Range types.
- JSONB.
- Custom operators.
- Function overloading.
- Inheritance.
- Extension-defined data types.

These capabilities allow it to handle both strongly structured business data and more flexible application data.

However, flexibility does not mean every application should store everything in JSONB. Relational modeling, constraints, and well-defined data types remain essential.

## **2. PostgreSQL versions and current release status**

As of **November 19, 2026**, the latest official release announcement available in the PostgreSQL news archive reports:

| Version            | Status                                                     |
| ------------------ | ---------------------------------------------------------- |
| PostgreSQL 19 RC 2 | Release candidate                                          |
| PostgreSQL 18.7    | Latest generally released stable major version             |
| PostgreSQL 17.12   | Supported maintenance release                              |
| PostgreSQL 16.16   | Supported maintenance release                              |
| PostgreSQL 15.20   | Supported maintenance release                              |
| PostgreSQL 14.25   | Final maintenance release; support ended November 12, 2026 |

The PostgreSQL 19 general-availability date was scheduled for November 19, 2026, but the latest official release announcement retrieved still identifies it as RC 2. Therefore, this reference uses PostgreSQL 18 documentation for stable production concepts and treats PostgreSQL 19 as a version to verify before adoption.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+4



**Practical recommendation:** Learn against PostgreSQL 18, understand the migration implications of PostgreSQL 19, and use an officially released, supported production version.

### **2.1 Important PostgreSQL generations**

| Version          | Notable development themes                                                                     |
| ---------------- | ---------------------------------------------------------------------------------------------- |
| PostgreSQL 9.x   | Replication, JSON, major scaling and administration developments.                              |
| PostgreSQL 10–11 | Declarative partitioning, logical replication, stored procedure improvements.                  |
| PostgreSQL 12–13 | More partitioning capabilities, indexing and query improvements.                               |
| PostgreSQL 14–15 | Performance, SQL capabilities, security, and operational improvements.                         |
| PostgreSQL 16    | Query execution, logical replication, monitoring, and parallelism improvements.                |
| PostgreSQL 17    | Vacuum and memory improvements, backup and replication enhancements.                           |
| PostgreSQL 18    | Further performance, I/O, indexing, and monitoring improvements.                               |
| PostgreSQL 19    | Development includes additional maintenance, planner, replication, and execution improvements. |

Useful official references:

- [PostgreSQL documentation](https://www.postgresql.org/docs/?utm_source=chatgpt.com)
- [Release notes](https://www.postgresql.org/docs/current/release.html?utm_source=chatgpt.com)
- [Versioning policy](https://www.postgresql.org/support/versioning/?utm_source=chatgpt.com)

# **3. Database fundamentals**

Before studying PostgreSQL internals, establish the underlying database concepts.

## **3.1 What is a database?**

A database is an organized collection of data that can be stored, queried, modified, and managed.

A database management system (DBMS) provides the mechanisms to manage that data.

A DBMS typically handles:

- Data storage.
- Query execution.
- Concurrency.
- Consistency.
- Access control.
- Transaction management.
- Recovery.
- Backup.
- Indexing.
- Data integrity.

### **Types of database systems**

| Database category | Data model                     | Examples                 |
| ----------------- | ------------------------------ | ------------------------ |
| Relational        | Tables and relationships       | PostgreSQL, MySQL        |
| Key-value         | Keys mapped to values          | Redis                    |
| Document          | JSON-like documents            | MongoDB                  |
| Wide-column       | Column-family storage          | Cassandra                |
| Graph             | Nodes and relationships        | Neo4j                    |
| Time-series       | Timestamp-oriented records     | TimescaleDB, InfluxDB    |
| Search engine     | Inverted-index-oriented search | Elasticsearch            |
| Object database   | Persistent objects             | Specialized object DBMSs |

PostgreSQL primarily uses a relational model but has extensibility that supports some document-oriented, geospatial, time-series, and analytical workloads.

## **3.2 Database versus DBMS versus database server**

These terms are related but not identical.

**Database**

The organized data itself.

**DBMS**

The software that manages databases.

**Database server**

The running service/process accepting database connections and executing requests.

**Database client**

The software that communicates with the server.

Examples:

```
Client:
    Python / Rust / psql / DBeaver

       |
       | SQL protocol
       v

PostgreSQL server
       |
       +---- Database A
       |
       +---- Database B
       |
       +---- Database C
```

## **3.3 SQL**

SQL (Structured Query Language) is the primary language used to define, query, and manipulate relational databases.

SQL has several major command categories:

| Category | Full name                    | Purpose                                                             |
| -------- | ---------------------------- | ------------------------------------------------------------------- |
| DDL      | Data Definition Language     | Define database objects.                                            |
| DML      | Data Manipulation Language   | Insert and modify data.                                             |
| DQL      | Data Query Language          | Retrieve data; commonly classified within DML in formal treatments. |
| DCL      | Data Control Language        | Control access privileges.                                          |
| TCL      | Transaction Control Language | Control transactions.                                               |

Examples:

**DDL**

```
CREATE TABLE users (...);

ALTER TABLE users ...;

DROP TABLE users;
```

**DML**

```
INSERT INTO users ...;

UPDATE users ...;

DELETE FROM users ...;
```

**Querying**

```
SELECT * FROM users;
```

**DCL**

```
GRANT SELECT ON users TO reporting_user;

REVOKE SELECT ON users FROM reporting_user;
```

**TCL**

```
BEGIN;

COMMIT;

ROLLBACK;
```

# **4. Relational database theory**

Essential foundation

PostgreSQL is built around the relational database model, so understanding relational theory will make database design and SQL behavior much easier to reason about.

## **4.1 Relation**

A relation is a mathematical representation of a set of tuples over attributes.

In practical SQL terminology, a table approximates this concept.

Consider:

**users**

| user_id | name  | email              |
| ------- | ----- | ------------------ |
| 1       | Alice | alice\@example.com |
| 2       | Bob   | bob\@example.com   |
| 3       | Carol | carol\@example.com |

The table has:

- A relation or table name.
- Attributes or columns.
- Tuples or rows.
- Domains describing permitted values.

A SQL table can contain duplicate rows unless constraints prevent them, so it is not exactly identical to the pure mathematical relation model.

## **4.2 Attribute**

An attribute is a named property of a relation.

For `users`:

- `user_id`
- `name`
- `email`

In SQL, attributes are columns.

## **4.3 Tuple**

A tuple represents one collection of attribute values.

In SQL, this is a row.

Example:

```
(1, 'Alice', 'alice@example.com')
```

## **4.4 Domain**

A domain describes the permitted values of an attribute.

For example:

```
amount:
    positive decimal values

currency:
    approved currency codes

status:
    SUCCESS | FAILED | PENDING
```

PostgreSQL can enforce domains through:

- Data types.
- `NOT NULL`.
- `CHECK`.
- `ENUM`.
- `CREATE DOMAIN`.
- Foreign keys.
- Unique constraints.

## **4.5 Keys**

Keys identify rows and represent relationships.

### **Superkey**

Any set of columns that uniquely identifies a row.

### **Candidate key**

A minimal superkey.

### **Primary key**

The selected candidate key used as the table's principal identifier.

### **Alternate key**

A candidate key that is not selected as the primary key.

### **Foreign key**

A column or set of columns referencing a key in another table, or in certain designs the same table.

### **Composite key**

A key formed from multiple columns.

Example:

```
CREATE TABLE merchant_terminal (
    merchant_id BIGINT NOT NULL,
    terminal_id BIGINT NOT NULL,

    PRIMARY KEY (merchant_id, terminal_id)
);
```

Both columns together identify a row.

## **4.6 Integrity constraints**

Database integrity is the property that stored data satisfies defined correctness rules.

Five useful categories:

| Constraint            | Purpose                                                 |
| --------------------- | ------------------------------------------------------- |
| Domain integrity      | Values must conform to permitted data types and ranges. |
| Entity integrity      | Rows have valid identifiers.                            |
| Referential integrity | Relationships reference valid records.                  |
| Uniqueness integrity  | Defined unique values are not duplicated.               |
| Business integrity    | Data satisfies application-specific rules.              |

Example:

```
CREATE TABLE merchants (
    merchant_id BIGINT PRIMARY KEY,

    merchant_name TEXT NOT NULL,

    email TEXT UNIQUE,

    status TEXT NOT NULL
        CHECK (status IN ('ACTIVE', 'SUSPENDED', 'CLOSED'))
);
```

Database constraints protect the integrity of data even when multiple applications or services access the same database.

**Important engineering principle:** Critical integrity rules should not exist only in frontend validation or application code. Enforce appropriate invariants at the database boundary as well.

# **5. Relational algebra and relational operations**

Relational algebra provides a theoretical foundation for understanding how database queries manipulate relations.

PostgreSQL translates SQL into an internal query representation and execution plan. A fundamental knowledge of relational operators helps you reason about query design and optimization.

## **5.1 Selection**

Selection filters rows based on a condition.

Mathematically:

\\[ \sigma\_{\text{condition}}(R) \\]

SQL:

```
SELECT *
FROM transactions
WHERE status = 'SUCCESS';
```

This selects rows satisfying the condition.

## **5.2 Projection**

Projection selects particular attributes.

\\[ \pi\_{\text{attributes}}(R) \\]

SQL:

```
SELECT transaction_id, amount
FROM transactions;
```

## **5.3 Union**

Union combines compatible relations while removing duplicate rows.

```
SELECT merchant_id FROM online_merchants

UNION

SELECT merchant_id FROM physical_merchants;
```

`UNION ALL` retains duplicates and typically avoids the extra duplicate-elimination step.

## **5.4 Difference**

Returns rows present in the first relation but not the second.

```
SELECT merchant_id FROM merchants

EXCEPT

SELECT merchant_id FROM suspended_merchants;
```

## **5.5 Intersection**

Returns rows present in both relations.

```
SELECT merchant_id FROM online_merchants

INTERSECT

SELECT merchant_id FROM active_merchants;
```

## **5.6 Cartesian product**

Combines every row of one relation with every row of another relation.

SQL:

```
SELECT *
FROM products
CROSS JOIN regions;
```

If the first relation has \\(m\\) rows and the second has \\(n\\) rows:

\\[ \text{Result cardinality} = m \times n \\]

This can create extremely large results when used without care.

## **5.7 Join**

A join combines related records.

```
SELECT
    t.transaction_id,
    m.merchant_name
FROM transactions t
JOIN merchants m
    ON t.merchant_id = m.merchant_id;
```

Joins are one of the most important capabilities in relational database systems.

# **6. PostgreSQL architecture**

Essential advanced concept

To understand PostgreSQL properly, think of it as a server engine comprising several collaborating subsystems.

These include:

- Connection management.
- Authentication.
- Query parsing.
- Query analysis and rewriting.
- Query optimization.
- Query execution.
- Shared memory and buffer management.
- Transaction processing.
- WAL generation.
- Background maintenance.
- Storage management.
- Replication.
- Crash recovery.

## **6.1 PostgreSQL high-level architecture**

[What is PostgreSQL: Architecture, Comparison, and Installation | Ramotion Agency](https://images.openai.com/static-rsc-4/FwlAC273MOwcZYkg3wVJdgYD1o8igBV8m1IpsfIwV40lppy5044yd0h31i3i0KCRWGsiRURa6cfSQ4beZQvMSX_i_xCPZouwEMca7WccTtJRRTjbO4UypM63ee71-_o5V_isa1aqeojaidcgJKxaJhYs2ofza3XKw3L5A8RKGCE?purpose=inline)

[ramotion.com](https://www.ramotion.com/blog/what-is-postgresql/?utm_source=chatgpt.com)

Conceptual architecture:

**Client applications**

Python · Rust · psql · JDBC · application servers

PostgreSQL wire protocol

**PostgreSQL connection handling**

Authentication · Session · Backend process

**Query processing**

Parser · Analyzer · Rewriter · Planner · Executor

**Shared memory**

Shared buffers · WAL buffers · Locks · Statistics

**Background processes**

WAL writer · Checkpointer · Autovacuum · Background writer

**Persistent storage**

Heap files · Indexes · WAL segments · Catalogs · Temporary files

The exact internal process architecture and execution details vary by PostgreSQL version and platform, but this is a useful general model.

## **6.2 Client-server architecture**

PostgreSQL uses a client-server architecture.

A client connects to a PostgreSQL server, authenticates, sends SQL statements, and receives responses.

Typical clients:

- `psql`.
- Python applications.
- Rust applications.
- Database GUI clients.
- BI tools.
- Framework database adapters.

Common communication:

```
Python Django Application
          |
          | PostgreSQL protocol
          v
PostgreSQL Server
          |
          v
Application Database
```

The server listens on a configured TCP port, traditionally `5432`, although deployment configurations can use other ports.

### **6.3 PostgreSQL connection lifecycle**

A conventional connection workflow:

1. Client opens a TCP connection.
2. Server handles the incoming connection.
3. Authentication negotiation occurs.
4. PostgreSQL creates or assigns the session backend.
5. Client sends a query.
6. Server parses, plans, and executes it.
7. Results are returned.
8. The session remains connected until closed or disconnected.

PostgreSQL commonly uses a process-per-connection architecture.

That means a large number of direct database connections introduces memory and process-management overhead.

This is one reason connection pooling is important.

### **6.4 Backend process**

A backend process handles a database session and executes its commands.

The process typically owns:

- Session-specific state.
- Query execution state.
- Local memory contexts.
- Transaction context.
- Temporary execution structures.

Multiple backend processes can operate simultaneously.

### **6.5 Shared memory**

PostgreSQL also uses shared memory for information that needs to be accessed by multiple server processes.

Important shared memory areas include:

- Shared buffers.
- WAL buffers.
- Lock-management structures.
- Transaction information.
- Shared statistics and other internal structures.

### **6.6 Shared buffers**

Shared buffers are PostgreSQL's internal cache for data pages.

When a query needs a page, PostgreSQL may:

1. Find it in shared buffers.
2. Read it from disk if it is not cached.
3. Store it in shared buffers.
4. Access the required records.

This reduces repeated physical disk access.

Important distinction: shared buffers are not the only cache involved. The operating system's filesystem cache also plays a substantial role.

### **6.7 Background processes**

Important PostgreSQL background processes include:

| Process                     | Purpose                                               |
| --------------------------- | ----------------------------------------------------- |
| Checkpointer                | Coordinates checkpoint processing.                    |
| Background writer           | Writes dirty shared-buffer pages progressively.       |
| WAL writer                  | Writes WAL data.                                      |
| Autovacuum launcher         | Coordinates autovacuum workers.                       |
| Autovacuum workers          | Perform vacuuming and statistics collection.          |
| Startup process             | Performs crash recovery and standby replay functions. |
| WAL sender                  | Sends WAL data to standbys or logical subscribers.    |
| WAL receiver                | Receives WAL data on a standby.                       |
| Logical replication workers | Execute logical replication tasks.                    |
| Archiver                    | Archives completed WAL segments when configured.      |

Not every installation has every process active at all times.

# **7. PostgreSQL query processing pipeline**

Understanding this pipeline is essential for SQL performance analysis.

When a client submits a query, PostgreSQL does not immediately execute the SQL text.

It transforms the query into internal representations and selects an execution plan.

## **7.1 The query lifecycle**

**SQL statement**

SELECT · INSERT · UPDATE · DELETE

**Parser**

Syntax checking and parse tree generation

**Parse analysis and transformation**

Name resolution · Type resolution · Semantic checking

**Query rewrite**

Views · Rules · Query transformations

**Planner / Optimizer**

Candidate plans · Join strategies · Index selection · Cost estimation

**Executor**

Scan · Filter · Join · Sort · Aggregate · Modify

**Result**

Rows · Command status · Errors

## **7.2 Parsing**

The parser checks SQL syntax and builds a parse tree.

Example:

```
SELECT transaction_id, amount
FROM transactions
WHERE status = 'SUCCESS';
```

At this stage, PostgreSQL identifies the statement's syntactic structure.

Invalid syntax produces a syntax error.

## **7.3 Parse analysis**

PostgreSQL resolves:

- Table references.
- Column names.
- Data types.
- Operators.
- Functions.
- Ambiguous identifiers.
- Expression semantics.

Example:

```
SELECT amount + '10'
FROM transactions;
```

The database must resolve how the string literal participates in the arithmetic expression.

## **7.4 Query rewriting**

The rewrite system can transform queries based on rules and view definitions.

For example, querying a view causes PostgreSQL to expand its underlying query structure.

## **7.5 Query planning and optimization**

The planner evaluates possible execution strategies.

For a join, it may consider:

- Nested loop join.
- Hash join.
- Merge join.

For table access, it may consider:

- Sequential scan.
- Index scan.
- Index-only scan.
- Bitmap heap scan.

It evaluates estimated costs based on:

- Table size.
- Available indexes.
- Statistics.
- Selectivity.
- Join relationships.
- Expected memory and I/O costs.

The selected plan is the planner's estimate of the cheapest suitable execution strategy, not a guarantee of the fastest measured result.

## **7.6 Query execution**

The executor runs the selected plan using execution nodes.

Each node performs part of the work.

Example:

```
SELECT *
FROM transactions
WHERE amount > 10000;
```

An execution plan might contain:

```
Seq Scan on transactions
  Filter: amount > 10000
```

The engine scans the relevant table pages and filters rows.

## **7.7 Why query understanding matters**

Two SQL statements that produce identical results can have substantially different performance.

Similarly, two identical SQL statements can perform differently as:

- Table sizes change.
- Data distribution changes.
- Indexes change.
- Statistics become stale.
- Cache state changes.
- PostgreSQL versions change.
- Configuration changes.

This is why query analysis is a continuing database-engineering responsibility.

# **8. PostgreSQL data types**

Essential technical reference

Choosing correct data types is one of the most important decisions in relational database design.

A database type determines:

- Which values are accepted.
- How data is represented and stored.
- Which operations are valid.
- Which comparison semantics apply.
- How values are indexed.
- How applications exchange data with the database.

PostgreSQL provides extensive built-in data types.

## **8.1 Numeric types**

| Type               | Description                                      | Typical application                            |
| ------------------ | ------------------------------------------------ | ---------------------------------------------- |
| `smallint`         | 2-byte signed integer                            | Small counters and enumerated numeric values.  |
| `integer` / `int`  | 4-byte signed integer                            | General identifiers and counters within range. |
| `bigint`           | 8-byte signed integer                            | Large identifiers and counters.                |
| `numeric`          | Exact decimal representation                     | Monetary and precision-sensitive calculations. |
| `decimal`          | Alias for numeric                                | Exact decimal representation.                  |
| `real`             | 4-byte floating point                            | Approximate scientific values.                 |
| `double precision` | 8-byte floating point                            | Approximate numerical computations.            |
| `smallserial`      | Auto-incrementing small integer convenience type | Small internal identifiers.                    |
| `serial`           | Auto-incrementing integer convenience type       | Traditional surrogate identifiers.             |
| `bigserial`        | Auto-incrementing big integer convenience type   | Large surrogate identifiers.                   |

### **8.1.1 Integer types**

PostgreSQL integer ranges:

| Type       | Storage | Range                           |
| ---------- | ------- | ------------------------------- |
| `smallint` | 2 bytes | -32,768 to 32,767               |
| `integer`  | 4 bytes | -2,147,483,648 to 2,147,483,647 |
| `bigint`   | 8 bytes | Approximately ±9.22 quintillion |

Example:

```
CREATE TABLE device_counters (
    device_id BIGINT PRIMARY KEY,
    transaction_count BIGINT NOT NULL DEFAULT 0
);
```

Use the smallest suitable type that satisfies the actual business and future growth constraints.

Do not select `bigint` for every column without considering storage, index size, and data-domain requirements.

### **8.1.2 Numeric and decimal**

For financial applications, `numeric` is particularly important.

```
CREATE TABLE account_balances (
    account_id BIGINT PRIMARY KEY,

    balance NUMERIC(20, 4) NOT NULL
);
```

`NUMERIC(20,4)` represents up to 20 significant decimal digits in total, with four digits after the decimal point.

Example:

```
INSERT INTO account_balances
VALUES (1, 123456.7890);
```

Advantages:

- Exact decimal representation for values within the defined precision.
- Configurable precision and scale.
- Suitable for decimal business calculations.

Trade-offs:

- More computationally expensive than ordinary machine integer arithmetic.
- Larger storage overhead in many cases.
- Requires a deliberate rounding policy.

### **8.1.3 Financial representation: integer minor units versus NUMERIC**

Two common designs:

**Integer minor units**

```
amount_minor_units BIGINT NOT NULL
```

Example:

```
USD 125.50 → 12550
```

Advantages:

- Straightforward exact integer arithmetic.
- Predictable storage.
- Convenient for payment protocol amounts with fixed currency scale.

Limitations:

- Currency exponents vary.
- Fractional calculations may require explicit rounding.
- The application must know the currency's scaling convention.

**NUMERIC**

```
amount NUMERIC(20, 4) NOT NULL
```

Advantages:

- Decimal arithmetic.
- Supports multiple precision requirements.
- Natural representation of fractional amounts.

Limitations:

- More expensive than fixed-width integer arithmetic.
- Requires carefully defined scale and rounding semantics.

For a multi-currency financial platform, select an explicit monetary model and store currency information alongside amounts.

### **8.1.4 Floating-point numbers**

```
temperature REAL;

measurement DOUBLE PRECISION;
```

Floating-point types are appropriate for approximate numerical values.

They are not generally suitable for exact monetary storage.

Example:

```
SELECT 0.1::double precision + 0.2::double precision;
```

Binary floating-point representation may introduce small precision differences.

### **8.1.5 Serial versus identity columns**

`serial` is a convenient historical construct that creates an integer column with a sequence-backed default.

```
CREATE TABLE users (
    user_id BIGSERIAL PRIMARY KEY
);
```

Modern PostgreSQL supports SQL-standard identity columns:

```
CREATE TABLE users (
    user_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY
);
```

Identity options:

```
GENERATED ALWAYS AS IDENTITY
```

and:

```
GENERATED BY DEFAULT AS IDENTITY
```

The distinction concerns whether an explicitly supplied value is permitted by default.

For new designs, identity columns are generally preferable to the legacy serial shorthand.

An identity column does not automatically enforce uniqueness; a `PRIMARY KEY` or `UNIQUE` constraint is still needed.

## **8.2 Character and text types**

| Type         | Purpose                                                  |
| ------------ | -------------------------------------------------------- |
| `char(n)`    | Fixed-length character field, blank-padded.              |
| `varchar(n)` | Variable-length string with a maximum character limit.   |
| `text`       | Variable-length text without a declared character limit. |

Example:

```
CREATE TABLE merchants (
    merchant_id BIGINT PRIMARY KEY,

    merchant_name VARCHAR(150) NOT NULL,

    description TEXT
);
```

### `char`**&#x20;versus&#x20;**`varchar`**&#x20;versus&#x20;**`text`

`char(n)` is not a general performance optimization.

For most application text, `text` or `varchar` is more practical.

Use bounded `varchar` when there is a meaningful domain limit, and use `CHECK` constraints where additional content validation is required.

## **8.3 Boolean**

```
CREATE TABLE users (
    user_id BIGINT PRIMARY KEY,

    active BOOLEAN NOT NULL DEFAULT TRUE
);
```

Possible values:

```
TRUE
FALSE
NULL
```

Remember that a Boolean column can represent three logical states when it allows `NULL`.

Do not use nullable Boolean fields unintentionally.

## **8.4 Date and time types**

| Type                  | Description                            |
| --------------------- | -------------------------------------- |
| `date`                | Calendar date.                         |
| `time`                | Time of day without timezone.          |
| `time with time zone` | Time of day with timezone information. |
| `timestamp`           | Timestamp without timezone.            |
| `timestamptz`         | Timestamp with time zone semantics.    |
| `interval`            | Duration or time interval.             |

### **Date**

```
CREATE TABLE holidays (
    holiday_date DATE PRIMARY KEY
);
```

### **Timestamp without timezone**

```
created_at TIMESTAMP NOT NULL;
```

Represents a calendar date and time without a timezone interpretation.

It does not identify an unambiguous global instant.

### **Timestamp with time zone**

```
created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP;
```

PostgreSQL normalizes the instant internally and renders it according to the session timezone.

Important distinction: PostgreSQL does not retain the original timezone identifier supplied with a `timestamptz` value.

For financial transaction systems, a useful convention is:

- Store transaction instants using `timestamptz`.
- Use UTC for application and storage conventions.
- Preserve the relevant business timezone separately where needed.
- Distinguish settlement dates from timestamps.
- Define idempotent and auditable event-time semantics.

### **Current time expressions**

```
SELECT CURRENT_DATE;

SELECT CURRENT_TIME;

SELECT CURRENT_TIMESTAMP;

SELECT NOW();

SELECT CLOCK_TIMESTAMP();
```

A subtle but important distinction:

- `CURRENT_TIMESTAMP` and `NOW()` represent the transaction start timestamp.
- `CLOCK_TIMESTAMP()` returns the actual current wall-clock time at the moment it is called.

This matters when measuring elapsed time inside a long-running transaction.

## **8.5 UUID**

UUID means Universally Unique Identifier.

PostgreSQL supports the `uuid` data type.

```
CREATE TABLE payment_requests (
    request_id UUID PRIMARY KEY,

    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

Generate:

```
SELECT gen_random_uuid();
```

UUID is useful for:

- Distributed identifiers.
- Public resource identifiers.
- Globally unique record references.
- Cross-system correlation.

UUID versions have different generation characteristics.

Be deliberate about index locality, predictability, and identifier exposure.

PostgreSQL 18 also includes native UUIDv7 generation support; verify the precise function available in your target major version before depending on it. [PostgreSQL UUID documentation](https://www.postgresql.org/docs/18/datatype-uuid.html?utm_source=chatgpt.com).

## **8.6 Binary data**

PostgreSQL provides:

```
BYTEA
```

Example:

```
CREATE TABLE firmware_artifacts (
    artifact_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    firmware_data BYTEA NOT NULL
);
```

`BYTEA` stores binary strings.

Applications can store:

- Binary documents.
- Images.
- Protocol payloads.
- Encrypted blobs.
- Firmware data.

For very large file storage, external object storage with database metadata is often preferable to placing all file contents directly in the relational database.

## **8.7 JSON and JSONB**

PostgreSQL supports two JSON-related types:

```
JSON
JSONB
```

### **JSON**

- Stores JSON text representation.
- Preserves the original textual representation.
- Requires processing to inspect structured content.

### **JSONB**

- Stores a decomposed binary representation.
- Does not preserve formatting or object-key order.
- Supports more efficient structural querying.
- Supports specialized indexing.

Example:

```
CREATE TABLE device_events (
    event_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    payload JSONB NOT NULL
);
```

Insert:

```
INSERT INTO device_events(payload)
VALUES (
    '{
        "device_id": "VB90-001",
        "status": "ONLINE",
        "temperature": 32.5
    }'
);
```

Access fields:

```
SELECT payload ->> 'device_id'
FROM device_events;
```

Difference between operators:

| Operator | Result                                |
| -------- | ------------------------------------- |
| `->`     | JSON or JSONB value.                  |
| `->>`    | Text value.                           |
| `#>`     | Nested JSON/JSONB value using a path. |
| `#>>`    | Nested text value using a path.       |

Example:

```
SELECT payload -> 'temperature'
FROM device_events;
```

```
SELECT payload ->> 'temperature'
FROM device_events;
```

### **JSONB containment**

```
SELECT *
FROM device_events
WHERE payload @> '{"status": "ONLINE"}';
```

### **JSONB indexing**

```
CREATE INDEX idx_device_events_payload
ON device_events
USING GIN (payload);
```

JSONB is extremely useful for event metadata and flexible attributes, but frequently queried business fields should generally be modeled as ordinary columns where constraints and relational semantics are important.

## **8.8 Arrays**

PostgreSQL supports array types.

```
CREATE TABLE devices (
    device_id BIGINT PRIMARY KEY,

    supported_protocols TEXT[]
);
```

Insert:

```
INSERT INTO devices
VALUES (
    1,
    ARRAY['MQTT', 'HTTP', 'HTTPS']
);
```

Array operations:

```
SELECT *
FROM devices
WHERE 'MQTT' = ANY(supported_protocols);
```

Containment:

```
SELECT *
FROM devices
WHERE supported_protocols @> ARRAY['MQTT'];
```

GIN indexes can help with appropriate array queries.

However, arrays should not become a substitute for proper many-to-many relational modeling when their elements need independent identities, relationships, constraints, or frequent updates.

## **8.9 Range types**

Range types represent intervals.

Common built-in types:

- `int4range`
- `int8range`
- `numrange`
- `tsrange`
- `tstzrange`
- `daterange`

Example:

```
CREATE TABLE maintenance_windows (
    window_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    scheduled_period TSTZRANGE NOT NULL
);
```

Insert:

```
INSERT INTO maintenance_windows(scheduled_period)
VALUES (
    tstzrange(
        '2026-12-01 10:00+05:45',
        '2026-12-01 11:00+05:45',
        '[)'
    )
);
```

Range operators are useful for:

- Scheduling.
- Booking.
- Time-window matching.
- Temporal validity.
- Resource availability.

## **8.10 Enumerated types**

PostgreSQL supports user-defined enum types.

```
CREATE TYPE transaction_status AS ENUM (
    'PENDING',
    'AUTHORIZED',
    'SUCCESS',
    'FAILED',
    'REVERSED'
);
```

Use it:

```
CREATE TABLE transactions (
    transaction_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    status transaction_status NOT NULL
);
```

Advantages:

- Restricts accepted values.
- Provides a clear database-level domain.
- Supports named semantic values.

Limitations:

- Adding new values requires schema changes.
- Removing or reordering values is more complicated.
- Direct use in external APIs requires careful versioning.

For frequently changing workflow states, a reference table with explicit transition rules can offer more flexibility.

## **8.11 Domains**

A domain creates a reusable constrained type.

```
CREATE DOMAIN positive_amount AS BIGINT
CHECK (VALUE > 0);
```

Use:

```
CREATE TABLE transactions (
    transaction_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    amount positive_amount NOT NULL
);
```

Domains can improve consistency when multiple tables share the same validation rule.

## **8.12 Composite types**

PostgreSQL allows user-defined composite types.

```
CREATE TYPE address_type AS (
    street TEXT,
    city TEXT,
    postal_code TEXT
);
```

Use:

```
CREATE TABLE merchants (
    merchant_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    business_address address_type
);
```

Composite types are useful for structured database functionality, although ordinary relational tables are often more appropriate when individual attributes must be independently constrained and indexed.

# **9. Schemas, tables, and database organization**

PostgreSQL provides multiple levels of organization.

Conceptually:

```
PostgreSQL Cluster
    |
    +---- Database: payment_platform
    |         |
    |         +---- Schema: core
    |         |       |
    |         |       +---- merchants
    |         |       +---- terminals
    |         |       +---- transactions
    |         |
    |         +---- Schema: reporting
    |                 |
    |                 +---- daily_reports
    |
    +---- Database: analytics
              |
              +---- Schema: public
                      |
                      +---- model_features
```

## **9.1 Cluster**

A PostgreSQL cluster refers to the collection of databases and shared server-managed storage managed by a PostgreSQL server instance.

It should not be confused with a distributed PostgreSQL cluster in the general high-availability sense.

## **9.2 Database**

Create:

```
CREATE DATABASE payment_platform;
```

Connect:

```
\c payment_platform
```

Each database has separate catalogs and logical database contents, though it shares the same server instance and its cluster-wide resources.

## **9.3 Schema**

Schemas organize tables and other objects inside a database.

Create:

```
CREATE SCHEMA payment;
```

Use:

```
CREATE TABLE payment.transactions (
    transaction_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    amount BIGINT NOT NULL
);
```

Query:

```
SELECT *
FROM payment.transactions;
```

### **9.4 The public schema**

PostgreSQL databases commonly include a `public` schema.

Applications need not place all objects in `public`.

A deliberate schema organization helps separate responsibilities:

```
core
payment
merchant
terminal
audit
reporting
integration
```

Avoid creating a large number of schemas without a clear organizational or security purpose.

## **9.5 Search path**

PostgreSQL uses a search path to resolve unqualified object names.

Inspect:

```
SHOW search_path;
```

Set:

```
SET search_path TO payment, public;
```

For security-sensitive services, be cautious with writable schemas in the search path.

Schema resolution can affect which function or object a statement references.

Prefer schema-qualifying important application objects, especially in sensitive database code.

# **10. SQL DDL — Creating and modifying database structures**

DDL operations define database objects.

The most important operations include:

- `CREATE`
- `ALTER`
- `DROP`
- `TRUNCATE`

## **10.1 CREATE TABLE**

Example: merchant management.

```
CREATE TABLE merchants (
    merchant_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    merchant_code VARCHAR(50) NOT NULL UNIQUE,

    merchant_name VARCHAR(150) NOT NULL,

    email VARCHAR(254),

    status VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### **10.2 CREATE TABLE with constraints**

```
CREATE TABLE terminals (
    terminal_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    merchant_id BIGINT NOT NULL,

    serial_number VARCHAR(100) NOT NULL UNIQUE,

    terminal_model VARCHAR(50) NOT NULL,

    status VARCHAR(20) NOT NULL DEFAULT 'ACTIVE',

    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_terminal_merchant
        FOREIGN KEY (merchant_id)
        REFERENCES merchants(merchant_id)
        ON DELETE RESTRICT,

    CONSTRAINT chk_terminal_status
        CHECK (
            status IN (
                'ACTIVE',
                'INACTIVE',
                'SUSPENDED',
                'RETIRED'
            )
        )
);
```

### **10.3 ALTER TABLE**

Add a column:

```
ALTER TABLE merchants
ADD COLUMN phone_number VARCHAR(30);
```

Change a column type:

```
ALTER TABLE merchants
ALTER COLUMN merchant_name TYPE VARCHAR(200);
```

Change a default:

```
ALTER TABLE merchants
ALTER COLUMN status SET DEFAULT 'PENDING';
```

Drop a default:

```
ALTER TABLE merchants
ALTER COLUMN status DROP DEFAULT;
```

Set not null:

```
ALTER TABLE merchants
ALTER COLUMN merchant_name SET NOT NULL;
```

Drop a column:

```
ALTER TABLE merchants
DROP COLUMN phone_number;
```

Change a constraint:

```
ALTER TABLE merchants
ADD CONSTRAINT chk_merchant_name
CHECK (length(trim(merchant_name)) > 0);
```

### **10.4 DROP**

```
DROP TABLE terminals;
```

With dependent objects:

```
DROP TABLE merchants CASCADE;
```

`CASCADE` can remove dependent objects. Use it very carefully, especially in production.

### **10.5 TRUNCATE**

```
TRUNCATE TABLE transactions;
```

This removes all rows using a table-level operation rather than a row-by-row `DELETE` workflow.

Other options:

```
TRUNCATE TABLE transactions
RESTART IDENTITY;
```

```
TRUNCATE TABLE transactions
CASCADE;
```

`TRUNCATE` has locking and referential-integrity implications. It is not a substitute for a carefully controlled production data-deletion process.

## **10.6 Temporary tables**

```
CREATE TEMP TABLE transaction_batch (
    transaction_id BIGINT,
    amount BIGINT
);
```

Temporary tables exist within the session and are generally dropped automatically at session end.

They are useful for:

- Intermediate calculations.
- Data transformations.
- Bulk processing.
- Staging workflows.

## **10.7 Unlogged tables**

```
CREATE UNLOGGED TABLE staging_events (
    event_id BIGINT,
    payload JSONB
);
```

Unlogged tables reduce WAL overhead for certain workloads.

They are not crash-safe in the same manner as logged tables and are not replicated through normal physical or logical replication behavior as ordinary logged data.

Use them only for reconstructable or disposable data where the durability trade-off is understood.

# **11. SQL DML — Working with records**

## **11.1 INSERT**

```
INSERT INTO merchants (
    merchant_code,
    merchant_name,
    status
)
VALUES (
    'MERCHANT-001',
    'Example Store',
    'ACTIVE'
);
```

### **Insert multiple rows**

```
INSERT INTO merchants (
    merchant_code,
    merchant_name
)
VALUES
    ('M001', 'Merchant One'),
    ('M002', 'Merchant Two'),
    ('M003', 'Merchant Three');
```

### **Returning inserted records**

```
INSERT INTO merchants (
    merchant_code,
    merchant_name
)
VALUES (
    'M004',
    'Merchant Four'
)
RETURNING merchant_id, merchant_code;
```

`RETURNING` is particularly useful for retrieving generated identifiers without a separate query.

## **11.2 UPDATE**

```
UPDATE merchants
SET status = 'SUSPENDED'
WHERE merchant_id = 1;
```

Update multiple fields:

```
UPDATE merchants
SET
    status = 'ACTIVE',
    merchant_name = 'Updated Store'
WHERE merchant_id = 1;
```

Return modified data:

```
UPDATE merchants
SET status = 'ACTIVE'
WHERE merchant_id = 1
RETURNING *;
```

**Critical practice:** Carefully check the `WHERE` condition before executing production updates.

An update without a filter affects every row.

## **11.3 DELETE**

```
DELETE FROM merchants
WHERE merchant_id = 1;
```

Return deleted rows:

```
DELETE FROM merchants
WHERE merchant_id = 1
RETURNING *;
```

Consider foreign keys and deletion policies before deleting parent entities.

## **11.4 UPSERT**

PostgreSQL supports conflict-aware inserts.

```
INSERT INTO merchants (
    merchant_code,
    merchant_name
)
VALUES (
    'M001',
    'Updated Merchant'
)
ON CONFLICT (merchant_code)
DO UPDATE
SET merchant_name = EXCLUDED.merchant_name;
```

`EXCLUDED` refers to the proposed row that could not be inserted because of a conflict.

Alternative:

```
INSERT INTO merchants (
    merchant_code,
    merchant_name
)
VALUES (
    'M002',
    'New Merchant'
)
ON CONFLICT (merchant_code)
DO NOTHING;
```

UPSERT is useful for:

- Import pipelines.
- Synchronization.
- Idempotent data insertion.
- Device registration.
- Controlled update workflows.

However, UPSERT does not automatically implement a complete business idempotency mechanism. Financial operations still need appropriate unique keys, lifecycle tracking, and transaction semantics.

## **11.5 MERGE**

PostgreSQL supports SQL `MERGE`.

```
MERGE INTO merchants AS target
USING merchant_staging AS source
ON target.merchant_code = source.merchant_code

WHEN MATCHED THEN
    UPDATE SET
        merchant_name = source.merchant_name

WHEN NOT MATCHED THEN
    INSERT (
        merchant_code,
        merchant_name
    )
    VALUES (
        source.merchant_code,
        source.merchant_name
    );
```

`MERGE` is helpful for set-based synchronization and data integration.

Understand its concurrency and uniqueness constraints rather than assuming it replaces every use of `INSERT ... ON CONFLICT`.

# **12. SQL SELECT — Querying data**

`SELECT` is one of the most frequently used SQL commands.

A query may contain:

```
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
OFFSET;
```

Conceptually, the logical processing order differs from the textual order.

An important simplified logical sequence is:

```
FROM / JOIN
      |
WHERE
      |
GROUP BY
      |
HAVING
      |
SELECT
      |
DISTINCT
      |
ORDER BY
      |
LIMIT / OFFSET
```

Actual PostgreSQL execution is planned and optimized, so the physical execution order does not need to follow this sequence.

## **12.1 Basic SELECT**

```
SELECT *
FROM transactions;
```

Explicit columns:

```
SELECT
    transaction_id,
    amount,
    status
FROM transactions;
```

Prefer explicit columns in production queries rather than `SELECT *`.

Benefits:

- Reduced unnecessary data transfer.
- More predictable interfaces.
- Easier schema evolution.
- Better readability.

## **12.2 WHERE**

```
SELECT *
FROM transactions
WHERE status = 'SUCCESS';
```

Multiple conditions:

```
SELECT *
FROM transactions
WHERE status = 'SUCCESS'
  AND amount > 10000;
```

### **12.3 Comparison operators**

```
=
<>
!=
>
<
>=
<=
```

### **12.4 BETWEEN**

```
SELECT *
FROM transactions
WHERE amount BETWEEN 1000 AND 10000;
```

`BETWEEN` includes both boundaries.

For time filtering, half-open ranges are frequently preferable:

```
SELECT *
FROM transactions
WHERE created_at >= TIMESTAMPTZ '2026-11-01 00:00:00+00'
  AND created_at <  TIMESTAMPTZ '2026-12-01 00:00:00+00';
```

This avoids ambiguity at adjacent reporting boundaries.

### **12.5 IN**

```
SELECT *
FROM transactions
WHERE status IN (
    'SUCCESS',
    'AUTHORIZED'
);
```

### **12.6 LIKE and ILIKE**

```
SELECT *
FROM merchants
WHERE merchant_name LIKE 'Example%';
```

`ILIKE` performs case-insensitive pattern matching under PostgreSQL's locale/collation behavior.

```
SELECT *
FROM merchants
WHERE merchant_name ILIKE '%store%';
```

For advanced linguistic or similarity search, PostgreSQL offers full-text search and the `pg_trgm` extension.

### **12.7 NULL handling**

SQL `NULL` represents missing or unknown information.

Do not use:

```
WHERE email = NULL;
```

Use:

```
WHERE email IS NULL;
```

Or:

```
WHERE email IS NOT NULL;
```

SQL uses three-valued logic:

- TRUE
- FALSE
- UNKNOWN

For example:

```
SELECT NULL = NULL;
```

The result is unknown, not true.

### **12.8 COALESCE**

Return the first non-null expression:

```
SELECT
    merchant_name,
    COALESCE(email, 'Not supplied') AS contact_email
FROM merchants;
```

### **12.9 NULLIF**

Return `NULL` when two expressions are equal.

```
SELECT
    amount / NULLIF(quantity, 0)
FROM transaction_items;
```

This helps avoid division by zero.

### **12.10 DISTINCT**

```
SELECT DISTINCT status
FROM transactions;
```

Retrieve unique combinations:

```
SELECT DISTINCT merchant_id, status
FROM transactions;
```

## **12.11 ORDER BY**

```
SELECT *
FROM transactions
ORDER BY created_at DESC;
```

Multiple sort keys:

```
SELECT *
FROM transactions
ORDER BY status ASC, created_at DESC;
```

## **12.12 LIMIT and OFFSET**

```
SELECT *
FROM transactions
ORDER BY created_at DESC
LIMIT 20;
```

Offset:

```
SELECT *
FROM transactions
ORDER BY created_at DESC
LIMIT 20 OFFSET 40;
```

For deep pagination, large offsets can become expensive.

A keyset pagination approach is often more efficient.

Example:

```
SELECT *
FROM transactions
WHERE created_at < TIMESTAMPTZ '2026-11-19 10:00:00+00'
ORDER BY created_at DESC
LIMIT 20;
```

For reliable pagination, include a unique tie-breaker such as the primary key:

```
SELECT *
FROM transactions
WHERE (created_at, transaction_id)
    < (
        TIMESTAMPTZ '2026-11-19 10:00:00+00',
        10000
    )
ORDER BY created_at DESC, transaction_id DESC
LIMIT 20;
```

A matching composite index can support this pattern effectively.

# **13. Aggregate functions and grouping**

Aggregate functions compute values from collections of rows.

Important functions:

| Function       | Purpose                         |
| -------------- | ------------------------------- |
| `COUNT()`      | Count rows or non-null values.  |
| `SUM()`        | Sum values.                     |
| `AVG()`        | Average.                        |
| `MIN()`        | Minimum.                        |
| `MAX()`        | Maximum.                        |
| `STRING_AGG()` | Concatenate values into text.   |
| `ARRAY_AGG()`  | Aggregate values into an array. |
| `JSON_AGG()`   | Aggregate values into JSON.     |
| `JSONB_AGG()`  | Aggregate values into JSONB.    |
| `BOOL_AND()`   | Logical AND aggregation.        |
| `BOOL_OR()`    | Logical OR aggregation.         |
| `STDDEV()`     | Standard deviation.             |
| `VARIANCE()`   | Variance.                       |

## **13.1 Basic aggregates**

```
SELECT
    COUNT(*) AS total_transactions,
    SUM(amount) AS total_amount,
    AVG(amount) AS average_amount,
    MIN(amount) AS minimum_amount,
    MAX(amount) AS maximum_amount
FROM transactions;
```

### **COUNT distinctions**

```
COUNT(*)
```

Counts rows.

```
COUNT(email)
```

Counts non-null email values.

```
COUNT(DISTINCT merchant_id)
```

Counts unique non-null merchant identifiers.

## **13.2 GROUP BY**

```
SELECT
    status,
    COUNT(*) AS total
FROM transactions
GROUP BY status;
```

This calculates transaction counts per status.

### **13.3 Multiple grouping columns**

```
SELECT
    merchant_id,
    status,
    COUNT(*) AS transaction_count
FROM transactions
GROUP BY merchant_id, status;
```

### **13.4 HAVING**

`WHERE` filters rows before aggregation.

`HAVING` filters aggregated groups.

```
SELECT
    merchant_id,
    SUM(amount) AS total_amount
FROM transactions
GROUP BY merchant_id
HAVING SUM(amount) > 100000;
```

## **13.5 Conditional aggregation**

```
SELECT
    merchant_id,

    COUNT(*) AS total,

    COUNT(*) FILTER (
        WHERE status = 'SUCCESS'
    ) AS successful,

    COUNT(*) FILTER (
        WHERE status = 'FAILED'
    ) AS failed,

    SUM(amount) FILTER (
        WHERE status = 'SUCCESS'
    ) AS successful_amount

FROM transactions
GROUP BY merchant_id;
```

This is very useful for financial reporting dashboards.

## **13.6 GROUPING SETS**

PostgreSQL supports multiple grouping granularities in one query.

```
SELECT
    merchant_id,
    status,
    SUM(amount)
FROM transactions
GROUP BY GROUPING SETS (
    (merchant_id, status),
    (merchant_id),
    (status),
    ()
);
```

Other grouping facilities include:

- `ROLLUP`
- `CUBE`
- `GROUPING()`

These are useful for multidimensional reports.

# **14. Joins and relationships**

Essential concept

Joins combine related rows across tables.

Suppose we have:

**merchants**

| merchant_id | merchant_name |
| ----------- | ------------- |
| 1           | Store A       |
| 2           | Store B       |
| 3           | Store C       |

**transactions**

| transaction_id | merchant_id | amount |
| -------------- | ----------- | ------ |
| 1001           | 1           | 1000   |
| 1002           | 1           | 2000   |
| 1003           | 2           | 3000   |

## **14.1 INNER JOIN**

Returns matching records from both relations.

```
SELECT
    m.merchant_name,
    t.transaction_id,
    t.amount
FROM merchants m
INNER JOIN transactions t
    ON m.merchant_id = t.merchant_id;
```

Merchant C does not appear because it has no matching transactions.

## **14.2 LEFT JOIN**

Returns all left-table records and matching right-table records.

```
SELECT
    m.merchant_name,
    t.transaction_id,
    t.amount
FROM merchants m
LEFT JOIN transactions t
    ON m.merchant_id = t.merchant_id;
```

Store C is included with `NULL` transaction values.

## **14.3 RIGHT JOIN**

Returns all right-table records and matching left-table records.

```
SELECT *
FROM merchants m
RIGHT JOIN transactions t
    ON m.merchant_id = t.merchant_id;
```

RIGHT JOIN is often expressible more clearly by reversing the table order and using LEFT JOIN.

## **14.4 FULL OUTER JOIN**

Returns matched and unmatched rows from both sides.

```
SELECT *
FROM merchants m
FULL OUTER JOIN transactions t
    ON m.merchant_id = t.merchant_id;
```

## **14.5 CROSS JOIN**

Returns the Cartesian product.

```
SELECT *
FROM merchants
CROSS JOIN currency_codes;
```

Use it intentionally because the row count can grow rapidly.

## **14.6 SELF JOIN**

A table can be joined to itself.

Example: employee-manager relationships.

```
SELECT
    employee.name,
    manager.name AS manager_name
FROM employees employee
LEFT JOIN employees manager
    ON employee.manager_id = manager.employee_id;
```

## **14.7 LATERAL JOIN**

A `LATERAL` subquery can reference columns from preceding `FROM` items.

Example: retrieve the latest transaction for each merchant.

```
SELECT
    m.merchant_id,
    m.merchant_name,
    latest.transaction_id,
    latest.created_at
FROM merchants m

LEFT JOIN LATERAL (
    SELECT
        t.transaction_id,
        t.created_at
    FROM transactions t
    WHERE t.merchant_id = m.merchant_id
    ORDER BY t.created_at DESC
    LIMIT 1
) latest ON TRUE;
```

A suitable index on `(merchant_id, created_at)` can make this pattern efficient.

## **14.8 EXISTS and NOT EXISTS**

Check whether matching records exist.

```
SELECT *
FROM merchants m
WHERE EXISTS (
    SELECT 1
    FROM transactions t
    WHERE t.merchant_id = m.merchant_id
);
```

Find merchants without transactions:

```
SELECT *
FROM merchants m
WHERE NOT EXISTS (
    SELECT 1
    FROM transactions t
    WHERE t.merchant_id = m.merchant_id
);
```

`EXISTS` is often clearer than joining and then eliminating duplicate rows when the actual requirement is only an existence test.

## **14.9 Join algorithm fundamentals**

PostgreSQL can choose different join algorithms.

| Join algorithm | Characteristics                                                   | Common conditions                                |
| -------------- | ----------------------------------------------------------------- | ------------------------------------------------ |
| Nested Loop    | For each outer row, searches matching inner rows.                 | Small outer input or selective indexed lookup.   |
| Hash Join      | Builds a hash table from one input and probes it using the other. | Equality joins and suitable memory availability. |
| Merge Join     | Reads two suitably ordered inputs and merges matching keys.       | Sorted inputs and compatible join conditions.    |

The SQL syntax does not directly dictate which physical algorithm is used.

The optimizer selects a plan based on estimated costs and available information.

# **15. Subqueries, CTEs, and advanced query construction**

## **15.1 Subqueries**

A subquery is a query nested inside another query.

Example:

```
SELECT *
FROM transactions
WHERE amount > (
    SELECT AVG(amount)
    FROM transactions
);
```

This returns transactions whose amount exceeds the overall average.

## **15.2 Scalar subqueries**

A scalar subquery returns one column and at most one row.

```
SELECT
    transaction_id,
    amount,
    (SELECT AVG(amount) FROM transactions) AS average_amount
FROM transactions;
```

A scalar subquery returning multiple rows causes an error.

## **15.3 Correlated subqueries**

A correlated subquery refers to columns from the outer query.

```
SELECT *
FROM merchants m
WHERE (
    SELECT COUNT(*)
    FROM transactions t
    WHERE t.merchant_id = m.merchant_id
) > 10;
```

The subquery depends on each outer merchant row, though the optimizer may transform the query into another execution strategy.

## **15.4 Common Table Expressions (CTEs)**

CTEs are introduced through `WITH`.

```
WITH successful_transactions AS (
    SELECT *
    FROM transactions
    WHERE status = 'SUCCESS'
)

SELECT
    merchant_id,
    SUM(amount)
FROM successful_transactions
GROUP BY merchant_id;
```

CTEs provide named intermediate query expressions.

They improve readability in complex SQL.

## **15.5 Multiple CTEs**

```
WITH successful AS (
    SELECT *
    FROM transactions
    WHERE status = 'SUCCESS'
),

merchant_totals AS (
    SELECT
        merchant_id,
        SUM(amount) AS total_amount
    FROM successful
    GROUP BY merchant_id
)

SELECT *
FROM merchant_totals
WHERE total_amount > 100000;
```

## **15.6 Recursive CTEs**

Recursive CTEs allow querying hierarchical structures.

Example: merchant organizational hierarchy.

```
WITH RECURSIVE merchant_tree AS (

    SELECT
        merchant_id,
        parent_merchant_id,
        merchant_name,
        1 AS depth
    FROM merchants
    WHERE parent_merchant_id IS NULL

    UNION ALL

    SELECT
        m.merchant_id,
        m.parent_merchant_id,
        m.merchant_name,
        mt.depth + 1
    FROM merchants m
    JOIN merchant_tree mt
        ON m.parent_merchant_id = mt.merchant_id
)

SELECT *
FROM merchant_tree
ORDER BY depth, merchant_id;
```

Potentially cyclic hierarchies require cycle detection or other defensive handling.

## **15.7 Data-modifying CTEs**

PostgreSQL supports data-modifying statements inside `WITH`.

```
WITH updated AS (
    UPDATE merchants
    SET status = 'SUSPENDED'
    WHERE merchant_id = 1
    RETURNING merchant_id, status
)

SELECT *
FROM updated;
```

This is useful for composing controlled data operations.

Understand that data-modifying CTEs have transaction and execution semantics of their own; they are not simply procedural scripts executed sequentially in textual order.

## **15.8 Window functions**

Advanced SQL

Window functions perform calculations across related rows without collapsing the individual rows into one grouped result.

This makes them particularly useful for transaction history, ranking, reconciliation, and reporting.

### **Basic syntax**

```
function_name()
OVER (
    PARTITION BY ...
    ORDER BY ...
)
```

### **Ranking functions**

| Function       | Purpose                          |
| -------------- | -------------------------------- |
| `ROW_NUMBER()` | Unique sequential position.      |
| `RANK()`       | Ranking with gaps after ties.    |
| `DENSE_RANK()` | Ranking without gaps after ties. |
| `NTILE(n)`     | Divide ordered rows into groups. |

Example:

```
SELECT
    merchant_id,
    transaction_id,
    amount,

    ROW_NUMBER() OVER (
        PARTITION BY merchant_id
        ORDER BY amount DESC
    ) AS transaction_rank

FROM transactions;
```

### **Running totals**

```
SELECT
    transaction_id,
    created_at,
    amount,

    SUM(amount) OVER (
        ORDER BY created_at, transaction_id
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total

FROM transactions;
```

### **Moving averages**

```
SELECT
    transaction_id,
    amount,

    AVG(amount) OVER (
        ORDER BY created_at
        ROWS BETWEEN 4 PRECEDING AND CURRENT ROW
    ) AS moving_average

FROM transactions;
```

### **LAG and LEAD**

Retrieve previous or next row values:

```
SELECT
    transaction_id,
    amount,

    LAG(amount) OVER (
        ORDER BY created_at, transaction_id
    ) AS previous_amount,

    LEAD(amount) OVER (
        ORDER BY created_at, transaction_id
    ) AS next_amount

FROM transactions;
```

Applications:

- Transaction sequence analysis.
- Financial trend analysis.
- Event comparison.
- Time-series calculations.
- Device telemetry.
- Reconciliation.

### **FIRST_VALUE and LAST_VALUE**

```
SELECT
    merchant_id,
    transaction_id,
    created_at,

    FIRST_VALUE(transaction_id) OVER (
        PARTITION BY merchant_id
        ORDER BY created_at
    ) AS first_transaction

FROM transactions;
```

Be aware that window frame defaults can affect `LAST_VALUE()` behavior. Define the intended frame explicitly when necessary.

### **Window functions versus GROUP BY**

| GROUP BY                            | Window functions                              |
| ----------------------------------- | --------------------------------------------- |
| Collapses rows into groups.         | Preserves individual rows.                    |
| Returns aggregate result per group. | Computes values over a window.                |
| Good for summary reports.           | Good for rankings, sequences, running totals. |

# **16. Data integrity and database constraints**

Constraints are central to reliable database design.

They protect data regardless of whether changes originate from Python, Rust, SQL clients, background workers, or external integrations.

## **16.1 NOT NULL**

```
CREATE TABLE merchants (
    merchant_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    merchant_name TEXT NOT NULL
);
```

Ensures a value must be supplied.

## **16.2 UNIQUE**

```
CREATE TABLE terminals (
    terminal_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    serial_number VARCHAR(100) UNIQUE NOT NULL
);
```

Ensures duplicate non-null values are not accepted under the constraint's uniqueness rules.

PostgreSQL normally permits multiple NULL values in a unique constraint because NULLs are distinct by default for uniqueness purposes. PostgreSQL also supports `NULLS NOT DISTINCT` for use cases that require NULL values to participate in uniqueness as equal values.

## **16.3 PRIMARY KEY**

```
CREATE TABLE merchants (
    merchant_id BIGINT PRIMARY KEY
);
```

A primary key provides uniqueness and non-null requirements.

A table can have only one primary-key constraint, but that key can contain multiple columns.

## **16.4 FOREIGN KEY**

```
CREATE TABLE terminals (
    terminal_id BIGINT PRIMARY KEY,

    merchant_id BIGINT NOT NULL,

    FOREIGN KEY (merchant_id)
        REFERENCES merchants(merchant_id)
);
```

A foreign key enforces referential integrity.

### **16.5 Foreign-key actions**

| Action        | Meaning                                                                  |
| ------------- | ------------------------------------------------------------------------ |
| `NO ACTION`   | Rejects the change if the constraint remains violated at its check time. |
| `RESTRICT`    | Prevents the referenced change when dependent rows exist.                |
| `CASCADE`     | Propagates the deletion or update to dependent records.                  |
| `SET NULL`    | Sets the referencing columns to NULL.                                    |
| `SET DEFAULT` | Sets referencing columns to their defaults.                              |

Example:

```
FOREIGN KEY (merchant_id)
REFERENCES merchants(merchant_id)
ON DELETE RESTRICT
```

For financial records, cascading deletion is usually inappropriate. Preserve historical transactions and use controlled status or archival workflows instead.

### **16.6 CHECK**

```
CREATE TABLE transactions (
    transaction_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    amount BIGINT NOT NULL,

    CONSTRAINT chk_positive_amount
        CHECK (amount > 0)
);
```

A check constraint rejects rows for which its expression evaluates to false. A NULL result does not itself violate a CHECK constraint, so use `NOT NULL` when the field is mandatory.

### **16.7 Composite unique constraints**

```
CREATE TABLE terminal_subscriptions (
    terminal_id BIGINT NOT NULL,
    topic VARCHAR(200) NOT NULL,

    CONSTRAINT unique_terminal_topic
        UNIQUE (terminal_id, topic)
);
```

Useful for preventing duplicate relationships.

### **16.8 Exclusion constraints**

PostgreSQL supports exclusion constraints for rules involving operators and potentially overlapping values.

Example concept: prevent overlapping maintenance windows for the same device.

```
CREATE EXTENSION IF NOT EXISTS btree_gist;

CREATE TABLE device_maintenance (
    maintenance_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    device_id BIGINT NOT NULL,

    maintenance_period TSTZRANGE NOT NULL,

    EXCLUDE USING GIST (
        device_id WITH =,
        maintenance_period WITH &&
    )
);
```

This combines equality on the device identifier with range-overlap checking.

It is useful for reservations and scheduling systems.

## **16.9 Deferrable constraints**

PostgreSQL supports constraints that can be checked at transaction commit rather than immediately after each statement, where the constraint type permits it.

```
CONSTRAINT fk_terminal_merchant
FOREIGN KEY (merchant_id)
REFERENCES merchants(merchant_id)
DEFERRABLE INITIALLY DEFERRED
```

This can help when a valid final transaction state temporarily violates relationships during a multi-statement operation.

Use deferral selectively. It makes transaction behavior more complex.

## **16.10 Constraint naming conventions**

Use descriptive names:

```
pk_merchants
fk_terminals_merchants
uq_merchants_merchant_code
chk_transactions_positive_amount
ex_device_maintenance_overlap
```

Consistency improves maintainability and troubleshooting.

# **17. Database normalization and relational design**

Essential database engineering

Database normalization is the process of organizing relational data to reduce redundancy, prevent anomalies, and maintain data integrity.

A well-normalized database separates distinct entities and represents their relationships explicitly.

## **17.1 Why normalization matters**

Consider an unnormalized transaction table:

| transaction_id | merchant_name | merchant_email | terminal_serial | amount |
| -------------- | ------------- | -------------- | --------------- | ------ |
| 1001           | Store A       | a\@example.com | VB90-01         | 1000   |
| 1002           | Store A       | a\@example.com | VB90-02         | 2000   |
| 1003           | Store B       | b\@example.com | VB90-03         | 1500   |

Merchant information is repeated.

This causes several potential problems.

**Update anomaly**

Changing the merchant email requires updating multiple transaction records.

**Insertion anomaly**

A merchant cannot easily be registered without creating an unrelated transaction.

**Deletion anomaly**

Deleting the last transaction associated with a merchant might accidentally remove the only copy of its details.

Normalization addresses these structural problems.

## **17.2 First Normal Form (1NF)**

A relation is in First Normal Form when its attributes contain values according to the intended atomic domain and it does not represent repeating groups through nested sets of independently addressable values.

Poor design:

| merchant_id | phone_numbers          |
| ----------- | ---------------------- |
| 1           | 9811111111, 9822222222 |

A normalized alternative:

**merchants**

| merchant_id | merchant_name |
| ----------- | ------------- |
| 1           | Store A       |

**merchant_phone_numbers**

| merchant_id | phone_number |
| ----------- | ------------ |
| 1           | 9811111111   |
| 1           | 9822222222   |

SQL:

```
CREATE TABLE merchant_phone_numbers (
    merchant_id BIGINT NOT NULL
        REFERENCES merchants(merchant_id),

    phone_number VARCHAR(30) NOT NULL,

    PRIMARY KEY (
        merchant_id,
        phone_number
    )
);
```

PostgreSQL supports arrays and JSONB, but using these does not automatically make a design unnormalized. The important question is whether the contained values need independent relational identity, constraints, querying, and relationships.

## **17.3 Second Normal Form (2NF)**

A relation is in 2NF when it is in 1NF and every non-prime attribute is fully functionally dependent on each candidate key, with no partial dependency on a proper subset of a composite candidate key.

Consider:

```
transaction_items
-----------------
transaction_id
product_id
product_name
quantity
unit_price
```

Suppose the primary key is:

```
(transaction_id, product_id)
```

If `product_name` depends only on `product_id`, it is partially dependent on the composite key.

Separate the entities:

```
products
--------
product_id
product_name
```

```
transaction_items
-----------------
transaction_id
product_id
quantity
unit_price
```

## **17.4 Third Normal Form (3NF)**

A relation is in 3NF when it is in 2NF and non-key attributes satisfy the required dependency conditions without inappropriate transitive dependencies on candidate keys.

Consider:

```
merchants
---------
merchant_id
merchant_name
district_id
district_name
```

If:

```
merchant_id → district_id
district_id → district_name
```

Then `district_name` is transitively dependent on `merchant_id`.

Separate the district:

```
CREATE TABLE districts (
    district_id BIGINT PRIMARY KEY,
    district_name VARCHAR(100) NOT NULL
);
```

```
CREATE TABLE merchants (
    merchant_id BIGINT PRIMARY KEY,
    merchant_name VARCHAR(150) NOT NULL,
    district_id BIGINT NOT NULL
        REFERENCES districts(district_id)
);
```

## **17.5 Boyce-Codd Normal Form (BCNF)**

BCNF is a stronger form of 3NF.

A relation satisfies BCNF when every non-trivial functional dependency has a determinant that is a superkey.

BCNF addresses some dependency anomalies that can remain in 3NF designs.

## **17.6 Fourth and Fifth Normal Forms**

Advanced normalization includes:

**Fourth Normal Form (4NF)**

Addresses non-trivial multivalued dependencies.

**Fifth Normal Form (5NF)**

Addresses join dependencies and decompositions where reconstruction requires multiple relations.

These are important relational theory concepts, but most everyday transactional applications focus primarily on 1NF through 3NF and, where warranted, BCNF.

## **17.7 Practical normalization strategy**

For a conventional transaction platform:

- Merchants belong in a merchant table.
- Terminals belong in a terminal table.
- Transactions belong in a transaction table.
- Transaction items belong in a transaction-item table.
- Users belong in a user table.
- Roles and permissions belong in appropriate access-control structures.
- Currency information belongs in an appropriately modeled currency reference.
- Audit events belong in a separate event or audit structure.

A typical relationship model:

```
Merchant
   |
   +----< Terminals
   |
   +----< Transactions
                 |
                 +----< Transaction Items

User
   |
   +----< User Roles
                 |
                 +----> Roles
```

## **17.8 Denormalization**

Denormalization deliberately introduces redundancy to improve certain access patterns.

Examples:

- Precomputed reporting totals.
- Cached merchant display names in event snapshots.
- Materialized views.
- Summary tables.
- Data warehouse fact tables.

Denormalization requires explicit maintenance and consistency strategies.

For example, a transaction record may preserve the merchant name as it appeared at transaction time for audit purposes. This is intentional historical snapshotting, not necessarily bad database design.

**Best practice:** Normalize operational source-of-truth data first. Denormalize specific read models when measurement demonstrates a need.

# **18. Relationships and entity modeling**

Database design should begin with the business domain, not with SQL table creation.

## **18.1 Entity**

An entity is a distinct business object represented in the database.

Examples:

- Merchant.
- Terminal.
- User.
- Transaction.
- Account.
- Currency.
- Settlement.
- Acquirer.
- Device.
- Payment request.

## **18.2 Entity Relationship Diagram (ERD)**

An ERD represents:

- Entities.
- Attributes.
- Keys.
- Relationships.
- Cardinalities.
- Optionality.

[ER Diagram (ERD) | Definition and Overview | Lucidchart](https://images.openai.com/static-rsc-4/IXqdSi6eRz_c1CguewNtMBzgX3SeJdELA0z2LWJPxTIbV2gnFxNjQ-iwv4EA3bwdnNTsKaHXpw5o3eUmL5WldGJakawmxIBcam7f4MXtiFZjwKHiqzeneuiwJVa2XFP69hZ51RT12_38ZjvuZvtYnJasbTzkQAqub6O7GYEBlUI?purpose=inline)

[lucidchart.com](https://www.lucidchart.com/blog/types-of-entity-relationship-diagrams?utm_source=chatgpt.com)

## **18.3 One-to-one relationship**

One row in one table relates to at most one row in another.

Example:

```
User 1 -------- 1 UserProfile
```

SQL:

```
CREATE TABLE user_profiles (
    user_id BIGINT PRIMARY KEY
        REFERENCES users(user_id),

    bio TEXT
);
```

Using the foreign key as the primary key enforces at most one profile per user.

## **18.4 One-to-many relationship**

One merchant can have multiple terminals.

```
Merchant 1 -------- N Terminals
```

The foreign key belongs on the many side:

```
CREATE TABLE terminals (
    terminal_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    merchant_id BIGINT NOT NULL
        REFERENCES merchants(merchant_id)
);
```

## **18.5 Many-to-many relationship**

A user can have multiple roles, and one role can be assigned to many users.

```
Users N -------- N Roles
```

Represent this through an association table:

```
CREATE TABLE user_roles (
    user_id BIGINT NOT NULL
        REFERENCES users(user_id),

    role_id BIGINT NOT NULL
        REFERENCES roles(role_id),

    assigned_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,

    PRIMARY KEY (user_id, role_id)
);
```

## **18.6 Cardinality**

Common forms:

- One-to-one.
- One-to-many.
- Many-to-many.
- Zero-or-one.
- Zero-or-many.
- Exactly-one-or-many.

Cardinality describes how entities relate. Optionality describes whether a relationship is required.

## **18.7 Surrogate versus natural keys**

**Surrogate key**

An artificial identifier such as a generated `BIGINT` or UUID.

**Natural key**

An identifier that already has business meaning, such as a government-issued code or established merchant code.

Comparison:

| Consideration | Surrogate                                                | Natural                                                   |
| ------------- | -------------------------------------------------------- | --------------------------------------------------------- |
| Stability     | Often easier to preserve                                 | Can change with business requirements                     |
| Meaning       | Little inherent business meaning                         | Meaningful domain identifier                              |
| Size          | Depends on chosen type                                   | Depends on domain                                         |
| Relationships | Commonly straightforward                                 | May expose sensitive or externally meaningful information |
| Uniqueness    | Requires separate business constraints where appropriate | Often naturally unique                                    |

A common production strategy is to use a surrogate primary key while separately enforcing uniqueness on appropriate business identifiers.

Example:

```
CREATE TABLE merchants (
    merchant_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

    merchant_code VARCHAR(50) NOT NULL UNIQUE
);
```

## **18.8 Database naming conventions**

Use consistent naming.

An example:

| Object                 | Convention                                          |
| ---------------------- | --------------------------------------------------- |
| Table                  | Plural nouns or established project convention      |
| Column                 | `snake_case`                                        |
| Primary key            | `<entity>_id`                                       |
| Foreign key            | Referenced entity key name                          |
| Index                  | `idx_<table>_<columns>`                             |
| Unique constraint      | `uq_<table>_<columns>`                              |
| Foreign-key constraint | `fk_<table>_<referenced_table>`                     |
| Check constraint       | `chk_<table>_<rule>`                                |
| View                   | `vw_<purpose>` where project convention requires it |

Avoid inconsistent abbreviations and unnecessarily cryptic names.

PostgreSQL folds unquoted identifiers to lowercase, so unquoted lowercase `snake_case` naming is generally the least surprising approach.

# **19. Transactions and ACID properties**

Critical for fintech and transaction systems

Transactions are one of the most important PostgreSQL concepts.

A database transaction groups multiple operations into a logical unit of work.

A transaction should preserve the defined correctness rules of the application.

## **19.1 ACID**

ACID describes four fundamental transaction properties.

A

**Atomicity**

A transaction succeeds as a unit or its transactional effects are rolled back.

C

**Consistency**

A transaction preserves the defined database integrity rules when moving between valid committed states.

I

**Isolation**

Concurrent transactions behave according to the selected isolation level and concurrency-control rules.

D

**Durability**

Successfully committed transactions survive supported crash and recovery scenarios according to the configured durability guarantees.

### **19.2 Atomicity example**

Suppose a transfer moves 500 units from Account A to Account B.

The operation requires:

1. Deduct 500 from Account A.
2. Credit 500 to Account B.
3. Record the transfer.

If one operation fails, the others must not become partially committed.

This is a suitable transactional unit.

### **19.3 Basic transaction syntax**

```
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 500
WHERE account_id = 2;

INSERT INTO transfers (
    source_account_id,
    destination_account_id,
    amount,
    status
)
VALUES (
    1,
    2,
    500,
    'COMPLETED'
);

COMMIT;
```

This is only an illustration; a production transfer also needs checks for sufficient funds, account status, currency consistency, concurrency, idempotency, and affected-row counts.

### **19.4 ROLLBACK**

```
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

ROLLBACK;
```

The transaction's changes are discarded.

### **19.5 Savepoints**

Savepoints provide partial rollback within a transaction.

```
BEGIN;

INSERT INTO transactions (
    transaction_id,
    amount
)
VALUES (
    1001,
    25000
);

SAVEPOINT before_optional_record;

INSERT INTO transaction_metadata (
    transaction_id,
    metadata
)
VALUES (
    1001,
    '{"source":"terminal"}'
);

ROLLBACK TO SAVEPOINT before_optional_record;

COMMIT;
```

The transaction retains operations before the savepoint while undoing subsequent transactional changes up to that savepoint.

### **19.6 Transaction states**

A simplified transaction lifecycle:

```
BEGIN
  |
  v
ACTIVE
  |
  +---- Error / ROLLBACK
  |          |
  |          v
  |       ABORTED
  |
  +---- COMMIT
             |
             v
          COMMITTED
```

A transaction that encounters certain SQL errors can enter an aborted state. In such cases, further ordinary statements may fail until the transaction is rolled back or rolled back to a valid savepoint.

## **19.7 Transaction boundaries**

One of the most important application-level responsibilities is selecting the correct transaction boundary.

For example, in a payment processing workflow, consider:

```
Receive authorization request
       |
Validate transaction
       |
Check idempotency
       |
Record pending request
       |
Call external acquirer
       |
Persist authorization outcome
       |
Publish or schedule notification
```

A single PostgreSQL transaction should not necessarily remain open throughout the entire external network call.

Holding database locks and connections while waiting for another service is generally undesirable.

A stronger design may use:

- Short database transactions.
- Durable pending states.
- Idempotency keys.
- Explicit outcome transitions.
- An outbox for asynchronous publication.
- Reconciliation for uncertain external outcomes.

This is particularly important because PostgreSQL cannot atomically commit a local database transaction and an unrelated external bank/acquirer operation without a distributed transaction protocol.

# **20. MVCC — Multi-Version Concurrency Control**

Essential advanced PostgreSQL concept

MVCC is one of PostgreSQL's defining internal mechanisms.

It allows transactions to access consistent versions of rows while multiple transactions concurrently modify data.

The important idea is that a row update does not simply replace the old row in a way that every transaction must immediately observe.

PostgreSQL maintains row versions and determines which versions are visible to a transaction's snapshot.

The PostgreSQL documentation explains that this model enables concurrent readers and writers while maintaining transactional consistency.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



## **20.1 Why MVCC exists**

Consider two database sessions:

```
Transaction A                  Transaction B
     |                              |
     | Read balance                 |
     |                              |
     |                         Update balance
     |                              |
     |                         Commit
     |                              |
     | Read using its snapshot      |
```

Without an appropriate concurrency model, one transaction could observe inconsistent intermediate changes made by another.

MVCC provides snapshot-based visibility.

## **20.2 Tuple versions**

A PostgreSQL heap tuple contains transaction-related metadata.

Important internal concepts include:

- `xmin`: Transaction identifier associated with the insertion of a tuple version.
- `xmax`: Transaction identifier associated with the deletion or superseding of a tuple version, subject to PostgreSQL's tuple-state rules.
- Command and visibility metadata.

A simplified example:

Suppose a row initially contains:

```
account_id = 1
balance = 1000
```

Transaction A updates the balance:

```
UPDATE accounts
SET balance = 900
WHERE account_id = 1;
```

Conceptually, the database creates a new version.

```
Old tuple version:
    balance = 1000

New tuple version:
    balance = 900
```

The old tuple is no longer considered live for new snapshots after the update commits, but it may remain physically present until cleanup is possible.

This is an important distinction:

**Logical deletion is not necessarily immediate physical removal.**

## **20.3 Snapshot visibility**

A transaction's snapshot determines which row versions it can see.

Visibility depends on transaction status and snapshot rules.

As a result:

- A transaction may see an earlier committed version.
- A transaction does not normally see another transaction's uncommitted data.
- Newer committed changes may or may not be visible depending on the isolation level and snapshot boundary.

## **20.4 MVCC and readers/writers**

PostgreSQL's MVCC design generally allows ordinary reads and writes to proceed concurrently without those reads blocking row modifications in the way conventional exclusive-read locking models might.

However, not all operations are lock-free.

For example:

- Updates acquire relevant locks.
- DDL can require strong table locks.
- Explicit `SELECT ... FOR UPDATE` locks rows.
- Transactions can block one another.
- Conflicts and deadlocks can occur.

The important lesson is that MVCC reduces unnecessary blocking; it does not eliminate locking.

## **20.5 Dead tuples and bloat**

When rows are updated or deleted, old tuple versions may become dead to future snapshots.

They cannot be reclaimed until PostgreSQL determines that no transaction can still require visibility of them.

Over time, dead tuples can accumulate.

This creates:

- Table bloat.
- Index bloat.
- Increased storage use.
- More pages to scan.
- Potential query degradation.

This is why PostgreSQL requires vacuuming.

# **21. VACUUM, ANALYZE, and autovacuum**

Essential database administration

PostgreSQL performs ongoing maintenance to reclaim reusable space, maintain transaction visibility information, and gather statistics for query optimization.

## **21.1 VACUUM**

`VACUUM` makes space from obsolete tuple versions reusable.

```
VACUUM transactions;
```

Regular vacuum generally makes space reusable inside the table rather than automatically returning all such space to the operating system.

### **21.2 VACUUM ANALYZE**

```
VACUUM ANALYZE transactions;
```

This performs vacuum work and collects query-planner statistics.

### **21.3 ANALYZE**

`ANALYZE` collects statistics describing table contents.

```
ANALYZE transactions;
```

PostgreSQL's optimizer uses these statistics to estimate row counts and determine suitable execution plans.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



### **21.4 VACUUM FULL**

```
VACUUM FULL transactions;
```

This rewrites the table to reclaim space more aggressively.

However, it requires an exclusive lock on the table and can introduce significant I/O and operational disruption.

It should not be used casually on busy production tables.

## **21.5 Autovacuum**

Autovacuum is PostgreSQL's automatic maintenance mechanism.

Its responsibilities include:

- Vacuuming tables.
- Cleaning dead tuples.
- Managing transaction-related cleanup.
- Collecting planner statistics.

It is normally enabled and should not be disabled as a routine performance measure.

### **21.6 Transaction ID wraparound**

PostgreSQL uses finite transaction identifiers.

Because transaction identifiers wrap around, PostgreSQL must prevent very old transaction IDs from being mistaken for newer ones.

Vacuum performs critical freezing work to maintain transaction-ID safety.

Failure to manage transaction wraparound can cause PostgreSQL to enter protective modes that interfere with normal writes.

This is an operational issue that should be monitored seriously.

### **21.7 Important maintenance statistics**

Inspect table statistics:

```
SELECT
    schemaname,
    relname,
    n_live_tup,
    n_dead_tup,
    last_vacuum,
    last_autovacuum,
    last_analyze,
    last_autoanalyze
FROM pg_stat_user_tables;
```

Inspect database statistics:

```
SELECT *
FROM pg_stat_database;
```

These metrics help diagnose maintenance problems and changing database workload characteristics.

# **22. Transaction isolation levels**

Critical concurrency concept

Isolation levels define how concurrent transactions observe one another's work and what anomalies are permitted.

PostgreSQL supports the standard SQL isolation-level terminology, with implementation-specific behavior.

The four conventional levels are:

1. Read Uncommitted.
2. Read Committed.
3. Repeatable Read.
4. Serializable.

## **22.1 Read Uncommitted**

SQL standard concept:

```
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

In PostgreSQL, Read Uncommitted behaves like Read Committed.

PostgreSQL does not expose dirty reads through this isolation level.

## **22.2 Read Committed**

PostgreSQL's default transaction isolation level.

```
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

Each ordinary statement obtains a new snapshot.

This means a transaction can see committed changes made by other transactions between its own statements.

Example:

```
Transaction A              Transaction B

BEGIN                      BEGIN

SELECT balance
→ 1000

                           UPDATE balance
                           SET balance = 900

                           COMMIT

SELECT balance
→ 900

COMMIT
```

The two reads can return different results.

### **Characteristics**

- Dirty reads: prevented.
- Non-repeatable reads: possible.
- Phantoms: possible.
- Serialization anomalies: possible.

Use cases:

- Conventional transactional applications.
- Most ordinary CRUD operations.
- Short business transactions where each statement can operate on the latest committed snapshot.

### **22.3 Repeatable Read**

```
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

In PostgreSQL, a transaction sees a consistent snapshot established by its first non-transaction-control statement.

Other transactions can commit changes, but the current transaction generally continues to see its original snapshot.

Example:

```
Transaction A              Transaction B

BEGIN REPEATABLE READ      BEGIN

SELECT balance
→ 1000

                           UPDATE balance
                           SET balance = 900

                           COMMIT

SELECT balance
→ 1000

COMMIT
```

The second read continues seeing the original row version.

PostgreSQL's implementation of Repeatable Read is stronger than the minimum SQL-standard definition regarding phantom reads: it prevents phantoms, but serialization anomalies are still possible.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



### **22.4 Serializable**

```
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

Serializable isolation aims to make successful committed transactions behave as though they executed in some serial order.

PostgreSQL implements this using Serializable Snapshot Isolation (SSI).

The system tracks relevant transaction dependencies and detects situations that could lead to serialization anomalies.

Some transactions may fail with a serialization error and must be retried.

### **Important application requirement**

Do not assume that a Serializable transaction always completes successfully.

Application code must handle errors such as:

```
SQLSTATE 40001
serialization_failure
```

A retry mechanism should:

1. Roll back the failed transaction.
2. Start a new transaction.
3. Re-execute the complete transaction logic.
4. Use a bounded retry policy.
5. Handle repeated failure.
6. Ensure external side effects are not duplicated.

This final point is especially important for payment processing.

### **22.5 Isolation-level comparison**

| Anomaly               | Read Committed | Repeatable Read         | Serializable                                     |
| --------------------- | -------------- | ----------------------- | ------------------------------------------------ |
| Dirty read            | Prevented      | Prevented               | Prevented                                        |
| Non-repeatable read   | Possible       | Prevented               | Prevented                                        |
| Phantom read          | Possible       | Prevented in PostgreSQL | Prevented                                        |
| Serialization anomaly | Possible       | Possible                | Prevented for successful serializable executions |

### **22.6 Choosing an isolation level**

| Application requirement         | Typical choice                                                 |
| ------------------------------- | -------------------------------------------------------------- |
| Ordinary CRUD                   | Read Committed                                                 |
| Consistent multi-query snapshot | Repeatable Read                                                |
| Complex concurrent invariants   | Serializable or explicit locking                               |
| Inventory reservation           | Carefully designed atomic updates or locking                   |
| Account transfers               | Transaction design with appropriate row locking and invariants |
| Reporting snapshot              | Repeatable Read, when a consistent snapshot is required        |

Isolation level alone does not guarantee correct business logic.

Correctness also depends on:

- Constraint design.
- Query design.
- Locking.
- Transaction boundaries.
- Retry behavior.
- Idempotency.

# **23. Locks and concurrency management**

MVCC handles ordinary snapshot visibility, but PostgreSQL also uses locks to coordinate conflicting operations.

Locks control access to database resources.

## **23.1 Lock categories**

PostgreSQL has several important lock categories.

### **Table-level locks**

Table-level locks protect operations affecting a table.

Important modes include:

- `ACCESS SHARE`
- `ROW SHARE`
- `ROW EXCLUSIVE`
- `SHARE UPDATE EXCLUSIVE`
- `SHARE`
- `SHARE ROW EXCLUSIVE`
- `EXCLUSIVE`
- `ACCESS EXCLUSIVE`

Examples:

```
SELECT * FROM transactions;
```

Acquires an `ACCESS SHARE` table lock.

```
INSERT INTO transactions (...) VALUES (...);
```

Acquires a `ROW EXCLUSIVE` table lock.

```
ALTER TABLE transactions ADD COLUMN reference TEXT;
```

Typically requires `ACCESS EXCLUSIVE`.

### **23.2 Row-level locking**

PostgreSQL supports row-locking clauses.

`FOR UPDATE`:

```
SELECT *
FROM accounts
WHERE account_id = 1
FOR UPDATE;
```

This locks selected rows against conflicting concurrent modifications and locking operations until transaction completion.

`FOR NO KEY UPDATE`:

```
SELECT *
FROM accounts
WHERE account_id = 1
FOR NO KEY UPDATE;
```

A weaker row lock than `FOR UPDATE`.

`FOR SHARE`:

```
SELECT *
FROM accounts
WHERE account_id = 1
FOR SHARE;
```

Protects selected rows against concurrent updates and deletion while permitting compatible shared locks.

`FOR KEY SHARE`:

```
SELECT *
FROM accounts
WHERE account_id = 1
FOR KEY SHARE;
```

A weaker lock that primarily protects referenced key values.

### **23.3 NOWAIT**

Do not wait for a conflicting lock:

```
SELECT *
FROM accounts
WHERE account_id = 1
FOR UPDATE NOWAIT;
```

The statement errors if the required lock cannot be acquired immediately.

### **23.4 SKIP LOCKED**

Skip rows that are already locked by conflicting transactions:

```
SELECT *
FROM jobs
WHERE status = 'PENDING'
ORDER BY created_at
FOR UPDATE SKIP LOCKED
LIMIT 10;
```

This is particularly useful for concurrent worker queues.

Example workflow:

```
Worker A                 Worker B
   |                         |
Select pending jobs       Select pending jobs
   |                         |
Lock first batch          Skip A's locked rows
   |                         |
Process batch             Process different batch
   |                         |
Commit                    Commit
```

`SKIP LOCKED` intentionally produces an inconsistent view and is suitable for queue-like processing, not general-purpose reporting.

### **23.5 Atomic conditional updates**

An alternative to reading and then writing a balance in separate application steps is to use a single conditional update.

```
UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1
  AND balance >= 500
RETURNING balance;
```

If no row is returned, the debit was not applied.

This is a useful way to combine the condition and mutation atomically within one SQL statement.

A full transfer still needs the corresponding credit and transfer record inside an appropriate transaction.

## **23.6 Deadlocks**

A deadlock occurs when transactions wait on one another in a cycle.

Example:

```
Transaction A                 Transaction B

Lock Account 1                 Lock Account 2

Request Account 2              Request Account 1

Wait                           Wait
```

Neither can continue.

PostgreSQL detects deadlocks and aborts one of the transactions to resolve the situation.

### **23.7 Deadlock prevention**

- Lock resources in a consistent order.
- Keep transactions short.
- Avoid unnecessary explicit locks.
- Reduce transaction scope.
- Use proper indexes to reduce the amount of work.
- Avoid interactive user input while a transaction holds locks.
- Design transaction boundaries carefully.

### **23.8 Advisory locks**

Advisory locks are application-defined locks.

They do not automatically correspond to a particular table or row.

Session-level example:

```
SELECT pg_advisory_lock(12345);
```

Release:

```
SELECT pg_advisory_unlock(12345);
```

Transaction-level example:

```
BEGIN;

SELECT pg_advisory_xact_lock(12345);

-- Protected application workflow.

COMMIT;
```

Advisory locks can coordinate:

- Scheduled jobs.
- Resource allocation.
- Application-level critical sections.
- Distributed task coordination.
- Unique business workflows.

They require a carefully designed key strategy and consistent application participation. They do not replace database constraints or transactional correctness.

## **23.9 Inspect active locks**

```
SELECT
    pid,
    locktype,
    mode,
    granted,
    relation::regclass
FROM pg_locks;
```

Inspect sessions:

```
SELECT
    pid,
    usename,
    state,
    wait_event_type,
    wait_event,
    query
FROM pg_stat_activity;
```

These views are important when investigating blocked queries, long-running transactions, and operational incidents.

# **24. Indexes and database access paths**

Essential performance engineering

An index is a separate data structure that PostgreSQL maintains to accelerate certain data access patterns.

An index helps locate relevant rows without having to scan the entire table.

The PostgreSQL documentation describes indexes as a performance optimization that also adds maintenance overhead to inserts, updates, and deletes.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL



## **24.1 Why indexes matter**

Consider a table with 10 million transactions.

```
SELECT *
FROM transactions
WHERE transaction_id = 1000001;
```

Without a useful index, PostgreSQL may need to scan a large portion of the table.

With an appropriate index, it can locate the matching entry much more efficiently.

Conceptually:

**Without index**

Sequential scan

Row 1

Row 2

...

Row N

**With index**

Index lookup

Index root

Index branch

Matching row

Conceptual comparison, not an exact representation of PostgreSQL's physical page layout.

## **24.2 Create an index**

```
CREATE INDEX idx_transactions_status
ON transactions(status);
```

Query:

```
SELECT *
FROM transactions
WHERE status = 'SUCCESS';
```

The planner may choose to use the index.

It is not guaranteed to do so.

## **24.3 Index cost**

Indexes provide read benefits but introduce costs:

- Additional storage.
- Maintenance on writes.
- More work for vacuum.
- Potential cache pressure.
- Additional deployment and schema-change considerations.

Creating an index on every column is not a good design strategy.

Create indexes based on actual query patterns and workload measurements.

## **24.4 B-tree index**

Default and most important index

B-tree is the default index type.

```
CREATE INDEX idx_transactions_created_at
ON transactions(created_at);
```

It is useful for:

- Equality.
- Range comparisons.
- Ordering.
- Min/max lookups.
- Prefix comparisons for suitable operators and collations.

Examples:

```
WHERE transaction_id = 1001
```

```
WHERE amount > 10000
```

```
ORDER BY created_at DESC
```

B-tree indexes support ordered access.

## **24.5 Unique indexes**

```
CREATE UNIQUE INDEX idx_merchants_code
ON merchants(merchant_code);
```

Unique indexes enforce uniqueness.

A `UNIQUE` constraint normally creates the corresponding unique index automatically.

Prefer the declarative constraint when uniqueness is part of the data model.

## **24.6 Hash indexes**

```
CREATE INDEX idx_transactions_hash
ON transactions
USING HASH(transaction_id);
```

Hash indexes support equality comparisons.

B-tree is more versatile for most ordinary relational access patterns.

Use hash indexes only after evaluating a specific workload and confirming their benefit.

## **24.7 GIN index**

GIN means Generalized Inverted Index.

It is particularly useful for composite values whose elements are queried individually.

Common applications:

- JSONB containment.
- Array containment.
- Full-text search.

JSONB example:

```
CREATE INDEX idx_events_payload_gin
ON device_events
USING GIN(payload);
```

An important distinction: the default JSONB GIN operator class and `jsonb_path_ops` support different operator sets and access patterns.

For workloads mainly using JSONB containment, `jsonb_path_ops` may be worth evaluating:

```
CREATE INDEX idx_events_payload_path
ON device_events
USING GIN(payload jsonb_path_ops);
```

Do not create both automatically. Evaluate the actual operators your application uses.

## **24.8 GiST index**

GiST means Generalized Search Tree.

It supports generalized indexing structures.

Applications:

- Geospatial operations through PostGIS.
- Range comparisons.
- Geometric operations.
- Exclusion constraints.
- Similarity-oriented extensions.

Example:

```
CREATE INDEX idx_maintenance_period
ON maintenance
USING GIST(maintenance_period);
```

## **24.9 SP-GiST index**

Space-partitioned Generalized Search Tree.

Useful for certain partitionable data structures, such as:

- Point location.
- Prefix trees.
- Geometric searching.
- Specialized data distributions.

## **24.10 BRIN index**

BRIN means Block Range Index.

BRIN stores summaries over physical block ranges rather than an individual detailed index entry for every row.

It is particularly useful for very large tables whose indexed values correlate with their physical row ordering.

Example:

```
CREATE INDEX idx_transactions_brin_created
ON transactions
USING BRIN(created_at);
```

Common application:

- Append-heavy event history.
- Time-ordered telemetry.
- Large historical datasets.

BRIN is compact but typically less selective than a suitable B-tree index.

## **24.11 Index comparison**

| Index   | Strong use case                       |
| ------- | ------------------------------------- |
| B-tree  | Equality, ranges, ordering            |
| Hash    | Equality comparisons                  |
| GIN     | JSONB, arrays, full-text search       |
| GiST    | Spatial, range, geometric, exclusion  |
| SP-GiST | Partitioned search structures         |
| BRIN    | Huge tables with physical correlation |

## **24.12 Composite indexes**

An index can cover multiple columns.

```
CREATE INDEX idx_transactions_merchant_created
ON transactions(merchant_id, created_at);
```

This can support:

```
SELECT *
FROM transactions
WHERE merchant_id = 10
ORDER BY created_at DESC;
```

### **Column order matters**

For a B-tree index:

```
(merchant_id, created_at)
```

is usually most effective when queries constrain the leading `merchant_id` column.

It also supports predicates involving both columns, and certain cases involving the leading column alone.

It is not equivalent to:

```
(created_at, merchant_id)
```

Choose column order according to actual filter and ordering patterns.

### **24.13 Covering indexes and INCLUDE**

An index can include additional non-key columns.

```
CREATE INDEX idx_transactions_merchant_status
ON transactions(merchant_id, status)
INCLUDE (amount, created_at);
```

This can help enable index-only scans for queries that need the indexed key columns and included values.

However, index-only scans also depend on heap visibility information.

Adding included columns increases index storage and write maintenance.

## **24.14 Partial indexes**

A partial index indexes only rows satisfying a condition.

```
CREATE INDEX idx_pending_transactions
ON transactions(created_at)
WHERE status = 'PENDING';
```

This can be useful when only a relatively small subset of a large table is queried frequently.

Example:

```
SELECT *
FROM transactions
WHERE status = 'PENDING'
ORDER BY created_at
LIMIT 100;
```

The query predicate must be compatible with the index predicate for the planner to use the partial index.

## **24.15 Expression indexes**

An expression index indexes the result of a computation.

```
CREATE INDEX idx_users_lower_email
ON users(lower(email));
```

Query:

```
SELECT *
FROM users
WHERE lower(email) = lower('USER@example.com');
```

This is useful for consistent case-insensitive lookup.

## **24.16 Index ordering**

PostgreSQL supports index ordering options.

```
CREATE INDEX idx_transactions_created_desc
ON transactions(created_at DESC);
```

A B-tree can often serve reverse-order queries without needing an explicitly reversed index.

However, mixed ordering and null-placement requirements can make the chosen definition relevant.

## **24.17 Index-only scans**

An index-only scan retrieves required values using the index without reading the corresponding heap tuples in cases where visibility information allows it.

Example:

```
CREATE INDEX idx_transactions_lookup
ON transactions(merchant_id)
INCLUDE (amount);
```

Query:

```
SELECT merchant_id, amount
FROM transactions
WHERE merchant_id = 10;
```

Whether this becomes an index-only scan depends on visibility-map state, planner costs, and query requirements.

## **24.18 Inspect indexes**

```
SELECT
    schemaname,
    tablename,
    indexname,
    indexdef
FROM pg_indexes
WHERE schemaname = 'public';
```

Index size:

```
SELECT
    indexrelname,
    pg_size_pretty(
        pg_relation_size(indexrelid)
    ) AS index_size
FROM pg_stat_user_indexes;
```

Monitor usage statistics, while remembering that statistics reset and access patterns change over time.

## **24.19 Create indexes concurrently**

For production tables, creating an index may interfere with concurrent write operations depending on the operation.

PostgreSQL supports:

```
CREATE INDEX CONCURRENTLY idx_transactions_status
ON transactions(status);
```

This reduces the ordinary write-blocking impact but involves additional work, time, and operational considerations.

Important:

- It cannot run inside an ordinary transaction block.
- It can take longer.
- It may leave an invalid index after failure.
- It requires checking the index state afterward.

For large production tables, index creation should be a controlled migration or operational procedure.

# **25. Query planner, optimization, and execution plans**

Essential performance engineering

A database query can be logically correct while still being inefficient.

PostgreSQL uses a cost-based query planner to select execution strategies.

Understanding how to inspect that strategy is one of the most valuable skills for working with large databases.

## **25.1 What is a query planner?**

The query planner evaluates alternative ways of executing a SQL statement.

For example:

```
SELECT *
FROM transactions
WHERE merchant_id = 1001;
```

Possible strategies:

**Sequential scan**

Read table pages and test each row.

**Index scan**

Use a relevant index to locate matching rows.

**Bitmap index and heap scan**

Collect matching tuple locations through indexes, then visit the appropriate heap pages.

The cheapest estimated strategy is selected based on the planner's cost model.

## **25.2 Cost-based optimization**

PostgreSQL estimates the cost of alternative operations.

It considers:

- Table size.
- Index availability.
- Estimated row counts.
- Column statistics.
- Selectivity.
- Data distribution.
- Join order.
- Join strategy.
- Available memory.
- I/O cost assumptions.
- CPU cost assumptions.
- Parallel execution opportunities.

### **25.3 Selectivity**

Selectivity describes how much of a dataset satisfies a predicate.

Suppose a transaction table contains 1 million rows.

```
WHERE status = 'SUCCESS'
```

If 950,000 rows match, the predicate is not particularly selective.

If only 100 rows match, the predicate is highly selective.

A selective condition often makes an index more useful, although the final decision depends on the complete query and cost estimates.

### **25.4 Cardinality estimation**

Cardinality refers to the estimated number of rows produced by an operation.

An incorrect cardinality estimate can result in an inefficient plan.

For example:

```
Estimated rows: 100
Actual rows: 500,000
```

The optimizer may choose a plan appropriate for a small result even though the actual result is large.

This is why maintaining good database statistics matters.

## **25.5 EXPLAIN**

`EXPLAIN` displays the planned execution strategy.

```
EXPLAIN
SELECT *
FROM transactions
WHERE status = 'SUCCESS';
```

Example output:

```
Seq Scan on transactions
  Filter: (status = 'SUCCESS')
```

This indicates a sequential scan followed by filtering.

## **25.6 EXPLAIN ANALYZE**

`EXPLAIN ANALYZE` executes the query and reports actual execution information.

```
EXPLAIN ANALYZE
SELECT *
FROM transactions
WHERE status = 'SUCCESS';
```

This provides:

- Estimated cost.
- Actual execution time.
- Estimated row count.
- Actual row count.
- Number of loops.
- Execution plan nodes.

**Important:** `EXPLAIN ANALYZE` actually executes the statement. For an `UPDATE`, `DELETE`, or other modifying statement, this means the data modification really occurs unless protected by an appropriate transaction that is rolled back.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+1



## **25.7 Detailed EXPLAIN options**

```
EXPLAIN (
    ANALYZE,
    BUFFERS,
    VERBOSE,
    SETTINGS,
    WAL,
    FORMAT JSON
)
SELECT *
FROM transactions
WHERE merchant_id = 1001;
```

Useful options:

| Option        | Purpose                                                     |
| ------------- | ----------------------------------------------------------- |
| `ANALYZE`     | Execute statement and report actual measurements.           |
| `BUFFERS`     | Report buffer activity.                                     |
| `VERBOSE`     | Provide additional details.                                 |
| `SETTINGS`    | Show relevant non-default planner settings.                 |
| `WAL`         | Report WAL-related activity for applicable operations.      |
| `TIMING`      | Report timing information per plan node when enabled.       |
| `SUMMARY`     | Include summary information.                                |
| `FORMAT JSON` | Provide machine-readable execution plan output.             |
| `MEMORY`      | Report relevant planner memory information where supported. |
| `SERIALIZE`   | Account for output serialization work.                      |

## **25.8 Understanding a plan**

Consider a simplified plan:

```
Nested Loop
  -> Index Scan using merchants_pkey
       Index Cond: merchant_id = 1001
  -> Index Scan using transactions_merchant_idx
       Index Cond: merchant_id = merchants.merchant_id
```

Interpretation:

1. PostgreSQL locates a merchant through its primary-key index.
2. It searches related transactions using a second index.
3. It combines the results through a nested-loop strategy.

### **Important plan terms**

| Term              | Meaning                                                           |
| ----------------- | ----------------------------------------------------------------- |
| Seq Scan          | Sequential table scan.                                            |
| Index Scan        | Index lookup with heap access.                                    |
| Index Only Scan   | Retrieves values from an index when visibility conditions permit. |
| Bitmap Index Scan | Produces matching tuple locations from an index.                  |
| Bitmap Heap Scan  | Fetches heap pages based on a bitmap.                             |
| Nested Loop       | Nested-loop join.                                                 |
| Hash Join         | Hash-based join.                                                  |
| Merge Join        | Sort/merge-based join.                                            |
| Sort              | Ordering operation.                                               |
| Aggregate         | Aggregation.                                                      |
| HashAggregate     | Hash-based aggregation.                                           |
| GroupAggregate    | Grouped aggregation over ordered input.                           |
| Gather            | Collects parallel worker results.                                 |
| Gather Merge      | Merges sorted results from parallel workers.                      |
| Materialize       | Stores intermediate results for reuse.                            |
| Memoize           | Caches results of parameterized operations in applicable plans.   |

## **25.9 Estimated versus actual rows**

Suppose:

```
Index Scan
  (rows=100)
  (actual rows=100000)
```

This is a large estimation discrepancy.

Possible causes:

- Stale statistics.
- Data skew.
- Correlated columns.
- Inadequate statistics targets.
- Parameter-sensitive behavior.
- Changes in data distribution.

Investigate before changing planner configuration.

## **25.10 Query optimization workflow**

1. **Identify the slow query**

   Use `pg_stat_statements`, application traces, or database monitoring.
2. **Inspect the query plan**

   Run `EXPLAIN (ANALYZE, BUFFERS)`.
3. **Compare estimates and actuals**

   Look for cardinality discrepancies and expensive plan nodes.
4. **Inspect indexes and predicates**

   Evaluate filtering, joins, sort requirements, and index usefulness.
5. **Check table statistics**

   Verify autovacuum and `ANALYZE` behavior.
6. **Improve SQL or schema design**

   Change only what measurement supports.
7. **Benchmark again**

   Verify that the improvement is real and does not degrade other workloads.

## **25.11 Statistics and extended statistics**

PostgreSQL gathers column statistics for planner decisions.

For example:

```
ANALYZE transactions;
```

To inspect available statistics:

```
SELECT *
FROM pg_stats
WHERE tablename = 'transactions';
```

If two columns are correlated, single-column statistics may be insufficient.

PostgreSQL supports extended statistics.

```
CREATE STATISTICS stats_merchant_status
    (dependencies, mcv, ndistinct)
ON merchant_id, status
FROM transactions;

ANALYZE transactions;
```

Statistics kinds:

| Kind           | Purpose                                           |
| -------------- | ------------------------------------------------- |
| `dependencies` | Functional dependency information.                |
| `mcv`          | Multicolumn most-common-value information.        |
| `ndistinct`    | Distinct-value estimates for column combinations. |

These statistics can improve estimates for queries involving correlated columns.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.postgresql.org\&sz=32)

PostgreSQL+2



# **26. Query optimization best practices**

## **26.1 Avoid SELECT \***

Prefer:

```
SELECT
    transaction_id,
    amount,
    status
FROM transactions;
```

Rather than retrieving unnecessary columns.

## **26.2 Avoid applying unnecessary functions to indexed columns**

Potentially problematic:

```
SELECT *
FROM transactions
WHERE DATE(created_at) = DATE '2026-11-19';
```

The expression may prevent ordinary use of an index on `created_at`.

Prefer a range:

```
SELECT *
FROM transactions
WHERE created_at >= TIMESTAMPTZ '2026-11-19 00:00:00+00'
  AND created_at <  TIMESTAMPTZ '2026-11-20 00:00:00+00';
```

The exact boundaries should reflect the business timezone.

Alternatively, use an appropriate expression index if the expression is genuinely the intended access pattern.

## **26.3 Avoid unnecessary DISTINCT**

`DISTINCT` may require extra sorting or hashing.

Do not add it simply to hide incorrect join cardinality.

Find the underlying reason for duplicated rows.

## **26.4 Avoid N+1 queries**

A common ORM performance problem.

For example:

```
for merchant in merchants:    transactions = Transaction.objects.filter(        merchant=merchant    )
```

This may execute one query to retrieve merchants and a separate query for each merchant's transactions.

Use appropriate eager loading or aggregation.

In Django:

```
Merchant.objects.prefetch_related("transactions")
```

or:

```
Merchant.objects.select_related("district")
```

Choose the appropriate method based on relationship type.

## **26.5 Batch operations**

Instead of issuing many separate inserts:

```
INSERT INTO transactions (merchant_id, amount)
VALUES
    (1, 100),
    (1, 200),
    (2, 300);
```

For large datasets, consider:

```
COPY transactions
FROM STDIN
WITH (FORMAT CSV);
```

Bulk operations can greatly reduce per-statement overhead.

## **26.6 Use pagination deliberately**

Prefer keyset pagination for large, continuously changing result sets.

Offset pagination can become expensive when the offset is very large and can produce unstable results under concurrent changes without a consistent snapshot.

## **26.7 Avoid unnecessary long-running transactions**

Long-running transactions can:

- Retain old MVCC snapshots.
- Delay dead-tuple cleanup.
- Increase vacuum pressure.
- Hold locks.
- Consume connections.
- Affect operational reliability.

Keep database transactions focused and short.

# **27. PostgreSQL administrative system catalogs**

PostgreSQL maintains internal catalogs and statistical views that describe the database itself.

These are essential for administration and debugging.

## **27.1 Information schema**

PostgreSQL supports the SQL-standard `information_schema`.

Examples:

```
SELECT *
FROM information_schema.tables;
```

Columns:

```
SELECT *
FROM information_schema.columns
WHERE table_name = 'transactions';
```

Constraints:

```
SELECT *
FROM information_schema.table_constraints;
```

This is useful for cross-database-compatible metadata queries.

## **27.2 PostgreSQL system catalogs**

Important system catalogs:

| Catalog         | Purpose                                       |
| --------------- | --------------------------------------------- |
| `pg_class`      | Tables, indexes, and other relation metadata. |
| `pg_namespace`  | Schemas.                                      |
| `pg_attribute`  | Relation attributes.                          |
| `pg_type`       | Data type definitions.                        |
| `pg_constraint` | Constraints.                                  |
| `pg_index`      | Index metadata.                               |
| `pg_database`   | Database metadata.                            |
| `pg_roles`      | Role definitions through a catalog view.      |
| `pg_proc`       | Functions and procedures.                     |
| `pg_statistic`  | Planner statistics.                           |
| `pg_depend`     | Object dependencies.                          |
| `pg_extension`  | Installed extensions.                         |

Example:

```
SELECT
    n.nspname AS schema_name,
    c.relname AS relation_name,
    c.relkind
FROM pg_class c
JOIN pg_namespace n
    ON n.oid = c.relnamespace
WHERE n.nspname NOT IN (
    'pg_catalog',
    'information_schema'
);
```

## **27.3 Important monitoring views**

| View                     | Purpose                                |
| ------------------------ | -------------------------------------- |
| `pg_stat_activity`       | Current database sessions and queries. |
| `pg_stat_database`       | Database-level activity statistics.    |
| `pg_stat_user_tables`    | Table statistics.                      |
| `pg_stat_user_indexes`   | Index access statistics.               |
| `pg_statio_user_tables`  | Table I/O statistics.                  |
| `pg_statio_user_indexes` | Index I/O statistics.                  |
| `pg_stat_replication`    | Replication sender information.        |
| `pg_stat_wal_receiver`   | WAL receiver information.              |
| `pg_locks`               | Lock information.                      |
| `pg_stat_archiver`       | WAL archiving statistics.              |

### **27.4 Inspect active queries**

```
SELECT
    pid,
    usename,
    application_name,
    client_addr,
    state,
    wait_event_type,
    wait_event,
    query_start,
    query
FROM pg_stat_activity
WHERE state <> 'idle'
ORDER BY query_start;
```

For operational monitoring, inspect only the information necessary and apply access controls because query text can sometimes contain sensitive values.

### **27.5 Find long-running queries**

```
SELECT
    pid,
    now() - query_start AS duration,
    state,
    wait_event_type,
    wait_event,
    query
FROM pg_stat_activity
WHERE state = 'active'
ORDER BY duration DESC;
```

### **27.6 Find unused or rarely used indexes**

```
SELECT
    schemaname,
    relname AS table_name,
    indexrelname AS index_name,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan ASC;
```

An index with zero scans is not automatically safe to remove.

It may be required for:

- Constraints.
- Referential integrity.
- Rare but critical operations.
- Periodic workloads.
- Workloads since the last statistics reset.

Always inspect its purpose before removal.

# **28. PostgreSQL — Knowledge checkpoints**

At this stage, you should understand the following foundational and intermediate database engineering concepts:

| Area              | Concepts to master                                           |
| ----------------- | ------------------------------------------------------------ |
| Relational theory | Relations, tuples, attributes, keys, functional dependencies |
| Database design   | ERD, cardinality, normalization, denormalization             |
| SQL               | DDL, DML, querying, aggregation, joins, CTEs                 |
| Advanced SQL      | Window functions, recursive queries, lateral joins           |
| Data types        | Numeric, temporal, UUID, JSONB, arrays, ranges               |
| Integrity         | Primary keys, foreign keys, unique, check, exclusion         |
| Transactions      | ACID, commit, rollback, savepoints                           |
| Concurrency       | MVCC, snapshots, isolation levels, row and table locks       |
| Maintenance       | Vacuum, analyze, autovacuum, transaction freezing            |
| Indexing          | B-tree, Hash, GIN, GiST, SP-GiST, BRIN                       |
| Optimization      | Query planner, cardinality, statistics, EXPLAIN ANALYZE      |
| Administration    | Catalogs, system views, session and index monitoring         |

## **29. Practical database design exercise**

Build a small PostgreSQL database representing a payment platform.

[Understanding OLTP | Tiger Data](https://images.openai.com/static-rsc-4/j70K75h8WYind52shkWTQwIsby7ia7n_W3-EIlusNHOvqpWxvjkvlS_eI11-__PfUu8mOpCuwkAUvZyhIrq7etKFrnhufx7dohBD4BVvCC0O5y6049-UJLaUyxdUZ3-1KVxvy7CQp7kHdlXxH64nirJAQpG-dHKZ2bNmOvGSK-M?purpose=inline)

[tigerdata.com](https://www.tigerdata.com/learn/understanding-oltp?utm_source=chatgpt.com)

Suggested entities:

```
payment_platform
│
├── core
│   ├── users
│   ├── roles
│   └── user_roles
│
├── merchant
│   ├── merchants
│   ├── merchant_documents
│   └── merchant_contacts
│
├── terminal
│   ├── terminals
│   ├── terminal_configurations
│   └── terminal_events
│
├── payment
│   ├── transactions
│   ├── transaction_events
│   ├── transaction_reversals
│   └── idempotency_records
│
├── settlement
│   ├── settlements
│   └── settlement_items
│
└── audit
    └── audit_events
```

Implement the following:

1. Design the ERD and establish all relationships.
2. Define data types, primary keys, foreign keys, and constraints.
3. Normalize the operational schema.
4. Implement transaction creation and idempotency.
5. Create appropriate indexes for realistic query patterns.
6. Write queries for daily transaction reports.
7. Implement reconciliation queries.
8. Test concurrent balance and transaction operations.
9. Inspect query plans and optimize slow queries.
10. Document data retention, auditability, backup, and recovery requirements.

For financial software, use synthetic data in development and keep payment identifiers, credentials, cryptographic material, and sensitive authentication data out of test fixtures and logs.

# **30. Next PostgreSQL installment**

The following advanced sections complete the PostgreSQL technical reference:

- **WAL and storage internals:** Heap files, pages, tuple layout, TOAST, WAL records, checkpoints, background writer.
- **Replication:** Physical, streaming, synchronous, asynchronous, logical replication, slots, failover.
- **Backup and recovery:** `pg_dump`, `pg_restore`, physical backup, WAL archiving, PITR, disaster recovery.
- **Security:** Roles, privileges, authentication, TLS, `pg_hba.conf`, RLS, auditing, encryption considerations.
- **Administration:** Configuration, memory, connection limits, autovacuum tuning, tablespaces, extensions.
- **Performance:** Connection pooling, partitioning, materialized views, query-statistics extensions, workload analysis.
- **Ecosystem:** PostGIS, pg_stat_statements, pgvector, TimescaleDB, PgBouncer, Patroni.
- **Python and Rust integration:** Django ORM, SQLAlchemy, Psycopg, SQLx, transactions, migrations, pooling.
- **Production architecture:** HA, observability, security, operations, deployment, migration strategy.

After PostgreSQL, we move to **Part 05 — Redis**, covering its data structures, internal architecture, persistence, replication, caching, streams, messaging, and distributed-system use cases.

## **Primary technical references**

- [PostgreSQL 18 documentation](https://www.postgresql.org/docs/18/?utm_source=chatgpt.com)
- [PostgreSQL 19 documentation and release-candidate reference](https://www.postgresql.org/docs/19/?utm_source=chatgpt.com)
- [Data definition](https://www.postgresql.org/docs/18/ddl.html?utm_source=chatgpt.com)
- [Data manipulation](https://www.postgresql.org/docs/18/dml.html?utm_source=chatgpt.com)
- [Concurrency control](https://www.postgresql.org/docs/18/mvcc.html?utm_source=chatgpt.com)
- [Indexes](https://www.postgresql.org/docs/18/indexes.html?utm_source=chatgpt.com)
- [Performance tips](https://www.postgresql.org/docs/18/performance-tips.html?utm_source=chatgpt.com)

**Key learning objective:** Become capable of designing, querying, optimizing, securing, and operating PostgreSQL databases—not merely connecting to them through an ORM.