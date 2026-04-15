# Logic Gates & Boolean Algebra — How Switches Become Math

> A transistor is just a switch. But connect two switches the right way and they can
> *decide* something. Connect a few billion of them and they can think — or at least
> do a convincing impression. This is the layer where physics stops and logic begins.

**Prerequisites:** [Physics & Electricity](physics-and-electricity.md) — You need to know what a transistor is.
**What it enables:** Computer architecture, CPU design, all digital circuits.

---

## 1. From Transistors to Gates

A transistor is a voltage-controlled switch. There are two flavors:

- **NMOS** — conducts (switch ON) when gate voltage is HIGH (1). Connects output to ground.
- **PMOS** — conducts (switch ON) when gate voltage is LOW (0). Connects output to power (Vdd).

We combine them into **CMOS** (Complementary MOS) circuits, where PMOS pulls output
HIGH and NMOS pulls it LOW. The output is always driven to a definite value, and no
current flows in the steady state — which is why your phone battery lasts.

### The NOT Gate (Inverter) — The Simplest Gate

```
        Vdd (power)
         |
      ┌──┴──┐
      │ PMOS │  ← ON when A=0
      └──┬──┘
  A ─────┤
      ┌──┴──┐
      │ NMOS │  ← ON when A=1
      └──┬──┘
         |
        GND

  When A=1: NMOS conducts → output pulled to GND → output is 0.
  When A=0: PMOS conducts → output pulled to Vdd → output is 1.
```

Input goes in, opposite comes out. That is negation in silicon.

### The NAND Gate — Two Transistors in Series

```
           Vdd            Vdd
            |              |
         ┌──┴──┐       ┌──┴──┐
         │PMOS │       │PMOS │
         └──┬──┘       └──┬──┘
   A ───────┤     B ──────┤
            └──────┬───────┘
                   ├──────── Output (Y)
            ┌──────┴──────┐
            │    NMOS     │
   A ───────┤             │
            └──────┬──────┘
            ┌──────┴──────┐
            │    NMOS     │
   B ───────┤             │
            └──────┬──────┘
                   |
                  GND

  PMOS transistors are in PARALLEL — either one can pull output HIGH.
  NMOS transistors are in SERIES — BOTH must be on to pull output LOW.

  Result: output is LOW only when A=1 AND B=1. Otherwise HIGH.
  That is: Y = NOT(A AND B). A NAND gate.
```

### The NOR Gate — Swap the Topology

The NOR gate inverts the NAND topology: PMOS transistors go in **series** (both must
be off to pull high) and NMOS go in **parallel** (either one pulls low).

```
  Result: output is HIGH only when A=0 AND B=0.
  That is: Y = NOT(A OR B). A NOR gate.
```

### Why NAND is Universal

Here is one of the most important facts in digital logic:

**You can build ANY logic gate using only NAND gates.**

- NOT from NAND: feed the same signal to both inputs. NAND(A, A) = NOT(A).
- AND from NAND: NAND followed by NOT. That is, NAND(NAND(A,B), NAND(A,B)).
- OR from NAND: NOT each input, then NAND. NAND(NOT(A), NOT(B)).

This means a chip factory only needs to manufacture one kind of gate. Everything else
is just wiring. NOR is also universal by the same argument, and in practice many
early computers (like the Apollo Guidance Computer) were built entirely from NOR gates.

---

## 2. The Six Fundamental Gates

Here is the master truth table for all six gates:

| A | B | NOT A | A AND B | A OR B | A NAND B | A NOR B | A XOR B |
|---|---|-------|---------|--------|----------|---------|---------|
| 0 | 0 |   1   |    0    |   0    |    1     |    1    |    0    |
| 0 | 1 |   1   |    0    |   1    |    1     |    0    |    1    |
| 1 | 0 |   0   |    0    |   1    |    1     |    0    |    1    |
| 1 | 1 |   0   |    1    |   1    |    0     |    0    |    0    |

What each gate "means" in plain English:

- **NOT** — "Flip the bit." Symbol: triangle with a bubble at the tip.
- **AND** — "Both must be true." Symbol: flat left edge, curved (D-shaped) right edge.
- **OR** — "At least one must be true." Symbol: curved left edge, pointed right edge.
- **NAND** — "Not both true." Symbol: AND shape with a bubble at the output.
- **NOR** — "Neither is true." Symbol: OR shape with a bubble at the output.
- **XOR** — "One or the other, but not both." Symbol: like OR with a double left curve.

XOR deserves special attention: it answers "are these two bits different?" and is
addition modulo 2 — exactly what you need to add single binary digits.

---

## 3. Boolean Algebra — The Math of 0 and 1

George Boole, in 1854, figured out that logical reasoning could be reduced to algebra.
Variables can only be 0 or 1. Operations map to gates:

- **AND** behaves like multiplication: `A * B` (written `AB`)
- **OR** behaves like addition: `A + B`
- **NOT** is written with a bar: `A'` or `NOT(A)` or `A̅`

This is not a metaphor. It is literally algebra with two values.

### Fundamental Laws

```
Identity:      A + 0 = A          A * 1 = A
Null:          A + 1 = 1          A * 0 = 0
Idempotent:    A + A = A          A * A = A
Complement:    A + A' = 1         A * A' = 0
Commutative:   A + B = B + A      A * B = B * A
Associative:   (A+B)+C = A+(B+C)  (AB)C = A(BC)
Distributive:  A(B + C) = AB + AC
               A + BC = (A+B)(A+C)   ← this one is NEW; normal algebra lacks it
```

### De Morgan's Laws — The Crown Jewels

```
NOT(A AND B)  =  (NOT A) OR (NOT B)
NOT(A OR B)   =  (NOT A) AND (NOT B)
```

Or in our notation:
```
(AB)' = A' + B'
(A + B)' = A'B'
```

**Proof by truth table (first law):**

| A | B | AB | (AB)' | A' | B' | A'+B' |
|---|---|----|-------|----|----|-------|
| 0 | 0 |  0 |   1   |  1 |  1 |   1   |
| 0 | 1 |  0 |   1   |  1 |  0 |   1   |
| 1 | 0 |  0 |   1   |  0 |  1 |   1   |
| 1 | 1 |  1 |   0   |  0 |  0 |   0   |

Columns `(AB)'` and `A'+B'` are identical. QED.

These laws are profoundly useful. They tell you how to "push" a NOT through an AND or OR
by flipping the operator. In hardware terms: a NAND gate is the same as OR-with-inverted-inputs.
A NOR gate is the same as AND-with-inverted-inputs.

### Why Simplification Matters

Consider the expression: `F = A'B + AB' + AB`

We can simplify:
```
F = A'B + AB' + AB
  = A'B + A(B' + B)       ← factor out A
  = A'B + A(1)            ← complement law
  = A'B + A
  = A + A'B               ← commutative
  = A + B                 ← absorption law: A + A'B = A + B
```

The original expression needs 5 gates. The simplified version needs 1 OR gate.
In a chip with billions of such expressions, simplification saves transistors,
power, heat, and money. This is why Boolean algebra is not optional.

---

## 4. Combinational Logic — Circuits That Compute

A **combinational circuit** has no memory. Its output depends entirely on its current
inputs. Change the inputs, the output changes (after a tiny propagation delay).
Nothing is remembered. Think of it as a pure function in hardware.

### The Half Adder — Adding Two Bits

When you add two single-bit numbers A and B, you get a sum bit and a carry bit:

```
  0+0 = 00    0+1 = 01    1+0 = 01    1+1 = 10
```

| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 |  0  |   0   |
| 0 | 1 |  1  |   0   |
| 1 | 0 |  1  |   0   |
| 1 | 1 |  0  |   1   |

Look at the truth tables: **Sum = A XOR B** and **Carry = A AND B**.

```
  A ─────┬──────[XOR]───── Sum
         │
  B ──┬──┘
      │
      └─────────[AND]───── Carry
```

Two gates. That is all it takes to add two bits.

### The Full Adder — Adding Three Bits

A half adder cannot handle carries from previous columns. A full adder takes three
inputs: A, B, and Carry-in (Cin), and produces Sum and Carry-out (Cout).

| A | B | Cin | Sum | Cout |
|---|---|-----|-----|------|
| 0 | 0 |  0  |  0  |   0  |
| 0 | 0 |  1  |  1  |   0  |
| 0 | 1 |  0  |  1  |   0  |
| 0 | 1 |  1  |  0  |   1  |
| 1 | 0 |  0  |  1  |   0  |
| 1 | 0 |  1  |  0  |   1  |
| 1 | 1 |  0  |  0  |   1  |
| 1 | 1 |  1  |  1  |   1  |

```
Sum  = A XOR B XOR Cin
Cout = (A AND B) OR (Cin AND (A XOR B))
```

A full adder is two half adders plus an OR gate:

```
  A ──────[XOR]──────┬──────[XOR]───── Sum
  B ──────┘          │        │
                     │   Cin ─┘
  A ──────[AND]──┐   │
  B ──────┘      │   └──[AND]──┐
                 │      Cin ───┘
                 └──────[OR]─────── Cout
```

### The Ripple-Carry Adder — Multi-Bit Addition

Chain N full adders to add N-bit numbers. The carry-out of each stage feeds the
carry-in of the next:

```
  Bit 0:   A0,B0,Cin=0 → [FA] → S0, C0
  Bit 1:   A1,B1,Cin=C0 → [FA] → S1, C1
  Bit 2:   A2,B2,Cin=C1 → [FA] → S2, C2
  Bit 3:   A3,B3,Cin=C2 → [FA] → S3, C3=Cout

  ┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐
  │  FA  │──▶│  FA  │──▶│  FA  │──▶│  FA  │──▶ Cout
  │ Bit0 │   │ Bit1 │   │ Bit2 │   │ Bit3 │
  └──────┘   └──────┘   └──────┘   └──────┘
     S0         S1         S2         S3
```

This is called "ripple-carry" because the carry ripples through each stage sequentially.
It works, but it is slow — bit 3 cannot finish until bit 2's carry arrives. Real CPUs
use smarter designs (carry-lookahead) to compute carries in parallel.

### The Multiplexer (MUX) — The If-Statement of Hardware

A MUX selects one of several inputs and forwards it to the output, based on a
selection signal. A 2-to-1 MUX:

```
  If Sel=0, output = A.    If Sel=1, output = B.

  Output = (Sel' * A) + (Sel * B)
```

```
  A ──[AND]──┐
  Sel'───┘   │
             [OR]───── Output
  B ──[AND]──┘
  Sel────┘
```

It is an if-statement: "if Sel=0, pick A; if Sel=1, pick B." In a CPU, multiplexers
are everywhere — choosing registers, ALU results, instruction paths. A 4-to-1 MUX uses
two selection bits; an 8-to-1 uses three. Pattern: N bits select one of 2^N inputs.

### The Decoder — Binary to One-Hot

A decoder takes an N-bit binary input and activates exactly one of 2^N output lines.

2-to-4 decoder:

| A1 | A0 | Y3 | Y2 | Y1 | Y0 |
|----|----|----|----|----|----|
|  0 |  0 |  0 |  0 |  0 |  1 |
|  0 |  1 |  0 |  0 |  1 |  0 |
|  1 |  0 |  0 |  1 |  0 |  0 |
|  1 |  1 |  1 |  0 |  0 |  0 |

Input "10" (binary 2) lights up output Y2. Decoders are used for memory addressing —
give it an address, it selects exactly that memory cell.

### ALU Sketch — Combining Everything

An Arithmetic Logic Unit combines all the pieces:

```
  A ────────────┐
                ├──[ADDER]──────────┐
  B ────────────┤                   │
                ├──[AND logic]──────┤
                ├──[OR logic]───────┼──[MUX]──── Result
                ├──[XOR logic]──────┤    ▲
                └──[NOT logic]──────┘    │
                                         │
  Operation ─────────────────────────────┘
  Select
```

The MUX at the end chooses which operation's result to output, based on an operation
code. Add? Select the adder output. AND? Select the AND output. This is the core of
computation: do all the operations simultaneously, then pick the one you wanted.

---

## 5. Sequential Logic — Introducing Memory and Time

Everything in Section 4 is stateless. Feed it inputs, get outputs, done. But
computation requires *memory* — a computer that cannot remember the last step
cannot take the next one. We need circuits that can *hold* a value.

The key insight: **feedback.** If you wire a gate's output back to its input,
the circuit can sustain a state. It remembers.

### The SR Latch — The Simplest Memory

Two NOR gates, cross-coupled (each one's output feeds the other's input):

```
  S ────[NOR]──┬──── Q
          ▲    │
          │    ▼
          └──[NOR]──── Q'
  R ─────────┘
```

- **S = Set:** forces Q to 1.
- **R = Reset:** forces Q to 0.
- **Both 0:** the latch *holds* its current value. This is memory.
- **Both 1:** forbidden — the output is undefined. Do not do this.

| S | R | Q (next) | Behavior   |
|---|---|----------|------------|
| 0 | 0 | Q (hold) | Remember   |
| 0 | 1 |    0     | Reset      |
| 1 | 0 |    1     | Set        |
| 1 | 1 | invalid  | Forbidden  |

How does hold work? Suppose Q=1. Then Q'=0. Top NOR gets (S=0, Q'=0) and outputs 1,
sustaining Q=1. Bottom NOR gets (R=0, Q=1) and outputs 0, sustaining Q'=0. The loop
is stable. The circuit remembers.

### The D Flip-Flop — One Clean Bit of Memory

The SR latch has problems: the forbidden state, and it reacts to inputs whenever they
change. We want something more disciplined.

A **D flip-flop** has one data input (D) and one clock input (CLK). It captures D at
the exact moment the clock transitions from 0 to 1 (the *rising edge*) and holds that
value until the next rising edge. Between edges, changes to D are ignored.

```
       ┌───────────┐
  D ──▶│           │
       │  D Flip-  ├──▶ Q
  CLK ─│  Flop     │
    ▲  │           ├──▶ Q'
    │  └───────────┘
    │
  Rising edge triggers capture
```

| CLK edge | D | Q (next) |
|----------|---|----------|
| Rising   | 0 |    0     |
| Rising   | 1 |    1     |
| No edge  | X | Q (hold) |

One flip-flop = one bit of memory. Every register, cache line, and piece of CPU state
is built from flip-flops.

### The Clock — The Heartbeat

The clock is a signal that oscillates between 0 and 1 at a fixed frequency:

```
        ┌───┐   ┌───┐   ┌───┐   ┌───┐
  CLK ──┘   └───┘   └───┘   └───┘   └───
        ^       ^       ^       ^
        │       │       │       │
      rising  rising  rising  rising
      edges   edges   edges   edges
```

A "3 GHz" CPU means this signal toggles 3 billion times per second. Every rising edge
is one "tick" — flip-flops capture new values and the machine advances one step. Between
ticks, combinational logic computes the next values. This is synchronous digital logic:
compute, capture, compute, capture, forever. The clock imposes discrete *time* on what
would otherwise be a tangled mess of racing signals.

### Registers — Groups of Flip-Flops

A register is just N flip-flops sharing the same clock, storing an N-bit value:

```
  D0 ──[FF]── Q0
  D1 ──[FF]── Q1     All FFs share
  D2 ──[FF]── Q2     the same CLK
  D3 ──[FF]── Q3
  ...
  D7 ──[FF]── Q7

  = one 8-bit register
```

Your CPU's registers (like x86's `rax`, `rbx`, etc.) are exactly this — 64 flip-flops
wired together, capturing a 64-bit value on each clock edge.

### Counters and Shift Registers

**Counter:** A register whose output feeds through a "+1" adder and loops back to its
input. Each clock tick, the stored value increments by one. The program counter in a
CPU is exactly this — stepping through memory addresses to fetch the next instruction.

**Shift register:** A chain of flip-flops where each one's output feeds the next one's
input. Each clock tick, data shifts one position down the chain. Used for
serial-to-parallel conversion, buffering, and delay lines.

---

## 6. From Gates to Bigger Things

Let us take stock of what we have built, from the bottom up:

```
  Transistors
      │
      ▼
  Logic Gates (NOT, NAND, NOR, AND, OR, XOR)
      │
      ├──▶ Combinational Logic (adders, MUX, decoder, ALU)
      │
      └──▶ Sequential Logic (latches, flip-flops, registers, counters)
              │
              ▼
         Registers + ALU + Control Logic = a CPU
```

This is the architecture of every digital computer: **registers** hold data, the
**ALU** operates on it, **control logic** reads instructions and orchestrates
everything, and the **clock** synchronizes it all one tick at a time.

The next layer answers: how do you organize these blocks into a machine that can
execute *any* program? That requires an instruction set, a fetch-decode-execute cycle,
and memory hierarchy — all built from the gates and flip-flops we covered here.

---

## Key Mental Models

- **Everything is NAND.** Any logic function, no matter how complex, can be decomposed
  into NAND gates. This means digital logic has a single universal primitive.

- **Combinational vs. sequential.** Combinational = pure function, no memory, instant
  (ignoring propagation delay). Sequential = has state, depends on history, needs a
  clock. Every digital circuit is a composition of both.

- **The clock creates time.** Without a clock, signals race and collide. The clock
  imposes discrete time steps, turning continuous electronics into a step-by-step
  machine — which is exactly what a computer is.

- **Abstraction layers are the whole game.** Transistors become gates. Gates become
  adders and flip-flops. Adders and flip-flops become ALUs and registers. ALUs and
  registers become a CPU. You never need to think about transistors when designing an
  ALU, and you never need to think about gates when writing software. Each layer hides
  the one below it.

---

## How This Connects

**From Layer 0 (Mathematics):** Boolean algebra is propositional logic applied to
circuits. The truth tables, De Morgan's laws, and simplification rules from
[Logic](../../mathematics/01-foundations/logic.md) are the mathematical foundation here.

**From Layer 0 (Physics):** Transistors — voltage-controlled switches in silicon — are
the physical substrate. Without understanding NMOS/PMOS, the circuits are just pictures.

**To Layer 2 (Computer Architecture):** This layer provides the building blocks.
Layer 2 organizes registers, ALU, MUX, and decoders into a machine with an instruction
set, a fetch-decode-execute cycle, caches, and pipelines. Gates are the bricks;
architecture is the blueprint.

---

## Hands-On

1. **Nand2Tetris (nand2tetris.org)** — Build a complete computer from NAND gates.
   Do the hardware chapters (1-5) to lock in this layer. Free.

2. **Logisim Evolution** — Free circuit simulator. Build gates, adders, MUX, ALU, and
   flip-flops visually. Drag, drop, wire, simulate.

3. **Paper exercises:**
   - Derive AND, OR, and NOT from only NAND gates.
   - Build a 4-bit ripple-carry adder from full adder blocks.
   - Prove both of De Morgan's laws by truth table.
   - Simplify: `F = A'B'C + A'BC + AB'C + ABC`. (Hint: factor out C.)
   - Design a 4-to-1 MUX using AND, OR, and NOT gates.

4. **Digital (by Helmut Neemann)** — Lightweight free simulator, good for quickly
   testing Boolean expressions and sequential circuits.

---

## Go Deeper

- *Code* by Charles Petzold — Best first book. Telegraph relays to a working CPU.
- *Digital Design and Computer Architecture* by Harris & Harris — The standard textbook.
- *The Elements of Computing Systems* by Nisan & Schocken — The Nand2Tetris textbook.
- Ben Eater's YouTube series — Builds an 8-bit CPU on breadboards from logic chips.

---

*Next: [Computer Architecture](computer-architecture.md) — How do logic gates become a general-purpose computer?*
