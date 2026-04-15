# Calculus — The Mathematics of Change

> Calculus is two ideas: the rate at which things change (derivatives),
> and the accumulation of those changes (integrals). They're inverses of each other.
> This discovery is arguably humanity's most powerful intellectual tool.

**Prerequisites:** [Analysis](analysis.md) (limits, continuity), [Number Systems](number-systems.md)
**What it enables:** Physics, engineering, optimization, differential equations, machine learning, signal processing — anything involving change or accumulation.

---

## Why Calculus Exists

The ancient Greeks could calculate areas of shapes with straight edges and volumes of simple solids. But the real world is curved, changing, dynamic. How fast is a planet moving RIGHT NOW (not on average)? What's the area under a curve? How do quantities that depend on each other change together?

Newton and Leibniz independently solved this in the 1680s. The result split into two halves — differentiation and integration — connected by the most important theorem in mathematics.

---

## Part I: Differentiation — Rates of Change

### 1. The Derivative

The **derivative** of f at point a is the instantaneous rate of change:

```
f'(a) = lim[h→0] (f(a+h) - f(a)) / h
```

Geometrically: the slope of the tangent line to f at point a.

**Example:**
```
f(x) = x²
f'(a) = lim[h→0] ((a+h)² - a²) / h
      = lim[h→0] (2ah + h²) / h
      = lim[h→0] (2a + h)
      = 2a

So f'(x) = 2x. The slope of x² at any point x is 2x.
At x=3: slope is 6 (steep).
At x=0: slope is 0 (flat bottom).
```

### 2. Derivative Rules — The Algebra of Rates

| Rule | Formula | What it means |
|------|---------|--------------|
| **Constant** | d/dx[c] = 0 | Constants don't change |
| **Power** | d/dx[xⁿ] = nxⁿ⁻¹ | The most-used rule |
| **Sum** | (f+g)' = f' + g' | Rates add |
| **Product** | (fg)' = f'g + fg' | Both parts change |
| **Quotient** | (f/g)' = (f'g - fg')/g² | Derived from product rule |
| **Chain** | d/dx[f(g(x))] = f'(g(x)) · g'(x) | Rates compose (multiply) |

The **chain rule** is the most important. It says: the rate of a composition is the product of the rates. This is EXACTLY how backpropagation works in neural networks.

### 3. What the Derivative Tells You

| f'(x) | Meaning |
|--------|---------|
| f'(x) > 0 | f is increasing at x |
| f'(x) < 0 | f is decreasing at x |
| f'(x) = 0 | f has a critical point (possible max, min, or inflection) |

**Second derivative f''(x):**
| f''(x) | Meaning |
|---------|---------|
| f''(x) > 0 | f is concave up (bowl shape) — critical point is a minimum |
| f''(x) < 0 | f is concave down (dome shape) — critical point is a maximum |

This is how optimization works: find where f'(x) = 0, check f''(x) to determine if it's a min or max.

### 4. Important Derivatives

```
d/dx[eˣ] = eˣ          (e is the unique base where this works — it's its own derivative)
d/dx[ln(x)] = 1/x
d/dx[sin(x)] = cos(x)
d/dx[cos(x)] = -sin(x)
```

The exponential function eˣ is special: it's the function whose rate of change equals itself. This is why it appears everywhere — radioactive decay, population growth, compound interest, probability distributions.

### 5. Multivariable Derivatives

For f(x, y) — a function of multiple variables:

**Partial derivatives:** Hold everything else constant, differentiate with respect to one variable.
```
f(x,y) = x²y + sin(y)
∂f/∂x = 2xy          (treat y as constant)
∂f/∂y = x² + cos(y)  (treat x as constant)
```

**The gradient:** Vector of all partial derivatives.
```
∇f = [∂f/∂x, ∂f/∂y] = [2xy, x² + cos(y)]
```

The gradient points in the direction of STEEPEST INCREASE. Move opposite to the gradient = steepest decrease. This is **gradient descent** — THE algorithm behind deep learning.

---

## Part II: Integration — Accumulation

### 6. The Definite Integral

The **definite integral** ∫ₐᵇ f(x)dx is the signed area under f from a to b.

Constructed as a limit of sums:
```
∫ₐᵇ f(x)dx = lim[n→∞] Σ f(xᵢ) · Δx
```

Split [a,b] into n pieces of width Δx. At each piece, measure the height f(xᵢ). Sum up all the little rectangles. As n → ∞, the approximation becomes exact.

**Example:**
```
∫₀¹ x² dx = ?

This is the area under y = x² from 0 to 1.
Answer: 1/3 (we'll see how to compute this below)
```

### 7. The Fundamental Theorem of Calculus

**The most important theorem in all of calculus — and arguably all of mathematics.**

It has two parts:

**Part 1:** If F(x) = ∫ₐˣ f(t)dt, then F'(x) = f(x).

Translation: the derivative of the "area so far" function is the original function. Differentiation UNDOES integration.

**Part 2:** If F is an antiderivative of f (meaning F' = f), then:

```
∫ₐᵇ f(x)dx = F(b) - F(a)
```

Translation: to compute the area under a curve, find any function whose derivative is f, and evaluate it at the endpoints.

**Example:**
```
∫₀¹ x² dx = [x³/3]₀¹ = 1/3 - 0 = 1/3

Because d/dx[x³/3] = x²
```

This is profound: it connects the geometry of area with the algebra of antiderivatives. Two seemingly unrelated problems are secretly the same problem.

### 8. Integration Techniques

| Technique | When to use |
|-----------|------------|
| **Substitution** (u-sub) | When the integrand has a function and its derivative: ∫f(g(x))g'(x)dx |
| **Integration by parts** | ∫u dv = uv - ∫v du (for products) |
| **Partial fractions** | For rational functions P(x)/Q(x) |
| **Trigonometric substitution** | For √(a²-x²), √(a²+x²), √(x²-a²) |

In practice, computers use numerical integration (Simpson's rule, Gaussian quadrature) because most integrals don't have closed-form solutions.

### 9. Multiple Integrals

For functions of several variables:

```
∫∫_R f(x,y) dA = volume under the surface z = f(x,y) over region R
```

Triple integrals give volumes in 3D. Higher-dimensional integrals appear in probability (integrating over parameter spaces), physics (path integrals), and ML (marginalizing over variables).

---

## Part III: Key Applications

### 10. Optimization
Find where f'(x) = 0 (critical points), then use f''(x) to classify:
- f'' > 0: local minimum
- f'' < 0: local maximum

In machine learning: the loss function L(θ) depends on model parameters θ. Training = finding θ that minimizes L. Gradient descent: θ ← θ - α∇L(θ).

### 11. Differential Equations
Equations involving functions and their derivatives:
```
dy/dx = ky    →    y = Ce^(kt)    (exponential growth/decay)
```

All of physics is expressed as differential equations. See [Differential Equations](../03-branches/differential-equations.md).

### 12. Taylor Series
Any smooth function can be approximated by polynomials:
```
f(x) ≈ f(a) + f'(a)(x-a) + f''(a)(x-a)²/2! + f'''(a)(x-a)³/3! + ...
```

This is how computers calculate transcendental functions (sin, cos, exp, log) — and how physicists simplify complex equations by keeping only the first few terms.

### 13. Line and Surface Integrals
Integration along curves and over surfaces. Used in:
- Physics: work done by a force along a path
- Electromagnetism: Maxwell's equations
- Fluid dynamics: flux through a surface

**Stokes' theorem and the divergence theorem** generalize the Fundamental Theorem of Calculus to higher dimensions — they're the deep structural reason that physics works the way it does.

---

## The Chain Rule and Backpropagation

This deserves special attention because it directly connects calculus to ML.

If a neural network computes: y = f₃(f₂(f₁(x))), then by the chain rule:

```
dy/dx = f₃'(f₂(f₁(x))) · f₂'(f₁(x)) · f₁'(x)
```

Each layer's derivative multiplies. **Backpropagation** is just the chain rule applied systematically from output to input, computing how each weight affects the loss.

```
∂L/∂w₁ = ∂L/∂y · ∂y/∂h₂ · ∂h₂/∂h₁ · ∂h₁/∂w₁
```

Every deep learning training step is this computation — pure calculus.

---

## What Calculus Powers

| Technology | How calculus appears |
|------------|---------------------|
| **Physics** | F = ma is a differential equation. All of mechanics, E&M, QM are calculus. |
| **Machine learning** | Gradient descent = calculus. Backpropagation = chain rule. |
| **Computer graphics** | Curves (Bézier, splines), lighting models, ray tracing |
| **Economics** | Marginal cost/revenue, optimization of utility functions |
| **Signal processing** | Fourier transforms are integrals of signals × complex exponentials |
| **Robotics** | Kinematics (velocity, acceleration), trajectory optimization |
| **Statistics** | Probability density functions, expected values, maximum likelihood |
| **Engineering** | Stress analysis, heat transfer, fluid flow — all PDEs |

---

## Key Insight

Calculus is two operations — derivative and integral — that are inverses of each other (Fundamental Theorem). Derivatives tell you about local behavior (slopes, rates, optimization). Integrals tell you about global behavior (totals, areas, accumulations). Together they describe any system that changes, which is... everything.

---

**Next:** [Discrete Math](../03-branches/discrete-math.md) — the mathematics of things that DON'T change continuously.
