# The Complete Map of Computer Science

> Computer science is a tower built on a single foundation — the transistor,
> a tiny switch controlled by electricity. Every layer exists because the layer
> below it couldn't solve a problem on its own. Understand the tower, and you
> understand the system that runs the modern world.

---

## How This Map Works

Computer science has eight layer groups. Each group builds on the ones below it — nothing at a higher layer can exist without the layers beneath it. This is the dependency structure of all computing.

---

## Layer Group 1: Hardware — From atoms to processors

Before there is software, there is physics. These three layers turn electrons into a machine that can compute.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Physics & Electricity](01-hardware/physics-and-electricity.md) | Electrons, semiconductors, transistors, chip fabrication | We need a physical switch that electricity can control. The transistor is that switch. |
| [Logic Gates & Boolean Algebra](01-hardware/logic-gates.md) | AND, OR, NOT, NAND; adders, flip-flops, multiplexers | Individual switches are useless. Combining them creates circuits that do math and remember. |
| [Computer Architecture](01-hardware/computer-architecture.md) | CPU, memory hierarchy, instruction sets, fetch-decode-execute | Logic gates alone can't run programs. Architecture organizes them into a general-purpose machine. |

---

## Layer Group 2: Systems — Software that manages hardware

Raw hardware can't run multiple programs, manage memory, or understand human-readable code. These layers solve that.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Operating Systems](02-systems/operating-systems.md) | Kernel, processes, virtual memory, file systems, scheduling | One CPU must serve many programs. The OS creates the illusion of infinite resources. |
| [Programming Languages & Compilers](02-systems/programming-languages.md) | Assembly, C, Python, Rust; compilers, interpreters, type systems | Humans can't write machine code at scale. Languages let us think in abstractions. |

---

## Layer Group 3: Theory — The science of computation

What can be computed? How fast? How do we organize information? These questions have answers independent of any particular machine.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Data Structures & Algorithms](03-theory/data-structures-and-algorithms.md) | Arrays, hash tables, trees, graphs; sorting, searching, Big-O, NP-completeness | Correct isn't enough — we need efficient. The difference between O(n) and O(n²) is the difference between "instant" and "heat death of the universe." |

---

## Layer Group 4: Infrastructure — The connective tissue

Single machines aren't enough. We need to store data permanently, connect machines across the planet, and make thousands of computers act as one.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Databases & Storage](04-infrastructure/databases.md) | Relational model, SQL, ACID, indexes, NoSQL, replication | RAM is volatile. We need data to survive power failures, and we need to query it fast. |
| [Networking & The Internet](04-infrastructure/networking.md) | TCP/IP, HTTP, DNS, routing, TLS, sockets, BGP | Isolated computers are limited. Connecting them multiplies their value by billions. |
| [Distributed Systems](04-infrastructure/distributed-systems.md) | Consensus, CAP theorem, MapReduce, message queues, microservices | One machine has limits. Distributing work across many machines introduces new, fundamental problems. |

---

## Layer Group 5: Security — Trust in a hostile world

Every layer above hardware is vulnerable. Security and cryptography are the immune system of the digital world.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Security & Cryptography](05-security/security-and-cryptography.md) | AES, RSA, hashing, TLS, authentication, common attacks | The internet is hostile. Without cryptography, no commerce, no privacy, no trust. |

---

## Layer Group 6: Engineering — The human side

Code that works once isn't enough. We need code that works forever, maintained by teams of humans who change over time.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Software Engineering](06-engineering/software-engineering.md) | Git, testing, CI/CD, system design, design patterns, technical debt | Software rots. Teams churn. Requirements change. Engineering practices fight entropy. |

---

## Layer Group 7: Intelligence — Machines that learn

Instead of programming every rule by hand, we let machines discover patterns from data.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [AI & Machine Learning](07-intelligence/machine-learning.md) | Neural networks, gradient descent, transformers, reinforcement learning | Some problems (vision, language, strategy) can't be solved by hand-written rules. Learning from data can. |

---

## Layer Group 8: Applications — The visible world

Everything users actually touch. Every app is a thin layer on top of this entire stack.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [The Application Layer](08-applications/applications.md) | Web, mobile, cloud, APIs, search engines, real-time systems | All the layers below are invisible to users. This layer is where the stack meets the human. |

---

## The Dependency Graph

```
Physics & Electricity
    │
    └── Logic Gates & Boolean Algebra
            │
            └── Computer Architecture (CPU, Memory, I/O)
                    │
                    ├── Operating Systems (processes, memory, files)
                    │       │
                    │       └── Programming Languages & Compilers
                    │               │
                    │               ├── Data Structures & Algorithms
                    │               │       │
                    │               │       ├── Databases & Storage
                    │               │       │
                    │               │       ├── Networking & The Internet
                    │               │       │       │
                    │               │       │       └── Distributed Systems
                    │               │       │
                    │               │       └── AI & Machine Learning
                    │               │
                    │               └── Software Engineering
                    │
                    └── Security & Cryptography (cross-cutting — touches everything)

                                            │
                                            ▼
                                    Applications (Web, Mobile, Cloud)
```

---

## What Powers What

| Technology | CS layers it runs on |
|---|---|
| Sending a text message | Physics → gates → CPU → OS → networking → encryption → app |
| Google Search | Distributed systems, algorithms (PageRank), databases, networking, ML |
| ChatGPT | Neural networks (transformers), GPU architecture, distributed training, networking, web |
| Video call (Zoom) | Networking (UDP/WebRTC), codecs (algorithms), OS (threads), security (encryption) |
| Banking app | Databases (ACID transactions), security (TLS, auth), networking, OS, architecture |
| Self-driving car | ML (vision, RL), OS (real-time), architecture (GPU), networking, security |
| Spotify streaming | Databases, networking (CDN), algorithms (compression, recommendation), distributed systems |
| Compiling code | Languages/compilers, OS (file system, processes), architecture (instruction set) |

---

## Recommended Study Order

**Phase 1 — Hardware:** Physics & Electricity → Logic Gates → Computer Architecture

**Phase 2 — Systems:** Operating Systems → Programming Languages & Compilers

**Phase 3 — Theory:** Data Structures & Algorithms

**Phase 4 — Infrastructure:** Databases → Networking → Distributed Systems

**Phase 5 — Security:** Security & Cryptography

**Phase 6 — Engineering:** Software Engineering

**Phase 7 — Intelligence:** AI & Machine Learning

**Phase 8 — Applications:** The Application Layer

---

## See Also

- [The Complete Map of Mathematics](../mathematics/00-map.md) — The mathematical foundations that CS is built on
- [The Complete Map of Business & Entrepreneurship](../business/00-map.md) — How technology becomes products, companies, and industries
