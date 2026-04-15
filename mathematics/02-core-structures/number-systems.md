# Number Systems — Building Numbers from Nothing

> Each number system was invented because the previous one couldn't answer a simple question.
> The chain: N → Z → Q → R → C is the story of mathematics refusing to accept "no solution."

**Prerequisites:** [Logic](../01-foundations/logic.md), [Set Theory](../01-foundations/set-theory.md), [Proof Methods](../01-foundations/proof-methods.md)
**What it enables:** All of arithmetic, algebra, analysis, calculus, and everything built on them.

---

## The Pattern

Every number system emerges from the same move:

1. We have a system that works for certain operations.
2. We encounter an equation that has no solution in that system.
3. We EXTEND the system to include the solution.
4. The new system preserves everything the old one could do, plus more.

This process is called **extension**, and it's one of the most fundamental patterns in all of mathematics.

---

## 1. Natural Numbers (N) — Counting

**The question:** What are the simplest possible numbers?

```
N = {0, 1, 2, 3, 4, ...}
```

### The Peano Axioms — Building N from scratch

You can construct ALL natural numbers from just two ingredients:

1. **0 exists.** (There is a starting point.)
2. **Every natural number n has a successor S(n).** (You can always go "one more.")
3. **0 is not the successor of anything.** (There's nothing before the start.)
4. **S(m) = S(n) → m = n.** (Different numbers have different successors — no collapsing.)
5. **Induction:** If a property holds for 0, and holding for n implies holding for S(n), then it holds for all natural numbers.

From this, everything follows:
```
1 = S(0)
2 = S(S(0))
3 = S(S(S(0)))
```

Addition: defined recursively.
```
n + 0 = n
n + S(m) = S(n + m)
```

Multiplication: repeated addition.
```
n × 0 = 0
n × S(m) = n × m + n
```

**What N can do:** Add, multiply. Both always produce natural numbers (N is "closed" under + and ×).

**What N can't do:** Solve 3 - 5 = ?. There's no natural number that works. This is the gap.

---

## 2. Integers (Z) — Subtraction

**The question that broke N:** What is 3 - 5?

**The fix:** Add negative numbers.

```
Z = {..., -3, -2, -1, 0, 1, 2, 3, ...}
```

### How it's actually constructed

Formally, we define integers as equivalence classes of pairs of natural numbers:

```
(a, b) represents a - b
(5, 3) represents 2
(3, 5) represents -2
(7, 7) represents 0
```

Multiple pairs represent the same integer: (5, 3) and (4, 2) both represent 2. We declare them equivalent: (a, b) ~ (c, d) if a + d = b + c.

This construction pattern — taking pairs and defining equivalence — recurs throughout mathematics.

**What Z gains:** Subtraction always works. Every equation a + x = b has a solution.

**What Z can't do:** Solve 2x = 3. There's no integer that works.

**Algebraic structure:** Z is a **ring** — a set with two operations (+, ×) satisfying certain properties. This connects to [Algebra](algebra.md).

---

## 3. Rationals (Q) — Division

**The question that broke Z:** What is 1 ÷ 3?

**The fix:** Add fractions.

```
Q = {p/q : p, q ∈ Z, q ≠ 0}
```

Again, formally defined via equivalence classes of pairs:
```
(p, q) represents p/q
(1, 2) and (2, 4) both represent 1/2
(p, q) ~ (r, s) if p × s = q × r
```

**What Q gains:** Division (by non-zero) always works. Q is **dense** — between any two rationals, there's another rational. The rationals seem to fill up the number line completely.

**But they don't.** There are gaps.

**What Q can't do:** Solve x² = 2. We proved √2 is irrational in [Proof Methods](../01-foundations/proof-methods.md). So there's a "hole" in the number line at √2 where no rational sits.

**Algebraic structure:** Q is a **field** — a ring where every non-zero element has a multiplicative inverse.

**Countability:** Despite being dense, Q is **countable** — you can list all rationals in a sequence (Cantor's pairing argument). Same size as N, in a precise sense.

---

## 4. Real Numbers (R) — Completeness

**The question that broke Q:** What numbers fill the gaps?

**The fix:** Complete the number line. Fill every gap.

R includes all rationals AND all irrationals (√2, π, e, ...).

### Two ways to construct R

**Dedekind Cuts:** A real number is a "cut" that splits Q into two halves — everything below the cut and everything above. √2 is the cut where the bottom contains all rationals whose square is less than 2.

**Cauchy Sequences:** A real number is an equivalence class of sequences of rationals that "converge" — the terms get arbitrarily close together. √2 is the limit of 1, 1.4, 1.41, 1.414, 1.4142, ...

Both constructions give the same R. The key property:

### The Completeness Axiom
**Every bounded, non-empty subset of R has a least upper bound (supremum).**

This is what Q lacks and R has. It's the single property that makes calculus work — limits, continuity, the intermediate value theorem, integration — all rest on completeness.

**What R gains:** No gaps. Limits exist. Calculus works.

**What R can't do:** Solve x² = -1. There's no real number whose square is negative.

**Algebraic structure:** R is a **complete ordered field** — the unique one, in fact.

**Uncountability:** R is **uncountable** — strictly "bigger" than N or Q. Proved by Cantor's diagonal argument. This is one of the most important results in all of mathematics.

---

## 5. Complex Numbers (C) — Algebraic Closure

**The question that broke R:** What is √(-1)?

**The fix:** Invent i, where i² = -1.

```
C = {a + bi : a, b ∈ R}
```

Every complex number has a **real part** (a) and an **imaginary part** (b).

### Arithmetic
```
Addition:       (a + bi) + (c + di) = (a+c) + (b+d)i
Multiplication: (a + bi)(c + di) = (ac - bd) + (ad + bc)i
```

### The Fundamental Theorem of Algebra
**Every non-constant polynomial with complex coefficients has at least one complex root.**

This means: C is **algebraically closed**. Every polynomial equation has a solution. The chain of extensions stops here — there's no equation in C that forces us to invent something beyond C.

```
x² + 1 = 0    → x = ±i          (no solution in R, solved in C)
x³ - 1 = 0    → x = 1, e^(2πi/3), e^(4πi/3)  (three roots, as expected for degree 3)
```

### Geometric interpretation
C is the 2D plane. a + bi corresponds to point (a, b).

Multiplication by i = rotation by 90°.
Multiplication by e^(iθ) = rotation by angle θ.

**Euler's formula:** e^(iθ) = cos(θ) + i·sin(θ)

At θ = π: **e^(iπ) + 1 = 0** — Euler's identity, linking five fundamental constants.

### Why complex numbers are not "imaginary"
The name is historical accident. Complex numbers are as real as real numbers — they're just points in a plane. They show up inevitably in:
- Quantum mechanics (wavefunctions are complex-valued)
- Electrical engineering (AC circuits use complex impedance)
- Signal processing (Fourier transforms use complex exponentials)
- Control theory (poles and zeros in the complex plane)
- Fluid dynamics (conformal mappings)

---

## 6. Modular Arithmetic — Clock Math

**A different direction:** Instead of extending, we WRAP AROUND.

```
In mod 12 (clock arithmetic):
  10 + 5 = 3  (because 15 mod 12 = 3)
  7 × 8 = 8   (because 56 mod 12 = 8)
```

Formally: we work in **Z/nZ** ("integers modulo n"). Two integers are "the same" if they have the same remainder when divided by n.

```
In Z/5Z: {0, 1, 2, 3, 4}
  3 + 4 = 2  (mod 5)
  2 × 3 = 1  (mod 5)  — so 2 and 3 are multiplicative inverses!
```

When n is prime, Z/nZ is a **finite field** — you can add, subtract, multiply, and divide (by non-zero). This is the foundation of:
- **RSA encryption** (arithmetic modulo large primes)
- **Hash functions** (modular arithmetic ensures fixed output size)
- **Error-correcting codes** (linear algebra over finite fields)
- **Computer arithmetic** (unsigned integers wrap around modulo 2³²)

---

## The Complete Picture

```
N ──[add negatives]──→ Z ──[add fractions]──→ Q ──[fill gaps]──→ R ──[add √-1]──→ C
│                      │                      │                  │                │
│ counting             │ subtraction          │ division          │ limits         │ all polynomial
│ addition             │ always works         │ always works      │ calculus       │ roots exist
│ multiplication       │                      │ dense             │ complete       │ algebraically
│                      │                      │ but has gaps      │ uncountable    │ closed
│                      │                      │ countable         │                │
Ring structure ────────Ring──────────────────Field──────────────Field────────────Field
```

**Side branch:**
```
Z ──[wrap around mod n]──→ Z/nZ ──[n prime]──→ Finite field → Cryptography
```

---

## Why This Matters

Every number on every screen, in every database, in every scientific formula — it lives in one of these systems. When your GPS computes your position, it works in R. When your browser establishes HTTPS, it works in Z/pZ. When a quantum computer processes information, it works in C. When you count items in a cart, you're in N.

The number systems aren't abstract curiosities. They're the addressing system of reality.

---

**Next:** [Algebra](algebra.md) — studying the STRUCTURE of operations, not just the numbers.
