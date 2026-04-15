# Information Theory — The Mathematics of Communication

> Claude Shannon (1948) asked: "What is the fundamental limit of communication?"
> His answer created information theory — the mathematical framework underlying
> all data compression, transmission, and storage. And now, machine learning.

**Prerequisites:** [Probability](../03-branches/probability.md), [Analysis](../02-core-structures/analysis.md) (logarithms, limits)
**What it enables:** Data compression (ZIP, MP3, JPEG), error correction, channel capacity (WiFi, 5G), ML loss functions, cryptography analysis.

---

## 1. What Is Information?

Information is **surprise**. If I tell you the sun rose this morning, that's not informative — it always happens. If I tell you it didn't rise, that's very informative — it's unexpected.

Shannon formalized this: the information content of an event with probability p is:

```
I(x) = -log₂(p(x))    bits
```

| Event | Probability | Information |
|-------|-------------|------------|
| Fair coin lands heads | 1/2 | 1 bit |
| Roll 6 on a fair die | 1/6 | 2.58 bits |
| Sun rises | ~1 | ~0 bits |
| Win the lottery | 1/10⁷ | 23.3 bits |

More surprising = more information = more bits needed to communicate it.

---

## 2. Entropy — Average Information

The **entropy** H(X) of a random variable X is the average information per outcome:

```
H(X) = -Σ p(x) log₂ p(x)
```

| Source | Entropy | Meaning |
|--------|---------|---------|
| Fair coin | 1 bit | Maximum uncertainty for binary |
| Biased coin (90% heads) | 0.47 bits | More predictable, less entropy |
| Constant (always heads) | 0 bits | No uncertainty |
| Fair die | 2.58 bits | More outcomes, more entropy |
| English text | ~1.0-1.5 bits/letter | Highly redundant (predictable) |

**Key property:** Entropy is maximized when all outcomes are equally likely. Any pattern or predictability REDUCES entropy.

**Why "entropy"?** Shannon asked von Neumann what to call it. Von Neumann said: "Call it entropy. Nobody understands entropy, so in a debate you'll always have the advantage."

---

## 3. Shannon's Source Coding Theorem — Compression Limits

**Theorem:** You cannot compress data below its entropy rate, on average. But you can get arbitrarily close.

If a source has entropy H bits per symbol, then:
- You need AT LEAST H bits per symbol on average
- There exist codes that achieve H + ε for any ε > 0

**Practical consequence:** 
- English text has ~1.3 bits/letter of entropy but is encoded in 8 bits/character (ASCII) → massive room for compression
- A fair die needs 2.58 bits/roll; coding it as 3 bits/roll is near-optimal
- You CANNOT losslessly compress random data (it's already at maximum entropy)

### Huffman coding
Assign shorter codes to more frequent symbols:
```
English letter frequencies → Huffman codes:
e (13%) → 3 bits
t (9%)  → 4 bits
z (0.1%) → 10+ bits
```

Average code length approaches entropy. Used in ZIP, PNG, and as a component of most compression algorithms.

---

## 4. Shannon's Channel Coding Theorem — Reliable Communication

**The noisy channel:** Messages get corrupted during transmission. Bits flip, signals distort.

**Channel capacity C:** The maximum rate at which information can be sent reliably through a noisy channel.

For a binary symmetric channel (each bit flips with probability p):
```
C = 1 - H(p)    bits per channel use
```

**Shannon's theorem:** If your transmission rate R < C, there exist error-correcting codes that make the error probability as small as desired. If R > C, errors are inevitable.

This was revolutionary — Shannon proved you CAN communicate reliably over noisy channels, up to a precise limit, without even showing HOW (the constructive codes came later).

**Real-world channels:**
- WiFi, 5G, satellite: shaped by Shannon's capacity formulas
- The Shannon limit C = B log₂(1 + SNR) for bandwidth B and signal-to-noise ratio SNR

---

## 5. Kullback-Leibler Divergence — Distance Between Distributions

```
D_KL(P ‖ Q) = Σ P(x) log(P(x)/Q(x))
```

Measures how different distribution Q is from the "true" distribution P. Not symmetric: D_KL(P‖Q) ≠ D_KL(Q‖P). Not a true distance, but a measure of information loss.

Properties:
- D_KL ≥ 0 always (Gibbs' inequality)
- D_KL = 0 iff P = Q

**Why this matters for ML:** Training a model is minimizing D_KL between the model distribution and the data distribution. Cross-entropy loss = entropy of data + KL divergence of model from data.

---

## 6. Cross-Entropy — The Loss Function of ML

```
H(P, Q) = -Σ P(x) log Q(x) = H(P) + D_KL(P ‖ Q)
```

Since H(P) is constant (determined by data), minimizing cross-entropy IS minimizing KL divergence.

**This is why cross-entropy is THE loss function for classification and language modeling:**
- Binary classification: H = -[y log(ŷ) + (1-y) log(1-ŷ)]
- Multi-class: H = -Σ yᵢ log(ŷᵢ)
- Language models: minimize cross-entropy of predicted next-token distribution vs actual

**Perplexity** = 2^H — the effective number of equally-likely choices the model sees. Lower perplexity = better language model. A perplexity of 20 means the model is as uncertain as choosing uniformly among 20 options.

---

## 7. Mutual Information — What X Tells You About Y

```
I(X; Y) = H(X) - H(X|Y) = H(Y) - H(Y|X) = H(X) + H(Y) - H(X,Y)
```

The reduction in uncertainty about X after observing Y.

- I(X;Y) = 0: X and Y are independent (knowing one tells you nothing)
- I(X;Y) = H(X): Y completely determines X

**Applications:**
- Feature selection in ML: choose features with high mutual information with the target
- Independent Component Analysis (ICA): find components that minimize mutual information
- Neuroscience: how much information does neural activity carry about a stimulus?

---

## 8. Coding Theory — Error Correction

### Repetition codes
Send each bit 3 times: 0→000, 1→111. Majority vote corrects single errors. But 3× overhead!

### Hamming codes
Add parity check bits at specific positions. Hamming(7,4): 4 data bits + 3 parity = correct any single error. Used in ECC RAM.

### Reed-Solomon codes
Work over finite fields. Can correct burst errors. Used in CDs, DVDs, QR codes, deep space communication (Voyager probes).

### LDPC and Turbo codes
Near-Shannon-limit codes. LDPC is used in WiFi (802.11n), 5G, solid-state drives. Turbo codes were used in 3G/4G.

### Polar codes
Provably achieve Shannon capacity. Used in 5G control channels.

---

## 9. Data Compression — Applied Information Theory

### Lossless compression (perfect reconstruction)
- **Huffman coding:** Variable-length prefix codes
- **Arithmetic coding:** Encode entire message as a single fraction. Optimal.
- **LZ77/LZ78/LZW:** Dictionary-based. Find repeated patterns. Powers gzip, ZIP, PNG.
- **Asymmetric Numeral Systems (ANS):** Modern alternative to arithmetic coding. Used in zstd, LZFSE.

### Lossy compression (some information lost)
- **JPEG:** DCT (discrete cosine transform) + quantization + Huffman
- **MP3:** Psychoacoustic model + MDCT + Huffman (removes sounds humans can't hear)
- **H.264/H.265:** Motion estimation + transform coding (video)
- **Neural compression:** Learned autoencoders that optimize rate-distortion

The rate-distortion theorem says: for a given allowed distortion, there's a minimum bitrate. You can trade quality for size, but there's a fundamental curve.

---

## What Information Theory Powers

| Technology | Information-theoretic concept |
|------------|------------------------------|
| **ZIP, gzip** | Entropy coding (Huffman, LZ) |
| **JPEG, MP3** | Rate-distortion theory, lossy coding |
| **WiFi, 5G** | Channel capacity, LDPC/Polar codes |
| **QR codes** | Reed-Solomon error correction |
| **Deep learning** | Cross-entropy loss, KL divergence |
| **Language models** | Perplexity, entropy of natural language |
| **Cryptography** | Entropy as randomness measure, one-time pad = H bits of key |
| **Streaming (Netflix)** | Adaptive bitrate = rate-distortion optimization |
| **Satellite communication** | Shannon limit, convolutional/turbo codes |
| **DNA storage** | Information density: 2 bits per base pair |

---

## Key Insight

Information theory reveals that information is a physical, measurable quantity — as real as mass or energy. It has conservation laws (you can't create information from nothing), fundamental limits (you can't compress below entropy, you can't communicate above channel capacity), and a precise currency (the bit). Shannon showed that the entire landscape of communication, compression, and now machine learning can be understood through one concept: the probabilistic structure of the source.

---

**Next:** [Fourier Analysis](fourier-analysis.md) — decomposing signals into their frequency components.
