# Probability & Statistics — The Mathematics of Uncertainty

> The world is uncertain. Probability quantifies that uncertainty.
> Statistics uses data to update our understanding of it.
> Together they form the mathematical backbone of science, medicine, AI, and decision-making.

**Prerequisites:** [Set Theory](../01-foundations/set-theory.md), [Analysis](../02-core-structures/analysis.md) (limits, integration), [Combinatorics](discrete-math.md)
**What it enables:** Machine learning, Bayesian inference, statistical mechanics, information theory, finance, epidemiology, experimental science.

---

## Part I: Probability — Quantifying Uncertainty

### 1. The Axioms (Kolmogorov, 1933)

A **probability space** (Ω, F, P) consists of:
- **Ω** (sample space): the set of all possible outcomes
- **F** (sigma-algebra): the set of events we can measure (subsets of Ω)
- **P** (probability measure): assigns a number to each event

**The three axioms:**
1. P(A) ≥ 0 for any event A (probabilities are non-negative)
2. P(Ω) = 1 (something must happen)
3. For disjoint events: P(A ∪ B) = P(A) + P(B) (probabilities add for non-overlapping events)

Everything in probability follows from these three rules. This is measure theory ([Analysis](../02-core-structures/analysis.md)) with total measure = 1.

### 2. Counting and Classical Probability

When all outcomes are equally likely:
```
P(event) = |favorable outcomes| / |total outcomes|
```

**Example:** Roll a fair die.
```
P(even) = |{2,4,6}| / |{1,2,3,4,5,6}| = 3/6 = 1/2
```

This requires combinatorics to count outcomes:
```
P(poker flush) = C(4,1)·C(13,5) / C(52,5) = 5,148 / 2,598,960 ≈ 0.2%
```

### 3. Conditional Probability and Independence

**Conditional probability:** The probability of A given that B has occurred:
```
P(A|B) = P(A ∩ B) / P(B)
```

"Knowing B is true, what's the chance of A?" This reweights the probability by restricting to the world where B happened.

**Independence:** A and B are independent if knowing one tells you nothing about the other:
```
P(A ∩ B) = P(A) · P(B)    ↔    P(A|B) = P(A)
```

Coin flips are independent. Drawing cards without replacement is NOT independent.

### 4. Bayes' Theorem — Updating Beliefs

```
P(A|B) = P(B|A) · P(A) / P(B)
```

In words: **posterior = likelihood × prior / evidence**

This reverses the direction of conditioning. You know P(symptoms | disease) from medical studies. Bayes gives you P(disease | symptoms) — what you actually need for diagnosis.

**Example:** A test is 99% accurate. The disease affects 1% of people.
```
P(disease | positive test) = P(positive | disease) · P(disease) / P(positive)
                           = 0.99 × 0.01 / (0.99×0.01 + 0.01×0.99)
                           = 0.5  (only 50%!)
```

Even with a 99% accurate test, a positive result is a coin flip when the disease is rare. The base rate matters enormously. This is why intuition about probability fails — humans systematically ignore prior probabilities.

### 5. Random Variables

A **random variable** X is a function from outcomes to numbers: X: Ω → R.

```
X = "number shown on die"
X = "sum of two dice"
X = "height of a randomly chosen person"
```

### Discrete distributions

| Distribution | What it models | PMF |
|-------------|---------------|-----|
| **Bernoulli(p)** | Single yes/no trial | P(X=1)=p, P(X=0)=1-p |
| **Binomial(n,p)** | Number of successes in n trials | C(n,k)pᵏ(1-p)ⁿ⁻ᵏ |
| **Geometric(p)** | Trials until first success | (1-p)ᵏ⁻¹p |
| **Poisson(λ)** | Events in a fixed interval | e⁻λλᵏ/k! |

### Continuous distributions

| Distribution | What it models | Key property |
|-------------|---------------|-------------|
| **Uniform(a,b)** | Equal likelihood on an interval | f(x) = 1/(b-a) |
| **Normal(μ,σ²)** | Bell curve / Gaussian | Appears everywhere (CLT) |
| **Exponential(λ)** | Time between events | Memoryless |

The **normal distribution** deserves special attention. The Central Limit Theorem says: the average of many independent random variables approaches a normal distribution, regardless of the underlying distribution. This is why the bell curve appears everywhere — heights, test scores, measurement errors, financial returns.

### 6. Expectation and Variance

**Expected value (mean):** The long-run average.
```
E[X] = Σ x · P(X=x)           (discrete)
E[X] = ∫ x · f(x) dx          (continuous)
```

**Variance:** How spread out the values are.
```
Var(X) = E[(X - E[X])²] = E[X²] - (E[X])²
```

**Standard deviation:** σ = √Var(X) (same units as X).

Key properties:
```
E[aX + b] = aE[X] + b
Var(aX + b) = a²Var(X)
E[X + Y] = E[X] + E[Y]                      (ALWAYS — even if dependent)
Var(X + Y) = Var(X) + Var(Y)                  (only if independent)
```

### 7. The Law of Large Numbers and Central Limit Theorem

**Law of Large Numbers:** As you take more samples, the sample average converges to the true mean.
```
(X₁ + X₂ + ... + Xₙ)/n → E[X]    as n → ∞
```

This is why casinos always win in the long run, and why polls become more accurate with more respondents.

**Central Limit Theorem (CLT):** The distribution of the sample average approaches a normal distribution as n grows, regardless of the original distribution.
```
(X̄ - μ) / (σ/√n) → N(0,1)    as n → ∞
```

The CLT is why the normal distribution is everywhere. It's one of the most remarkable facts in mathematics.

---

## Part II: Statistics — Learning from Data

### 8. Estimation

Given data, estimate unknown parameters.

**Maximum Likelihood Estimation (MLE):** Choose the parameter value that makes the observed data most probable.
```
θ̂_MLE = argmax_θ P(data | θ)
```

**Example:** You flip a coin 100 times, get 60 heads. MLE says p̂ = 0.60.

**Bayesian estimation:** Start with a prior belief P(θ), update with data:
```
P(θ | data) ∝ P(data | θ) · P(θ)
```

MLE gives a single best guess. Bayesian gives a full distribution over guesses.

### 9. Hypothesis Testing

**The framework:**
1. State a null hypothesis H₀ (e.g., "the coin is fair")
2. Compute a test statistic from data
3. Calculate the p-value: probability of seeing data this extreme IF H₀ is true
4. If p-value < threshold (usually 0.05), reject H₀

**Common tests:**
- **t-test:** Is the mean different from some value?
- **chi-squared test:** Do observed frequencies match expected ones?
- **ANOVA:** Are means different across multiple groups?

**The p-value is NOT P(H₀ is true).** It's P(data | H₀). Confusing these is the most common statistical error.

### 10. Regression

Model the relationship between variables:

**Linear regression:** y = β₀ + β₁x + ε
- β₁ = slope (how much y changes per unit change in x)
- Estimated by least squares — pure linear algebra: β̂ = (XᵀX)⁻¹Xᵀy

**Logistic regression:** For binary outcomes (yes/no):
```
P(Y=1|x) = 1 / (1 + e^(-(β₀ + β₁x)))
```

This is the simplest neural network — a single-layer classifier.

---

## Part III: Advanced Topics

### 11. Stochastic Processes

Random phenomena evolving over time.

**Markov chains:** The future depends only on the present, not the past.
```
P(Xₙ₊₁ | Xₙ, Xₙ₋₁, ..., X₀) = P(Xₙ₊₁ | Xₙ)
```

Applications: PageRank (random walk on the web), MCMC sampling, queuing theory, speech recognition (HMMs).

**Brownian motion:** Continuous random walk. Models stock prices, particle movement, diffusion.

**Poisson process:** Events occurring randomly in time at a constant average rate. Models: server requests, radioactive decay, customer arrivals.

### 12. Information Theory

Founded by Claude Shannon (1948). See [Information Theory](../04-applied/information-theory.md) for the full treatment.

**Entropy:** H(X) = -Σ P(x) log₂ P(x) — the average amount of surprise/information in a random variable.

**Key results:**
- A fair coin has maximum entropy (1 bit) — most uncertain
- A biased coin has lower entropy — more predictable
- You can't compress below the entropy rate (Shannon's source coding theorem)
- Noisy channels have a capacity (Shannon's channel coding theorem)

---

## What Probability & Statistics Powers

| Technology | How probability/statistics is used |
|------------|-----------------------------------|
| **Machine learning** | Loss functions, SGD, Bayesian models, generalization bounds |
| **A/B testing** | Hypothesis testing to compare product variants |
| **Spam filters** | Naive Bayes classifier — P(spam \| words) via Bayes' theorem |
| **Weather forecasting** | Probabilistic models, ensemble methods, Bayesian updates |
| **Finance** | Portfolio theory (Markowitz), Black-Scholes (stochastic calculus), risk models |
| **Medicine** | Clinical trials, diagnostic accuracy, epidemiological models |
| **Natural language** | Language models: P(next word \| previous words) |
| **Quantum mechanics** | Fundamentally probabilistic — wavefunctions give probability amplitudes |
| **Recommendation systems** | Collaborative filtering, probabilistic matrix factorization |
| **Search engines** | Relevance ranking, click-through probability models |

---

## Key Insight

Probability is the language of uncertainty, and statistics is the practice of learning from incomplete information. In a deterministic world, we'd need only logic and algebra. But the real world is noisy, incomplete, and uncertain — and so probability and statistics are not optional branches of math but essential tools for navigating reality. The fact that modern AI is fundamentally probabilistic (language models predict probability distributions over next tokens) shows how central this branch has become.

---

**Next:** [Number Theory](number-theory.md) — the ancient study of integers that now secures the internet.
