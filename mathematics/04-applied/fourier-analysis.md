# Fourier Analysis — Decomposing Signals into Frequencies

> Any signal — sound, image, electromagnetic wave — can be broken into
> a sum of simple sine waves at different frequencies. This idea powers
> audio, image processing, telecommunications, and quantum mechanics.

**Prerequisites:** [Calculus](../02-core-structures/calculus.md) (integration), [Linear Algebra](../02-core-structures/linear-algebra.md) (orthogonal bases), [Number Systems](../02-core-structures/number-systems.md) (complex numbers)
**What it enables:** Audio processing (MP3), image compression (JPEG), telecommunications (OFDM/5G), medical imaging (MRI, CT), signal filtering, spectral analysis, solving PDEs.

---

## The Central Idea

**Fourier's claim (1807):** Any periodic function can be written as a sum of sines and cosines.

```
f(t) = a₀/2 + Σ [aₙcos(nωt) + bₙsin(nωt)]
```

This was controversial — how can sharp, jagged functions be built from smooth waves? But it's true (with precise conditions), and it's one of the most useful ideas in all of science.

**Analogy:** A musical chord is several notes played simultaneously. Fourier analysis decomposes the chord back into individual notes (frequencies). Every signal is a "chord" of frequencies.

---

## 1. Fourier Series — Periodic Functions

For a function f(t) with period T and fundamental frequency ω = 2π/T:

```
f(t) = a₀/2 + Σ [aₙcos(nωt) + bₙsin(nωt)]
```

The coefficients (how much of each frequency is present):
```
aₙ = (2/T) ∫₀ᵀ f(t)cos(nωt) dt
bₙ = (2/T) ∫₀ᵀ f(t)sin(nωt) dt
```

### Complex form (more elegant)
Using Euler's formula e^(iθ) = cos(θ) + i·sin(θ):

```
f(t) = Σ cₙ · e^(inωt)

cₙ = (1/T) ∫₀ᵀ f(t) · e^(-inωt) dt
```

Each cₙ gives the amplitude and phase of frequency nω.

### Examples

**Square wave** (alternating +1 and -1):
```
f(t) = (4/π)[sin(ωt) + sin(3ωt)/3 + sin(5ωt)/5 + ...]
```
Only odd harmonics. The jagged square is built from smooth sines — add more terms, get closer to the square shape.

**Sawtooth wave:**
```
f(t) = (2/π)[sin(ωt) - sin(2ωt)/2 + sin(3ωt)/3 - ...]
```
All harmonics with decreasing amplitude.

### Why sines and cosines?

They're the **eigenfunctions of differentiation** — differentiating a sine gives a cosine (same frequency). This means linear differential equations become algebraic in the Fourier basis. A hard calculus problem becomes easy algebra.

They also form an **orthogonal basis** for function space:
```
∫₀ᵀ sin(mωt)sin(nωt) dt = 0    when m ≠ n
```

Decomposing into Fourier components is a change of basis in an infinite-dimensional vector space — the same operation as changing coordinates in linear algebra.

---

## 2. Fourier Transform — Non-Periodic Functions

For functions that aren't periodic, the discrete frequencies become a continuous spectrum:

```
F(ω) = ∫_{-∞}^{∞} f(t) · e^(-iωt) dt        (transform)

f(t) = (1/2π) ∫_{-∞}^{∞} F(ω) · e^(iωt) dω  (inverse transform)
```

F(ω) is the **frequency spectrum** — it tells you how much of each frequency is present in f.

### Key properties

| Property | Time domain | Frequency domain |
|----------|------------|-----------------|
| **Linearity** | af + bg | aF + bG |
| **Time shift** | f(t - t₀) | F(ω) · e^(-iωt₀) |
| **Frequency shift** | f(t) · e^(iω₀t) | F(ω - ω₀) |
| **Convolution** | f * g | F · G (multiplication!) |
| **Differentiation** | f'(t) | iωF(ω) |
| **Scaling** | f(at) | (1/\|a\|)F(ω/a) |

**The convolution theorem** is the killer app: convolution in time = multiplication in frequency. Filtering a signal (convolution with a kernel) becomes pointwise multiplication in the frequency domain — much faster.

### The Uncertainty Principle

A signal cannot be simultaneously localized in BOTH time and frequency:

```
Δt · Δω ≥ 1/2
```

Sharp in time → spread in frequency. Sharp in frequency → spread in time.

This is Heisenberg's uncertainty principle in quantum mechanics (position ↔ time, momentum ↔ frequency). It's also why a short beep has a wide frequency spread, and a pure tone (narrow frequency) must ring for a long time.

---

## 3. Discrete Fourier Transform (DFT)

Computers can't compute integrals over continuous functions. The DFT works with N discrete samples:

```
F[k] = Σ_{n=0}^{N-1} f[n] · e^(-2πikn/N)     k = 0, 1, ..., N-1
```

This transforms N time samples into N frequency components.

### The Fast Fourier Transform (FFT)

**Cooley-Tukey (1965):** Compute the DFT in O(N log N) instead of O(N²).

The idea: split the DFT of size N into two DFTs of size N/2 (divide and conquer). This recursion turns an impractical computation into a fast one.

**Impact:** The FFT is one of the most important algorithms ever invented. It made digital signal processing practical.

```
N = 1,000,000
Naive DFT: 10¹² operations
FFT: 20,000,000 operations (50,000× faster)
```

---

## 4. Applications

### Audio processing
- **Equalizer:** Fourier transform → boost/cut specific frequencies → inverse transform
- **MP3 compression:** Modified DCT → quantize small frequency components → encode
- **Noise removal:** Identify noise frequencies, zero them out, inverse transform
- **Pitch detection:** Find the dominant frequency in the spectrum
- **Auto-tune:** Shift frequencies to the nearest correct pitch

### Image processing
- **JPEG compression:** 2D Discrete Cosine Transform (DCT) on 8×8 pixel blocks → quantize → encode. High-frequency components (fine detail) are discarded first.
- **Image filtering:** Blur = low-pass filter. Sharpen = high-pass filter. Edge detection = band-pass filter. All done via frequency domain multiplication.

### Telecommunications
- **OFDM (WiFi, 4G/5G):** Split data across many frequencies simultaneously. Each subcarrier is a Fourier component. The FFT is computed in hardware millions of times per second.
- **AM/FM radio:** Modulate signal by shifting it to a carrier frequency (frequency shifting property).

### Medical imaging
- **MRI:** Measures frequency response of hydrogen atoms in a magnetic field. The image IS a Fourier transform — inverse FFT reconstructs the anatomical image.
- **CT scan:** Fourier slice theorem connects X-ray projections to 2D frequency domain.

### Solving PDEs
The heat equation ∂u/∂t = α∂²u/∂x² in Fourier space becomes:
```
dû/dt = -αk²û    →    û(k,t) = û(k,0)·e^(-αk²t)
```

Each frequency component decays independently. High frequencies (fine detail) decay fastest — this is why the heat equation smooths things out.

---

## 5. Wavelets — Localized Frequency Analysis

Fourier analysis tells you WHICH frequencies are present but not WHEN they occur (the uncertainty principle). Wavelets fix this by using localized basis functions.

A **wavelet** is a short oscillation that dies off:
```
ψ_{a,b}(t) = (1/√a) · ψ((t-b)/a)
```
- a: scale (analogous to frequency)
- b: position (when in time)

**Wavelet transform:** Decompose a signal into time-localized frequency components.

Applications:
- **JPEG 2000:** Uses wavelets instead of DCT — better compression at low bitrates
- **Denoising:** Wavelet thresholding removes noise while preserving edges
- **Seismology:** Analyze earthquake signals (transient events)
- **Financial time series:** Detect multi-scale patterns

---

## 6. Laplace and Z Transforms

### Laplace transform (continuous systems)
```
F(s) = ∫₀^∞ f(t)e^{-st} dt     (s ∈ C)
```

Generalizes the Fourier transform to complex frequencies. Turns differential equations into algebraic equations.

```
ODE: y'' + 3y' + 2y = sin(t)
Laplace: (s² + 3s + 2)Y(s) = 1/(s²+1) + initial conditions
Solve for Y(s), then inverse Laplace to get y(t).
```

Used in: control theory (transfer functions), circuit analysis, signal processing.

### Z-transform (discrete systems)
```
F(z) = Σ f[n]z^{-n}
```

The discrete equivalent of the Laplace transform. Turns difference equations into algebraic equations. Foundation of digital filter design.

---

## What Fourier Analysis Powers

| Technology | How Fourier analysis is used |
|------------|------------------------------|
| **MP3/AAC audio** | Modified DCT for compression |
| **JPEG images** | DCT on pixel blocks |
| **WiFi/5G** | OFDM: data modulated on Fourier frequencies |
| **MRI** | Image IS the inverse Fourier transform |
| **Speech recognition** | Spectrograms (short-time Fourier transform) |
| **Shazam** | Audio fingerprinting via spectrogram peaks |
| **Noise-cancelling headphones** | Frequency-domain identification of noise |
| **Solving PDEs** | Spectral methods, Fourier decomposition |
| **Optics** | Diffraction IS Fourier transform of aperture |
| **Quantum mechanics** | Position/momentum are Fourier transform pairs |

---

## Key Insight

Fourier analysis is a change of perspective: instead of asking "what's the value at each moment in time?" you ask "how much of each frequency is present?" Many problems that are complicated in one perspective become simple in the other. Convolution becomes multiplication. Differential equations become algebraic. And the FFT — the algorithm that makes this perspective shift computationally cheap — is one of the most consequential algorithms ever created, silently running billions of times per second across the world's hardware.

---

**Next:** [Cryptography](cryptography.md) — securing information with mathematics.
