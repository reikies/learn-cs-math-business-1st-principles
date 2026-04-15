# AI & Machine Learning — Machines That Learn

> For most of computing's history, we told machines exactly what to do — step by step,
> rule by rule. Machine learning flipped the script: give the machine data and a goal,
> and let it figure out the rules itself. This is the most consequential shift in
> computing since the internet.

**Prerequisites:** [Programming Languages](../02-systems/programming-languages.md), [Data Structures & Algorithms](../03-theory/data-structures-and-algorithms.md) — You need code and mathematical thinking. Linear algebra, calculus, and probability from [Mathematics](../../mathematics/00-map.md) deepen understanding enormously.
**What it enables:** Language models (ChatGPT), computer vision, recommendation systems, self-driving cars, drug discovery, robotics.

---

## Why Machine Learning Exists

Some problems can't be solved by writing rules:

| Problem | Why rules fail |
|---------|---------------|
| Recognize a cat in a photo | Millions of possible appearances — no rule covers them all |
| Translate English to Japanese | Grammar is context-dependent, idiomatic, ambiguous |
| Recommend a song you'll like | Taste is subjective and evolves |
| Detect fraud | Fraudsters constantly change tactics |
| Play Go | More possible games than atoms in the universe |

The insight: instead of writing rules, **learn them from data.**

```
Traditional programming:  Rules + Data → Answers
Machine learning:         Data + Answers → Rules (a model)
```

---

## 1. The Core Idea — Learning from Data

Every ML system has the same structure:

```
Training Data  →  [Learning Algorithm]  →  Model  →  Predictions
(examples)          (adjusts model)       (the rules)  (on new data)
```

**Training data:** Examples of inputs paired with correct outputs (in supervised learning).
**Model:** A mathematical function with adjustable parameters.
**Learning:** Finding the parameter values that make the model's outputs match the training data.
**Prediction:** Applying the trained model to new, unseen data.

The fundamental question: **does the model generalize?** Memorizing the training data is useless — the test is whether it works on data it has never seen.

---

## 2. Types of Machine Learning

### Supervised Learning
You have labeled data: inputs paired with correct outputs.

```
Input: photo of cat  →  Label: "cat"
Input: photo of dog  →  Label: "dog"
```

The model learns to map inputs to outputs. Two subtypes:
- **Classification:** Predict a category (cat vs dog, spam vs not spam)
- **Regression:** Predict a number (house price, temperature tomorrow)

### Unsupervised Learning
You have data with no labels. The model finds structure on its own.

- **Clustering:** Group similar items (customer segments, gene families)
- **Dimensionality reduction:** Compress data while keeping structure (PCA, t-SNE)
- **Anomaly detection:** Find things that don't fit the pattern

### Reinforcement Learning
An agent interacts with an environment, takes actions, and receives rewards. It learns to maximize total reward over time.

```
Agent  ──action──►  Environment
  ▲                     │
  └──── reward + state ─┘
```

Used for: game playing (AlphaGo, AlphaZero), robotics, recommendation systems, RLHF for LLMs.

### Self-Supervised Learning
The model creates its own labels from raw data. Example: mask a word in a sentence and predict it. This is how modern LLMs are pre-trained — no human labeling needed.

---

## 3. Linear Models — The Foundation

The simplest model: a straight line.

**Linear regression:**
```
y = w₁x₁ + w₂x₂ + ... + wₙxₙ + b
```

- **x** values are features (inputs)
- **w** values are weights (learned parameters)
- **b** is the bias (intercept)

To predict house price: x₁ = square footage, x₂ = bedrooms, x₃ = location score. The model learns weights that best fit the training data.

**Logistic regression** (for classification): wrap linear regression in a sigmoid function to output probabilities between 0 and 1.

```
P(spam) = sigmoid(w₁x₁ + w₂x₂ + ... + b)
        = 1 / (1 + e^(-z))    where z is the linear combination
```

These models are simple, fast, and interpretable. They're the baseline that every other model must beat.

---

## 4. The Loss Function — Measuring How Wrong You Are

A **loss function** quantifies the gap between the model's prediction and the truth.

| Task | Loss function | Intuition |
|------|--------------|-----------|
| Regression | Mean Squared Error (MSE) | Average of (prediction - actual)² |
| Classification | Cross-Entropy Loss | How surprised you are by the wrong answer |
| Ranking | Hinge Loss | Penalize when correct ranking is violated |

Learning = minimizing the loss function. The model adjusts its parameters to make the loss as small as possible.

---

## 5. Gradient Descent — How Models Learn

**Gradient descent** is the optimization algorithm at the heart of nearly all ML.

The idea: you're on a hilly landscape (the loss surface). You want to find the lowest point (minimum loss). You can't see the whole landscape — you can only feel the slope beneath your feet.

```
1. Start at a random point (random parameters)
2. Compute the gradient (slope) of the loss at the current point
3. Take a step in the direction of steepest descent
4. Repeat until the loss stops decreasing
```

```
Loss
 │  *
 │   \
 │    \        *
 │     \      / \
 │      \    /   \
 │       \  /     \___*___  ← local minimum
 │        \/
 │         * ← global minimum
 └─────────────────────── Parameters
```

**The learning rate** controls step size:
- Too large → overshoot the minimum, diverge
- Too small → converge too slowly
- Just right → smooth convergence

**Stochastic Gradient Descent (SGD):** Instead of computing the gradient on the entire dataset (expensive), compute it on a random mini-batch. Noisier but much faster. The noise actually helps escape local minima.

**Variants:** Adam (adaptive learning rates, momentum), RMSProp, AdaGrad. Adam is the default choice for most deep learning.

---

## 6. Neural Networks — Layers of Simple Functions

A neural network is a function built by composing simple layers:

```
Input Layer     Hidden Layer(s)     Output Layer
  x₁ ──────►  [neuron]  ──────►  [neuron] ──► prediction
  x₂ ──────►  [neuron]  ──────►
  x₃ ──────►  [neuron]  ──────►
```

Each **neuron** computes:
```
output = activation(w₁x₁ + w₂x₂ + ... + b)
```

The **activation function** introduces nonlinearity — without it, stacking layers would just be another linear function. Common activations:

| Activation | Formula | Used for |
|-----------|---------|----------|
| ReLU | max(0, x) | Hidden layers (default) |
| Sigmoid | 1/(1+e⁻ˣ) | Binary classification output |
| Softmax | eˣⁱ / Σeˣʲ | Multi-class classification output |
| Tanh | (eˣ - e⁻ˣ)/(eˣ + e⁻ˣ) | Hidden layers (older networks) |

### Why depth matters

A single hidden layer can theoretically approximate any function (universal approximation theorem). But **deep** networks (many layers) learn hierarchical features much more efficiently:

```
Image → Layer 1: edges → Layer 2: textures → Layer 3: parts → Layer 4: objects
        (lines,          (fur, skin,          (eyes, ears,      (cat, dog,
         corners)         stripes)              wheels)           car)
```

Each layer builds on the patterns discovered by the previous layer. This hierarchical representation is why deep learning works so well.

### Backpropagation — How deep networks learn

Backpropagation is just the chain rule of calculus applied to compute gradients through the network:

```
Forward pass: compute predictions
    Input → Layer 1 → Layer 2 → ... → Output → Loss

Backward pass: compute gradients
    Loss → ∂L/∂Output → ∂L/∂Layer2 → ∂L/∂Layer1 → ∂L/∂Weights
```

For each weight in the network, backprop computes: "how much would the loss change if I nudged this weight slightly?" Then gradient descent uses those gradients to update every weight.

---

## 7. Convolutional Neural Networks (CNNs) — Seeing

CNNs are designed for grid-structured data like images.

Key idea: **convolution.** Instead of connecting every input to every neuron, slide a small filter (e.g., 3x3) across the image, detecting local patterns.

```
Input image:          Filter:        Feature map:
┌───────────────┐     ┌─────┐        ┌───────────┐
│ . . # # . . . │     │ 1 0 │        │ detect    │
│ . # # # # . . │  *  │ 0 1 │   →    │ diagonal  │
│ # # . . # # . │     └─────┘        │ edges     │
│ # . . . . # . │                     └───────────┘
└───────────────┘
```

**Pooling** layers downsample, making the representation smaller and more invariant to exact position.

Architecture: Convolution → ReLU → Pool → Convolution → ReLU → Pool → ... → Flatten → Dense → Output

CNNs power: image classification, object detection, medical imaging, self-driving car perception.

---

## 8. Recurrent Neural Networks (RNNs) and Sequence Models

RNNs process sequences by maintaining a hidden state that gets updated at each step:

```
  x₁        x₂        x₃        x₄
  │          │          │          │
  ▼          ▼          ▼          ▼
[RNN] ──► [RNN] ──► [RNN] ──► [RNN] ──► output
  h₁         h₂         h₃         h₄
```

Each step sees the current input AND the previous hidden state. This gives the network "memory" of what came before.

**Problem:** Vanilla RNNs forget quickly (vanishing gradient problem). **LSTM** (Long Short-Term Memory) and **GRU** (Gated Recurrent Unit) solve this with gating mechanisms that control what to remember and forget.

RNNs were the go-to for language tasks until transformers replaced them.

---

## 9. Transformers & Attention — The Architecture Behind Modern AI

The transformer (Vaswani et al., 2017, "Attention Is All You Need") is the most important architecture in modern AI.

### The attention mechanism

The key insight: when processing a word, not all other words in the sentence are equally relevant. **Attention** lets the model learn which parts of the input to focus on.

```
"The cat sat on the mat because it was tired"
                                   ↑
                            "it" attends to "cat"
                            (not "mat") — learned!
```

Mechanically:
1. Each token produces three vectors: Query (Q), Key (K), Value (V)
2. Attention score = dot product of Q with every K (how relevant is each token?)
3. Softmax the scores to get weights
4. Output = weighted sum of V vectors

```
Attention(Q, K, V) = softmax(QKᵀ / √d) · V
```

**Multi-head attention:** Run multiple attention computations in parallel, each learning different relationships (syntax in one head, semantics in another).

### The transformer block

```
Input embeddings
    │
    ▼
┌─────────────────────┐
│ Multi-Head Attention │ ←── "what should I pay attention to?"
├─────────────────────┤
│ Add & LayerNorm     │ ←── residual connection
├─────────────────────┤
│ Feed-Forward Network│ ←── process each position independently
├─────────────────────┤
│ Add & LayerNorm     │
└─────────────────────┘
    │
    ▼
  Output
```

Stack N of these blocks (GPT-4 likely has 100+). The depth and width determine the model's capacity.

### Why transformers won

| Property | RNNs | Transformers |
|----------|------|--------------|
| Parallelism | Sequential (slow to train) | Fully parallel (fast on GPUs) |
| Long-range dependencies | Struggle with long sequences | Attention sees everything |
| Scalability | Limited | Scales to billions of parameters |

### Large Language Models (LLMs)

GPT, Claude, Llama — these are transformer models trained on massive text datasets to predict the next token.

**Pre-training:** Predict the next word in trillions of words of text. The model learns grammar, facts, reasoning, code — all as a side effect of next-word prediction.

**Fine-tuning:** Adapt the pre-trained model for specific tasks using smaller, curated datasets.

**RLHF (Reinforcement Learning from Human Feedback):** Humans rank model outputs. A reward model learns human preferences. The LLM is fine-tuned to maximize the reward. This is what makes models helpful and safe.

**Scaling laws:** Bigger models + more data + more compute = better performance, following predictable power laws. This is why the AI industry is building ever-larger models.

---

## 10. Overfitting and Regularization

**Overfitting:** The model memorizes the training data but fails on new data. Like a student who memorizes answers but can't solve new problems.

```
Training accuracy: 99%
Test accuracy:     62%   ← overfitting!
```

**Underfitting:** The model is too simple to capture the patterns.

```
Training accuracy: 55%
Test accuracy:     53%   ← underfitting!
```

### Regularization techniques (fighting overfitting):

| Technique | How it works |
|-----------|-------------|
| **L2 regularization** | Penalize large weights (add λ·‖w‖² to loss) |
| **Dropout** | Randomly disable neurons during training (forces redundancy) |
| **Early stopping** | Stop training when validation loss starts increasing |
| **Data augmentation** | Create more training data by transforming existing examples |
| **Batch normalization** | Normalize layer outputs (stabilizes training) |

### The bias-variance trade-off

- **Bias:** Error from wrong assumptions (model too simple)
- **Variance:** Error from sensitivity to training data (model too complex)
- The sweet spot is in between — complex enough to capture patterns, simple enough to generalize

---

## 11. The Data Pipeline

ML is 20% models, 80% data.

```
Raw Data → Cleaning → Feature Engineering → Split → Training → Evaluation → Deployment
```

**Data cleaning:** Handle missing values, remove duplicates, fix inconsistencies.
**Feature engineering:** Transform raw data into useful inputs (normalize, encode categories, create derived features).
**Train/validation/test split:** Typically 70/15/15. NEVER evaluate on training data.
**Evaluation metrics:**

| Task | Metrics | What they measure |
|------|---------|-------------------|
| Classification | Accuracy, precision, recall, F1, AUC-ROC | How well you classify |
| Regression | MSE, MAE, R² | How close your predictions are |
| Ranking | NDCG, MAP | How well you order results |
| Generation | Perplexity, BLEU, human eval | How good the output is |

---

## 12. The Hardware — Why AI Needs GPUs

Neural network training is dominated by **matrix multiplication** — multiplying huge matrices of weights and activations.

CPUs: optimized for sequential, branching logic. ~100 cores.
GPUs: optimized for parallel, uniform computation. ~10,000 cores.

```
Matrix multiply: A (4096 × 4096) × B (4096 × 4096)
  CPU: process elements somewhat sequentially
  GPU: process thousands of elements simultaneously
  Speedup: 10-100x
```

| Hardware | Maker | Used for |
|----------|-------|----------|
| H100 / B200 GPU | NVIDIA | Training large models |
| TPU | Google | Training and inference at Google |
| Apple Neural Engine | Apple | On-device inference (iPhone, Mac) |
| Inferentia / Trainium | AWS | Cloud inference and training |

This connects directly back to [Computer Architecture](../01-hardware/computer-architecture.md) — the GPU section. AI's hunger for compute is driving chip design.

---

## 13. AI Safety and Alignment

As AI systems become more capable, ensuring they behave as intended becomes critical:

- **Alignment:** Making AI pursue the goals we actually want, not proxy objectives
- **Hallucination:** Models confidently stating false information
- **Bias:** Models reflecting and amplifying biases in training data
- **Misuse:** Using AI for disinformation, surveillance, autonomous weapons
- **Control:** Maintaining human oversight as systems become more autonomous

This is an active research field, not a solved problem.

---

## Key Mental Models

1. **Learning = optimization:** All of ML is: define a loss function that measures how wrong the model is, then minimize it. The loss function IS the objective — get it wrong and the model optimizes for the wrong thing.

2. **The unreasonable effectiveness of scale:** Many breakthroughs in AI came not from new algorithms but from running existing algorithms on more data with more compute. Transformers existed before GPT — scale is what made them transformative.

3. **No free lunch:** No single model works best for all problems. The right model depends on your data, constraints, and what you're trying to learn. Simple models often beat complex ones on small datasets.

4. **Generalization is everything:** A model that perfectly memorizes training data is useless. The only thing that matters is performance on unseen data. This tension between fitting training data and generalizing drives most of ML theory.

5. **Features > algorithms:** Often, better input features (the right representation of data) matter more than which algorithm you use. This is why deep learning is powerful — it learns features automatically.

---

## How This Connects

**From below:**
- [Computer Architecture](../01-hardware/computer-architecture.md): GPUs and specialized hardware make training possible
- [Programming Languages](../02-systems/programming-languages.md): Python (NumPy, PyTorch, TensorFlow) is the lingua franca of ML
- [Data Structures](../03-theory/data-structures-and-algorithms.md): Efficient data loading, batching, and preprocessing
- [Databases](../04-infrastructure/databases.md): Where training data lives and model outputs are stored
- [Distributed Systems](../04-infrastructure/distributed-systems.md): Training large models across many machines
- [Mathematics](../../mathematics/00-map.md): Linear algebra (matrix ops), calculus (gradients), probability (uncertainty), optimization (training)

**To above:**
- [Applications](../08-applications/applications.md): ML powers search, recommendations, translation, generation, autonomous systems

---

## Hands-On

1. **Train a linear model:** Use scikit-learn to fit a linear regression on a simple dataset (housing prices, etc.). Plot predictions vs actuals.

2. **Build a neural network from scratch:** Implement a 2-layer neural network in Python (just NumPy, no frameworks). Forward pass, loss computation, backpropagation, gradient descent. This is the single most illuminating exercise in ML.

3. **Train a CNN:** Use PyTorch to classify MNIST digits (handwritten 0-9). Get >98% accuracy. Then try CIFAR-10 (real images) — it's much harder.

4. **Fine-tune a pre-trained model:** Use Hugging Face to fine-tune a pre-trained BERT or GPT model on a text classification task. See how transfer learning works.

5. **Explore attention:** Visualize attention patterns in a transformer model. See which words attend to which other words.

6. **Kaggle competition:** Enter a beginner Kaggle competition. The data preprocessing and feature engineering will teach you more than any textbook.

---

## Go Deeper

- **"Deep Learning" by Goodfellow, Bengio, Courville** — The definitive deep learning textbook (free online)
- **fast.ai** — Practical deep learning course (top-down approach, code first)
- **3Blue1Brown's neural network series** — The best visual explanation of neural networks and backpropagation
- **Andrej Karpathy's "Neural Networks: Zero to Hero"** — Build GPT from scratch on YouTube
- **"Attention Is All You Need" (Vaswani et al.)** — The transformer paper. Readable and foundational.
- **Stanford CS229 (Machine Learning)** and **CS231n (Computer Vision)** — Lecture videos online
- **"The Illustrated Transformer" by Jay Alammar** — Visual walkthrough of transformer architecture

---

*Next: [The Application Layer](../08-applications/applications.md) — Where all these layers meet the human.*
