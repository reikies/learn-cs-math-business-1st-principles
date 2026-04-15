# Differential Equations — The Mathematics of Systems

> A differential equation describes how something changes.
> Solving it tells you how the system evolves over time.
> All of physics, most of engineering, and much of biology is written in this language.

**Prerequisites:** [Calculus](../02-core-structures/calculus.md) (derivatives, integrals), [Linear Algebra](../02-core-structures/linear-algebra.md) (for systems)
**What it enables:** Physics (all of it), engineering, population modeling, epidemiology, control theory, weather simulation, financial modeling.

---

## Why Differential Equations Exist

Nature doesn't hand you formulas. It hands you RELATIONSHIPS between quantities and their rates of change:

- "The force on a spring is proportional to its displacement" → F = -kx → mx'' = -kx
- "Radioactive material decays at a rate proportional to how much is left" → dN/dt = -λN
- "Heat flows from hot to cold" → ∂T/∂t = α∇²T

The equation relating a quantity to its derivatives IS the differential equation. Solving it means finding the function that satisfies the relationship.

---

## Part I: Ordinary Differential Equations (ODEs)

One independent variable (usually time).

### 1. First-Order ODEs

**General form:** dy/dx = f(x, y)

#### Separable equations
```
dy/dx = g(x)h(y)  →  ∫dy/h(y) = ∫g(x)dx
```

**Example — Exponential growth/decay:**
```
dy/dt = ky
Separate: dy/y = k dt
Integrate: ln|y| = kt + C
Solution: y(t) = y₀ · eᵏ���
```

k > 0: exponential growth (population, compound interest, viral spread)
k < 0: exponential decay (radioactive decay, cooling, drug metabolism)

This single equation models an enormous range of phenomena. Doubling time = ln(2)/k.

#### Linear first-order: dy/dx + P(x)y = Q(x)
Solved using an integrating factor μ(x) = e^(∫P(x)dx).

### 2. Second-Order Linear ODEs

**General form:** y'' + p(x)y' + q(x)y = f(x)

**The big three:**

#### Simple harmonic oscillator (no damping, no forcing)
```
y'' + ω²y = 0
Solution: y = A cos(ωt) + B sin(ωt)
```
Models: springs, pendulums (small angle), LC circuits, sound waves, light. This is the most fundamental oscillation in physics.

#### Damped oscillator
```
y'' + 2γy' + ω²y = 0
```
Three regimes depending on γ vs ω:
- **Underdamped** (γ < ω): oscillates with decaying amplitude
- **Critically damped** (γ = ω): fastest return to equilibrium without oscillating
- **Overdamped** (γ > ω): slow exponential return, no oscillation

Critical damping is the engineering sweet spot — car suspensions, door closers, and servo systems are tuned here.

#### Forced oscillator
```
y'' + 2γy' + ω²y = F₀cos(Ωt)
```
**Resonance:** When driving frequency Ω ≈ natural frequency ω, amplitude explodes (if damping is small). This destroys bridges (Tacoma Narrows), shatters wine glasses, and is used constructively in MRI, radio tuning, and microwave ovens.

### 3. Systems of ODEs

Multiple quantities changing simultaneously:
```
dx/dt = ax + by
dy/dt = cx + dy
```

In matrix form: **x'(t) = Ax(t)**

Solution depends on eigenvalues of A:
- **Real negative eigenvalues:** Stable — system returns to equilibrium
- **Real positive eigenvalues:** Unstable — system diverges
- **Complex eigenvalues:** Oscillation (imaginary part = frequency, real part = growth/decay)
- **Pure imaginary:** Perpetual oscillation (no damping)

**Phase portraits** visualize the behavior: spirals, saddle points, centers, nodes.

### 4. Existence and Uniqueness

**Picard-Lindelöf theorem:** If f(x,y) and ∂f/∂y are continuous near a point, then dy/dx = f(x,y) with y(x₀) = y₀ has a UNIQUE local solution.

Translation: for "nice" equations, the initial conditions completely determine the future. This is **determinism** in classical physics — and it's a theorem, not an assumption.

---

## Part II: Partial Differential Equations (PDEs)

Multiple independent variables (space + time, or multiple spatial dimensions).

### 5. The Big Three PDEs

These three equations describe nearly all classical physics:

#### The Heat Equation
```
∂u/∂t = α∇²u
```
Models: heat diffusion, chemical diffusion, smoothing/blurring in image processing.

**Behavior:** Irregularities smooth out over time. A spike of heat spreads and flattens. This is also how Gaussian blur works — it literally solves the heat equation.

#### The Wave Equation
```
∂²u/∂t² = c²∇²u
```
Models: vibrating strings, sound waves, electromagnetic waves, seismic waves.

**Behavior:** Disturbances propagate at speed c without changing shape (in ideal conditions).

**Maxwell's equations reduce to the wave equation** — which is how Maxwell predicted electromagnetic waves and identified light as one.

#### Laplace's/Poisson's Equation
```
∇²u = 0    (Laplace — no sources)
∇²u = f    (Poisson — with sources)
```
Models: steady-state heat, electric potential, gravitational potential, fluid flow.

**Behavior:** The solution at any point equals the average of surrounding values. This is the mathematical basis of "equilibrium."

### 6. Solving PDEs

Most PDEs cannot be solved in closed form. Major techniques:

**Separation of variables:** Assume u(x,t) = X(x)T(t). Split the PDE into two ODEs.

**Fourier series/transforms:** Decompose into frequency components. Each component satisfies a simpler equation. See [Fourier Analysis](../04-applied/fourier-analysis.md).

**Numerical methods:** Discretize space and time into a grid. Approximate derivatives with finite differences. This is how computers simulate physics. See [Numerical Methods](../04-applied/numerical-methods.md).

---

## Part III: Dynamical Systems and Chaos

### 7. Dynamical Systems

A dynamical system is any system whose state evolves according to a fixed rule. ODEs define continuous dynamical systems.

**Fixed points:** States where the system doesn't change (x' = 0). Can be stable (attracting) or unstable (repelling).

**Limit cycles:** Periodic orbits that nearby trajectories approach. Models heartbeats, predator-prey cycles, circadian rhythms.

**Bifurcations:** Qualitative changes in behavior as a parameter varies. Example: a stable equilibrium suddenly becomes unstable and splits into a limit cycle.

### 8. Chaos

**Chaos:** Deterministic systems with sensitive dependence on initial conditions.

The poster child: the **Lorenz system** (simplified weather model):
```
dx/dt = σ(y - x)
dy/dt = x(ρ - z) - y
dz/dt = xy - βz
```

Properties of chaotic systems:
- **Deterministic:** Equations have unique solutions (Picard-Lindelöf)
- **Sensitive dependence:** Tiny changes in initial conditions lead to wildly different outcomes
- **Bounded:** Trajectories stay in a finite region
- **Aperiodic:** Never exactly repeats

This is why weather forecasting has fundamental limits — not because our models are wrong, but because the atmosphere is a chaotic system. Small measurement errors grow exponentially (the "butterfly effect").

**Lyapunov exponents** quantify the rate of divergence. Positive exponent = chaos.

### 9. Control Theory

Given a dynamical system, how do you steer it?

**State-space model:**
```
x'(t) = Ax(t) + Bu(t)     (state equation)
y(t)  = Cx(t) + Du(t)      (output equation)
```
- x: state vector
- u: control input
- y: observed output
- A, B, C, D: matrices

**Controllability:** Can you reach any state from any other? (Rank condition on [B, AB, A²B, ...])

**Observability:** Can you determine the internal state from outputs? (Rank condition on [C; CA; CA²; ...])

**Feedback control (PID):**
```
u(t) = Kp·e(t) + Ki·∫e(t)dt + Kd·de/dt
```
P = proportional (react to error now), I = integral (eliminate persistent bias), D = derivative (anticipate future error).

PID controllers are in every thermostat, cruise control, drone stabilizer, and industrial process on Earth.

---

## What Differential Equations Power

| Technology | The equation |
|------------|-------------|
| **Bridges, buildings** | Stress/strain equations (Navier-Cauchy) |
| **Weather forecasting** | Navier-Stokes equations (fluid dynamics) |
| **GPS satellites** | Orbital mechanics (Newton/Einstein field equations) |
| **MRI scanners** | Bloch equations (nuclear spin dynamics) |
| **Drug dosing** | Pharmacokinetic compartment models (first-order ODEs) |
| **Circuit design** | Kirchhoff's laws → systems of ODEs |
| **Climate models** | Coupled ocean-atmosphere PDEs |
| **Autopilot / drones** | Control theory (LQR, Kalman filter) |
| **Epidemiology** | SIR model: dS/dt = -βSI, dI/dt = βSI - γI, dR/dt = γI |
| **Neural ODEs** | ML model where layers are continuous: dh/dt = f(h,t,θ) |

---

## Key Insight

Differential equations are the language nature speaks. Newton wrote F = ma, which is a differential equation (acceleration = second derivative of position). Maxwell wrote four equations, and electromagnetic waves — light, radio, X-rays — fell out. Schrödinger wrote one equation, and quantum mechanics emerged. The remarkable thing is not that the physical world obeys math, but that such simple equations describe such complex behavior — and that the same equations (heat, wave, Laplace) appear in domain after domain.

---

**Next:** [Geometry & Topology](geometry-topology.md) — the mathematics of shape, space, and what's preserved under deformation.
