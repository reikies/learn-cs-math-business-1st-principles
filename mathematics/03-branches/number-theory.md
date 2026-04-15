# Number Theory — The Queen of Mathematics

> Number theory studies the properties of integers — especially primes.
> For 2,000 years it was pure math with "no applications."
> Then the internet needed encryption, and number theory became essential infrastructure.

**Prerequisites:** [Number Systems](../02-core-structures/number-systems.md) (integers, modular arithmetic), [Proof Methods](../01-foundations/proof-methods.md)
**What it enables:** Cryptography (RSA, Diffie-Hellman, elliptic curves), hash functions, error-correcting codes, random number generation.

---

## 1. Divisibility and Primes

**Divisibility:** a divides b (written a | b) if b = ka for some integer k.
```
3 | 12   (12 = 4 × 3)
5 ∤ 12   (12/5 is not an integer)
```

**Prime:** An integer p > 1 whose only divisors are 1 and itself.
```
Primes: 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, ...
```

### The Fundamental Theorem of Arithmetic
Every integer n > 1 has a UNIQUE prime factorization (up to order).
```
360 = 2³ × 3² × 5
1001 = 7 × 11 × 13
```

This is why primes are the "atoms" of integers. Every integer is a unique product of primes, just as every molecule is a unique arrangement of atoms.

### How many primes are there?
**Infinitely many.** (Euclid, ~300 BC)

Proof by contradiction: If p₁, p₂, ..., pₙ were ALL the primes, consider N = p₁p₂...pₙ + 1. N is not divisible by any pᵢ (remainder 1). So either N is prime, or N has a prime factor not in our list. Either way, contradiction. □

### The Prime Number Theorem
The density of primes near N is approximately 1/ln(N).

```
Near 1,000: about 1 in 7 numbers is prime
Near 1,000,000: about 1 in 14
Near 10¹⁰⁰: about 1 in 230
```

Primes thin out, but never vanish. There are always more ahead.

---

## 2. The Division Algorithm and GCD

**Division algorithm:** For any a, b > 0, there exist unique q (quotient) and r (remainder) such that:
```
a = bq + r,    0 ≤ r < b
```

**GCD (Greatest Common Divisor):** The largest number dividing both a and b.
```
gcd(12, 18) = 6
gcd(17, 31) = 1    (coprime)
```

### The Euclidean Algorithm
The fastest way to compute GCD — over 2,300 years old and still used in every cryptographic library:
```
gcd(252, 105):
  252 = 2 × 105 + 42
  105 = 2 × 42  + 21
   42 = 2 × 21  + 0    ← stop
gcd = 21
```

Replace larger with remainder, repeat. Runs in O(log(min(a,b))) steps.

### Bézout's Identity
For any a, b, there exist integers x, y such that:
```
ax + by = gcd(a, b)
```

Found by running the Euclidean algorithm backwards (Extended Euclidean Algorithm). This is how modular inverses are computed — the core of RSA.

---

## 3. Modular Arithmetic — Deep Dive

In Z/nZ, we work with remainders after dividing by n:
```
17 ≡ 2 (mod 5)     (17 and 2 have the same remainder when divided by 5)
```

Arithmetic works as expected:
```
(a + b) mod n = ((a mod n) + (b mod n)) mod n
(a × b) mod n = ((a mod n) × (b mod n)) mod n
```

### Modular inverses
a⁻¹ mod n exists iff gcd(a, n) = 1.

When n is prime, every non-zero element has an inverse — Z/pZ is a field.

```
3⁻¹ mod 7 = 5    (because 3 × 5 = 15 ≡ 1 mod 7)
```

### Fermat's Little Theorem
If p is prime and p ∤ a:
```
aᵖ⁻¹ ≡ 1 (mod p)
```

Equivalently: aᵖ ≡ a (mod p) for all a.

**Application:** Fast computation of modular inverses: a⁻¹ ≡ aᵖ⁻² (mod p).

### Euler's Theorem (generalization)
For any n, if gcd(a, n) = 1:
```
a^φ(n) ≡ 1 (mod n)
```

where **φ(n)** (Euler's totient) = count of integers from 1 to n coprime to n.

```
φ(10) = 4    (the numbers 1, 3, 7, 9 are coprime to 10)
φ(p) = p - 1 (for prime p)
φ(pq) = (p-1)(q-1) (for distinct primes p, q)
```

**This is the mathematical heart of RSA encryption.** The formula φ(pq) = (p-1)(q-1) is why RSA uses products of two primes.

---

## 4. The Chinese Remainder Theorem (CRT)

If gcd(m, n) = 1, then the system:
```
x ≡ a (mod m)
x ≡ b (mod n)
```
has a unique solution modulo mn.

**Example:**
```
x ≡ 2 (mod 3)
x ≡ 3 (mod 5)
Solution: x ≡ 8 (mod 15)
```

CRT lets you break a computation modulo a large number into independent computations modulo smaller numbers, then combine. Used in:
- RSA decryption speedup (factor of 4 faster)
- Secret sharing schemes
- Parallel computation

---

## 5. Primality Testing and Factoring

### Primality testing: Is n prime?

**Trial division:** Test all divisors up to √n. Works for small n. O(√n).

**Miller-Rabin test:** Probabilistic. Pick random witnesses. If n passes k rounds, it's prime with probability > 1 - 4⁻ᵏ. Used in practice.

**AKS test (2002):** Deterministic polynomial-time. Proved PRIMES ∈ P. Theoretical breakthrough, but Miller-Rabin is faster in practice.

### Factoring: Given n, find its prime factors.

**This is the hard problem that secures RSA.**

| Algorithm | Complexity | Notes |
|-----------|-----------|-------|
| Trial division | O(√n) | Only for small n |
| Pollard's rho | O(n^(1/4)) | Good for small factors |
| Quadratic sieve | sub-exponential | Works up to ~100 digits |
| Number field sieve | sub-exponential | Best known for large numbers |

The number field sieve is the fastest known classical algorithm, but it's still sub-exponential — way too slow for 2048-bit RSA keys. However, **Shor's algorithm** on a quantum computer would factor in polynomial time, breaking RSA. This is why post-quantum cryptography is being developed.

---

## 6. Quadratic Residues and Reciprocity

**Quadratic residue:** a is a QR mod p if x² ≡ a (mod p) has a solution.

```
Mod 7: 1²≡1, 2²≡4, 3²≡2. So QRs mod 7 are {1, 2, 4}.
```

**Legendre symbol (a/p):**
- = 1 if a is a QR mod p
- = -1 if not
- = 0 if p | a

**Quadratic Reciprocity (Gauss, "the golden theorem"):**
For odd primes p ≠ q:
```
(p/q)(q/p) = (-1)^((p-1)/2 · (q-1)/2)
```

This lets you flip the "is a a square mod p?" question — enormously useful for computation. Gauss gave six different proofs. It connects to deep structures in algebraic number theory.

---

## 7. Cryptographic Applications

### RSA
```
Key generation:
1. Choose large primes p, q
2. Compute n = pq
3. Compute φ(n) = (p-1)(q-1)
4. Choose e coprime to φ(n) (usually 65537)
5. Compute d = e⁻¹ mod φ(n) (using Extended Euclidean Algorithm)
Public key: (n, e). Private key: d.

Encryption: c = mᵉ mod n
Decryption: m = cᵈ mod n

Why it works: By Euler's theorem, m^(ed) ≡ m^(1 + kφ(n)) ≡ m (mod n)
```

Security rests on: factoring n into p and q is hard. If you can factor, you can compute φ(n) and find d.

### Diffie-Hellman Key Exchange
```
Public: prime p, generator g
Alice picks secret a, sends g^a mod p
Bob picks secret b, sends g^b mod p
Shared secret: g^(ab) mod p  (both can compute this)
```

Security rests on the **discrete logarithm problem:** given g, g^a mod p, finding a is hard.

### Elliptic Curve Cryptography (ECC)
Uses the group structure of points on elliptic curves (y² = x³ + ax + b) over finite fields. Same discrete log idea but with much smaller keys for equivalent security:
- RSA 3072-bit ≈ ECC 256-bit

Used in TLS, Bitcoin (ECDSA), Signal protocol.

---

## What Number Theory Powers

| Technology | Number theory used |
|------------|-------------------|
| **HTTPS/TLS** | RSA or ECDSA certificates, Diffie-Hellman key exchange |
| **Bitcoin** | ECDSA signatures, SHA-256 (uses modular arithmetic) |
| **SSH** | RSA or Ed25519 (elliptic curve) authentication |
| **Signal/WhatsApp** | X25519 (elliptic curve Diffie-Hellman) |
| **Random number generators** | Linear congruential: x_{n+1} = (ax_n + c) mod m |
| **Hash tables** | Hash functions often use modular arithmetic |
| **Error-correcting codes** | Reed-Solomon codes use finite field arithmetic |

---

## Key Insight

Number theory is the perfect example of mathematics that seemed "useless" for millennia then became indispensable. Hardy famously bragged that number theory had no applications. Decades later, RSA encryption — built entirely on prime factorization and modular arithmetic — secures trillions of dollars in daily commerce. The lesson: mathematical truth doesn't expire, and you can never predict which "useless" theory will become critical infrastructure.

---

**Next:** [Differential Equations](differential-equations.md) — the mathematics of how things change over time.
