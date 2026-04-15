# Numerical Methods — Making Math Computable

> Most math has no closed-form solution. Numerical methods find approximate
> answers using algorithms — turning continuous mathematics into discrete computation.
> Every simulation, every ML model, every physics engine runs on these techniques.

**Prerequisites:** [Calculus](../02-core-structures/calculus.md), [Linear Algebra](../02-core-structures/linear-algebra.md), [Analysis](../02-core-structures/analysis.md) (convergence, error)
**What it enables:** Scientific computing, engineering simulation, weather prediction, computer graphics, ML training, financial modeling.

---

## Why Numerical Methods Exist

Most equations can't be solved exactly:
- ∫₀¹ e^(-x²) dx — no closed form
- Navier-Stokes for turbulent flow — no general solution
- Most matrix eigenvalue problems — no formula for n > 4

Numerical methods bridge the gap between mathematical models and actual computation. The tradeoff: you get an APPROXIMATE answer, but you can bound the error and improve accuracy.

---

## 1. Floating Point Arithmetic — Where It Starts

Computers represent real numbers in **IEEE 754 floating point:**

```
(-1)^s × 1.mantissa × 2^exponent

float32:  1 sign bit + 8 exponent bits + 23 mantissa bits ≈ 7 decimal digits
float64:  1 sign bit + 11 exponent bits + 52 mantissa bits ≈ 16 decimal digits
```

### Key limitations

**Machine epsilon:** The smallest ε such that 1 + ε ≠ 1. For float64: ε ≈ 2.2 × 10⁻¹⁶.

**Not all numbers are representable:** 0.1 has no exact binary representation. This is why 0.1 + 0.2 ≠ 0.3 in most languages.

**Catastrophic cancellation:** Subtracting two nearly equal numbers can destroy precision.
```
1.000000001 - 1.000000000 = 0.000000001
But if each has 10 digits of precision, the result has only 1 digit of precision.
```

**Overflow/underflow:** Numbers too large or small to represent.

Understanding floating point is essential — numerical bugs come from ignoring these properties.

---

## 2. Solving Linear Systems: Ax = b

The most fundamental computation in numerical math.

### Direct methods

**Gaussian elimination / LU decomposition:** Factor A = LU (lower × upper triangular). Then solve Lz = b, Ux = z by back/forward substitution. Cost: O(n³).

**Cholesky decomposition:** When A is symmetric positive definite: A = LLᵀ. Half the cost of LU. Used in Gaussian processes, Kalman filters.

**QR decomposition:** A = QR (orthogonal × upper triangular). More numerically stable. Used for least squares problems.

### Iterative methods (for large sparse systems)

| Method | Idea | Cost per iteration |
|--------|------|-------------------|
| **Jacobi** | Update each variable from others' previous values | O(n²) or O(nnz) |
| **Gauss-Seidel** | Use updated values immediately | O(n²) or O(nnz) |
| **Conjugate Gradient** | Minimize ‖Ax - b‖ along conjugate directions | O(nnz) |
| **GMRES** | Generalized minimal residual (for non-symmetric) | O(k·nnz) |

Iterative methods are essential when n is millions (as in PDE discretizations). They exploit sparsity — most real-world matrices are mostly zeros.

---

## 3. Solving Nonlinear Equations: f(x) = 0

### Bisection method
If f(a) < 0 and f(b) > 0, there's a root between a and b (IVT). Cut the interval in half, repeat. Guaranteed but slow — one bit of precision per step.

### Newton's method
```
x_{n+1} = x_n - f(x_n) / f'(x_n)
```

Use the tangent line as an approximation. Converges **quadratically** — digits of accuracy roughly DOUBLE each step. But can diverge if started poorly.

Newton's method is also the basis for many optimization algorithms (Newton's method for optimization uses the Hessian).

### Fixed-point iteration
Rewrite f(x) = 0 as x = g(x). Iterate x_{n+1} = g(x_n). Converges if |g'| < 1 near the fixed point.

---

## 4. Numerical Integration (Quadrature)

Approximate ∫ₐᵇ f(x)dx.

| Method | Idea | Error |
|--------|------|-------|
| **Trapezoidal rule** | Approximate f with line segments | O(h²) |
| **Simpson's rule** | Approximate f with parabolas | O(h⁴) |
| **Gaussian quadrature** | Choose optimal sample points and weights | Exact for polynomials up to degree 2n-1 |
| **Monte Carlo** | Random sampling, average | O(1/√N) — slow but works in any dimension |

**Monte Carlo integration** is critical in high dimensions where grid methods fail (curse of dimensionality). It powers: particle physics simulations, Bayesian inference (MCMC), financial risk calculation, graphics rendering (path tracing).

---

## 5. Numerical ODEs

Approximate solutions to dy/dt = f(t, y).

### Euler's method (simplest)
```
y_{n+1} = y_n + h · f(t_n, y_n)
```
Step forward using the current slope. Error is O(h).

### Runge-Kutta methods (workhorse)
```
RK4 (the classic 4th-order method):
k₁ = f(t, y)
k₂ = f(t + h/2, y + h·k₁/2)
k₃ = f(t + h/2, y + h·k₂/2)
k₄ = f(t + h, y + h·k₃)
y_{n+1} = y_n + (h/6)(k₁ + 2k₂ + 2k₃ + k₄)
```
Error is O(h⁴). The standard method for most ODE problems.

### Stiff equations
Some systems have components that change on very different timescales. Explicit methods need tiny steps. **Implicit methods** (backward Euler, BDF) handle stiffness by solving an equation at each step. Used in chemical kinetics, circuit simulation.

### Adaptive step size
Adjust h based on estimated error — take large steps where the solution is smooth, small steps where it changes rapidly.

---

## 6. Numerical PDEs

### Finite Difference Method
Replace derivatives with differences on a grid:
```
∂u/∂x ≈ (u(x+h) - u(x-h)) / (2h)
∂²u/∂x² ≈ (u(x+h) - 2u(x) + u(x-h)) / h²
```

Turn the PDE into a large system of algebraic equations. Solve with linear algebra.

### Finite Element Method (FEM)
Divide the domain into elements (triangles, tetrahedra). Approximate the solution as a combination of simple basis functions on each element. Leads to a sparse linear system.

**FEM is the standard for engineering simulation:** structural analysis, heat transfer, fluid dynamics, electromagnetics. Every bridge, aircraft, and building designed today uses FEM.

### Finite Volume Method
Used for conservation laws (fluid dynamics). Ensures that conserved quantities (mass, momentum, energy) are exactly conserved in the discretization. Powers computational fluid dynamics (CFD).

---

## 7. Numerical Linear Algebra

### Eigenvalue computation
- **Power iteration:** Find the dominant eigenvalue/eigenvector. O(n²) per iteration.
- **QR algorithm:** Find ALL eigenvalues. The standard algorithm. O(n³).
- **Lanczos/Arnoldi:** For large sparse matrices — find a few eigenvalues without full decomposition.

### SVD computation
- Uses a variant of QR iteration on the bidiagonal form.
- For sparse matrices: randomized SVD samples columns to approximate. Used in large-scale recommendation systems.

### Condition number
```
κ(A) = ‖A‖ · ‖A⁻¹‖
```
Measures how much errors in input amplify in the output. High condition number = the problem is ill-conditioned (small changes in data cause large changes in answer). This is why some computations are "unstable" — it's the problem, not just the algorithm.

---

## 8. Key Principles

**Stability:** Does the algorithm amplify errors, or keep them controlled? An unstable algorithm is useless regardless of its theoretical accuracy.

**Convergence:** Does the numerical solution approach the true solution as the step size shrinks?

**Consistency + stability = convergence** (Lax equivalence theorem for linear PDEs). If the discretization correctly represents the equation (consistency) and doesn't blow up (stability), the answer converges.

**The curse of dimensionality:** Grid-based methods scale as O(N^d) where d is dimension. In 10 dimensions with 100 grid points per axis: 100¹⁰ = 10²⁰ points — impossible. This is why Monte Carlo and ML approaches dominate in high dimensions.

---

## What Numerical Methods Power

| Technology | Numerical method |
|------------|-----------------|
| **Weather forecasting** | Finite difference/volume methods for atmospheric PDEs |
| **Structural engineering** | FEM for stress analysis |
| **Computer graphics** | Numerical ODE solvers (physics simulation), ray marching |
| **Machine learning** | Floating point matrix operations, SGD convergence |
| **Computational chemistry** | DFT (density functional theory), molecular dynamics |
| **Finance** | Monte Carlo simulation for options pricing |
| **Aerospace** | CFD for aerodynamics, FEM for structural integrity |
| **Video games** | Physics engines use Euler/Verlet integration |
| **Medical devices** | FEM for implant design, numerical models of blood flow |

---

## Key Insight

Numerical methods are where pure mathematics meets engineering reality. The equations of physics and engineering are exact, but solving them requires approximation. The art is controlling the approximation — knowing how much error you introduce, where it accumulates, and how to minimize it. Every number on every screen has passed through floating point arithmetic. Every simulation that designs a product, predicts weather, or trains an AI model runs on these algorithms.

---

**Next:** [Information Theory](information-theory.md) — the mathematics of communication and compression.
