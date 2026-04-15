# Cryptography — Securing Information with Mathematics

> Cryptography turns mathematical hardness into security guarantees.
> The fact that some problems are easy to create but hard to reverse
> is what makes secure communication, digital money, and privacy possible.

**Prerequisites:** [Number Theory](../03-branches/number-theory.md) (modular arithmetic, primes), [Algebra](../02-core-structures/algebra.md) (groups, fields), [Probability](../03-branches/probability.md)
**What it enables:** HTTPS, digital signatures, cryptocurrency, secure messaging, password storage, zero-knowledge proofs.

---

## The Core Idea

Cryptography exploits **asymmetry** — operations that are easy in one direction and hard in reverse:

| Easy direction | Hard direction |
|---------------|---------------|
| Multiply two primes | Factor the product |
| Compute g^x mod p | Find x given g^x mod p (discrete log) |
| Compute hash H(m) | Find m given H(m) (preimage) |
| Multiply points on elliptic curve | Reverse the multiplication (ECDLP) |

If you can do the hard direction, you break the system. Modern cryptography assumes these hard problems remain hard (no efficient algorithms are known).

---

## 1. Symmetric Encryption — Shared Secrets

Both parties share the same secret key.

### AES (Advanced Encryption Standard)
The most widely used cipher. Operates on 128-bit blocks, with 128/192/256-bit keys.

Each round applies:
1. **SubBytes:** Substitute each byte via an S-box (nonlinear, defined over GF(2⁸))
2. **ShiftRows:** Rotate rows of the state matrix
3. **MixColumns:** Matrix multiplication over GF(2⁸)
4. **AddRoundKey:** XOR with round key

10-14 rounds of these operations. The math is finite field arithmetic — every operation is invertible, enabling decryption.

**Where it's used:** Disk encryption (FileVault, BitLocker), TLS (after key exchange), VPNs, database encryption. AES hardware instructions are built into modern CPUs.

### The key exchange problem
Symmetric crypto needs both parties to have the same key. But how do you share a key securely if you don't already have a secure channel? This is solved by asymmetric crypto.

---

## 2. Asymmetric Encryption — Public/Private Keys

### RSA (Rivest-Shamir-Adleman, 1977)

**Key generation:**
1. Choose large primes p, q (~1024 bits each)
2. Compute n = pq
3. Compute φ(n) = (p-1)(q-1)
4. Choose e coprime to φ(n) (typically 65537)
5. Compute d = e⁻¹ mod φ(n) (Extended Euclidean Algorithm)

**Public key:** (n, e). **Private key:** d.

**Encrypt:** c = mᵉ mod n
**Decrypt:** m = cᵈ mod n

**Why it works:** Euler's theorem guarantees m^(ed) ≡ m (mod n).
**Why it's secure:** Computing d requires φ(n), which requires factoring n into p and q. Factoring is hard.

**Key sizes:** 2048-bit minimum today. 4096-bit recommended for long-term security.

### Diffie-Hellman Key Exchange (1976)

Two parties agree on a shared secret over a public channel without ever transmitting it:

```
Public parameters: prime p, generator g

Alice: picks secret a, sends A = g^a mod p
Bob:   picks secret b, sends B = g^b mod p

Shared secret:
  Alice computes: B^a = g^(ab) mod p
  Bob computes:   A^b = g^(ab) mod p
```

An eavesdropper sees g^a and g^b but can't compute g^(ab) (discrete logarithm problem).

### Elliptic Curve Cryptography (ECC)

Elliptic curves: y² = x³ + ax + b over a finite field.

Points on the curve form a group under geometric "addition." The ECDLP: given points P and Q = kP, find k.

**Advantage:** Same security with much smaller keys.
- RSA 3072-bit ≈ ECC 256-bit
- Faster computation, less bandwidth

**Used in:** TLS 1.3, Bitcoin (secp256k1), Signal/WhatsApp (Curve25519), SSH (Ed25519).

---

## 3. Hash Functions — One-Way Fingerprints

A **cryptographic hash function** H maps arbitrary input to a fixed-size output:

```
H("hello") = 2cf24dba5fb0a30e...  (256 bits for SHA-256)
H("hello.") = b22b1d9...          (completely different — avalanche effect)
```

**Properties:**
1. **Preimage resistance:** Given H(m), can't find m
2. **Second preimage resistance:** Given m₁, can't find m₂ ≠ m₁ with H(m₁) = H(m₂)
3. **Collision resistance:** Can't find ANY m₁ ≠ m₂ with H(m₁) = H(m₂)

**Common hashes:**
- SHA-256 (Bitcoin, TLS, general purpose)
- SHA-3 / Keccak (different design, backup standard)
- BLAKE3 (very fast, modern)

**Applications:**
- **Password storage:** Store H(password + salt), not the password
- **Data integrity:** File checksums, git commit hashes
- **Blockchain:** Block hash chains, proof-of-work
- **Digital signatures:** Sign H(message), not the full message

---

## 4. Digital Signatures — Mathematical Proof of Authorship

**Sign:** Use private key to create a signature of a message.
**Verify:** Anyone with the public key can check the signature is valid.

Properties:
- Only the private key holder can create valid signatures
- Anyone can verify
- The signature is bound to the specific message (can't be transferred)

### RSA signatures
```
Sign:   σ = H(m)^d mod n
Verify: check that σ^e mod n = H(m)
```

### ECDSA (Bitcoin, TLS)
Uses elliptic curve math. Signature is a pair (r, s) derived from the message hash and private key.

### Ed25519 (modern standard)
Edwards curve, deterministic signatures, fast. Used in SSH, Signal, Tor.

---

## 5. Error-Correcting Codes — Reliable Transmission

Technically coding theory, not cryptography, but deeply related mathematically.

**Problem:** Data gets corrupted during transmission or storage. How do we detect and correct errors?

### Hamming codes
Add parity bits at power-of-2 positions. Hamming(7,4): encode 4 data bits as 7 bits, correct any single-bit error.

### Reed-Solomon codes
Work over finite fields GF(2^m). Can correct burst errors (multiple consecutive corrupted symbols).

**Used in:** CDs, DVDs, Blu-ray, QR codes, deep space communication (Voyager, Mars rovers), RAID storage.

### LDPC codes
Sparse parity-check matrices. Near-Shannon-limit performance. Used in WiFi (802.11n/ac/ax), 5G, SSDs.

---

## 6. Zero-Knowledge Proofs

Prove you know something WITHOUT revealing it.

**Example (the "Ali Baba cave"):**
A cave has two paths that meet at a locked door. You want to prove you know the password without revealing it. The verifier waits outside while you enter. They call a random path for you to exit from. If you know the password, you always exit correctly. If you don't, you fail 50% of the time. Repeat k times → probability of faking drops to 2⁻ᵏ.

**ZK-SNARKs (Zero-Knowledge Succinct Non-Interactive Arguments of Knowledge):**
Prove a computation was done correctly without revealing the inputs. Used in:
- Privacy-preserving cryptocurrencies (Zcash)
- Scaling blockchains (zk-rollups on Ethereum)
- Identity verification without revealing personal data

The math behind ZK proofs involves elliptic curve pairings, polynomial commitments, and algebraic geometry.

---

## 7. Post-Quantum Cryptography

Quantum computers running **Shor's algorithm** would break:
- RSA (factoring becomes easy)
- Diffie-Hellman (discrete log becomes easy)
- ECC (ECDLP becomes easy)

**Quantum-resistant alternatives:**
| Approach | Hard problem | Status |
|----------|-------------|--------|
| **Lattice-based** (CRYSTALS-Kyber/Dilithium) | Shortest vector in a lattice | NIST standard (ML-KEM, ML-DSA) |
| **Hash-based** (SPHINCS+) | Hash function security | NIST standard (SLH-DSA) |
| **Code-based** (McEliece) | Decoding random linear codes | Candidate |
| **Isogeny-based** (SIKE) | Isogeny between elliptic curves | Broken in 2022! |

The transition to post-quantum crypto is underway. TLS is already deploying hybrid modes.

---

## What Cryptography Powers

| Technology | Cryptographic primitive |
|------------|------------------------|
| **HTTPS** | TLS: ECDHE key exchange + AES encryption + RSA/ECDSA certificates |
| **Bitcoin** | ECDSA signatures + SHA-256 hashing + proof-of-work |
| **Signal/WhatsApp** | X25519 key agreement + AES-256 + Ed25519 signatures |
| **SSH** | Ed25519 or RSA authentication + AES encryption |
| **Password storage** | bcrypt/scrypt/Argon2 (hash-based) |
| **Git** | SHA-1/SHA-256 for commit/tree/blob identification |
| **Digital signatures** | PDF signing, code signing, email (PGP/S-MIME) |
| **VPN** | IKEv2/WireGuard: DH key exchange + ChaCha20/AES |
| **Disk encryption** | AES-XTS (FileVault, BitLocker, LUKS) |

---

## Key Insight

Cryptography is applied number theory, algebra, and probability. Its security rests not on secrecy of the algorithm (Kerckhoffs' principle: the algorithm should be public) but on the mathematical hardness of specific problems. The entire trust infrastructure of the internet — that your bank connection is private, that software updates are authentic, that your messages are confidential — is a tower of mathematical proofs standing on the assumption that certain problems have no efficient solution.

---

**Next:** [ML Math](ml-math.md) — the mathematical convergence point that powers artificial intelligence.
