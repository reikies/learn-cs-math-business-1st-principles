# Physics & Electricity — From Atoms to Switches

> Everything a computer does — every calculation, every pixel, every packet — is electrons
> moving through carefully arranged materials. Before we can understand computing,
> we need to understand the physical world that makes it possible.

**Prerequisites:** None. This is where it all begins.
**What it enables:** Logic gates, digital circuits, all of computing.

---

## Why This Matters

A computer is not magic. It is billions of tiny switches, flipping on and off billions of times
per second, orchestrated to do useful work. Those switches are made of atoms, controlled by
voltage, built from silicon. If you understand this layer — really understand it — everything
above it becomes "just engineering."

This document takes you from the atom to the transistor to the chip. By the end, you'll know
*why* a computer works, not just *that* it works.

---

## 1. Atoms, Electrons, and Charge

### The atom, simplified

Every atom has three players:

| Particle | Charge   | Location         | Mass        |
|----------|----------|------------------|-------------|
| Proton   | Positive | Nucleus (center) | Heavy       |
| Neutron  | Neutral  | Nucleus (center) | Heavy       |
| Electron | Negative | Orbiting shells  | ~1/1836 of proton |

Normally, an atom has equal protons and electrons, so the charges cancel out — it's
electrically **neutral**. But electrons in the outermost shell (the **valence shell**) are
loosely held. They can be knocked free, shared, or stolen.

This is the whole game. Computing exists because we can **control the movement of electrons**.

### Charge

Charge is the fundamental property that makes electricity possible. Like charges repel,
opposite charges attract. The unit is the **coulomb** (C). One electron carries a charge of
approximately -1.6 x 10^-19 coulombs — unimaginably tiny, but in bulk, electrons create
forces we can harness.

### The three quantities: voltage, current, resistance

Think of water flowing through a pipe. The analogy isn't perfect, but it's useful:

```
Water analogy:           Electrical equivalent:
─────────────           ──────────────────────
Water pressure      →   Voltage (V, in volts)
Water flow rate     →   Current (I, in amperes)
Pipe narrowness     →   Resistance (R, in ohms)
```

**Voltage (V)** — the "pressure" or potential difference that pushes electrons through a
conductor. A battery creates a voltage difference between its terminals: electrons *want* to
flow from the negative terminal (excess electrons) to the positive terminal (deficit of
electrons). Measured in **volts (V)**.

**Current (I)** — the actual flow of electrons. Specifically, the number of coulombs of
charge passing a point per second. 1 ampere = 1 coulomb per second. Measured in **amperes (A)**.

**Resistance (R)** — how much a material opposes the flow of electrons. A thick copper wire
has low resistance. A thin tungsten filament has high resistance. Measured in **ohms (Omega)**.

### Ohm's Law

The relationship between these three is simple and fundamental:

```
    V = I x R

    Voltage = Current x Resistance
```

Rearranged:
```
    I = V / R      (more voltage or less resistance → more current)
    R = V / I      (more voltage for the same current → more resistance)
```

**Example:** A 9V battery connected across a 100-ohm resistor:
```
    I = V / R = 9 / 100 = 0.09 A = 90 milliamps
```

**Power** is how much energy is consumed per second:
```
    P = V x I      (watts = volts x amps)
    P = I^2 x R    (substituting V = IR)
    P = V^2 / R    (substituting I = V/R)
```

This matters enormously for chip design — billions of transistors switching means a lot of
power dissipated as heat. Power management is one of the hardest problems in modern computing.

### Why this matters for computing

We can control electron flow. That means we can build a device where "electrons flowing"
represents one state (say, 1) and "electrons not flowing" represents another state (say, 0).
Two states. Binary. That's the seed of everything.

---

## 2. Conductors, Insulators, Semiconductors

Not all materials treat electrons the same way.

### Conductors

Materials where electrons flow easily. The valence electrons are so loosely bound that they
form a "sea" of free electrons drifting through the material.

```
Copper (Cu): 1 valence electron, very loosely held
             Resistivity: ~1.7 x 10^-8 ohm-meters
             Used for: wires, PCB traces, chip interconnects
```

Other good conductors: silver (best, but expensive), gold (resists corrosion — used for
contacts), aluminum (lightweight — used in power lines and some chip layers).

### Insulators

Materials where electrons are tightly bound to their atoms. Virtually no free electrons
are available to carry current.

```
Rubber:   Resistivity: ~10^13 ohm-meters
Glass:    Resistivity: ~10^12 ohm-meters
Silicon dioxide (SiO2): used as insulating layers in chips
```

The ratio of resistivity between a good conductor and a good insulator is roughly
10^20 — that's a 1 followed by 20 zeros. Nature gives us an enormous range to work with.

### Semiconductors — the interesting ones

Semiconductors sit between conductors and insulators. In their pure form, they're poor
conductors. But by carefully adding impurities, we can control their conductivity with
extraordinary precision.

```
Silicon (Si): 4 valence electrons
              Resistivity: ~10^3 ohm-meters (pure)
              Can be tuned from near-insulator to near-conductor
```

**Why silicon?** Four valence electrons is the sweet spot. Silicon atoms bond with four
neighbors, sharing one electron each, forming a perfect **crystal lattice**:

```
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- Si --- Si --- Si

    Each line = a shared electron pair (covalent bond)
    Every silicon atom shares 4 bonds → full outer shell (8 electrons)
```

In pure silicon at room temperature, thermal energy occasionally knocks an electron free,
creating a tiny amount of conductivity. But this isn't enough to be useful. To make silicon
do interesting things, we need to **dope** it.

---

## 3. Doping and the P-N Junction

### N-type doping: adding extra electrons

Take pure silicon and introduce a tiny amount of **phosphorus** (5 valence electrons).
Phosphorus slots into the crystal lattice, bonding with 4 silicon neighbors. But it has
one electron left over — a free electron with no bond to hold it in place.

```
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- [P] -- Si --- Si        P = Phosphorus (5 valence electrons)
        |      |e-    |      |         e- = extra free electron
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- Si --- Si --- Si
```

Result: the material now has free electrons available to carry current. It's called
**N-type** ("N" for negative charge carriers). The phosphorus atom is called a **donor**
because it donates a free electron.

The material is still electrically neutral overall — phosphorus has an extra proton too.
But it has mobile charge carriers that can flow when voltage is applied.

### P-type doping: creating holes

Now introduce **boron** (3 valence electrons) into pure silicon. Boron bonds with 4
silicon neighbors, but it's one electron short. This missing electron is called a **hole**.

```
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- [B] -- Si --- Si        B = Boron (3 valence electrons)
        |      |o     |      |         o = hole (missing electron)
        Si --- Si --- Si --- Si
        |      |      |      |
        Si --- Si --- Si --- Si
```

A neighboring electron can jump into the hole, which effectively moves the hole to where
that electron was. Holes behave like positive charge carriers moving through the material.

Result: **P-type** silicon ("P" for positive charge carriers). Boron is called an
**acceptor** because it accepts electrons.

### The P-N Junction: where it gets magical

Place P-type and N-type silicon next to each other:

```
    P-type            |          N-type
    (holes = ○)       |     (electrons = ●)
                      |
    ○ ○ ○ ○ ○ ○      |      ● ● ● ● ● ●
    ○ ○ ○ ○ ○ ○      |      ● ● ● ● ● ●
    ○ ○ ○ ○ ○ ○      |      ● ● ● ● ● ●
```

At the boundary, electrons from the N side drift into the P side and fill holes. This
creates a thin region with no free charge carriers — the **depletion zone**:

```
    P-type       Depletion Zone       N-type
    (holes)      (no carriers)     (electrons)
                      
    ○ ○ ○ ○    |  -  -  +  +  |    ● ● ● ●
    ○ ○ ○ ○    |  -  -  +  +  |    ● ● ● ●
    ○ ○ ○ ○    |  -  -  +  +  |    ● ● ● ●
               
               The - and + are fixed charges
               (ionized atoms that can't move)
```

The fixed charges in the depletion zone create an electric field that opposes further
movement. It's a natural barrier — equilibrium is reached.

### Forward bias: opening the valve

Connect a battery with the positive terminal to the P-side and negative to the N-side:

```
      (+) ──── P  |  N ──── (-)
                  ↓
             Current flows!
```

The external voltage pushes holes toward the junction and electrons toward the junction,
shrinking the depletion zone. If the voltage is high enough (about 0.7V for silicon),
the barrier collapses and current flows freely.

### Reverse bias: closing the valve

Flip the battery:

```
      (-) ──── P  |  N ──── (+)
                  ↓
             No current (almost)
```

The external voltage pulls holes and electrons *away* from the junction, widening the
depletion zone. The barrier grows. No current flows (except a negligible leakage current).

### The diode

A P-N junction is a **diode** — a one-way valve for electrons.

```
    Forward bias (V > 0.7V):  current flows    → ON
    Reverse bias:             no current        → OFF
```

Diode symbol:
```
        ──▶|──
    Anode (P)  Cathode (N)
    Current flows this way →
```

This is already remarkable: a device made of carefully doped silicon that allows current in
one direction but blocks it in the other. But we need something more powerful for computing.
We need a switch we can control *electrically*.

---

## 4. The Transistor — The Atom of Computing

### The idea

A diode is a one-way valve. A transistor is a **valve with a handle** — a switch controlled
by a voltage signal. Apply a voltage to the control input, and current either flows or
doesn't between the other two terminals.

This is the most important device in human history. Not an exaggeration.

### MOSFET: the transistor that runs the world

MOSFET stands for **Metal-Oxide-Semiconductor Field-Effect Transistor**. It has three
terminals:

| Terminal | Role                                    |
|----------|-----------------------------------------|
| Source   | Where current comes from                |
| Drain    | Where current goes to                   |
| Gate     | The control input (electrically isolated)|

The gate is separated from the semiconductor channel by a thin insulating layer of silicon
dioxide (SiO2). No current ever flows through the gate — it works purely by electric field,
which is why it's called a "field-effect" transistor. This means the gate consumes almost
no power to hold its state.

### NMOS transistor (N-channel MOSFET)

Built on P-type silicon with N-type source and drain regions:

```
                    Gate
                     |
            ─────────┴─────────
            |   SiO2 (oxide)  |
    ════════╪═════════════════╪════════
    Source  │                 │  Drain
    (N)     │    P-type body  │  (N)
            │                 │
    ════════╧═════════════════╧════════
```

**Gate LOW (0V):** No conductive channel exists between source and drain. The transistor
is OFF. No current flows.

**Gate HIGH (e.g., 5V or 1V depending on the technology):** The electric field from the
gate repels holes in the P-type body and attracts electrons, creating a thin N-type channel
between source and drain. Current flows. The transistor is ON.

```
    NMOS summary:
    Gate = HIGH  →  Switch ON   (current flows: source ↔ drain)
    Gate = LOW   →  Switch OFF  (no current)
```

### PMOS transistor (P-channel MOSFET)

The complement of NMOS. Built on N-type silicon with P-type source and drain:

```
    PMOS summary:
    Gate = LOW   →  Switch ON   (current flows)
    Gate = HIGH  →  Switch OFF  (no current)
```

PMOS is the mirror image of NMOS — opposite doping, opposite behavior.

### CMOS: the winning combination

**CMOS** = Complementary Metal-Oxide-Semiconductor. It pairs an NMOS and a PMOS transistor
together. This is the technology used in virtually every modern chip.

Why? Because of power consumption.

```
    CMOS Inverter (simplest CMOS circuit):

         VDD (power supply, e.g., 1V)
          |
        ──┤ PMOS   (ON when input is LOW)
          |
    IN ───┤──── OUT
          |
        ──┤ NMOS   (ON when input is HIGH)
          |
         GND (ground, 0V)
```

**When input = HIGH:**
- NMOS is ON → connects output to GND → output = LOW
- PMOS is OFF → disconnected from VDD
- No path from VDD to GND → **no static power consumed**

**When input = LOW:**
- PMOS is ON → connects output to VDD → output = HIGH
- NMOS is OFF → disconnected from GND
- No path from VDD to GND → **no static power consumed**

In either stable state, there's no direct path from power to ground. Current only flows
briefly *during* the transition. This is why CMOS revolutionized computing — it made it
possible to put billions of transistors on a chip without melting it.

### The transistor as a switch

Strip away all the physics, and here's what we're left with:

```
    ┌──────────────────────────────────────┐
    │  A transistor is an electrically      │
    │  controlled switch.                   │
    │                                       │
    │  Input voltage → output connection    │
    │  HIGH or LOW  → ON or OFF             │
    │                                       │
    │  That's it. That's the atom of        │
    │  computing.                           │
    └──────────────────────────────────────┘
```

Everything above this — logic gates, adders, memory, CPUs, operating systems, web browsers,
AI models — is built from combinations of this one device.

---

## 5. Chip Fabrication — Building at the Nanometer Scale

How do you actually *make* billions of transistors on a chip the size of your fingernail?

### Step 1: Grow a crystal

Start with extremely pure silicon (99.9999999% pure — "nine nines"). Melt it at 1,414C
and slowly pull a rotating seed crystal out of the molten silicon. The result is a single
cylindrical **ingot** of silicon with a perfect crystal structure, about 300mm (12 inches)
in diameter.

### Step 2: Slice into wafers

Diamond saws cut the ingot into thin discs called **wafers**, each about 0.75mm thick.
The wafers are polished to a mirror finish — surface imperfections must be smaller than
the features we're about to print.

### Step 3: Photolithography — printing with light

This is where the magic happens. Photolithography is essentially photography at the
nanometer scale.

```
    The process:

    1. Coat wafer with PHOTORESIST (light-sensitive chemical)

    2. Shine UV LIGHT through a MASK (like a stencil of the circuit pattern)
       ┌─────────────┐
       │  UV Source   │
       └──────┬──────┘
              ▼
       ┌─────────────┐
       │    Mask      │  ← Contains the circuit pattern
       │  ▓▓░░▓▓░░▓  │     (▓ = opaque, ░ = transparent)
       └──────┬──────┘
              ▼
       ┌─────────────┐
       │ Photoresist  │  ← Exposed areas become soluble
       ├─────────────┤
       │   Silicon    │
       └─────────────┘

    3. DEVELOP: wash away exposed (or unexposed) photoresist

    4. ETCH: remove silicon/oxide where photoresist was washed away

    5. Strip remaining photoresist

    6. DEPOSIT new materials (metal, oxide, etc.)

    7. Repeat 50-100+ times for different layers
```

The mask contains the pattern for one layer of the chip. A modern processor has **dozens**
of layers stacked on top of each other — transistors at the bottom, then metal wires
connecting them, layer upon layer.

### Step 4: Etching and deposition

- **Etching** removes material where the pattern says there should be gaps
- **Deposition** adds material where the pattern says there should be connections
- **Ion implantation** shoots dopant atoms (phosphorus, boron) into precise locations to
  create the N-type and P-type regions of each transistor

### Step 5: Stacking layers

A modern chip isn't flat — it's a skyscraper of layers:

```
    Cross-section of a modern chip (simplified):

    ─────────────────────────── Metal Layer 10+ (global wiring)
    ─────────────────────────── Metal Layer 9
           ...                  (many metal layers)
    ─────────────────────────── Metal Layer 2
    ─────────────────────────── Metal Layer 1 (local wiring)
    ═══════════════════════════ Transistor layer (the actual logic)
    ─────────────────────────── Silicon substrate
```

Lower metal layers connect nearby transistors. Upper metal layers carry signals and power
across the entire chip. The vias (vertical connections between layers) stitch everything
together.

### Step 6: Testing and packaging

Each die (chip) on the wafer is tested. Defective ones are marked. Working dies are cut
apart, mounted in packages with pins/balls for connecting to circuit boards, and tested
again.

### The nanometer number

When you hear "5nm chip" or "3nm process," that number is the **technology node** — roughly
corresponding to the smallest feature size. But in modern nodes, it's more of a marketing
label than a literal measurement. What matters is:

| Node    | Approx. transistors per mm^2 | Used in                |
|---------|------------------------------|------------------------|
| 14nm    | ~30 million                  | Older Intel, AMD CPUs  |
| 7nm     | ~90 million                  | Apple A13, AMD Zen 2   |
| 5nm     | ~170 million                 | Apple M1, AMD Zen 4    |
| 3nm     | ~300 million                 | Apple M3, latest chips |
| 2nm     | ~500 million (projected)     | Coming soon            |

### Moore's Law

In 1965, Gordon Moore observed that the number of transistors on a chip roughly doubles
every two years. This isn't a law of physics — it's an observation about engineering
progress. For decades it held, driven by shrinking transistors. As we approach atomic
limits, the doubling has slowed, but innovation continues through:

- **3D transistor designs** (FinFET, Gate-All-Around)
- **Chiplet architectures** (multiple small dies in one package)
- **New materials** (beyond silicon)
- **Advanced packaging** (stacking chips vertically)

### Key companies

| Company | Role                                                    |
|---------|---------------------------------------------------------|
| TSMC    | Largest contract chip manufacturer. Makes chips for Apple, AMD, NVIDIA, etc. |
| ASML    | Only company making EUV lithography machines ($200M+ each). Monopoly. |
| Intel   | Designs AND manufactures chips. Catching up on process tech. |
| Samsung | Manufactures chips for itself and others. Competes with TSMC. |
| NVIDIA  | Designs GPUs (manufactured by TSMC). Dominant in AI/ML hardware. |

The entire digital world runs through an absurdly narrow supply chain. ASML's EUV machines
are arguably the most complex machines humans have ever built.

---

## 6. Analog to Digital — Why Binary Wins

### The noise problem

In the real world, signals are **analog** — continuously varying. A voltage might be
3.127V or 3.128V or anything in between. The problem: noise. Every wire picks up
interference from neighboring wires, radio waves, thermal fluctuations, cosmic rays.

```
    Analog signal after transmission:

    Original:  ~~~∿∿∿~~~∿∿∿~~~
    Received:  ~~∿~∿∿~~∿~∿∿~∿~   ← Which bumps are signal? Which are noise?
```

If you copy an analog signal, you copy the noise too. After enough copies, the original
information is unrecoverable. This is why a photocopy of a photocopy looks terrible.

### The digital solution

Instead of using the full range of voltages, only care about two levels: **HIGH** and
**LOW**. Define a threshold and ignore everything in between.

```
    Voltage
    5V ─── ┌──────────────────────  HIGH zone (interpreted as 1)
           │
    3V ─── │- - - - - - - - - - -   Threshold
           │
    0V ─── └──────────────────────  LOW zone (interpreted as 0)
```

Now noise doesn't matter, as long as it doesn't push a HIGH signal below the threshold or
a LOW signal above it. We get perfect reconstruction — a received signal can be "cleaned
up" to exact 0s and 1s, then retransmitted. A digital copy is identical to the original.
The millionth copy is identical to the first.

This is why digital won. Not because it's more precise (analog can represent infinitely
fine gradations), but because it's **robust against noise** and **perfectly reproducible**.

### Binary: counting in base 2

We have two states (HIGH/LOW, 1/0), so we count in **base 2**:

| Decimal | Binary | How to read it                           |
|---------|--------|------------------------------------------|
| 0       | 0000   | 0                                        |
| 1       | 0001   | 1                                        |
| 2       | 0010   | one 2                                    |
| 3       | 0011   | one 2 + one 1                            |
| 4       | 0100   | one 4                                    |
| 5       | 0101   | one 4 + one 1                            |
| 7       | 0111   | one 4 + one 2 + one 1                    |
| 8       | 1000   | one 8                                    |
| 10      | 1010   | one 8 + one 2                            |
| 15      | 1111   | one 8 + one 4 + one 2 + one 1            |
| 255     | 11111111 | eight 1s = 2^8 - 1                     |

Each digit is a power of 2, just as each decimal digit is a power of 10:

```
    Binary:   1    0    1    1
              |    |    |    |
              2^3  2^2  2^1  2^0
              8    4    2    1
              
              8 + 0 + 2 + 1 = 11 (decimal)
```

### Bits and bytes

| Unit     | Size          | Can represent                     |
|----------|---------------|-----------------------------------|
| 1 bit    | 0 or 1        | 2 distinct values                 |
| 1 byte   | 8 bits        | 256 values (0 to 255)             |
| 1 KB     | 1,024 bytes   | A short email                     |
| 1 MB     | 1,024 KB      | A photo, a minute of MP3          |
| 1 GB     | 1,024 MB      | A movie (compressed)              |
| 1 TB     | 1,024 GB      | A large hard drive                |

### Everything is a number

This is the deep insight. Once you have bits, you can encode **anything** as a number:

| Data type | Encoding scheme                                              |
|-----------|--------------------------------------------------------------|
| Text      | Each character gets a number. 'A' = 65, 'B' = 66, ... (ASCII/Unicode) |
| Color     | Red/Green/Blue each 0-255. White = (255, 255, 255)           |
| Images    | Grid of pixels, each pixel = a color = a number              |
| Sound     | Sample the audio wave thousands of times per second, store each sample as a number |
| Video     | A sequence of images + audio + timing                        |
| Programs  | Instructions are numbers. "ADD" might be 0001, "STORE" might be 0010 |

There is no "text" inside a computer. No "pictures." No "sound." There are only numbers,
and there are conventions for interpreting those numbers. A JPEG file is just a sequence of
bytes. Your image viewer knows the JPEG convention and reconstructs the image.

This is liberating: any problem that can be expressed as manipulating numbers can be solved
by a computer. And it turns out that virtually everything can be expressed as manipulating
numbers.

---

## Key Mental Models

- **The transistor is a voltage-controlled switch.** Forget the physics details when
  thinking about higher layers. A transistor is: "if this wire is HIGH, connect those two
  wires; if LOW, disconnect them." That's the abstraction boundary.

- **CMOS gives us switches that cost almost no power to hold.** Power is consumed only
  during switching. This is why we can have billions of transistors on a chip without it
  instantly melting.

- **Digital beats analog because of noise immunity.** Two voltage levels with a fat
  threshold means we can copy, transmit, and store information perfectly. This one trick
  enables everything.

- **Everything is a number.** Text, images, sound, video, programs — all encoded as
  patterns of bits. The hardware doesn't know or care what the bits "mean." Meaning lives
  in software and convention.

---

## How This Connects Upward

Layer 1 (Logic Gates & Boolean Algebra) needs exactly three things from this layer:

1. **Transistors as switches.** NMOS turns on when gate is HIGH, PMOS turns on when gate is
   LOW. Combine them (CMOS) to build gates that output HIGH or LOW based on their inputs.

2. **Binary representation.** HIGH voltage = 1, LOW voltage = 0. Two states, perfectly
   distinguishable. All data and instructions encoded in this binary alphabet.

3. **Fabrication makes it real.** We can print billions of these switches on a single chip,
   connect them with metal wires, and run them at gigahertz speeds. The physics supports
   the abstraction.

From these three facts, everything else follows. Logic gates (AND, OR, NOT) are just
arrangements of CMOS transistors. Adders are arrangements of logic gates. ALUs are
arrangements of adders. CPUs are arrangements of ALUs plus memory. And so on, all the way
up to the browser you're reading this in.

---

## Hands-On

1. **Ohm's Law calculator.** Write a program (in any language) that takes any two of V, I,
   R and computes the third. Add power calculation (P = VI).

2. **Binary conversion.** Write functions to convert decimal to binary and back. Do it by
   hand first for the numbers 42, 100, and 255. Then code it.

3. **Watch "How Semiconductors Work" by Branch Education** on YouTube. The 3D animations
   of P-N junctions and MOSFET operation are worth a thousand ASCII diagrams.

4. **Explore a virtual circuit simulator.** Falstad Circuit Simulator (falstad.com/circuit)
   lets you build circuits with resistors, diodes, and transistors and see current flow in
   real time. Build a voltage divider. Build a diode circuit. See forward/reverse bias.

5. **Read a chip die shot.** Search for "CPU die shot annotated" and try to identify the
   major blocks: cores, cache, memory controller, I/O. Notice how repetitive the structure
   is — that's billions of identical transistors.

6. **Count transistors.** Look up the transistor count of your phone's processor and your
   computer's processor. Compare to the 2,300 transistors in the Intel 4004 (1971). That
   progression is Moore's Law made tangible.

---

## Go Deeper

**Books:**
- *Code: The Hidden Language of Computer Hardware and Software* — Charles Petzold.
  The best book for going from electricity to a working computer. Gentle, visual, thorough.
- *The Art of Electronics* — Horowitz & Hill. The bible of practical electronics. Dense but
  indispensable if you want real depth.
- *But How Do It Know?* — J. Clark Scott. Builds a complete computer from NAND gates up.
  Excellent companion to this curriculum.

**Courses:**
- Nand2Tetris (nand2tetris.org) — Build a computer from logic gates to an operating system.
  The single best "from-first-principles" course in computer science.
- MIT 6.002 (Circuits and Electronics) on MIT OpenCourseWare — rigorous treatment of
  circuits and semiconductor physics.

**YouTube channels:**
- **Ben Eater** — Builds a working 8-bit computer on breadboards. Extraordinary.
- **Branch Education** — Beautiful 3D animations of how chips, transistors, and SSDs work.
- **Asianometry** — Deep dives into semiconductor industry and chip fabrication.
- **3Blue1Brown** — Not hardware-specific, but the best math visualizations on the internet.
  Relevant for understanding signals, binary, and information theory.

**Other:**
- Visual 6502 (visual6502.org) — Interactive transistor-level simulation of the MOS 6502
  CPU. See every transistor switch as the chip executes instructions.

---

*Next: [Logic Gates & Boolean Algebra](logic-gates.md) — How do we arrange these switches into circuits that can add, compare, and remember?*
