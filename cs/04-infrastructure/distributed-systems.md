# Distributed Systems — How Thousands of Computers Act as One

> A single computer is fast, but fragile and finite. The moment you need
> more storage than one disk, more throughput than one CPU, or more
> reliability than one power supply, you enter the world of distributed
> systems — where everything that could go wrong *will* go wrong, and
> you have to build correct systems anyway.

**Prerequisites:** [Networking](networking.md), [Databases](databases.md) — You need network communication and data persistence.
**What it enables:** Cloud computing, global-scale applications, fault-tolerant services.

---

## 1. Why Distribute?

Three forces push us toward distributed systems:

**Scale.** A single machine has limits — finite RAM, finite disk, finite CPU cycles per second. Google processes over 8.5 billion searches per day. No single machine on Earth can handle that. You *must* spread the work across thousands of machines.

**Fault tolerance.** Hard drives fail. Power supplies die. Entire datacenters go offline when a backhoe cuts a fiber cable. If your system runs on one machine, that machine is a **single point of failure**. Distribute the work, and any individual failure becomes survivable.

**Latency.** Physics is non-negotiable. Light takes about 130 milliseconds to travel from New York to Tokyo through fiber. If your server is in Virginia and your user is in Mumbai, every request pays a round-trip tax. Put servers near your users, and the speed of light becomes your friend instead of your enemy.

### The Cost

Distribution is not free. The moment you spread computation across multiple machines connected by a network, *everything gets harder*:

- **Consistency** — How do you make sure all machines agree on the current state?
- **Partial failure** — Some nodes can fail while others keep running. Your system is in a state that doesn't happen on a single machine.
- **Debugging** — A bug that only manifests when message A arrives at node 3 before message B arrives at node 7, under network congestion, is nightmarishly hard to reproduce.
- **Coordination** — Getting machines to do the right thing in the right order requires protocols that are subtle, complex, and easy to get wrong.

The golden rule: **don't distribute unless you have to.** A single powerful machine is simpler, faster for many workloads, and far easier to reason about. Only distribute when the three forces above demand it.

---

## 2. The Fallacies of Distributed Computing

In 1994, Peter Deutsch (with additions from James Gosling) articulated **eight assumptions** that newcomers to distributed computing always make — and that are always wrong:

1. **The network is reliable.** Packets get dropped, connections time out, switches fail. You must handle every request potentially failing.

2. **Latency is zero.** Even on a local network, a round trip takes hundreds of microseconds. Across the internet, it's tens to hundreds of milliseconds. Chatty protocols that make many small requests are punished severely.

3. **Bandwidth is infinite.** Transferring a terabyte of data across a network takes real time. Sometimes it's faster to ship a hard drive (this is called "sneakernet," and AWS literally offers a truck for it — the Snowmobile).

4. **The network is secure.** Every message can be intercepted, modified, or replayed. You must authenticate and encrypt.

5. **Topology doesn't change.** Nodes join and leave. Network paths shift. Load balancers reroute traffic. Your system must handle a network that is constantly reconfiguring itself.

6. **There is one administrator.** In reality, your distributed system spans teams, organizations, and cloud providers, each with their own policies, update schedules, and failure modes.

7. **Transport cost is zero.** Serializing data, sending it over the wire, deserializing it on the other end — all of this takes CPU, memory, and time. It's not free.

8. **The network is homogeneous.** You'll deal with different operating systems, different hardware, different network stacks, different versions of your own software running simultaneously.

Why do these matter? Because every distributed systems bug you'll ever encounter traces back to violating one of these assumptions. You assumed the network was reliable, so you didn't add retry logic. You assumed latency was zero, so you made 500 sequential remote calls where one batched call would do. These fallacies aren't academic — they're a checklist of things that *will* bite you.

---

## 3. Failure Models

On a single machine, failure is usually total: the machine is either working or it's not. In a distributed system, failure comes in shades.

### Crash Failures

The simplest kind: a node **stops and doesn't come back** (crash-stop) or **stops and eventually recovers** (crash-recovery). The node doesn't do anything wrong — it just stops doing anything at all.

This is the failure model most algorithms assume, because it's the easiest to reason about. If a node crashes, the remaining nodes can detect its absence (via heartbeat timeouts) and proceed.

### Omission Failures

Messages get **lost**. The node is running fine, but the network drops a packet. The sender sends a request; the receiver never gets it. Or the receiver sends a response; the sender never gets it.

From the sender's perspective, an omission failure looks exactly like a crash failure — you sent a message and got no response. You can't tell whether the receiver is dead or the network ate your message. This ambiguity is the root of enormous complexity.

### Byzantine Failures

The scariest kind: a node **behaves arbitrarily**. It might send wrong data, send conflicting messages to different nodes, or actively try to sabotage the system. Named after the **Byzantine Generals Problem** (Lamport, 1982).

Byzantine faults are rare in a controlled datacenter (though hardware bugs and software bugs can produce Byzantine-like behavior). They're common in adversarial environments like public blockchains, where nodes are controlled by untrusted parties.

**The cost of Byzantine fault tolerance is steep.** To tolerate `f` Byzantine nodes, you need at least `3f + 1` total nodes. That's because you need enough honest nodes to outvote the liars even when the liars coordinate. To tolerate 1 Byzantine node, you need 4 total. To tolerate 10, you need 31. This overhead is why most internal distributed systems don't bother with BFT — they assume crash failures only.

```
Failure Model Spectrum:
  
  Crash-stop → Crash-recovery → Omission → Byzantine
  ─────────────────────────────────────────────────────►
  Easiest to handle              Hardest to handle
  Fewest assumptions             Most assumptions
  Cheapest                       Most expensive
```

---

## 4. Time and Ordering

Here's a problem that doesn't exist on a single machine: **what time is it?**

On one machine, there's one clock. Events happen in a clear order. In a distributed system, each machine has its own clock, and those clocks **drift**. Even with NTP (Network Time Protocol), clocks across machines can disagree by milliseconds. That's an eternity when your database is processing thousands of transactions per second.

### Why Ordering Matters

Imagine two users, Alice and Bob, both editing a shared document:
- Alice changes the title to "Draft v2" at her local time 10:00:00.000
- Bob changes the title to "Final Version" at his local time 10:00:00.001

Bob's change was 1 millisecond later, so it should win, right? But what if Alice's clock was 5 milliseconds ahead? In real time, Bob's edit actually happened first. Without a reliable shared clock, you can't tell.

### Happened-Before (Lamport, 1978)

Leslie Lamport had a brilliant insight: **forget about real time. Focus on causality.**

Event A **happened-before** event B (written A → B) if:
1. A and B are on the same node, and A occurred first, OR
2. A is a message send and B is the corresponding receive, OR
3. There exists some event C such that A → C and C → B (transitivity)

If neither A → B nor B → A, the events are **concurrent** — there's no causal relationship between them. This isn't a limitation; it's a feature. We only need to order events that are causally related.

### Lamport Clocks

Each node maintains a counter. Before each event, increment the counter. When sending a message, include the counter. When receiving a message, set your counter to `max(your_counter, received_counter) + 1`.

This gives you a **logical clock** that respects causality: if A → B, then `clock(A) < clock(B)`. But the converse isn't true — `clock(A) < clock(B)` doesn't mean A → B. Lamport clocks can't detect concurrency.

### Vector Clocks

To fix this, use a **vector clock**: each node maintains a vector of counters, one per node. Now you can tell the full story:
- If every entry in clock(A) ≤ corresponding entry in clock(B), with at least one strict <, then A → B
- If neither clock dominates the other, A and B are concurrent

```
Node 1: [2, 0, 0]  sends msg to Node 2
Node 2: [2, 1, 0]  receives, merges
Node 3: [0, 0, 1]  independent event — concurrent with both

Comparing [2, 1, 0] and [0, 0, 1]:
  2 > 0, but 0 < 1 → neither dominates → CONCURRENT
```

Vector clocks are more expensive (they grow with the number of nodes) but give you the full causal picture. Systems like Amazon's Dynamo used them to detect conflicting writes.

---

## 5. Consensus

The **fundamental problem** of distributed systems: getting N nodes to agree on a single value.

This sounds trivially easy. Node 1 proposes "yes," node 2 proposes "no," they vote, majority wins. But what happens if node 1 crashes before sending its vote? What if the network partitions and nodes 1-3 can't talk to nodes 4-5? What if messages arrive in different orders?

### The FLP Impossibility (1985)

Fischer, Lynch, and Paterson proved a devastating result: **in an asynchronous system where even one node can crash, there is no algorithm that guarantees consensus will be reached in bounded time.**

"Asynchronous" means there's no upper bound on message delivery time — a message might take 1 millisecond or 1 hour, and you can't tell the difference between a slow node and a dead one.

This doesn't mean consensus is impossible in practice. It means you can't have all three of: safety (never agree on wrong value), liveness (always eventually decide), and fault tolerance. Practical algorithms work around FLP by using timeouts, randomization, or partial synchrony assumptions.

### Paxos

Invented by Leslie Lamport (published in 1998, conceived much earlier), Paxos is the **classic consensus algorithm**. It's also notoriously hard to understand — Lamport himself published a second paper called "Paxos Made Simple" because nobody understood the first one.

The basic flow of single-decree Paxos (agreeing on one value):

**Phase 1 — Prepare:**
1. A **proposer** picks a proposal number `n` and sends `Prepare(n)` to a majority of **acceptors**
2. Each acceptor responds with a **promise** not to accept any proposal numbered less than `n`, plus any value it has already accepted

**Phase 2 — Accept:**
1. If the proposer receives promises from a majority, it sends `Accept(n, value)` — using either its own value or the highest-numbered value from the promises
2. Each acceptor accepts the proposal if it hasn't promised to a higher number

**Phase 3 — Learn:**
1. Once a majority of acceptors accept, the value is **chosen**
2. Learners are notified of the result

The key insight: requiring a majority at every step ensures that any two majorities overlap, so the system can never choose two different values.

In practice, Multi-Paxos extends this to agree on a sequence of values (a log), and uses a stable leader to skip Phase 1 in the common case, making it much more efficient.

### Raft

Diego Ongaro and John Ousterhout designed Raft in 2014 specifically because Paxos was too hard to teach, implement, and debug. Raft achieves the same safety guarantees but breaks the problem into three clean sub-problems:

**Leader Election:**
- Nodes are in one of three states: follower, candidate, or leader
- If a follower doesn't hear from a leader within a random timeout, it becomes a candidate and requests votes
- A candidate that gets votes from a majority becomes the leader
- The random timeout prevents split votes (usually)

**Log Replication:**
- The leader receives client requests, appends them to its log, and replicates them to followers
- Once a majority of followers acknowledge an entry, it's **committed**
- The leader then applies the entry to its state machine and responds to the client

**Safety:**
- A candidate can only win an election if its log is at least as up-to-date as a majority of nodes
- This ensures the leader always has all committed entries

Raft is the basis of etcd (used by Kubernetes), CockroachDB, TiKV, and many other systems.

### Why Consensus Matters

Consensus isn't just an academic exercise. You need it for:
- **Leader election** — Picking which node is the primary in a replicated database
- **Distributed locks** — Ensuring only one process accesses a resource at a time
- **Configuration management** — Ensuring all nodes agree on the current cluster membership
- **Atomic commit** — Ensuring all participants in a distributed transaction agree to commit or abort

---

## 6. Consistency Models

When data is replicated across multiple nodes, you have to decide: **what guarantees do readers get?** This is the consistency model, and it's a spectrum — not a binary choice.

### Linearizability (Strong Consistency)

The system behaves as if there's a single copy of the data. Every read returns the most recent write. If Alice writes a value and then tells Bob (even out-of-band, via phone), Bob's read will see Alice's write.

This is the easiest model to reason about — it matches our single-machine intuition. But it's expensive: every write must be acknowledged by a majority of replicas before returning, and reads may need to check with the leader or a quorum.

**Used by:** ZooKeeper (for its core operations), etcd, Spanner (Google's globally distributed database using GPS clocks and atomic clocks to achieve this).

### Sequential Consistency

All nodes see operations in the **same order**, but that order doesn't need to match real-time. If Alice writes A then B, everyone sees A before B. But Alice's write might not be visible to Bob for a while.

Think of it like a single-threaded program executing all operations — the order is consistent, but there's no guarantee about *when* each operation is "seen."

### Causal Consistency

If event A **caused** event B (A happened-before B), then everyone sees A before B. But concurrent events (with no causal relationship) can appear in different orders on different nodes.

This is weaker than sequential consistency but captures what most applications actually need: if I post a message and then post a reply, everyone should see my original message before my reply. But two independent posts by different people can appear in either order.

### Eventual Consistency

The weakest useful guarantee: if you stop writing, **eventually** all replicas will converge to the same state. But "eventually" might mean seconds, minutes, or longer. While converging, different replicas may return different values for the same key.

This is the model used by DNS, many NoSQL databases, and most CDN caches. It's fast (writes are acknowledged immediately by any replica) but requires applications to tolerate stale reads.

### The Spectrum

```
Strong ◄──────────────────────────────────────────► Weak

Linearizable → Sequential → Causal → Eventual

Easier to                               Easier to
reason about                            make fast
Harder to                               Harder to
make fast                               reason about
```

Most real systems don't pick one extreme. They offer tunable consistency — for example, Cassandra lets you choose per-query whether you want ONE, QUORUM, or ALL replicas to respond.

### The CAP Theorem

Eric Brewer's CAP theorem (2000, proven by Gilbert and Lynch in 2002) states that a distributed system can provide at most two of three guarantees:

- **Consistency** (every read gets the most recent write)
- **Availability** (every request gets a response)
- **Partition tolerance** (the system works despite network partitions)

Since network partitions *will* happen in any real distributed system, the real choice is between **CP** (consistent but may be unavailable during partitions) and **AP** (available but may return stale data during partitions).

In practice, CAP is more of a guiding intuition than a precise engineering tool. The PACELC model is more nuanced: during a **P**artition, choose **A** or **C**; **E**lse (normal operation), choose **L**atency or **C**onsistency.

---

## 7. Replication Patterns

Once you've decided to have multiple copies of your data, you need a strategy for keeping them in sync.

### Single-Leader Replication

One node is the **leader** (also called primary or master). All writes go to the leader. The leader streams changes to **followers** (replicas or secondaries). Reads can go to any replica.

```
  Client Writes           Client Reads
       │                  │    │    │
       ▼                  ▼    ▼    ▼
   ┌────────┐         ┌────┐┌────┐┌────┐
   │ Leader │────────►│ F1 ││ F2 ││ F3 │
   │ (write)│  sync   │    ││    ││    │
   └────────┘         └────┘└────┘└────┘
```

**Pros:** Simple, well-understood, no write conflicts.
**Cons:** Leader is a bottleneck for writes. Leader failure requires failover (electing a new leader), which can be tricky.

**Synchronous vs asynchronous replication:**
- Synchronous: leader waits for follower acknowledgment before confirming write. Guarantees the follower has the data. Slow.
- Asynchronous: leader confirms the write immediately, streams to followers in the background. Fast but risks data loss if the leader crashes before replicating.
- Semi-synchronous: wait for *one* follower to confirm (so at least two copies exist), stream to the rest asynchronously.

Most relational databases (PostgreSQL, MySQL) use single-leader replication.

### Multi-Leader Replication

Multiple nodes can accept writes. Each leader replicates its changes to all other leaders. This is essential for **cross-datacenter replication** — you don't want users in Europe writing to a leader in Virginia.

```
  Datacenter A          Datacenter B
  ┌────────┐            ┌────────┐
  │Leader A│◄──────────►│Leader B│
  │        │  async     │        │
  └───┬────┘  repl.     └───┬────┘
      │                     │
   ┌──┴──┐              ┌──┴──┐
   │Foll.│              │Foll.│
   └─────┘              └─────┘
```

**The big problem:** write conflicts. If user A updates a row via Leader A and user B updates the same row via Leader B at the same time, you have a conflict. You must resolve it somehow.

### Leaderless Replication

No designated leader at all. Any node can accept reads and writes. To write, the client sends the write to **multiple replicas** simultaneously. To read, the client reads from **multiple replicas** and takes the most recent value.

This is the **Dynamo-style** approach (Amazon's Dynamo paper, 2007). It uses quorum-based reads and writes:

- **N** = total replicas
- **W** = number of replicas that must acknowledge a write
- **R** = number of replicas that must respond to a read
- As long as **W + R > N**, the read set and write set overlap, so the reader will see the latest value

Common configuration: N=3, W=2, R=2. This tolerates one node failure for both reads and writes.

Cassandra, Riak, and Voldemort use this pattern.

### Conflict Resolution

When multiple writes can happen concurrently (multi-leader or leaderless), conflicts are inevitable. Strategies:

**Last-Writer-Wins (LWW):** Attach a timestamp to each write. The highest timestamp wins. Simple, but lossy — concurrent writes are silently discarded. Also, timestamps can be unreliable (clock skew).

**CRDTs (Conflict-free Replicated Data Types):** Data structures designed so that concurrent updates can always be merged automatically without conflict. Examples: counters (add all increments), sets (union), registers (track all concurrent values). Mathematically guaranteed to converge. Limited to data types where a merge function exists.

**Application-level resolution:** Detect conflicts and let the application decide. For example, show the user both versions and let them pick (like Google Docs' conflict resolution for offline edits).

---

## 8. Partitioning / Sharding

Replication gives you copies of the same data. **Partitioning** (also called **sharding**) splits different data across different machines. You partition when one machine can't hold all your data, or when one machine can't handle all the queries.

### Range Partitioning

Assign a range of keys to each partition. Keys "a" through "m" go to partition 1, "n" through "z" go to partition 2.

**Pros:** Range queries are efficient — to find all users with names starting with "J", you know exactly which partition to ask.
**Cons:** Hotspots. If most of your data has keys in the "s" range (say, lots of users named "Smith"), one partition gets hammered while others sit idle.

### Hash Partitioning

Hash each key and assign it to a partition based on the hash value. `partition = hash(key) mod N`.

**Pros:** Even distribution (assuming a good hash function). No hotspots from skewed key distributions.
**Cons:** Range queries are destroyed. To find all users with names starting with "J", you'd need to query every partition.

### Consistent Hashing

Plain hash partitioning has a problem: when you add or remove a node, `hash(key) mod N` changes for almost every key, requiring a massive data reshuffle. **Consistent hashing** arranges the hash space in a ring, and each node is responsible for a section. Adding a node only requires redistributing keys from one neighbor, not from every node.

### Cross-Partition Queries

The big cost of partitioning: queries that span multiple partitions are **expensive**. A join between a user table partitioned by user_id and an order table partitioned by order_id requires contacting potentially all partitions of both tables. This is why denormalization and careful partition key choice are critical in distributed databases.

### Rebalancing

When you add new nodes or when data grows unevenly, you need to **rebalance** — move data between partitions. This must happen online (without downtime) and without killing performance. Most systems do this gradually, moving a few partitions at a time.

---

## 9. MapReduce and Batch Processing

In 2004, Jeff Dean and Sanjay Ghemawat at Google published a paper that changed how we think about large-scale data processing.

### The Insight

Most large-scale computations follow the same pattern:
1. Take a huge dataset
2. Process each piece independently (embarrassingly parallel)
3. Combine the results

MapReduce formalizes this into two programmer-defined functions:

**Map:** Takes an input key-value pair, produces intermediate key-value pairs.
**Reduce:** Takes an intermediate key and all its associated values, produces final output.

```
Input Data (distributed across machines)
    │
    ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│  Map 1  │ │  Map 2  │ │  Map 3  │    ← Run in parallel
└────┬────┘ └────┬────┘ └────┬────┘
     │           │           │
     ▼           ▼           ▼
   Shuffle (group by key, send to reducers)
     │           │           │
     ▼           ▼           ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│Reduce 1 │ │Reduce 2 │ │Reduce 3 │    ← Run in parallel
└────┬────┘ └────┬────┘ └────┬────┘
     │           │           │
     ▼           ▼           ▼
         Output (stored back to disk)
```

**Classic example — word count:**
- Map: for each word in a document, emit `(word, 1)`
- Shuffle: group all pairs by word
- Reduce: for each word, sum up the 1s

The framework handles distribution, parallelism, fault tolerance (re-run failed tasks), and data movement. The programmer just writes Map and Reduce.

### Hadoop

**Apache Hadoop** (2006) was the open-source implementation of MapReduce, built on top of **HDFS** (Hadoop Distributed File System, inspired by Google's GFS). It democratized large-scale data processing — suddenly any company could process petabytes.

### Evolution Beyond MapReduce

MapReduce writes intermediate results to disk between stages. For iterative algorithms (machine learning, graph processing), this is painfully slow.

**Apache Spark** (2014): Keeps intermediate data in memory using **Resilient Distributed Datasets (RDDs)**. This made iterative workloads 10-100x faster. Spark also provides a richer API — you can chain transformations (map, filter, join, group) without writing separate MapReduce jobs.

**Apache Flink** and similar systems blurred the line between batch and stream processing, enabling **unified** processing of bounded and unbounded data.

---

## 10. Stream Processing

Batch processing answers questions about data that already exists. **Stream processing** answers questions about data as it arrives.

### Message Brokers

The foundation of stream processing is the **message broker** — a system that receives messages from **producers** and delivers them to **consumers**.

**Apache Kafka** (2011, LinkedIn): A distributed commit log. Messages are written to **topics**, which are split into **partitions**. Each partition is an append-only log. Consumers track their position (offset) in the log and read forward. Messages are retained for a configurable period, so consumers can replay history.

Kafka is not just a message queue — it's a durable, ordered, replayable event log. This makes it a backbone for event-driven architectures.

**RabbitMQ:** A traditional message broker using the AMQP protocol. Messages are delivered to consumers and acknowledged. Once acknowledged, they're gone. Better for task queues where you don't need replay.

**Apache Pulsar:** Combines Kafka-style log storage with RabbitMQ-style queue semantics. Separates storage from compute.

### Event Sourcing

Instead of storing the current state of your data (like a traditional database), store every **event** that ever happened: "user created," "item added to cart," "payment processed." The current state is derived by replaying all events.

**Advantages:** Complete audit trail. Can reconstruct state at any point in time. Can derive new views (read models) from the same event stream. Natural fit for distributed systems.

**Disadvantages:** Event log grows forever. Rebuilding state from scratch is slow (mitigated by periodic snapshots). Schema evolution of events is tricky.

### CQRS (Command Query Responsibility Segregation)

Separate the **write model** (optimized for recording events) from the **read model** (optimized for querying). The write side appends events to a log. The read side consumes those events and builds materialized views tailored to specific query patterns.

This lets you have denormalized, query-optimized read stores without sacrificing the clean write model. The tradeoff: the read model is **eventually consistent** with the write model, because there's a propagation delay.

### Real-Time Analytics

Stream processing enables computations that would be impossible or impractical in batch:
- Fraud detection (flag suspicious transactions in milliseconds, not hours)
- Real-time dashboards and monitoring
- Dynamic pricing (adjust prices based on current demand)
- Anomaly detection (spot server failures from log streams)

---

## 11. Microservices vs. Monoliths

This isn't purely a distributed systems topic, but the choice between monolith and microservices is fundamentally a question about distribution.

### The Monolith

A **monolith** is a single deployable unit. All your code — user management, billing, search, recommendations — lives in one codebase, one process, one deployment.

**Advantages:**
- Simple to develop, test, and deploy (at first)
- Function calls between components are fast and reliable (no network)
- Transactions are easy (one database, ACID)
- No network failure modes between components

**Disadvantages:**
- As it grows, the codebase becomes unwieldy
- One team's change can break another team's feature
- You must deploy everything together — a small fix requires redeploying the whole system
- Scaling is all-or-nothing (you can't scale just the search component)

### Microservices

**Microservices** break the application into many small, independently deployable services, each owning its own data and communicating over the network (HTTP/REST, gRPC, message queues).

**Advantages:**
- Independent deployment: update one service without touching others
- Technology diversity: each service can use the best language/database for its job
- Independent scaling: scale the hot service, leave the cold ones alone
- Organizational alignment: each team owns a service end-to-end

**Disadvantages:**
- Network calls replace function calls (slower, less reliable)
- Distributed transactions are hard (no more easy ACID)
- Debugging requires distributed tracing across services
- Operational complexity: you're now running dozens or hundreds of services

### The Distributed Monolith Anti-Pattern

The worst of both worlds: you have many services, but they're so tightly coupled that you must deploy them together, they share a database, and a change in one requires changes in five others. You have all the complexity of microservices with none of the benefits.

This usually happens when you split the monolith along the wrong boundaries, or when services share too much — shared databases, shared libraries with business logic, synchronous call chains.

### Supporting Infrastructure

Microservices need infrastructure a monolith doesn't:

- **Service discovery:** How does service A find service B? (Consul, etcd, DNS-based)
- **API gateway:** A single entry point that routes requests to the right service and handles cross-cutting concerns (authentication, rate limiting)
- **Service mesh:** A dedicated infrastructure layer (like Istio or Linkerd) that handles service-to-service communication, retries, circuit breaking, and observability — without changing application code
- **Distributed tracing:** Following a request as it hops through 15 services (Jaeger, Zipkin)
- **Circuit breakers:** If service B is failing, stop calling it instead of cascading the failure (Hystrix pattern)

### When to Use Which

Start with a monolith. Seriously. Microservices are a solution to organizational and scaling problems that you probably don't have yet. Split when:
- Your team is large enough that people step on each other's toes
- You have clear domain boundaries
- You need to scale specific components independently
- You're willing to invest in the operational infrastructure

---

## 12. Real-World Distributed Systems

Theory matters, but let's see how the giants actually built things.

### Google

Google pioneered many of the ideas we've discussed:

**GFS (Google File System, 2003):** Distributed file system designed for large sequential reads and appends. Single master tracking metadata, many chunk servers storing 64MB chunks. Optimized for throughput over latency.

**MapReduce (2004):** We covered this above. Transformed how Google (and later the world) processed large datasets.

**Bigtable (2006):** A distributed wide-column store built on top of GFS. Think of it as a sorted map from `(row_key, column_key, timestamp)` to a value. Influenced HBase, Cassandra, and many others.

**Spanner (2012):** Google's globally distributed, strongly consistent database. The magic: it uses GPS clocks and atomic clocks to create **TrueTime**, an API that tells you the current time with bounded uncertainty. By waiting out the uncertainty, Spanner achieves external consistency (linearizability) across datacenters worldwide. This is the system that proved you don't *have* to give up strong consistency at global scale — if you're willing to invest in specialized hardware.

### Amazon

**Dynamo (2007):** A key-value store emphasizing availability over consistency. Popularized leaderless replication, consistent hashing, vector clocks for conflict detection, and "always writable" design. Its ideas directly influenced Cassandra, Riak, and Voldemort.

**S3 (Simple Storage Service):** Object storage at planetary scale. Stores trillions of objects. Designed for eleven nines (99.999999999%) of durability. Uses replication across multiple availability zones, with checksums to detect bit rot.

**Lambda (2014):** Serverless computing — upload a function, AWS runs it in response to events, scales automatically. You don't manage servers at all. This is distribution abstracted away entirely.

### Netflix

**Chaos Engineering:** Netflix famously runs **Chaos Monkey** (kills random instances), **Chaos Gorilla** (kills entire availability zones), and other tools that deliberately break their production system. The philosophy: if your system can't handle random failures, you'll discover that at the worst possible time. Better to break things on purpose during business hours.

**Microservices at Scale:** Netflix runs hundreds of microservices handling billions of requests per day. They pioneered many patterns: circuit breakers (Hystrix), client-side load balancing (Ribbon), service discovery (Eureka), and API gateway (Zuul).

**Key lesson from Netflix:** Resilience is a feature you build and test, not something you hope for.

---

## Key Mental Models

- **The network is the enemy.** Every network call can fail, be slow, or return stale data. Design for this from day one.
- **There are no global facts.** In a distributed system, each node has its own view of the world. Consensus is how you create shared truth, and it's expensive.
- **Everything is a tradeoff.** Consistency vs. availability. Latency vs. durability. Simplicity vs. scalability. Know which tradeoffs your system is making and why.
- **Failures are normal, not exceptional.** At Google's scale, hardware fails constantly — multiple disks, machines, and network links every day. Your architecture must treat failure as a routine event, not a crisis.

---

## How This Connects

- **Networking** gave you the communication primitives — TCP, UDP, DNS. Distributed systems build on these and deal with what happens when they fail.
- **Databases** gave you storage and transactions on one machine. Distributed databases extend these concepts across many machines, trading simplicity for scale.
- **Operating systems** taught you about concurrency on one machine (threads, locks, semaphores). Distributed systems face the same problems, but without shared memory — only messages.
- **Cloud computing** is distributed systems packaged as a service. Every AWS, GCP, or Azure product is a distributed system underneath.

---

## Hands-On Exercises

1. **Build a key-value store with replication.** Implement a simple key-value store with three nodes. Start with single-leader replication. Then simulate the leader crashing and implement failover.

2. **Implement Raft.** Work through the Raft paper and implement the leader election and log replication protocols. The Raft website (raft.github.io) has excellent visualizations and test suites.

3. **Experiment with consistency.** Set up a Cassandra cluster (even locally with Docker). Write data with consistency level ONE and read with ONE. Now try QUORUM. Observe the behavior when you kill a node.

4. **Build a MapReduce framework.** Implement a simple MapReduce in your language of choice. Distribute the Map phase across threads or processes, implement the shuffle, and run Reduce. Then try it across actual networked machines.

5. **Play with Kafka.** Set up a Kafka cluster, create topics, produce and consume messages. Build a simple stream processing pipeline: producers generate events, consumers maintain a real-time aggregate.

6. **Chaos engineering.** Deploy a multi-service application (even a toy one) and randomly kill services, add network latency, or drop packets. See what breaks. Fix it.

---

## Go Deeper

**The essential read:**
- **Designing Data-Intensive Applications** by Martin Kleppmann — The single best book on distributed systems in practice. Covers replication, partitioning, consistency, batch processing, stream processing, and the future of data systems. If you read one book on this topic, make it this one.

**Papers that changed the field:**
- *Time, Clocks, and the Ordering of Events in a Distributed System* (Lamport, 1978)
- *The Byzantine Generals Problem* (Lamport, Shostak, Pease, 1982)
- *Impossibility of Distributed Consensus with One Faulty Process* (Fischer, Lynch, Paterson, 1985)
- *Dynamo: Amazon's Highly Available Key-Value Store* (DeCandia et al., 2007)
- *In Search of an Understandable Consensus Algorithm* (Ongaro and Ousterhout, 2014) — the Raft paper
- *MapReduce: Simplified Data Processing on Large Clusters* (Dean and Ghemawat, 2004)
- *Spanner: Google's Globally Distributed Database* (Corbett et al., 2012)

**Other books:**
- **Distributed Systems** by Maarten van Steen and Andrew Tanenbaum — comprehensive textbook
- **Understanding Distributed Systems** by Roberto Vitillo — a concise, modern introduction

**Online resources:**
- MIT 6.824 (Distributed Systems) — lecture videos and labs freely available
- The Jepsen project (jepsen.io) — Kyle Kingsbury's rigorous testing of distributed databases (fascinating and humbling reading)
- Aphyr's "Call Me Maybe" series — accessible analyses of real distributed systems' failure modes

---

*Next: [Security & Cryptography](../05-security/security-and-cryptography.md) — How do we trust anything in a world of adversaries?*
