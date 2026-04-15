# Analysis — The Rigorous Study of Limits and Continuity

> Calculus asks "what's the answer?" Analysis asks "why is the answer valid?"
> It puts iron-clad foundations under the intuitive ideas of limits, continuity, and convergence.

**Prerequisites:** [Number Systems](number-systems.md) (especially the completeness of R), [Set Theory](../01-foundations/set-theory.md)
**What it enables:** Rigorous calculus, probability theory (via measure theory), differential equations, numerical analysis, functional analysis.

---

## Why Analysis Exists

Calculus works. Newton and Leibniz used it to describe planetary motion, and it gave correct answers. But for 150 years, nobody could explain WHY it worked. What does "infinitely small" actually mean? What does "approaches" mean precisely?

Mathematicians like Cauchy and Weierstrass resolved this by replacing vague intuition with precise definitions. The result: analysis — calculus with proofs.

---

## 1. Sequences and Limits

A **sequence** is an infinite ordered list: a₁, a₂, a₃, ...

```
1, 1/2, 1/3, 1/4, ...         → approaches 0
1, -1, 1, -1, ...              → bounces, no limit
1, 1.4, 1.41, 1.414, ...      → approaches √2
```

### The epsilon-delta definition of a limit

A sequence (aₙ) **converges** to limit L if:

```
∀ε > 0, ∃N ∈ N: n > N → |aₙ - L| < ε
```

In words: no matter how small a tolerance ε you demand, I can find a point N after which EVERY term is within ε of L.

This is the foundation. No hand-waving about "getting closer." It's a precise, checkable, logical statement using quantifiers from logic.

**Example:** Prove that 1/n → 0.

```
Given ε > 0, choose N > 1/ε (Archimedean property guarantees such N exists).
For n > N: |1/n - 0| = 1/n < 1/N < ε.  □
```

### Important limit laws
If aₙ → L and bₙ → M, then:
- aₙ + bₙ → L + M
- aₙ · bₙ → L · M
- aₙ / bₙ → L / M (if M ≠ 0)

These let us compute limits of complex sequences from simple ones.

---

## 2. Series — Adding Infinitely Many Things

A **series** is the sum of a sequence: S = Σ aₙ = a₁ + a₂ + a₃ + ...

This sum is defined as the limit of partial sums: Sₙ = a₁ + a₂ + ... + aₙ.

### Key series

**Geometric series:**
```
Σ rⁿ (n=0 to ∞) = 1/(1-r)    when |r| < 1
```
This is everywhere — compound interest, signal processing, probability distributions.

**Harmonic series:**
```
1 + 1/2 + 1/3 + 1/4 + ... = ∞    (diverges!)
```
Even though the terms go to zero, the sum is infinite. Terms going to zero is NECESSARY for convergence but not SUFFICIENT.

**p-series:**
```
Σ 1/nᵖ converges if p > 1, diverges if p ≤ 1
```

### Convergence tests
- **Comparison test:** If 0 ≤ aₙ ≤ bₙ and Σbₙ converges, then Σaₙ converges.
- **Ratio test:** If |aₙ₊₁/aₙ| → L < 1, series converges. If L > 1, diverges.
- **Integral test:** Σf(n) converges iff ∫f(x)dx converges.

### Power series
```
f(x) = Σ cₙxⁿ = c₀ + c₁x + c₂x² + ...
```

Many functions ARE their power series:
```
eˣ = 1 + x + x²/2! + x³/3! + ...       (converges for all x)
sin(x) = x - x³/3! + x⁵/5! - ...        (converges for all x)
1/(1-x) = 1 + x + x² + x³ + ...         (converges for |x| < 1)
```

**Taylor series:** Any smooth function can be approximated by polynomials near a point. This is how computers calculate sin, cos, exp — they evaluate truncated power series.

---

## 3. Continuity — No Jumps

A function f is **continuous** at point a if:

```
∀ε > 0, ∃δ > 0: |x - a| < δ → |f(x) - f(a)| < ε
```

In words: small changes in input produce small changes in output. No sudden jumps.

Equivalent (and simpler): f is continuous at a if lim(x→a) f(x) = f(a). The limit equals the value.

### Important theorems about continuous functions

**Intermediate Value Theorem (IVT):** If f is continuous on [a,b] and f(a) < y < f(b), then there exists c ∈ (a,b) with f(c) = y.

Translation: a continuous function that goes from negative to positive must cross zero somewhere. This is how root-finding algorithms work (bisection method).

**Extreme Value Theorem:** A continuous function on a closed bounded interval [a,b] achieves its maximum and minimum.

Both theorems REQUIRE the completeness of R. They fail for Q.

### Uniform continuity
Regular continuity: for each point a, δ can depend on a.
**Uniform** continuity: one δ works for ALL points simultaneously.

On closed bounded intervals, continuous functions are automatically uniformly continuous (Heine-Cantor theorem). This matters for proving that integration works.

---

## 4. Metric Spaces — Generalizing Distance

A **metric space** (X, d) is a set X with a distance function d: X × X → R satisfying:
1. d(x,y) ≥ 0, with d(x,y) = 0 iff x = y
2. d(x,y) = d(y,x) (symmetry)
3. d(x,z) ≤ d(x,y) + d(y,z) (triangle inequality)

This generalizes everything above to work in any "space with a notion of distance":

| Metric space | Distance function |
|-------------|-------------------|
| R with \|x - y\| | Usual real number distance |
| R² with Euclidean distance | √((x₁-x₂)² + (y₁-y₂)²) |
| Discrete metric | d(x,y) = 0 if x=y, 1 otherwise |
| Function spaces | Various norms measuring "distance between functions" |
| Strings with edit distance | Number of character changes to transform one into another |

Sequences, limits, continuity, convergence — all defined identically in any metric space. Write the proof once, apply it everywhere.

**In CS:** Edit distance between strings, similarity metrics in ML, convergence of iterative algorithms — all metric space concepts.

---

## 5. Completeness — When Limits Stay Inside

A metric space is **complete** if every Cauchy sequence converges to a point IN the space.

(A Cauchy sequence: terms get arbitrarily close to EACH OTHER — they look like they should converge.)

- R is complete (by construction — this was the whole point of building R from Q)
- Q is NOT complete (the sequence 1, 1.4, 1.41, ... is Cauchy in Q but converges to √2 ∉ Q)

**Banach spaces:** Complete normed vector spaces. The natural setting for functional analysis — infinite-dimensional linear algebra used in PDEs, quantum mechanics, and optimization.

**Hilbert spaces:** Complete inner product spaces. The language of quantum mechanics. Every state is a vector in a Hilbert space, and measurement is projection.

---

## 6. Measure Theory — The Foundation of Modern Analysis

Classical integration (Riemann) works for "nice" functions but fails for pathological ones. Measure theory fixes this.

**Idea:** Instead of slicing the domain into intervals (Riemann), slice the RANGE into intervals and measure the size of their preimages.

A **measure** μ assigns a "size" to subsets of a set:
- μ(∅) = 0
- μ is non-negative
- μ of a union of disjoint sets = sum of individual measures

The **Lebesgue measure** on R: the "length" of intervals, extended to arbitrary sets. Almost every subset of R is measurable.

### Why this matters

**Probability theory IS measure theory.** A probability space is just a measure space where the total measure is 1. This is the Kolmogorov axiomatization that put probability on rigorous footing.

The **Lebesgue integral** extends integration to vastly more functions than Riemann could handle, and has much better limit-interchange properties. This is essential for:
- Probability (expected values, conditional expectations)
- Fourier analysis (L² spaces)
- Functional analysis (operator theory)

---

## 7. What Analysis Powers

| Application | Which part of analysis |
|-------------|----------------------|
| **Computer arithmetic** | Convergence of algorithms, error bounds, floating point analysis |
| **Machine learning** | Convergence of gradient descent, generalization bounds |
| **Probability/statistics** | Measure theory provides the foundation |
| **Signal processing** | L² function spaces, Fourier convergence theorems |
| **Physics** | PDEs, functional analysis, Hilbert spaces in quantum mechanics |
| **Numerical methods** | Error analysis, convergence rates, stability |
| **Economics** | Fixed-point theorems (Nash equilibria), measure-theoretic probability |

---

## Key Insight

Analysis is what happens when you take intuitive ideas ("getting closer," "smooth," "adding up infinitely many things") and make them precise enough to actually PROVE things. Every time a computer evaluates sin(x) using a polynomial approximation, it relies on Taylor's theorem. Every time an ML model converges during training, that convergence is governed by the theorems of analysis. It's the invisible quality control behind all of computation.

---

**Next:** [Calculus](calculus.md) — the practical application of analysis: derivatives and integrals.
