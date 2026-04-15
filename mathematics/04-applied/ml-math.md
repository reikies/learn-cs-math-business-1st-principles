# The Math of Machine Learning — Where Everything Converges

> Machine learning is not a new branch of mathematics. It's the convergence point —
> the place where linear algebra, calculus, probability, optimization, and information
> theory all meet to solve a single problem: learning patterns from data.

**Prerequisites:** [Linear Algebra](../02-core-structures/linear-algebra.md), [Calculus](../02-core-structures/calculus.md), [Probability](../03-branches/probability.md), [Optimization](optimization.md), [Information Theory](information-theory.md)
**What it enables:** Neural networks, transformers, generative AI, computer vision, NLP, recommendation systems, autonomous vehicles.

---

## The Learning Problem

Given data {(x₁, y₁), ..., (xₙ, yₙ)}, find a function f that maps inputs x to outputs y — and generalizes to unseen data.

Every ML algorithm is a recipe for:
1. Defining a function family (model architecture)
2. Defining a loss function (what "good" means)
3. Finding the best function in that family (optimization)
4. Ensuring it works on new data (generalization)

Each step is a different area of mathematics.

---

## 1. Linear Algebra in ML

### The forward pass is matrix multiplication

A neural network layer:
```
h = σ(Wx + b)
```
- W: weight matrix (parameters to learn)
- x: input vector
- b: bias vector
- σ: activation function (nonlinearity)

A deep network: y = f_L(f_{L-1}(...f_1(x)...)) — each f_i is a matrix multiply + nonlinearity.

**The entire forward pass is a sequence of matrix multiplications.** This is why GPUs (optimized for matrix math) dominate ML.

### Embeddings are vectors

Words, tokens, images — all represented as vectors in high-dimensional space.

```
"king" → [0.2, 0.8, -0.1, ...]  (300 or more dimensions)
"queen" → [0.3, 0.7, 0.1, ...]
```

Famous result: king - man + woman ≈ queen (vector arithmetic captures semantic relationships).

### Attention is matrix operations

The transformer attention mechanism:
```
Attention(Q, K, V) = softmax(QKᵀ/√d_k) · V
```
- Q, K, V: query, key, value matrices (linear projections of input)
- QKᵀ: dot products between all pairs (similarity matrix)
- softmax: normalizes to attention weights
- Multiply by V: weighted combination of values

This is entirely linear algebra — matrix multiplications and a softmax normalization.

### SVD and dimensionality reduction

**PCA (Principal Component Analysis):** Project data onto the directions of maximum variance.
1. Compute covariance matrix Σ = (1/n)XᵀX
2. Find eigenvectors (or use SVD: X = UΣVᵀ)
3. Keep top k eigenvectors as new coordinates

This compresses data while preserving the most important structure. Used for visualization, denoising, and preprocessing.

---

## 2. Calculus in ML

### Backpropagation IS the chain rule

Training requires computing ∂L/∂wᵢ for every weight — how does each weight affect the loss?

For a composition y = f₃(f₂(f₁(x))):
```
∂L/∂w₁ = (∂L/∂y) · (∂y/∂h₂) · (∂h₂/∂h₁) · (∂h₁/∂w₁)
```

Each factor is a local derivative at one layer. **Backpropagation** computes all these derivatives efficiently in one backward pass through the network — pure chain rule, applied systematically.

### Gradients guide optimization

```
θ_{t+1} = θ_t - α · ∂L/∂θ
```

Every training step: compute the gradient of the loss with respect to ALL parameters, move in the opposite direction.

For a model with billions of parameters, this gradient is a billion-dimensional vector — computed efficiently using the chain rule and automatic differentiation.

### Key derivatives in ML

| Function | Derivative | Where it appears |
|----------|-----------|-----------------|
| σ(x) = 1/(1+e⁻ˣ) (sigmoid) | σ(1-σ) | Binary classification output |
| tanh(x) | 1 - tanh²(x) | RNN activation |
| ReLU(x) = max(0,x) | 1 if x>0, 0 if x<0 | Most common activation |
| softmax(xᵢ) = eˣⁱ/Σeˣʲ | softmax(xᵢ)(δᵢⱼ - softmax(xⱼ)) | Multi-class output, attention |
| log(x) | 1/x | Cross-entropy loss |

### The vanishing/exploding gradient problem
When networks are deep, gradients are products of many factors. If factors are <1, gradients shrink exponentially (vanish). If >1, they explode. Solutions:
- **ReLU:** Derivative is exactly 1 for positive inputs (no shrinkage)
- **Residual connections:** y = f(x) + x — gradient has a direct path
- **Layer normalization:** Keeps activations in a stable range
- **Careful initialization:** Xavier/He initialization sets initial weights to preserve gradient magnitude

---

## 3. Probability in ML

### Probabilistic interpretation of models

A classifier outputs P(y|x) — the probability of each class given the input.

A language model outputs P(next_token | previous_tokens) — probability distribution over vocabulary.

### Maximum Likelihood Estimation

Training = maximizing the likelihood of the observed data under the model:
```
θ* = argmax_θ Π P(yᵢ | xᵢ; θ)
```

Taking the log (equivalent, since log is monotonic):
```
θ* = argmax_θ Σ log P(yᵢ | xᵢ; θ)
```

**Minimizing cross-entropy loss IS maximizing log-likelihood.** The most common loss functions are all likelihood-based:

| Loss | Probabilistic interpretation |
|------|------------------------------|
| MSE (mean squared error) | Gaussian likelihood |
| Cross-entropy | Categorical/Bernoulli likelihood |
| KL divergence | Information loss from approximation |

### Bayesian inference in ML

Instead of point estimates, maintain distributions over parameters:
```
P(θ | data) ∝ P(data | θ) · P(θ)
```

- **Bayesian neural networks:** Weights are distributions, not single values. Uncertainty quantification built in.
- **Gaussian processes:** Non-parametric Bayesian model. The function itself is a random variable.
- **Variational inference:** Approximate intractable posteriors with simpler distributions by minimizing KL divergence.

### Generative models

**VAE (Variational Autoencoder):** Learn a latent space where data lives. Encoder maps data → latent distribution, decoder maps latent → data. Trained by maximizing a lower bound on log-likelihood (ELBO).

**Diffusion models:** Start with data, gradually add noise until it's pure noise. Learn to reverse the process. Based on stochastic differential equations and score matching.

**Autoregressive models (GPT):** Model P(x₁, x₂, ..., xₙ) = P(x₁)P(x₂|x₁)P(x₃|x₁,x₂)... — the chain rule of probability applied to sequences.

---

## 4. Optimization in ML

### SGD and variants

Full gradient descent is too expensive — computing ∂L/∂θ over the entire dataset at each step. **SGD** uses a random mini-batch instead:

```
θ ← θ - α · (1/|B|) Σ_{i∈B} ∇L(xᵢ, yᵢ; θ)
```

The gradient estimate is noisy but unbiased — it points in the right direction on average.

### Adam optimizer
```
m_t = β₁m_{t-1} + (1-β₁)g_t           (momentum — moving average of gradients)
v_t = β₂v_{t-1} + (1-β₂)g_t²          (adaptive learning rate — moving average of squared gradients)
θ_t = θ_{t-1} - α · m̂_t / (√v̂_t + ε)
```

Adapts the learning rate per-parameter. Default choice for most deep learning.

### The loss landscape

Neural network loss functions are highly non-convex — millions of dimensions, countless local minima and saddle points.

Surprisingly, this works because:
- In high dimensions, most critical points are saddle points, not local minima
- Local minima that exist tend to have similar loss values (the landscape is "flat" near the bottom)
- SGD noise helps escape sharp minima (implicit regularization)

### Regularization (preventing overfitting)

| Technique | Math | Effect |
|-----------|------|--------|
| **L2 regularization** | Add λ‖θ‖² to loss | Penalizes large weights |
| **L1 regularization** | Add λ‖θ‖₁ to loss | Promotes sparsity (many weights → 0) |
| **Dropout** | Randomly zero activations with probability p | Ensemble effect, prevents co-adaptation |
| **Early stopping** | Stop before training loss reaches minimum | Implicit regularization via limited optimization |
| **Data augmentation** | Transform training data (flip, rotate, crop) | Increases effective dataset size |

---

## 5. Information Theory in ML

### Cross-entropy loss
```
L = -Σ y_true · log(y_predicted)
```

This IS the cross-entropy H(P, Q) where P is the data distribution and Q is the model. Minimizing this = making the model's probability assignments match reality.

### Bits-per-character / Perplexity
Language model quality measured as:
```
Perplexity = 2^(cross-entropy)
```

A perplexity of 10 means the model is as uncertain as uniform choice among 10 options. State-of-the-art models achieve perplexity under 10 on English text.

### Mutual information and representation learning
Good representations have high mutual information I(representation; target) while being compressed (low I(representation; input)). This is the **information bottleneck** principle.

---

## 6. The Key Architectures (Mathematically)

### Multilayer Perceptron (MLP)
```
h₁ = σ(W₁x + b₁)
h₂ = σ(W₂h₁ + b₂)
y  = W₃h₂ + b₃
```
Universal approximation: with enough hidden units, an MLP can approximate any continuous function.

### Convolutional Neural Network (CNN)
Convolution: (f * g)[n] = Σ f[k]g[n-k] — a weighted sliding window.

```
Output[i,j] = Σ_{m,n} Input[i+m, j+n] · Kernel[m,n]
```

Convolution in spatial domain = multiplication in Fourier domain. CNNs learn frequency-selective filters.

**Key properties:** Translation invariance (same pattern detected anywhere), parameter sharing (one kernel applied everywhere), hierarchical features (edges → textures → parts → objects).

### Recurrent Neural Network (RNN)
```
h_t = σ(W_h h_{t-1} + W_x x_t + b)
```

A dynamical system — the hidden state evolves over time. Theoretically: a universal computer (Turing complete). Practically: suffers from vanishing gradients over long sequences.

**LSTM/GRU:** Add gating mechanisms to control information flow. The gates are sigmoid functions deciding what to remember and forget.

### Transformer
```
Attention(Q,K,V) = softmax(QKᵀ/√d) · V

MultiHead(Q,K,V) = Concat(head₁, ..., headₕ)W_O
  where headᵢ = Attention(QWᵢQ, KWᵢK, VWᵢV)
```

**Self-attention:** Every token attends to every other token. O(n²) in sequence length — all-pairs similarity via dot products.

**Why it works:** Attention can learn arbitrary dependencies between positions. No sequential bottleneck (unlike RNNs). Highly parallelizable (unlike RNNs).

**The FFN block:** Two linear layers with nonlinearity. Operates independently per position. Believed to store factual knowledge.

---

## The Mathematical Stack of a Single Training Step

```
1. Sample mini-batch           (Probability — random sampling)
2. Forward pass                (Linear Algebra — matrix multiplications)
3. Compute loss                (Information Theory — cross-entropy)
4. Backward pass               (Calculus — chain rule / backpropagation)
5. Update parameters           (Optimization — Adam/SGD step)
6. Regularize                  (Statistics — bias-variance tradeoff)
```

Every training step uses ALL the core branches of mathematics simultaneously. This is why ML is the ultimate convergence point of the mathematical system.

---

## What ML Math Powers

| Application | Math that matters most |
|-------------|----------------------|
| **GPT/LLMs** | Linear algebra (attention), probability (next-token prediction), optimization (training) |
| **Image recognition** | Convolution (Fourier theory), optimization, probability |
| **AlphaFold** | Attention, geometric deep learning (SE(3) equivariance), optimization |
| **Stable Diffusion** | Stochastic differential equations, variational inference, U-Net architecture |
| **Recommendation systems** | Matrix factorization (SVD), collaborative filtering |
| **Reinforcement learning** | Dynamic programming (Bellman), probability (policy gradient), optimization |
| **Self-driving cars** | Computer vision (CNNs), control theory, probabilistic state estimation |

---

## Key Insight

Machine learning is not one field of math — it's the applied synthesis of nearly every field in this map. Linear algebra provides the computational substrate. Calculus provides the training signal. Probability provides the modeling framework. Optimization provides the learning algorithm. Information theory provides the objective. The reason ML has been so transformative is that it sits at the intersection of all these mature mathematical frameworks, combining their power to solve problems that no single framework could address alone.

---

**Back to:** [The Complete Map](../00-map.md)
