# The Complete Map of Mathematics

> Mathematics is a tree that grows from a single seed — the question
> "what can we prove to be true, starting from almost nothing?"
> Every branch exists because someone asked a question that the existing branches couldn't answer.

---

## How This Map Works

Mathematics has four layers. Each layer builds on the one below it — nothing at a higher layer can exist without the layers beneath it. This is the dependency structure of all mathematical knowledge.

---

## Layer 0: Foundations — What is truth? What is proof?

Before we can do any math, we need to agree on the rules of reasoning itself.

| Topic | What it is | Why it exists |
|-------|-----------|---------------|
| [Logic](01-foundations/logic.md) | AND, OR, NOT, implication, quantifiers | The rules of valid reasoning. Without this, nothing else works. |
| [Set Theory](01-foundations/set-theory.md) | Collections of objects, ZFC axioms | Everything in math is built from sets — numbers, functions, spaces. |
| [Proof Methods](01-foundations/proof-methods.md) | Direct proof, contradiction, induction | The "programming languages" of mathematical reasoning. |

---

## Layer 1: Core Structures — The atoms of mathematics

With logic and sets in hand, we can build the objects that all of mathematics operates on.

### Numbers & Arithmetic

| Topic | What it is | The question that created it |
|-------|-----------|------------------------------|
| [Number Systems](02-core-structures/number-systems.md) | N, Z, Q, R, C | Each number system exists because the previous one couldn't answer a question. |

The chain: **N** (natural) → "what if we could subtract?" → **Z** (integers) → "what if we could divide?" → **Q** (rationals) → "what about the gaps?" → **R** (reals) → "what is sqrt(-1)?" → **C** (complex)

### Algebra — The study of structure

| Topic | What it is | Why it matters |
|-------|-----------|----------------|
| [Algebra](02-core-structures/algebra.md) | Groups, rings, fields, vector spaces | The study of structure itself — symmetry, operations, and their properties. |
| [Linear Algebra](02-core-structures/linear-algebra.md) | Vectors, matrices, transformations, eigenvalues | THE workhorse of applied math. Powers ML, graphics, quantum computing, everything. |

### Analysis — The study of change and limits

| Topic | What it is | Why it matters |
|-------|-----------|----------------|
| [Analysis](02-core-structures/analysis.md) | Limits, continuity, convergence | What does "approaches" mean precisely? The rigorous foundation under calculus. |
| [Calculus](02-core-structures/calculus.md) | Derivatives and integrals | The math of change and accumulation. Newton and Leibniz's gift to civilization. |

---

## Layer 2: Major Branches — Built on Layer 1

Each branch was born because the core structures alone couldn't answer some class of questions.

### Discrete Mathematics — The math of computers

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Discrete Math](03-branches/discrete-math.md) | Combinatorics, graph theory, Boolean algebra, automata | Algorithms, networks, circuits, computability theory. |

### Geometry & Topology — The math of shape and space

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Geometry & Topology](03-branches/geometry-topology.md) | Euclidean → analytic → differential → topology | Physics, computer graphics, robotics, general relativity. |

### Probability & Statistics — The math of uncertainty

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Probability & Statistics](03-branches/probability.md) | Random variables, distributions, inference, information theory | ML, medicine, finance, science itself. |

### Differential Equations — The math of systems

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Differential Equations](03-branches/differential-equations.md) | ODEs, PDEs, dynamical systems, control theory | Physics, engineering, weather, epidemiology. |

### Number Theory — The math of integers

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Number Theory](03-branches/number-theory.md) | Primes, divisibility, modular arithmetic | Cryptography — RSA, Diffie-Hellman, elliptic curves. |

---

## Layer 3: Applied & Computational — Where math meets the real world

### Optimization — Finding the best

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Optimization](04-applied/optimization.md) | Linear programming, convex optimization, gradient descent, game theory | ML training, logistics, economics, resource allocation. |

### Numerical Methods — Making math computable

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Numerical Methods](04-applied/numerical-methods.md) | Floating point, numerical linear algebra, simulation | Every computer simulation, every ML model, every physics engine. |

### Information Theory — The math of communication

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Information Theory](04-applied/information-theory.md) | Entropy, compression, channel capacity | Data compression, telecommunications, ML loss functions. |

### Fourier Analysis & Signal Processing

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Fourier Analysis](04-applied/fourier-analysis.md) | Decomposing signals into frequencies | Audio, images, MRI, telecommunications, MP3/JPEG. |

### Cryptography — Securing information

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [Cryptography](04-applied/cryptography.md) | RSA, elliptic curves, hash functions, error-correcting codes | HTTPS, Bitcoin, secure communication, QR codes. |

### The Math of Machine Learning

| Topic | What it is | What it powers |
|-------|-----------|----------------|
| [ML Math](04-applied/ml-math.md) | The convergence of linear algebra, calculus, probability, and optimization | Neural networks, transformers, generative AI. |

---

## The Dependency Graph

```
Logic & Set Theory
    │
    ├── Numbers (N → Z → Q → R → C)
    │       │
    │       ├── Analysis (limits → calculus → real analysis → measure theory)
    │       │       │
    │       │       ├── Probability & Statistics
    │       │       ├── Differential Equations → Control Theory
    │       │       ├── Fourier Analysis → Signal Processing
    │       │       └── Numerical Methods
    │       │
    │       └── Number Theory → Cryptography
    │
    ├── Algebra (groups → rings → fields → vector spaces)
    │       │
    │       ├── Linear Algebra → ML, Graphics, Quantum Computing
    │       ├── Abstract Algebra → Cryptography, Coding Theory
    │       └── Boolean Algebra → Digital Circuits, Programming
    │
    ├── Discrete Math (combinatorics, graphs, formal languages)
    │       │
    │       ├── Algorithm Analysis & Complexity Theory
    │       ├── Network Theory
    │       └── Automata & Computability → Programming Languages
    │
    └── Geometry (Euclidean → analytic → differential → topology)
            │
            ├── Physics (relativity, quantum mechanics)
            ├── Computer Graphics & Vision
            └── Robotics & Control
```

---

## What Powers What

| Technology | Math it runs on |
|---|---|
| Google Search | Linear algebra (PageRank), graph theory, probability |
| GPS | Differential equations, relativity corrections, numerical methods |
| Encryption (HTTPS) | Number theory, elliptic curves, modular arithmetic |
| AI / LLMs | Linear algebra, calculus, probability, optimization, information theory |
| Smartphones | Signal processing (Fourier), error-correcting codes, optimization |
| Weather forecasts | PDEs (fluid dynamics), numerical methods, statistics |
| Video streaming | Information theory, Fourier analysis, compression algorithms |
| Financial markets | Stochastic calculus, probability, optimization, game theory |
| Medical imaging | Fourier transforms, linear algebra, inverse problems |
| Quantum computing | Linear algebra, complex numbers, group theory, topology |

---

## Recommended Study Order

**Phase 1 — Foundations:** Logic → Set Theory → Proof Methods

**Phase 2 — Core Structures:** Number Systems → Algebra → Linear Algebra → Analysis → Calculus

**Phase 3 — Branches:** Discrete Math → Probability → Number Theory → Differential Equations → Geometry/Topology

**Phase 4 — Applied:** Optimization → Numerical Methods → Information Theory → Fourier Analysis → Cryptography → ML Math

---

## See Also

- [The Complete Map of Computer Science](../cs/00-map.md) — The systems that mathematics powers
- [The Complete Map of Business & Entrepreneurship](../business/00-map.md) — Where math meets finance, optimization, and decision-making
