# Linear Algebra — The Workhorse of Modern Computation

> If you could only learn one branch of mathematics for practical purposes,
> this is it. Linear algebra is the operating system of applied math.

**Prerequisites:** [Number Systems](number-systems.md), [Algebra](algebra.md) (vector spaces)
**What it enables:** Machine learning, computer graphics, quantum computing, signal processing, statistics, physics, optimization — nearly everything computational.

---

## Why Linear Algebra Exists

Most real-world systems are either linear or can be APPROXIMATED as linear (near any point, smooth functions look linear — that's what derivatives are). Linear systems are the ones we can actually solve, and the tools for solving them form linear algebra.

The core question: **Given a system of linear equations, what are the solutions, and what structure do they have?**

---

## 1. Vectors — Direction and Magnitude

A **vector** is an ordered list of numbers.

```
v = [3, 4]        — a 2D vector
w = [1, 0, -2]    — a 3D vector
x = [x₁, x₂, ..., xₙ]  — an n-dimensional vector
```

Two operations define what vectors can do:

**Addition:** component-wise
```
[1, 2] + [3, 4] = [4, 6]
```

**Scalar multiplication:** multiply every component
```
3 · [1, 2] = [3, 6]
```

### Geometric interpretation
In R², a vector is an arrow from the origin. Addition is tip-to-tail. Scalar multiplication stretches or shrinks.

### The dot product
```
v · w = v₁w₁ + v₂w₂ + ... + vₙwₙ
[1, 2] · [3, 4] = 3 + 8 = 11
```

The dot product measures:
- **Length:** ‖v‖ = √(v · v)
- **Angle:** v · w = ‖v‖‖w‖cos(θ)
- **Orthogonality:** v · w = 0 ↔ v and w are perpendicular

This one operation connects algebra to geometry and powers everything from search engine similarity scores to physics.

---

## 2. Linear Combinations and Span

A **linear combination** of vectors v₁, ..., vₖ is:
```
c₁v₁ + c₂v₂ + ... + cₖvₖ    (where c₁, ..., cₖ are scalars)
```

The **span** of a set of vectors is the set of ALL possible linear combinations — every point you can reach by adding and scaling those vectors.

```
span({[1,0], [0,1]}) = all of R²     (you can reach any point)
span({[1,0]})         = the x-axis   (only horizontal vectors)
span({[1,2], [2,4]})  = a line       (the second vector is just 2× the first — redundant)
```

### Linear independence
Vectors are **linearly independent** if no vector in the set can be written as a linear combination of the others. Equivalently: the only way to get zero is with all coefficients zero.

```
c₁v₁ + c₂v₂ + ... + cₖvₖ = 0  →  c₁ = c₂ = ... = cₖ = 0
```

Independent vectors each "add a new dimension." Dependent vectors are redundant.

### Basis and dimension
A **basis** for a vector space is a set of vectors that is:
1. Linearly independent (no redundancy)
2. Spans the whole space (reaches everything)

The number of vectors in a basis is the **dimension**. R³ has dimension 3. Any three independent vectors in R³ form a basis.

---

## 3. Matrices — Linear Transformations

A **matrix** is a rectangular array of numbers. An m×n matrix has m rows and n columns.

```
A = [1  2  3]     — a 2×3 matrix
    [4  5  6]
```

### Matrices ARE linear transformations

Every matrix represents a linear function. An m×n matrix A defines a function f: Rⁿ → Rᵐ by:

```
f(x) = Ax    (matrix-vector multiplication)
```

This function is LINEAR: f(ax + by) = af(x) + bf(y).

And conversely: every linear function between finite-dimensional spaces IS a matrix (once you choose bases).

### Matrix-vector multiplication
```
[1  2] [5]   [1·5 + 2·6]   [17]
[3  4] [6] = [3·5 + 4·6] = [39]
```

Each row of A dotted with x gives one component of the output.

### Matrix-matrix multiplication
(AB)ᵢⱼ = row i of A · column j of B

```
[1 2] [5 6]   [1·5+2·7  1·6+2·8]   [19 22]
[3 4] [7 8] = [3·5+4·7  3·6+4·8] = [43 50]
```

Matrix multiplication = **composition of transformations**. AB means "first apply B, then apply A."

**NOT commutative:** AB ≠ BA in general. Order matters — rotate then scale ≠ scale then rotate.

### What matrices can do (geometric transformations in 2D)

| Matrix | Effect |
|--------|--------|
| `[cos θ  -sin θ; sin θ  cos θ]` | Rotation by angle θ |
| `[s 0; 0 s]` | Uniform scaling by s |
| `[1 0; 0 -1]` | Reflection across x-axis |
| `[1 k; 0 1]` | Shearing |
| `[0 0; 0 0]` | Projection to origin (collapse everything) |
| `[1 0; 0 1]` | Identity (do nothing) |

Computer graphics is built on composing these matrices.

---

## 4. Systems of Linear Equations

The central computational problem:

```
2x + 3y = 7
 x -  y = 1
```

In matrix form: **Ax = b**

```
[2  3] [x]   [7]
[1 -1] [y] = [1]
```

### Gaussian elimination
The algorithm: use row operations to reduce the matrix to triangular form, then solve by back-substitution.

```
[2  3 | 7]  →  [1 -1 | 1]  →  [1 -1 | 1]  →  solution: x=2, y=1
[1 -1 | 1]     [2  3 | 7]     [0  5 | 5]
```

Three possible outcomes:
1. **Unique solution** — the system is fully determined
2. **Infinitely many solutions** — the system is underdetermined (more unknowns than constraints)
3. **No solution** — the system is inconsistent (contradictory constraints)

### The determinant
For a square matrix A, det(A) is a single number that tells you:
- **det(A) ≠ 0:** A is invertible, Ax = b has a unique solution
- **det(A) = 0:** A is singular, the transformation collapses space (loses information)

Geometrically: det(A) = the factor by which A scales volumes. det = 0 means the transformation squashes space into a lower dimension.

```
det([a b; c d]) = ad - bc
```

---

## 5. The Four Fundamental Subspaces

Every m×n matrix A defines four subspaces that completely characterize it:

| Subspace | Definition | Dimension |
|----------|-----------|-----------|
| **Column space** C(A) | All possible outputs Ax | rank r |
| **Null space** N(A) | All x where Ax = 0 | n - r |
| **Row space** C(Aᵀ) | Span of the rows | rank r |
| **Left null space** N(Aᵀ) | All y where Aᵀy = 0 | m - r |

The **rank** r is the number of independent rows (= number of independent columns). It measures how much of the output space the transformation actually uses.

**The rank-nullity theorem:** rank + nullity = n (number of columns). What A "uses" plus what A "destroys" = the full input space.

---

## 6. Eigenvalues and Eigenvectors — The DNA of a Matrix

An **eigenvector** of A is a non-zero vector v that A merely SCALES:

```
Av = λv
```

A doesn't change v's direction — just its length, by the factor λ (the **eigenvalue**).

### Why this matters

Most vectors get both rotated and scaled by a matrix. Eigenvectors are the special directions that ONLY get scaled. They reveal the "natural axes" of the transformation.

### Finding eigenvalues
```
Av = λv
Av - λv = 0
(A - λI)v = 0
```

This has a non-zero solution only when det(A - λI) = 0. This gives the **characteristic polynomial**, whose roots are the eigenvalues.

### Diagonalization
If A has n independent eigenvectors, it can be decomposed:

```
A = PDP⁻¹
```

where D is diagonal (eigenvalues on the diagonal) and P contains the eigenvectors.

This is enormously powerful:
```
A¹⁰⁰ = PD¹⁰⁰P⁻¹
```

Raising a diagonal matrix to a power is trivial — just raise each diagonal entry. This turns expensive matrix computations into cheap ones.

### Where eigenvalues appear

| Application | How eigenvalues are used |
|-------------|------------------------|
| **Google PageRank** | The ranking is the eigenvector of the web's link matrix |
| **Principal Component Analysis (PCA)** | Eigenvectors of the covariance matrix = directions of maximum variance |
| **Quantum mechanics** | Observable measurements are eigenvalues of Hermitian operators |
| **Vibration analysis** | Natural frequencies of a bridge/building are eigenvalues |
| **Stability analysis** | A system is stable iff all eigenvalues have negative real part |
| **Markov chains** | Long-term behavior determined by the eigenvalue-1 eigenvector |

---

## 7. Singular Value Decomposition (SVD)

**Every** matrix (not just square ones) can be decomposed:

```
A = UΣVᵀ
```

- U: orthogonal matrix (left singular vectors)
- Σ: diagonal matrix (singular values σ₁ ≥ σ₂ ≥ ... ≥ 0)
- V: orthogonal matrix (right singular vectors)

The singular values tell you the "importance" of each component. If some singular values are tiny, you can drop them for an approximation:

```
A ≈ U₁Σ₁V₁ᵀ    (keep only the top k singular values)
```

This is **low-rank approximation** — the best possible compression of a matrix.

### Applications
- **Image compression:** An image is a matrix. SVD keeps the most important components. A 1000×1000 image (1M values) might be well-approximated by rank 50 (100K values — 10× compression).
- **Recommendation systems:** User-item matrices are low-rank (people's tastes are correlated).
- **Natural language processing:** Word embeddings via LSA/LSI use SVD.
- **Noise reduction:** Small singular values are noise. Drop them.

---

## 8. Orthogonality and Projections

Two vectors are **orthogonal** if their dot product is zero. An **orthonormal** set is orthogonal with each vector having length 1.

**Projection** of v onto a subspace W: find the point in W closest to v.

```
proj_w(v) = (v · w / w · w) · w    (projection onto a line)
```

### Least squares
When Ax = b has NO exact solution (overdetermined system), the **least squares** solution minimizes ‖Ax - b‖²:

```
x̂ = (AᵀA)⁻¹Aᵀb
```

This is projection: we project b onto the column space of A — the best approximation.

**Least squares IS linear regression.** The entire field of fitting models to data is a projection in linear algebra.

### Gram-Schmidt process
Given any basis, produce an orthonormal basis. Essential for numerical stability and QR decomposition.

---

## 9. Linear Algebra in Practice

| Domain | What's happening |
|--------|-----------------|
| **Neural networks** | Forward pass = matrix multiplications. Weights are matrices. Training adjusts them. |
| **Computer graphics** | Every rotation, scaling, perspective projection = 4×4 matrix multiplication |
| **Search engines** | PageRank = finding the dominant eigenvector of a massive sparse matrix |
| **Data science** | PCA, regression, SVD — all pure linear algebra |
| **Quantum computing** | Qubits are vectors in C². Gates are unitary matrices. Measurement is projection. |
| **Control systems** | State-space models: x(t+1) = Ax(t) + Bu(t). Stability from eigenvalues. |
| **Signal processing** | Fourier transform is a change of basis. Filters are diagonal in frequency basis. |
| **Cryptography** | Error-correcting codes use matrix operations over finite fields |

---

## Key Insight

Linear algebra is not about lines. It's about **transformation** — what happens when you multiply, how information is preserved or destroyed, where the action really happens (eigenvectors), and how to compress without losing much (SVD). The reason it appears everywhere is that linearity is the first useful approximation to anything, and the tools of linear algebra are the ones we can actually compute with.

---

**Next:** [Analysis](analysis.md) — making "approaches," "converges," and "limit" precise.
