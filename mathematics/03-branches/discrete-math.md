# Discrete Mathematics — The Math of Computers

> Continuous math (calculus) studies things that flow smoothly.
> Discrete math studies things that jump — integers, graphs, logical statements, algorithms.
> Computers are discrete machines. This is their native mathematics.

**Prerequisites:** [Logic](../01-foundations/logic.md), [Set Theory](../01-foundations/set-theory.md), [Proof Methods](../01-foundations/proof-methods.md)
**What it enables:** Algorithm analysis, data structures, networks, databases, cryptography, programming language theory, combinatorial optimization.

---

## Why Discrete Math Exists

Calculus is the math of the continuous — smooth curves, flowing water, planets in orbit. But computers don't do continuous. They do discrete: bits, steps, states. They operate on integers, make binary choices, traverse finite structures.

Discrete math is the mathematical foundation for everything a computer does.

---

## 1. Combinatorics — The Art of Counting

How many ways can something happen? This question drives algorithms, probability, and cryptography.

### Fundamental principles

**Rule of product:** If task A has m outcomes and task B has n outcomes, doing A then B has m × n outcomes.
```
3 shirts × 4 pants = 12 outfits
```

**Rule of sum:** If task A has m outcomes and task B has n outcomes, doing A OR B has m + n outcomes (if they don't overlap).

### Permutations — Ordered arrangements

**How many ways to arrange n objects?** n! = n × (n-1) × ... × 2 × 1

```
3 books on a shelf: 3! = 6 arrangements
10 runners finishing a race: 10! = 3,628,800 orderings
```

**How many ways to choose and arrange k objects from n?**
```
P(n,k) = n!/(n-k)!
```

### Combinations — Unordered selections

**How many ways to choose k objects from n (order doesn't matter)?**
```
C(n,k) = n! / (k!(n-k)!)
```

Also written as (n choose k) or "n choose k."

```
Choose 3 cards from 52: C(52,3) = 22,100
Choose a 5-person team from 20: C(20,5) = 15,504
```

### The Binomial Theorem
```
(x + y)ⁿ = Σ C(n,k) · xᵏ · yⁿ⁻ᵏ    (k from 0 to n)
```

The coefficients form Pascal's triangle. This connects combinatorics to algebra and probability.

### Why counting matters for CS

**Complexity analysis:** If a brute-force search has C(n,k) possibilities, knowing that number tells you whether brute force is feasible.

**Cryptography:** RSA security rests on the combinatorial explosion of factoring. A 2048-bit number has ~2²⁰⁴⁸ possible factor pairs — astronomically many.

**Probability:** P(event) = (favorable outcomes) / (total outcomes). You need combinatorics to count both.

---

## 2. Graph Theory — The Math of Connection

A **graph** G = (V, E) consists of:
- **Vertices** (V): nodes, points
- **Edges** (E): connections between vertices

```
Social network: vertices = people, edges = friendships
Internet: vertices = routers, edges = links
Map: vertices = cities, edges = roads
```

### Types of graphs

| Type | Description |
|------|-----------|
| **Undirected** | Edges have no direction (friendships) |
| **Directed (digraph)** | Edges have direction (Twitter follows, web links) |
| **Weighted** | Edges have values (road distances, bandwidth) |
| **Tree** | Connected graph with no cycles |
| **DAG** | Directed acyclic graph — no directed cycles |
| **Bipartite** | Vertices split into two groups, edges only between groups |
| **Complete (Kₙ)** | Every vertex connected to every other |

### Key concepts

**Degree:** Number of edges at a vertex. In a social network, degree = number of friends.

**Path:** A sequence of vertices connected by edges. **Shortest path** problems are fundamental.

**Cycle:** A path that returns to its starting vertex.

**Connectivity:** A graph is **connected** if there's a path between every pair of vertices.

**Planarity:** Can the graph be drawn without edge crossings? (Maps always can; the complete graph K₅ cannot.)

### The most important graph algorithms

| Algorithm | Problem | Complexity |
|-----------|---------|-----------|
| **BFS** (Breadth-First Search) | Shortest path (unweighted), level-order traversal | O(V + E) |
| **DFS** (Depth-First Search) | Cycle detection, topological sort, connected components | O(V + E) |
| **Dijkstra's** | Shortest path (weighted, non-negative) | O((V+E) log V) |
| **Bellman-Ford** | Shortest path (negative weights allowed) | O(VE) |
| **Kruskal's / Prim's** | Minimum spanning tree | O(E log V) |
| **Topological sort** | Order tasks respecting dependencies | O(V + E) |

### Trees — The most important subclass

A **tree** is a connected graph with no cycles. Equivalently: n vertices, n-1 edges, connected.

```
Trees are everywhere:
- File systems (directory hierarchy)
- HTML/DOM (nested elements)
- Parse trees (compiler output)
- Decision trees (ML)
- Binary search trees (data structure)
- Spanning trees (network backbone)
```

### Euler and Hamilton

**Euler path:** Visit every EDGE exactly once. Exists iff 0 or 2 vertices have odd degree.
(The Königsberg bridge problem — the birth of graph theory, 1736.)

**Hamilton path:** Visit every VERTEX exactly once. Determining if one exists is NP-complete.

This gap — Euler is easy, Hamilton is (believed) hard — is a microcosm of the P vs NP question.

### What graph theory powers

| Application | Graph model |
|-------------|-------------|
| **Google Maps** | Weighted graph, shortest path (Dijkstra/A*) |
| **Social networks** | Friend graphs, community detection, influence propagation |
| **Internet routing** | Network graph, BGP, spanning trees |
| **Compilers** | Dependency graphs, control flow graphs, SSA form |
| **Scheduling** | DAGs with topological ordering |
| **Recommendation systems** | Bipartite user-item graphs |
| **PageRank** | Directed web graph, random walk, eigenvector |

---

## 3. Boolean Algebra — The Logic of Circuits

Boolean algebra operates on {0, 1} (or {true, false}) with three operations:

```
AND (∧):  0∧0=0, 0∧1=0, 1∧0=0, 1∧1=1
OR  (∨):  0∨0=0, 0∨1=1, 1∨0=1, 1∨1=1
NOT (¬):  ¬0=1, ¬1=0
```

This is the algebra from [Logic](../01-foundations/logic.md), restricted to two values and treated as actual computation.

### Key identities
```
De Morgan's:    ¬(a ∧ b) = ¬a ∨ ¬b
                ¬(a ∨ b) = ¬a ∧ ¬b
Absorption:     a ∧ (a ∨ b) = a
                a ∨ (a ∧ b) = a
XOR:            a ⊕ b = (a ∧ ¬b) ∨ (¬a ∧ b)
```

### Universal gates
NAND alone (or NOR alone) can build ANY Boolean function. Every digital circuit can be constructed from just one gate type. This is why NAND flash memory has that name.

### What Boolean algebra powers
- **CPU design:** Every logic gate, ALU, and control unit is Boolean algebra in silicon
- **Programming:** Boolean expressions in if/while/for conditions
- **Database queries:** WHERE clauses are Boolean predicates
- **Digital circuit design:** Karnaugh maps and Quine-McCluskey minimize Boolean functions for efficient circuits

---

## 4. Recurrences — Counting Recursive Things

A **recurrence relation** defines a sequence in terms of previous terms:

```
Fibonacci: F(n) = F(n-1) + F(n-2), F(0)=0, F(1)=1
Gives: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
```

### Solving recurrences (for algorithm analysis)

**The Master Theorem:** For recurrences of the form T(n) = aT(n/b) + f(n):

| Case | Condition | Result |
|------|-----------|--------|
| 1 | f(n) grows slower than n^(log_b a) | T(n) = Θ(n^(log_b a)) |
| 2 | f(n) grows at the same rate | T(n) = Θ(n^(log_b a) · log n) |
| 3 | f(n) grows faster | T(n) = Θ(f(n)) |

**Example:** Merge sort: T(n) = 2T(n/2) + O(n). Here a=2, b=2, f(n)=n, n^(log₂2)=n. Case 2: T(n) = Θ(n log n).

---

## 5. Formal Languages and Automata

**Formal languages** are sets of strings over an alphabet. They connect to both math and CS theory.

### The Chomsky Hierarchy

| Type | Language class | Machine that recognizes it | Example |
|------|---------------|---------------------------|---------|
| 3 | Regular | Finite automaton (DFA/NFA) | `a*b+` (regex) |
| 2 | Context-free | Pushdown automaton | Balanced parentheses |
| 1 | Context-sensitive | Linear bounded automaton | aⁿbⁿcⁿ |
| 0 | Recursively enumerable | Turing machine | Any computable language |

**Regular expressions** (Type 3) power text search (grep, regex in programming).
**Context-free grammars** (Type 2) power programming language parsers.
**Turing machines** (Type 0) define the boundary of what is computable AT ALL.

### The Halting Problem
No algorithm can determine, for every program-input pair, whether the program halts. This is proved using diagonalization (same technique as Cantor's uncountability proof). It's the most important impossibility result in CS.

---

## 6. Pigeonhole Principle and Extremal Combinatorics

**Pigeonhole:** n+1 items into n containers → some container has ≥ 2 items.

Seemingly trivial, it proves deep results:
- In any group of 13 people, at least 2 share a birth month
- Any lossless compression algorithm makes some files LARGER (not all files can shrink)
- In any sequence of n²+1 distinct numbers, there's a monotone subsequence of length n+1

**Ramsey theory:** Sufficiently large structures must contain ordered substructures. "Complete disorder is impossible." For any coloring of edges of a large enough complete graph, you'll find a monochromatic clique.

---

## Key Insight

Discrete math is the native mathematical language of computation. Every algorithm traverses a graph, every conditional is Boolean logic, every complexity analysis uses combinatorics and recurrences, every programming language is a formal language. While calculus describes the continuous natural world, discrete math describes the digital one we've built.

---

**Next:** [Probability & Statistics](probability.md) — the mathematics of uncertainty.
