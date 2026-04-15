# Optimization — Finding the Best

> Given a landscape of possibilities, optimization finds the peak (or valley).
> It's the mathematical engine behind machine learning, logistics, economics,
> and any system that needs to make the "best" decision.

**Prerequisites:** [Calculus](../02-core-structures/calculus.md) (gradients), [Linear Algebra](../02-core-structures/linear-algebra.md) (matrices), [Analysis](../02-core-structures/analysis.md) (convergence)
**What it enables:** Machine learning training, operations research, control systems, economics, resource allocation, circuit design.

---

## The Core Problem

```
minimize f(x)
subject to constraints g(x) ≤ 0, h(x) = 0
```

Find the input x that makes the objective function f as small (or large) as possible, while satisfying constraints.

---

## 1. Unconstrained Optimization

### Calculus approach (for smooth functions)

**Necessary condition:** At a minimum x*, ∇f(x*) = 0 (gradient is zero).
**Sufficient condition:** The Hessian H(x*) is positive definite (all eigenvalues > 0).

This is the multivariable version of "derivative = 0, second derivative > 0."

### Gradient Descent — THE Algorithm

```
x_{t+1} = x_t - α · ∇f(x_t)
```

Move in the direction opposite to the gradient (steepest descent). α is the **learning rate**.

- Too large α: overshoot, diverge
- Too small α: converge too slowly
- Just right: sweet spot that depends on the problem

**This is how every neural network is trained.** The loss function L(θ) depends on millions of parameters θ. Gradient descent (and its variants) adjusts θ to minimize L.

### Gradient Descent Variants

| Variant | Idea | Used in |
|---------|------|---------|
| **Stochastic GD (SGD)** | Use a random subset (mini-batch) of data per step | All of deep learning |
| **Momentum** | Accumulate velocity from past gradients | Escaping local minima, faster convergence |
| **Adam** | Adaptive learning rates per parameter + momentum | Default optimizer in most ML |
| **L-BFGS** | Approximate second-order (uses Hessian info) | Smaller-scale optimization |
| **Newton's method** | Use exact Hessian: x -= H⁻¹∇f | Fast convergence but expensive per step |

### The Problem of Local Minima

Gradient descent finds LOCAL minima — valleys that may not be the deepest valley globally.

For convex functions: every local minimum IS the global minimum (no fooling).
For non-convex functions (neural networks): local minima are everywhere, but empirically, saddle points are a bigger issue than bad local minima in high dimensions.

---

## 2. Convex Optimization — The Sweet Spot

A function is **convex** if the line segment between any two points on its graph lies above the graph. Bowl-shaped.

```
f(λx + (1-λ)y) ≤ λf(x) + (1-λ)f(y)    for λ ∈ [0,1]
```

**Why convexity matters:** EVERY local minimum is global. No traps. Gradient descent always finds the best answer.

A **convex optimization problem** has a convex objective and convex feasible region. These problems are "solved" — efficient algorithms exist with guarantees.

### Examples of convex problems

| Problem | What it is |
|---------|-----------|
| **Least squares** | min ‖Ax - b‖² — linear regression |
| **Support Vector Machines** | Maximize margin between classes — convex quadratic program |
| **Linear programming** | Linear objective, linear constraints |
| **Lasso/Ridge regression** | Least squares + convex regularization |
| **Semidefinite programming** | Optimize over positive semidefinite matrices |

---

## 3. Linear Programming (LP)

```
minimize   cᵀx
subject to Ax ≤ b, x ≥ 0
```

Everything is linear — objective and constraints. Despite simplicity, this solves a huge range of real problems.

**The Simplex Method (Dantzig, 1947):** Walk along edges of the feasible polytope, improving the objective at each step. Exponential worst-case but incredibly fast in practice.

**Interior Point Methods:** Travel through the INTERIOR of the feasible region. Polynomial time guaranteed.

### Applications
- Airlines: crew scheduling, flight routing
- Supply chains: shipping optimization
- Diet problem: cheapest meal meeting nutritional requirements
- Network flow: maximum flow / minimum cut

### Integer Programming (IP)
Same as LP but variables must be integers. NP-hard in general. Solved by branch-and-bound, cutting planes. Used for scheduling, facility location, vehicle routing.

---

## 4. Constrained Optimization — Lagrange Multipliers

For: minimize f(x) subject to g(x) = 0

**The Lagrangian:** L(x, λ) = f(x) + λg(x)

At the optimum: ∇f = -λ∇g (the gradients are parallel).

The Lagrange multiplier λ measures the "price" of the constraint — how much the optimum would improve if the constraint were relaxed slightly. This has deep economic interpretation (shadow prices).

### KKT Conditions (inequality constraints)
For minimize f(x) subject to g(x) ≤ 0:

1. ∇f + λ∇g = 0 (stationarity)
2. g(x) ≤ 0 (feasibility)
3. λ ≥ 0 (dual feasibility)
4. λg(x) = 0 (complementary slackness — either constraint is active or multiplier is zero)

KKT conditions are the foundation of constrained optimization theory and duality.

---

## 5. Game Theory — Optimization with Opponents

When multiple agents optimize simultaneously, each affecting the others.

**Nash equilibrium:** A state where no player can improve by unilaterally changing strategy.

**The Minimax Theorem (von Neumann):** In zero-sum games, there exists a mixed strategy equilibrium. The optimal strategy is: maximize your minimum payoff (pessimistic optimal).

### Applications
- Economics: market equilibria, auction design
- AI: adversarial training (GANs), multi-agent reinforcement learning
- Security: defender-attacker models
- Mechanism design: designing rules so rational agents produce desired outcomes (used by Google for ad auctions)

---

## 6. Dynamic Programming — Optimization over Time

Break a sequential decision problem into stages:
```
V(state) = min_action [cost(state, action) + V(next_state)]
```

**Bellman equation:** The optimal value at each state equals the best immediate action plus the optimal value of the resulting state.

This is the principle behind:
- Shortest path algorithms (Dijkstra, Floyd-Warshall)
- Reinforcement learning (Q-learning, policy gradient)
- Sequence alignment (bioinformatics)
- Optimal control

---

## What Optimization Powers

| Technology | Optimization type |
|------------|------------------|
| **Deep learning training** | SGD/Adam (unconstrained, non-convex) |
| **Airline scheduling** | Integer/linear programming |
| **Google ad auctions** | Game theory, mechanism design |
| **Portfolio optimization** | Convex optimization (Markowitz) |
| **Robotics path planning** | Dynamic programming, trajectory optimization |
| **Compiler optimization** | Graph algorithms, integer programming |
| **Supply chain logistics** | Linear/integer programming, network flow |
| **Reinforcement learning** | Dynamic programming (Bellman equation) |
| **SVM classifiers** | Convex quadratic programming |
| **Neural architecture search** | Combinatorial optimization |

---

## Key Insight

Optimization is the link between mathematical models and decisions. A model tells you how a system behaves; optimization tells you how to make it behave BEST. The fundamental tension is between the quality of the solution and the cost of finding it — convex problems give perfect solutions cheaply, NP-hard problems may require approximations, and high-dimensional landscapes (like neural network training) require clever heuristics that work in practice even without theoretical guarantees.

---

**Next:** [Numerical Methods](numerical-methods.md) — making all of this computable.
