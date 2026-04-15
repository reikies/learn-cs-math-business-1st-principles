# Proof Methods — How We Establish Truth

> A proof is a logical argument that shows a statement MUST be true,
> given the axioms and rules we've agreed on. No exceptions, no hand-waving.

**Prerequisites:** [Logic](logic.md), [Set Theory](set-theory.md)
**What it enables:** Everything. Every theorem in every branch of mathematics is established by proof.

---

## Why Proofs Exist

Intuition is unreliable. Things that seem obviously true sometimes aren't (for 300 years people assumed every continuous function is differentiable "almost everywhere" — wrong). Things that seem impossible are sometimes provable (Cantor proved there are different sizes of infinity — and was attacked for it).

Proofs exist to **separate what IS true from what merely FEELS true.**

---

## 1. Direct Proof

**Strategy:** Assume the premises, apply logical steps, arrive at the conclusion.

**Template:**
```
To prove: If P, then Q.
1. Assume P is true.
2. [logical steps using definitions, axioms, known theorems]
3. Therefore Q is true.
```

**Example:** Prove that the sum of two even numbers is even.

```
Let a and b be even numbers.
By definition, a = 2m for some integer m, and b = 2n for some integer n.
Then a + b = 2m + 2n = 2(m + n).
Since m + n is an integer, a + b = 2(m + n) is even.  □
```

The □ (or QED) marks the end of a proof.

**When to use:** When the path from hypothesis to conclusion is relatively straightforward. This is the "happy path" of proof.

---

## 2. Proof by Contradiction

**Strategy:** Assume the statement is FALSE, show this leads to an impossibility.

**Template:**
```
To prove: P is true.
1. Assume P is false (assume ¬P).
2. [derive logical consequences]
3. Arrive at a contradiction (something that is both true and false).
4. Since our assumption led to contradiction, ¬P must be false, so P is true.
```

**Example:** Prove that √2 is irrational.

```
Assume √2 is rational.
Then √2 = p/q where p, q are integers with no common factors (lowest terms).
Squaring: 2 = p²/q², so p² = 2q².
This means p² is even, so p must be even (if p were odd, p² would be odd).
Write p = 2k for some integer k.
Then (2k)² = 2q², so 4k² = 2q², so q² = 2k².
This means q² is even, so q must be even.
But now both p and q are even — they share factor 2.
This contradicts our assumption that p/q was in lowest terms.
Therefore √2 is irrational.  □
```

This proof is over 2,000 years old (attributed to the Greeks) and it's still beautiful.

**When to use:** When proving something directly seems hard, but assuming the opposite quickly leads to nonsense. Especially useful for proving things DON'T exist or CAN'T happen.

---

## 3. Proof by Contrapositive

**Strategy:** Instead of proving "if P then Q," prove the equivalent "if not Q, then not P."

Recall from logic: `P → Q` is logically equivalent to `¬Q → ¬P`.

**Template:**
```
To prove: If P, then Q.
Equivalently prove: If ¬Q, then ¬P.
1. Assume Q is false.
2. [logical steps]
3. Therefore P is false.
```

**Example:** Prove that if n² is even, then n is even.

```
Contrapositive: If n is odd, then n² is odd.
Assume n is odd, so n = 2k + 1 for some integer k.
Then n² = (2k+1)² = 4k² + 4k + 1 = 2(2k² + 2k) + 1.
This is odd.  □
```

**When to use:** When the direct proof is awkward but the contrapositive flows naturally. Look at both directions before choosing.

---

## 4. Mathematical Induction — The Domino Principle

**The most important proof technique in discrete math and computer science.**

**Strategy:** Prove a statement P(n) holds for all natural numbers n ≥ some base.

**Template:**
```
To prove: P(n) for all n ≥ 0.

Base case: Show P(0) is true.
Inductive step: Show that IF P(k) is true (for some arbitrary k), THEN P(k+1) is true.

Since P(0) is true, and P(k) → P(k+1), we get:
P(0) → P(1) → P(2) → P(3) → ...
Therefore P(n) holds for all n ≥ 0.
```

It's dominoes: prove the first one falls, prove each one knocks over the next.

**Example:** Prove that 1 + 2 + 3 + ... + n = n(n+1)/2 for all n ≥ 1.

```
Base case: n = 1. Left side: 1. Right side: 1(2)/2 = 1. ✓

Inductive step: Assume 1 + 2 + ... + k = k(k+1)/2 for some k.
We need to show: 1 + 2 + ... + k + (k+1) = (k+1)(k+2)/2.

Left side = [1 + 2 + ... + k] + (k+1)
         = k(k+1)/2 + (k+1)          [by inductive hypothesis]
         = k(k+1)/2 + 2(k+1)/2
         = (k+1)(k+2)/2               ✓

By induction, the formula holds for all n ≥ 1.  □
```

### Strong Induction
Instead of assuming just P(k), assume P(0), P(1), ..., P(k) are ALL true, then prove P(k+1). Useful when the next case depends on multiple previous cases (like Fibonacci).

### Structural Induction
Induction on the structure of objects rather than numbers — used to prove properties of trees, lists, expressions, and any recursively defined structure. This is the backbone of programming language theory.

**Why induction matters in CS:**
- Proving recursive algorithms correct
- Proving loop invariants
- Proving properties of data structures
- The foundation of recursion itself

---

## 5. Proof by Construction (Existence Proofs)

**Strategy:** To prove something exists, build it.

**Example:** Prove there exists an irrational number a such that a^a is rational. Nah wait — let's do something cleaner.

**Example:** Prove that for every n, there exist n consecutive composite (non-prime) numbers.

```
Consider the sequence: (n+1)! + 2, (n+1)! + 3, ..., (n+1)! + (n+1).

(n+1)! + 2 is divisible by 2  (since (n+1)! is divisible by 2)
(n+1)! + 3 is divisible by 3  (since (n+1)! is divisible by 3)
...
(n+1)! + k is divisible by k  (since (n+1)! is divisible by k)

So we have n consecutive numbers, all composite.  □
```

We didn't just prove they exist — we showed how to find them.

**Why this matters in CS:** Constructive proofs are algorithms. If you constructively prove a solution exists, you've also shown how to compute it.

---

## 6. Proof by Cases (Exhaustion)

**Strategy:** Break the problem into all possible cases and prove each one.

**Example:** Prove |a × b| = |a| × |b| for all real numbers a, b.

```
Case 1: a ≥ 0 and b ≥ 0 → |ab| = ab = |a||b| ✓
Case 2: a ≥ 0 and b < 0 → |ab| = a(-b) = |a||b| ✓
Case 3: a < 0 and b ≥ 0 → |ab| = (-a)b = |a||b| ✓
Case 4: a < 0 and b < 0 → |ab| = (-a)(-b) = ab = |a||b| ✓

All cases covered.  □
```

**When to use:** When there are a small number of cases and each can be handled. Computer-assisted proofs (like the Four Color Theorem) use this approach at massive scale.

---

## 7. Diagonalization — Cantor's Technique

**Strategy:** Construct a new object that differs from every object in a given list.

**The classic:** Prove that the real numbers are uncountable.

```
Assume we can list all reals between 0 and 1:
  r₁ = 0.d₁₁ d₁₂ d₁₃ d₁₄ ...
  r₂ = 0.d₂₁ d₂₂ d₂₃ d₂₄ ...
  r₃ = 0.d₃₁ d₃₂ d₃₃ d₃₄ ...
  ...

Construct x = 0.e₁ e₂ e₃ ... where eᵢ ≠ dᵢᵢ (differs from rᵢ in the i-th digit).

x differs from r₁ (in digit 1), from r₂ (in digit 2), from every rᵢ (in digit i).
So x is not in our list — but x is a real number between 0 and 1.
Contradiction: we couldn't list all reals.  □
```

**Why this matters in CS:** The same technique proves the Halting Problem is undecidable. You literally cannot write a program that determines whether any given program halts. The proof structure is identical to Cantor's.

---

## 8. The Pigeonhole Principle

**Strategy:** If you put n+1 objects into n boxes, at least one box has 2+ objects.

Seems trivial. But it's shockingly powerful.

**Example:** In any group of 367 people, at least two share a birthday. (366 possible birthdays, 367 people.)

**Example:** In any sequence of n² + 1 distinct numbers, there exists an increasing or decreasing subsequence of length n + 1. (Erdős–Szekeres theorem.)

**In CS:** Proves hash collisions are inevitable. Proves compression can't shrink ALL files (if it could, pigeonhole says two files would compress to the same output).

---

## 9. Common Proof Mistakes

| Mistake | What goes wrong |
|---------|----------------|
| Assuming what you're trying to prove | Circular reasoning — you've proven nothing |
| Proving only one direction | "If P then Q" is not "If Q then P" — you need both for "iff" |
| Confusing "for all" and "there exists" | An example is not a proof of "for all"; a proof of "for all" doesn't need examples |
| Dividing by something that might be zero | Silently assuming a variable is nonzero |
| Treating induction hypothesis as given | The induction hypothesis is an assumption within the inductive step, not a proven fact on its own |

---

## Why This Matters

Every theorem, every algorithm correctness proof, every security guarantee, every type system soundness argument — they all reduce to these techniques. When someone says "this encryption is secure," what they mean is: "there is a proof, using these methods, that breaking it requires solving a problem we believe is hard."

Proof methods are the compiler of mathematics. The ideas are the source code. The proofs are the compiled, verified, trustworthy output.

---

**Next:** [Number Systems](../02-core-structures/number-systems.md) — building numbers from the ground up.
