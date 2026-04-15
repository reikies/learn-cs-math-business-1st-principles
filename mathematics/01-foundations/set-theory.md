# Set Theory — Building Everything from Nothing

> If logic is the operating system, set theory is the file system.
> Every mathematical object — numbers, functions, spaces, structures — is built from sets.

**Prerequisites:** [Logic](logic.md)
**What it enables:** All of mathematics. Numbers, functions, relations, algebra, analysis, probability — everything is defined in terms of sets.

---

## Why Set Theory Exists

After establishing the rules of reasoning (logic), mathematicians needed raw material to reason ABOUT. The question became: what is the simplest possible object we can build everything from?

The answer: **collections**. A set is just a collection of objects, and remarkably, that's enough to construct all of mathematics.

---

## 1. What Is a Set?

A **set** is an unordered collection of distinct objects, called **elements** or **members**.

```
A = {1, 2, 3}              — a set of three numbers
B = {red, green, blue}     — a set of three colors
C = {1, {2, 3}, cat}       — sets can contain anything, including other sets
∅ = {}                      — the empty set (contains nothing)
```

Notation:
- `x ∈ A` means "x is an element of A" (x belongs to A)
- `x ∉ A` means "x is NOT an element of A"

Two fundamental properties:
1. **No duplicates:** {1, 1, 2} = {1, 2}
2. **No order:** {1, 2, 3} = {3, 1, 2}

---

## 2. Defining Sets

### Roster notation (list the elements)
```
A = {2, 4, 6, 8, 10}
```

### Set-builder notation (describe the elements)
```
A = {x ∈ N : x is even and x ≤ 10}
"The set of all natural numbers x such that x is even and at most 10"
```

This is where logic meets sets — the condition after the colon is a **predicate** from logic.

### Important named sets
```
N = {0, 1, 2, 3, ...}     — natural numbers
Z = {..., -2, -1, 0, 1, 2, ...} — integers
Q = {p/q : p, q ∈ Z, q ≠ 0}    — rationals
R                                 — real numbers (can't list them)
C                                 — complex numbers
∅                                 — the empty set
```

---

## 3. Set Operations — Combining Sets

These mirror the logical connectives exactly.

### Union (A ∪ B) — "or"
Everything in A OR B (or both).
```
{1, 2, 3} ∪ {3, 4, 5} = {1, 2, 3, 4, 5}
```
Corresponds to logical OR (∨).

### Intersection (A ∩ B) — "and"
Everything in BOTH A AND B.
```
{1, 2, 3} ∩ {3, 4, 5} = {3}
```
Corresponds to logical AND (∧).

### Difference (A \ B) — "but not"
Everything in A that is NOT in B.
```
{1, 2, 3} \ {3, 4, 5} = {1, 2}
```

### Complement (Aᶜ) — "not"
Everything NOT in A (relative to some universal set U).
```
If U = {1,2,3,4,5} and A = {1,2}, then Aᶜ = {3,4,5}
```
Corresponds to logical NOT (¬).

### De Morgan's Laws (again!)
```
(A ∪ B)ᶜ = Aᶜ ∩ Bᶜ
(A ∩ B)ᶜ = Aᶜ ∪ Bᶜ
```
Same pattern as in logic — this is not a coincidence. Sets and logic are two views of the same thing.

---

## 4. Subsets and Power Sets

### Subset (A ⊆ B)
Every element of A is also in B.
```
{1, 2} ⊆ {1, 2, 3}    — yes
{1, 4} ⊆ {1, 2, 3}    — no (4 is not in the right set)
∅ ⊆ A                   — always true (the empty set is a subset of everything)
A ⊆ A                   — always true (every set is a subset of itself)
```

Two sets are **equal** if and only if A ⊆ B AND B ⊆ A. This is how you prove set equality.

### Power Set P(A)
The set of ALL subsets of A.
```
A = {1, 2}
P(A) = {∅, {1}, {2}, {1, 2}}
```

If A has n elements, P(A) has **2ⁿ** elements. This grows explosively fast — a set with 10 elements has 1,024 subsets. With 64 elements: more subsets than atoms in the observable universe.

**Why this matters:** Power sets show up everywhere in computer science. The set of all possible states of n boolean variables is a power set. Configuration spaces, permission systems, and search spaces are all power sets.

---

## 5. Cartesian Products and Tuples

The **Cartesian product** A × B is the set of all ordered pairs (a, b) where a ∈ A and b ∈ B.

```
A = {1, 2}
B = {x, y}
A × B = {(1,x), (1,y), (2,x), (2,y)}
```

Unlike sets, pairs ARE ordered: (1, 2) ≠ (2, 1).

|A × B| = |A| × |B| (hence the name "product")

**Why this matters:**
- Coordinate systems: R × R = R² (the 2D plane — every point is a pair)
- Databases: a table row is a tuple from a Cartesian product of column types
- Type systems: a struct/record is a product type

---

## 6. Relations

A **relation** R from A to B is a subset of A × B — it picks out which pairs are "related."

```
A = {1, 2, 3}, B = {1, 2, 3}
"less than" = {(1,2), (1,3), (2,3)} ⊆ A × B
```

### Properties of relations (on a set A)

| Property | Definition | Example |
|----------|-----------|---------|
| Reflexive | ∀a: (a,a) ∈ R | "equals" (everything equals itself) |
| Symmetric | (a,b) ∈ R → (b,a) ∈ R | "is a sibling of" |
| Antisymmetric | (a,b) ∈ R ∧ (b,a) ∈ R → a = b | "≤" (if a≤b and b≤a then a=b) |
| Transitive | (a,b) ∈ R ∧ (b,c) ∈ R → (a,c) ∈ R | "less than" (a<b and b<c → a<c) |

### Equivalence Relations (reflexive + symmetric + transitive)
These partition a set into non-overlapping classes. Examples:
- "same remainder when divided by 3" splits integers into classes {…,-3,0,3,6,…}, {…,-2,1,4,7,…}, {…,-1,2,5,8,…}
- "same file extension" partitions files into groups

### Partial Orders (reflexive + antisymmetric + transitive)
Examples: ≤ on numbers, ⊆ on sets, "is a prerequisite of" on courses.

---

## 7. Functions — The Most Important Relations

A **function** f: A → B is a relation where every element of A is paired with EXACTLY ONE element of B.

```
f: {1, 2, 3} → {a, b, c}
f = {(1,a), (2,b), (3,c)}
```

- **A** is the **domain** (inputs)
- **B** is the **codomain** (possible outputs)
- **f(x)** is the **image** of x (the actual output for input x)

### Types of functions

| Type | Definition | Intuition |
|------|-----------|-----------|
| Injective (one-to-one) | f(a) = f(b) → a = b | No two inputs map to the same output |
| Surjective (onto) | ∀b ∈ B, ∃a ∈ A: f(a) = b | Every output is hit |
| Bijective | Injective AND surjective | Perfect pairing — invertible |

**Why bijections matter:** They prove two sets have the same "size." This is how we compare infinities — Cantor's diagonal argument shows R is "bigger" than N even though both are infinite.

### Function composition
```
If f: A → B and g: B → C, then (g ∘ f): A → C
(g ∘ f)(x) = g(f(x))
```

This is the mathematical version of piping: `x |> f |> g`.

---

## 8. Cardinality — Measuring Size

The **cardinality** |A| of a set is its "size."

For finite sets: |{a, b, c}| = 3

For infinite sets, things get weird:
- |N| = |Z| = |Q| = ℵ₀ (countably infinite — you can list them)
- |R| = 𝔠 (uncountably infinite — you CANNOT list them)

**Cantor's theorem:** |P(A)| > |A| for any set A. There are always more subsets than elements. This means there's no "biggest" infinity — there's an infinite tower of infinities.

---

## 9. The Axioms (ZFC) — Making It Rigorous

Naive set theory (just "a set is a collection") leads to paradoxes:

**Russell's Paradox:** Let R = {x : x ∉ x} (the set of all sets that don't contain themselves). Is R ∈ R?
- If yes → by definition, R ∉ R. Contradiction.
- If no → by definition, R ∈ R. Contradiction.

This crashed the foundations of mathematics in 1901. The fix: **Zermelo-Fraenkel axioms with Choice (ZFC)** — a carefully restricted set of rules for building sets that avoids such paradoxes.

The key axioms (intuitively):
1. **Empty set exists:** ∅ is a set
2. **Extensionality:** Two sets are equal iff they have the same elements
3. **Pairing:** Given sets a, b, the set {a, b} exists
4. **Union:** Given a set of sets, their union exists
5. **Power set:** P(A) exists for any set A
6. **Infinity:** An infinite set exists (this gives us N)
7. **Separation:** You can form subsets by predicates (but carefully — avoiding Russell)
8. **Replacement:** You can transform sets through functions
9. **Choice:** Given a collection of non-empty sets, you can choose one element from each

The Axiom of Choice seems obvious but has strange consequences — it proves the Banach-Tarski paradox (you can decompose a sphere and reassemble it into two identical spheres).

---

## 10. What Set Theory Powers

| Domain | How sets appear |
|--------|----------------|
| **All of math** | Numbers, functions, spaces, structures — all defined as sets |
| **Databases** | Tables are sets of tuples. SQL operations (JOIN, UNION, INTERSECT) are set operations |
| **Programming** | Types are sets of values. Set/HashSet data structures. Type theory is built on sets |
| **Probability** | A probability space is a set (sample space) with a measure. Events are subsets |
| **Formal verification** | Program states are sets. Specifications describe allowed sets of behaviors |

---

## Key Insight

Set theory is almost comically simple in its ingredients — just collections and membership. But from this, literally everything in mathematics can be constructed. Numbers are sets. Functions are sets. Spaces are sets. The entire mathematical universe emerges from asking "what collections can we form, and what can we do with them?"

---

**Next:** [Proof Methods](proof-methods.md) — the techniques for establishing mathematical truth.
