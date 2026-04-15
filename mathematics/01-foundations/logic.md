# Logic — The Rules of Valid Reasoning

> Before we can prove anything, we need to agree on what counts as a valid argument.
> Logic is that agreement. It is the operating system on which all of mathematics runs.

**Prerequisites:** None. This is where it all begins.
**What it enables:** Set theory, proof methods, every branch of mathematics, Boolean algebra, programming, circuits.

---

## Why Logic Exists

Humans argue. We make claims, draw conclusions, and disagree. But sometimes an argument *feels* right and is wrong, and sometimes an argument *feels* wrong and is right. Logic exists to settle this — not by intuition, but by structure.

The core question: **Given some statements we accept as true, what else MUST be true?**

---

## 1. Propositions — The Atoms

A **proposition** is a statement that is either true or false. Not both, not neither.

```
"2 + 2 = 4"          → TRUE (proposition)
"The sky is green"    → FALSE (proposition)
"Close the door"      → NOT a proposition (it's a command)
"Is it raining?"      → NOT a proposition (it's a question)
"x > 5"              → NOT a proposition yet (depends on x — this is a predicate, covered below)
```

Every logical system is built by combining propositions using **connectives**.

---

## 2. The Five Connectives — How We Combine Truths

### NOT (¬) — Negation
Flips true to false, false to true.

| p | ¬p |
|---|-----|
| T | F |
| F | T |

"It is NOT raining" is the negation of "It is raining."

### AND (∧) — Conjunction
True only when BOTH parts are true.

| p | q | p ∧ q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

"It is raining AND I have an umbrella" — both must hold.

### OR (∨) — Disjunction
True when AT LEAST ONE part is true. (This is *inclusive* or — both can be true.)

| p | q | p ∨ q |
|---|---|-------|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

### IMPLICATION (→) — If...then
"If p then q." This is the most important and most misunderstood connective.

| p | q | p → q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | T |
| F | F | T |

The crucial insight: **a false premise implies anything.** "If pigs fly, then I'm the president" is technically true — because pigs don't fly, the promise is never tested.

The only way an implication is FALSE is when the premise is true but the conclusion is false. You made a promise and broke it.

**Why this matters everywhere:**
- In programming: `if (condition) { action }` — when condition is false, nothing happens (the statement doesn't "fail")
- In mathematics: "If n is even, then n² is even" — we only need to check the cases where n IS even
- In circuits: An implication gate outputs 0 only when input is 1 and output is 0

### BICONDITIONAL (↔) — If and only if
True when both sides have the SAME truth value.

| p | q | p ↔ q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

"You pass if and only if you score above 60" — scoring above 60 guarantees passing, and passing guarantees you scored above 60.

---

## 3. Tautologies, Contradictions, and Logical Equivalence

A **tautology** is a statement that is ALWAYS true, regardless of the truth values of its parts.

```
p ∨ ¬p          → always true  (law of excluded middle)
p → p           → always true
¬(p ∧ ¬p)      → always true  (law of non-contradiction)
```

A **contradiction** is ALWAYS false: `p ∧ ¬p`

Two statements are **logically equivalent** (≡) when they have identical truth tables:
```
¬(p ∧ q) ≡ ¬p ∨ ¬q     (De Morgan's Law)
¬(p ∨ q) ≡ ¬p ∧ ¬q     (De Morgan's Law)
p → q    ≡ ¬p ∨ q       (implication as disjunction)
p → q    ≡ ¬q → ¬p      (contrapositive)
```

**De Morgan's Laws are everywhere:**
- In programming: `!(a && b)` is `!a || !b`
- In circuits: NAND and NOR gate equivalences
- In databases: query optimization rewrites

---

## 4. Predicates and Quantifiers — Scaling Up

Propositions are limited — they're fixed statements. What about "x is even"? That depends on x. This is a **predicate**: a proposition with variables.

```
P(x) = "x is even"
P(4) = TRUE
P(7) = FALSE
```

To make universal claims about predicates, we need **quantifiers**:

### Universal Quantifier (∀) — "For all"
```
∀x ∈ N: x + 0 = x
"For every natural number x, adding zero gives x"
```

To PROVE a ∀ statement: show it works for an arbitrary element.
To DISPROVE a ∀ statement: find ONE counterexample.

### Existential Quantifier (∃) — "There exists"
```
∃x ∈ Z: x + 5 = 0
"There exists an integer x such that x + 5 = 0"  (yes: x = -5)
```

To PROVE a ∃ statement: exhibit ONE example.
To DISPROVE a ∃ statement: show NO element works (which means proving ∀ of the negation).

### Negating Quantifiers
This is critical and catches people constantly:
```
¬(∀x: P(x))  ≡  ∃x: ¬P(x)
"NOT everything is P"  =  "something is NOT P"

¬(∃x: P(x))  ≡  ∀x: ¬P(x)
"NOTHING is P"  =  "everything is NOT P"
```

**In programming terms:**
- `∀x: P(x)` is like `array.every(x => P(x))`
- `∃x: P(x)` is like `array.some(x => P(x))`
- Negating "every" gives "some not"; negating "some" gives "every not"

---

## 5. Rules of Inference — Valid Argument Forms

These are the "legal moves" in logical reasoning:

### Modus Ponens (the most fundamental rule)
```
If p then q.
p is true.
Therefore, q is true.

p → q
p
∴ q
```
Example: "If it rains, the ground is wet. It's raining. Therefore the ground is wet."

### Modus Tollens (the contrapositive in action)
```
If p then q.
q is false.
Therefore, p is false.

p → q
¬q
∴ ¬p
```
Example: "If it rained, the ground would be wet. The ground is dry. Therefore it didn't rain."

### Hypothetical Syllogism (chaining implications)
```
p → q
q → r
∴ p → r
```
This is how we build long chains of reasoning — and it's how compilers chain type inferences.

### Disjunctive Syllogism (process of elimination)
```
p ∨ q
¬p
∴ q
```
"It's either A or B. It's not A. So it must be B."

---

## 6. First-Order Logic — The Full System

Combine everything:
- Propositions and predicates
- Connectives (¬, ∧, ∨, →, ↔)
- Quantifiers (∀, ∃)
- Rules of inference

This gives you **first-order logic** (FOL) — powerful enough to express virtually all of mathematics.

Example of a FOL statement:
```
∀ε > 0, ∃δ > 0: (|x - a| < δ) → (|f(x) - f(a)| < ε)
```
This is the epsilon-delta definition of continuity. All of calculus rests on this one logical sentence.

---

## 7. What Logic Powers in the Real World

| Technology | How logic is used |
|------------|-------------------|
| **CPUs** | Every circuit is built from AND, OR, NOT gates — direct implementation of propositional logic |
| **Programming** | if/else, while loops, boolean expressions are all propositional logic |
| **Databases (SQL)** | WHERE clauses are logical predicates with quantifiers |
| **Type systems** | Type checking is logical inference (Curry-Howard correspondence: proofs = programs) |
| **AI / Formal verification** | Theorem provers use inference rules to verify software correctness |
| **Search engines** | Boolean search queries use AND, OR, NOT |

---

## Key Insight

Logic seems "obvious" — and that's the point. It formalizes what obvious means. Every time you write an `if` statement, compose a SQL query, or design a circuit, you're using logic. It's invisible because it's everywhere, like the air mathematics breathes.

---

**Next:** [Set Theory](set-theory.md) — building objects from nothing using logic as the foundation.
