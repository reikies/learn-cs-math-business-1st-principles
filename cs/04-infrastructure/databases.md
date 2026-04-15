# Databases & Storage — Persisting and Querying Data

> Every interesting program eventually needs to remember something. The moment your
> data matters more than your process, you need a database. This chapter is about how
> we store, organize, retrieve, and protect data — from a single SQLite file to a
> globally distributed cluster serving millions of queries per second.

**Prerequisites:** [Data Structures & Algorithms](../03-theory/data-structures-and-algorithms.md) — You need B-trees, hash tables, and Big-O.

**What it enables:** Web apps, distributed systems, ML pipelines — any system that needs persistent data.

---

## 1. Why Databases Exist

Let's start with the obvious question: why not just use files?

You could write data to a text file. People did this for decades. But you quickly run
into five problems:

1. **Persistence** — RAM disappears when the power goes out. You need data on disk.
   Files handle this, but...
2. **Structure** — A flat file has no schema. How do you find "all orders placed last
   Tuesday by customers in Tokyo"? You'd have to parse the entire file yourself.
3. **Querying** — Even with structure, searching is slow without indexes. A million-row
   CSV means scanning a million rows for every lookup.
4. **Concurrency** — Two processes writing to the same file at the same time? Corruption.
   You need locking, coordination, isolation.
5. **Integrity** — What if your program crashes halfway through writing? You get half an
   order, a debit without a credit, corrupted state.

A database gives you all five. It's not magic — it's a carefully engineered system built
on the data structures you already know (B-trees, hash tables, linked lists) arranged to
solve these exact problems.

The history matters: in the 1960s, databases were hierarchical (tree-shaped) or
network-based (graph-shaped). Both were tightly coupled to physical storage. Changing
how you queried meant rewriting the storage layer. Then came a revolution.

---

## 2. The Relational Model

In 1970, Ted Codd at IBM published "A Relational Model of Data for Large Shared Data
Banks." His insight was deceptively simple: **represent data as tables**.

A **table** (relation) is a set of rows. Each **row** (tuple) is a single record. Each
**column** (attribute) is a named, typed field. That's it.

```
customers
+---------+-----------+----------+
| id (PK) | name      | city     |
+---------+-----------+----------+
| 1       | Tanaka    | Tokyo    |
| 2       | Mueller   | Berlin   |
| 3       | Garcia    | Madrid   |
+---------+-----------+----------+

orders
+----------+-------------+--------+------------+
| id (PK)  | customer_id | amount | created_at |
+----------+-------------+--------+------------+
| 101      | 1           | 250.00 | 2025-03-15 |
| 102      | 1           | 89.50  | 2025-03-18 |
| 103      | 3           | 412.00 | 2025-03-18 |
+----------+-------------+--------+------------+
```

**Keys** hold it all together:
- A **primary key** (PK) uniquely identifies each row. `customers.id` is a primary key.
- A **foreign key** (FK) references another table's primary key. `orders.customer_id`
  points to `customers.id`. This is how you express relationships without duplicating data.

### Normalization — Eliminating Redundancy

Why not store the customer's name and city in every order row? Because then when Tanaka
moves from Tokyo to Osaka, you'd have to update every single order. Miss one, and your
data contradicts itself.

**Normalization** is the process of organizing tables to eliminate redundancy:

- **1NF (First Normal Form):** Every cell contains a single value, not a list. No
  repeating groups. If a customer has three phone numbers, you don't cram them into one
  column — you make a `phone_numbers` table.

- **2NF (Second Normal Form):** Every non-key column depends on the *entire* primary
  key. If your table has a composite key `(order_id, product_id)` and a column
  `product_name` that depends only on `product_id`, that column belongs in a separate
  `products` table.

- **3NF (Third Normal Form):** No non-key column depends on another non-key column.
  If you have `zip_code` and `city` in the same table, and city is determined by
  zip_code, then city should live in a `zip_codes` table.

The intuition: **each fact is stored in exactly one place.** Update it once, it's
correct everywhere.

Why did this model dominate for 50 years? Because it separates the *logical* structure
of data from the *physical* storage. You describe *what* the data looks like. The
database decides *how* to store it efficiently. This separation of concerns turned out
to be one of the most powerful ideas in all of computing.

---

## 3. SQL — The Language of Relational Databases

SQL (Structured Query Language) is how you talk to relational databases. It was designed
in the 1970s at IBM, standardized in 1986, and remains the most widely used database
language in the world.

SQL is **declarative**: you describe *what* you want, not *how* to get it. The database
engine figures out the fastest way to execute your request. This is a profound
difference from imperative programming.

### Reading Data

```sql
-- All customers in Tokyo
SELECT name, city
FROM customers
WHERE city = 'Tokyo';

-- All orders over $100 with the customer name
SELECT c.name, o.amount, o.created_at
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE o.amount > 100
ORDER BY o.created_at DESC;

-- Total spending per customer, only those who spent over $200
SELECT c.name, SUM(o.amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.name
HAVING SUM(o.amount) > 200
ORDER BY total_spent DESC;
```

The execution order matters: `FROM` (pick tables) -> `JOIN` (combine) -> `WHERE`
(filter rows) -> `GROUP BY` (aggregate) -> `HAVING` (filter groups) -> `SELECT`
(pick columns) -> `ORDER BY` (sort). This is why you can't use a column alias
from `SELECT` in your `WHERE` clause — `WHERE` runs first.

### Writing Data

```sql
-- Insert a new customer
INSERT INTO customers (name, city)
VALUES ('Kim', 'Seoul');

-- Update a customer's city
UPDATE customers
SET city = 'Osaka'
WHERE name = 'Tanaka';

-- Delete all orders older than a year
DELETE FROM orders
WHERE created_at < '2024-03-01';
```

### Defining Structure

```sql
CREATE TABLE products (
    id          SERIAL PRIMARY KEY,
    name        VARCHAR(255) NOT NULL,
    price       DECIMAL(10, 2) NOT NULL CHECK (price > 0),
    category_id INTEGER REFERENCES categories(id),
    created_at  TIMESTAMP DEFAULT NOW()
);
```

Notice the constraints: `NOT NULL`, `CHECK`, `REFERENCES`. The database enforces these
on every write. You can't insert a product with a negative price. You can't reference
a category that doesn't exist. The database is a gatekeeper for data quality.

### Subqueries

```sql
-- Customers who have placed at least one order over $300
SELECT name
FROM customers
WHERE id IN (
    SELECT customer_id
    FROM orders
    WHERE amount > 300
);
```

The key insight: SQL queries compose. The inner query produces a set of IDs. The outer
query uses that set as a filter. You can nest these arbitrarily.

Why does SQL endure? Because declarative beats imperative for data access. You don't
write loops, manage cursors, or think about disk pages. You describe the result you
want, and a sophisticated optimizer (more on this below) turns your description into an
efficient execution plan. You think in *sets*, not *procedures*.

---

## 4. ACID Transactions

Here's the scenario that keeps database engineers up at night:

```
Transfer $500 from Account A to Account B:
  1. Read A's balance: $1000
  2. Subtract $500 from A: write $500
  3. Add $500 to B: write...
  --- CRASH ---
```

The power goes out between steps 2 and 3. Account A lost $500. Account B never got it.
$500 has vanished from the universe.

**ACID** is the set of guarantees that prevents this:

### Atomicity — All or Nothing

A transaction is a group of operations that either all succeed or all fail. There is no
halfway. If the system crashes during a transfer, the entire transfer is rolled back.
Both accounts return to their pre-transaction state.

```sql
BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 500 WHERE id = 'A';
  UPDATE accounts SET balance = balance + 500 WHERE id = 'B';
COMMIT;
-- If anything fails between BEGIN and COMMIT, everything is rolled back.
```

### Consistency — Rules Always Hold

The database moves from one valid state to another. If you have a constraint that says
`balance >= 0`, no transaction can leave an account negative. A transaction that would
violate this is rejected entirely.

### Isolation — Concurrent Transactions Don't Interfere

Two people transferring money at the same time shouldn't produce impossible results.
Isolation means each transaction behaves *as if* it were running alone, even though
hundreds may be executing simultaneously.

There are levels of isolation (read uncommitted, read committed, repeatable read,
serializable), trading strictness for performance. Most databases default to "read
committed" as a pragmatic compromise.

### Durability — Committed Means Permanent

Once the database says "COMMIT successful," that data survives power failures, crashes,
and disk errors (assuming you have backups for disk failure). This is typically achieved
through a write-ahead log (WAL) — the database writes the intended changes to a log on
disk *before* modifying the actual data files.

### Why This Matters

Without ACID, you get lost money, oversold inventory, double bookings, and corrupted
state after crashes. But ACID is not free — every guarantee requires coordination,
locking, and disk writes. This is precisely why some systems deliberately weaken these
guarantees when correctness can be traded for speed.

---

## 5. How Databases Work Internally

Time to look under the hood. A database is not a black box — it's a stack of
well-defined components.

### Storage Engine: Data on Disk

Data lives on disk, read and written in fixed-size **pages** (typically 4-8KB) — the
minimum unit a disk can transfer.

**Heap file:** Rows appended in insertion order, packed into pages. Finding a specific
row means scanning every page — O(n). Fine for small tables, terrible for large ones.

**Row vs column storage:**
- **Row-oriented** (PostgreSQL, MySQL): Pages contain complete rows. Great for OLTP
  workloads where you read/write entire records.
- **Column-oriented** (ClickHouse, Redshift): Pages contain values from a single
  column. Great for OLAP analytics across millions of rows but few columns. Compresses
  beautifully because adjacent values share a type.

### B-Tree Indexes

This is the workhorse of database indexing. You studied B-trees in data structures — now
here's why they matter.

A B-tree index on `customers.city` creates a balanced tree where:
- Internal nodes contain keys and pointers to child nodes
- Leaf nodes contain keys and pointers to the actual rows on disk
- The tree stays balanced, so every lookup is **O(log n)** — even with billions of rows

Why B-trees and not binary search trees? Because disk I/O is expensive. A B-tree node
holds hundreds of keys (sized to fit one disk page), so the tree is very shallow.
A billion-row table might need only 3-4 disk reads to find any row. Binary search trees
would need ~30 levels — 30 disk reads.

```
                    [Garcia | Mueller | Tanaka]
                   /          |           \
          [Adams, Chen]  [Kim, Lee]   [Wang, Zhang]
          /    |    \    /    |   \    /    |     \
        [...] [...] [...] [...] [...] [...] [...]  [...]
         ^
         Leaf nodes point to actual rows on disk
```

### Hash Indexes

For exact-match lookups (`WHERE id = 42`), a hash index gives you **O(1)** access.
Hash the key, go directly to the page.

The catch: hash indexes can't do range queries (`WHERE age BETWEEN 20 AND 30`) or
ordering (`ORDER BY age`). The hash destroys ordering. This is why B-trees are the
default index type in nearly every database.

### Query Optimizer

When you write:

```sql
SELECT c.name, SUM(o.amount)
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE c.city = 'Tokyo'
GROUP BY c.name;
```

The database doesn't execute this literally. The **query optimizer** considers many
possible execution plans:

- **Table scan:** Read every row. Simple, but O(n).
- **Index scan:** Use an index on `city` to find Tokyo customers, then look up their
  orders. Much faster if Tokyo customers are a small fraction.
- **Index-only scan:** If the index contains all needed columns, don't read the table
  at all.
- **Join order:** Should we start with customers and find their orders, or start with
  orders and find their customers? The answer depends on table sizes and filter
  selectivity.

The optimizer uses **statistics** (row counts, value distributions, index availability)
to estimate the cost of each plan and picks the cheapest one. Use `EXPLAIN ANALYZE` to
see the chosen plan — it's one of the most important tools in database work.

### Write-Ahead Log (WAL)

Before modifying any data page, the database writes the change to a sequential log
file. Sequential writes are much cheaper than random writes, so this is fast.

On crash recovery: read the WAL, replay any changes that weren't applied to data
files, roll back incomplete transactions. The WAL turns random writes into sequential
writes — a massive performance win that makes Durability practical.

### Buffer Pool

The **buffer pool** is a cache of disk pages held in RAM. Every read checks the buffer
pool first — cache hit means no disk I/O. Cache miss triggers a disk read, and
something old gets evicted (usually LRU). A well-tuned database serves most reads
from the buffer pool. Databases love RAM not for computation, but for caching.

---

## 6. NoSQL — When Relational Doesn't Fit

The relational model is powerful, but it makes trade-offs: fixed schemas need
migrations, JOINs can be slow, horizontal scaling is hard with strong consistency, and
some data (graphs, documents, time series) doesn't fit neatly into tables.

The "NoSQL" movement (2000s-2010s) produced four main families:

### Key-Value Stores

**What:** A giant hash table. You store and retrieve values by key.

**Examples:** Redis, Amazon DynamoDB, Memcached

**Use when:** Blazing-fast lookups by known key. Session storage, caching, feature
flags, leaderboards. Redis keeps everything in RAM — microsecond reads.

**Trade-off:** No complex queries. You need to know the key.

### Document Stores

**What:** Store JSON-like documents. Each document can have a different structure.

**Examples:** MongoDB, CouchDB, Firestore

```json
{
  "_id": "order_103",
  "customer": { "name": "Garcia", "city": "Madrid" },
  "items": [
    { "product": "Laptop", "price": 999.00 },
    { "product": "Mouse", "price": 29.00 }
  ],
  "total": 1028.00
}
```

**Use when:** Your data is naturally hierarchical (nested objects), your schema evolves
rapidly, or you want to read an entire entity in one operation without JOINs.

**Trade-off:** Relationships between documents are awkward. If you embed the customer
in every order, updating the customer means updating every order. If you reference by
ID, you're reinventing foreign keys without the database's help.

### Column-Family Stores

**What:** Rows with dynamic columns, optimized for write-heavy workloads and wide rows.

**Examples:** Apache Cassandra, HBase

**Use when:** You're writing massive volumes of data (IoT sensors, event logs, time
series) across many machines and need high write throughput. Cassandra was built at
Facebook for inbox search.

**Trade-off:** Limited query patterns. You must design your tables around your queries
(the opposite of the relational approach where you normalize first, query however you
want later).

### Graph Databases

**What:** Store nodes and edges directly, optimized for traversing relationships.

**Examples:** Neo4j, Amazon Neptune, TigerGraph

**Use when:** Your core question is about relationships. Social networks (friends of
friends), fraud detection (suspicious transaction chains), recommendation engines,
knowledge graphs.

**Trade-off:** Not great for bulk analytical queries. Overkill if your data is
fundamentally tabular.

### Schema-on-Write vs Schema-on-Read

This is the deeper philosophical split:

- **Schema-on-write** (relational): Define structure before writing. The database
  rejects anything that doesn't fit. You pay upfront, but reads always find clean data.
- **Schema-on-read** (document/key-value): Write whatever you want; the application
  interprets structure at read time. Flexible to start, but complexity shifts to your
  code. (Eventually, every document store user builds an ad hoc schema validator.)

Neither is universally better. It depends on how well you understand your data model
and how often it changes.

---

## 7. Replication and Sharding

One machine has limits: disk space, I/O throughput, availability. When one machine
isn't enough, you distribute.

### Replication — Copies of Your Data

Replication means maintaining the same data on multiple machines.

**Why:** Two reasons.
1. **Read scaling:** Send reads to replicas, reducing load on the primary.
2. **Fault tolerance:** If one machine dies, another has the data.

Three architectures:

**Leader-follower:** One leader handles all writes; followers replicate and serve reads.
Simple and widely used (PostgreSQL, MySQL). If the leader dies, promote a follower.

**Multi-leader:** Multiple nodes accept writes and sync with each other. Useful for
multi-datacenter setups. Problem: conflicting writes need resolution, and that's hard.

**Leaderless:** Any node accepts reads and writes. Uses quorum: write to W nodes,
read from R nodes. If W + R > N, you're guaranteed fresh reads. Cassandra and DynamoDB
use this. Flexible, but you may read stale data.

### Sharding (Partitioning) — Splitting Your Data

Sharding means dividing data across multiple machines so that each machine holds a
subset.

**By key range:** Orders 1-1M on shard 1, 1M-2M on shard 2. Simple, supports range
queries, but prone to hotspots if recent data clusters on one shard.

**By hash:** Hash the key, mod by number of shards. Even distribution, but range
queries now hit every shard.

**The hard parts:** Cross-shard JOINs require coordination between machines. Adding
shards means rebalancing data (consistent hashing helps). Distributed transactions
need protocols like two-phase commit, which are slow and fragile. This is why
horizontal scaling of relational databases is genuinely difficult.

---

## 8. The CAP Theorem

Brewer's theorem (proved by Gilbert & Lynch, 2002): a distributed system can guarantee
at most two of three properties:

- **Consistency:** Every read returns the most recent write
- **Availability:** Every request gets a response
- **Partition tolerance:** The system works despite network partitions

Since partitions are inevitable, the real choice is between **C** and **A** during one:

**CP systems:** Refuse requests during partitions rather than return stale data. HBase,
MongoDB (default config), etcd.

**AP systems:** Keep serving during partitions, accepting possibly stale reads. Nodes
reconcile later. Cassandra, DynamoDB.

### PACELC — A Better Framework

CAP only covers partition scenarios. PACELC extends it: during a **P**artition, choose
**A** or **C**. **E**lse (normal operation), choose **L**atency or **C**onsistency.

| System    | During Partition | Normal Operation |
|-----------|-----------------|------------------|
| DynamoDB  | Availability    | Latency          |
| Cassandra | Availability    | Latency          |
| MongoDB   | Consistency     | Latency          |
| HBase     | Consistency     | Consistency      |

Even without partitions, strong consistency costs latency because nodes must coordinate.

---

## 9. Major Databases in Practice

Choosing a database is an engineering decision, not a religious one:

| Database       | Type             | Best For                                      |
|---------------|------------------|-----------------------------------------------|
| **PostgreSQL** | Relational       | General purpose. Rich features (JSONB, full-text search, extensions). The default choice if you don't have a specific reason to pick something else. |
| **MySQL**      | Relational       | Web applications. Widely deployed, battle-tested. Powers much of the web (WordPress, many SaaS products). |
| **SQLite**     | Embedded relational | Single-process applications. No server needed — it's a library that reads/writes a single file. Mobile apps, desktop apps, embedded systems, local development. |
| **MongoDB**    | Document         | Rapid prototyping, content management, catalogs. When your schema genuinely varies per record. |
| **Redis**      | Key-value (in-memory) | Caching, session storage, rate limiting, real-time leaderboards. Microsecond reads. |
| **Cassandra**  | Column-family    | High write throughput at massive scale. Time-series data, event logging, IoT. No single point of failure. |
| **DynamoDB**   | Key-value / Document | Serverless applications on AWS. Fully managed, scales automatically. Pay-per-request pricing. |
| **Elasticsearch** | Search engine | Full-text search, log analytics, real-time dashboards. Built on Apache Lucene. Not a primary data store — use alongside one. |

A common real-world pattern: PostgreSQL as the source of truth, Redis as a cache,
Elasticsearch for search. Different tools for different access patterns.

---

## Key Mental Models

**1. Everything is a trade-off between reads and writes.**
An index speeds up reads but slows down writes (because the index must be updated).
Normalization speeds up writes (update in one place) but slows down reads (JOIN
required). Denormalization speeds up reads but makes writes dangerous. There's no
free lunch.

**2. The storage hierarchy dictates design.**
RAM: ~100ns access. SSD: ~100us. HDD: ~10ms. That's 5 orders of magnitude. Every
database design decision — B-tree fanout, buffer pools, WAL, batch writes — exists
because of these numbers. If RAM were as cheap as disk, databases would be trivially
simple.

**3. Distributed systems multiply complexity non-linearly.**
Going from one database server to two is harder than going from two to ten. The
fundamental difficulty isn't scaling — it's maintaining correctness (consistency,
durability, ordering) when machines can fail independently and networks can partition.

**4. Your access patterns determine your database.**
Don't pick a database and then design your access patterns around it. Understand how
your application reads and writes data, then pick the database that fits. A social
graph needs a graph database. A cache needs a key-value store. A financial ledger
needs ACID transactions.

---

## How This Connects

- **Operating Systems** gave you processes, memory, and file systems. Databases build
  directly on top of these — OS files for storage, OS memory for buffer pools, OS
  concurrency primitives for transaction isolation.
- **Data Structures & Algorithms** gave you B-trees and hash tables — the backbone of
  every database index.
- **Networking** (next chapter) shows how databases talk to applications and how
  distributed databases coordinate across the network.
- **Distributed Systems** goes deeper into consensus (Paxos, Raft) and the theoretical
  limits of distributed computation.

---

## Hands-On Projects

1. **Build a SQL query portfolio.** Install PostgreSQL locally. Load a real dataset
   (Kaggle has hundreds). Write 20 queries of increasing complexity: filters, joins,
   subqueries, window functions, CTEs. Use `EXPLAIN ANALYZE` on each and learn to
   read execution plans.

2. **Build a key-value store from scratch.** In your language of choice, implement a
   simple persistent key-value store using an append-only log file. Then add a hash
   index. Then add compaction (merging old log segments). You've just built a toy
   version of Bitcask, the storage engine behind Riak.

3. **Model and normalize a real domain.** Pick something you know (a library system,
   a restaurant, an e-commerce store). Design the schema: identify entities,
   relationships, primary keys, foreign keys. Normalize to 3NF. Then denormalize
   selectively and explain why.

4. **Compare databases head-to-head.** Write a small application that stores and
   retrieves data from both PostgreSQL and MongoDB. Measure insert throughput, query
   latency, and schema evolution effort. Form your own opinions.

5. **Explore replication.** Set up a PostgreSQL leader-follower pair using Docker.
   Write to the leader, read from the follower. Kill the leader and practice failover.

---

## Go Deeper

- **Designing Data-Intensive Applications** by Martin Kleppmann — The single best
  book on databases and distributed data systems. Covers everything in this chapter
  and more, with exceptional clarity. If you read one book, make it this one.

- **Database Internals** by Alex Petrov — A deep dive into how storage engines,
  B-trees, distributed databases, and consensus protocols actually work. More
  implementation-focused than DDIA.

- **The Art of PostgreSQL** by Dimitri Fontaine — A practical guide to writing
  excellent SQL and understanding PostgreSQL's strengths.

- **Use The Index, Luke** (use-the-index-luke.com) — A free, practical guide to
  database indexing and SQL performance. Invaluable for anyone who writes queries.

- **CMU Database Group lectures** (Andy Pavlo) — Freely available university lectures
  covering database internals at a graduate level. Both the intro and advanced courses
  are excellent.

---

*Next: [Networking & The Internet](networking.md) — How do computers talk to each other across the planet?*
