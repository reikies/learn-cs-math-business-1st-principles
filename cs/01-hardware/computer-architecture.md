# Computer Architecture — How Logic Gates Become a Computer

> "We can now build any logic gate, any arithmetic circuit, any memory cell.
> But a pile of gates is not a computer — just as a pile of bricks is not a house.
> Architecture is the blueprint that arranges simple parts into something that can
> execute *any* computation, given enough time and memory."

**Prerequisites:** [Logic Gates & Boolean Algebra](logic-gates.md) — You need gates, flip-flops, and ALUs.

**What it enables:** Operating systems, compilers, all software.

---

## 1. The Stored-Program Concept

Before the 1940s, "programming" a computer meant physically rewiring it. Each new
problem required a new configuration of cables and switches. The ENIAC, for example,
could take *weeks* to reprogram.

Then came the insight that changed everything.

### Von Neumann Architecture

John von Neumann (building on ideas from Eckert, Mauchly, and Turing) proposed:
**store the program in memory, right alongside the data it operates on.**

```
┌─────────────────────────────────────────────┐
│                  MEMORY                      │
│  ┌──────────┐  ┌──────────┐                 │
│  │Instructions│  │   Data   │                │
│  │ (program) │  │          │                 │
│  └──────────┘  └──────────┘                 │
└────────────────────┬────────────────────────┘
                     │ (single bus)
              ┌──────┴──────┐
              │     CPU     │
              │  ┌───────┐  │
              │  │  ALU   │  │
              │  ├───────┤  │
              │  │Control │  │
              │  │ Unit   │  │
              │  ├───────┤  │
              │  │Registers│ │
              │  └───────┘  │
              └─────────────┘
```

This is powerful for two reasons:

1. **Programs become data.** You can write programs that manipulate other programs
   (compilers, interpreters, self-modifying code).
2. **General purpose.** The same hardware runs any program — you just load different
   instructions into memory.

### Harvard Architecture

The alternative: separate memory (and buses) for instructions and data.

```
  Instruction Memory ──bus──► CPU ◄──bus── Data Memory
```

Advantage: CPU can fetch an instruction and read data *simultaneously* — no
contention. Many microcontrollers (like Arduino's AVR chips) and DSPs use Harvard
architecture. Modern CPUs are actually a hybrid: they use split L1 caches
(one for instructions, one for data) but unified main memory.

### The Von Neumann Bottleneck

Here is the dark side: since instructions and data share one bus to memory, the CPU
spends a lot of time *waiting* for data to arrive. The single bus becomes a traffic
jam. This bottleneck — identified by John Backus in his 1977 Turing Award lecture —
is one of the central problems in computer architecture. Much of what follows in this
document (caches, pipelining, parallelism) exists to work around it.

---

## 2. The CPU

The Central Processing Unit has three main components. You already built simplified
versions of each from gates in the previous layer. Now we connect them.

### 2.1 The ALU (Arithmetic Logic Unit)

The ALU performs all computation:
- **Arithmetic:** add, subtract, multiply, divide
- **Logic:** AND, OR, NOT, XOR
- **Comparison:** equal, less than, greater than (sets status flags)

It takes two inputs and a control signal telling it *which* operation to perform.
It produces a result and a set of **flags** (zero, carry, overflow, negative).

### 2.2 The Control Unit

The conductor of the orchestra. It:
- Reads the current instruction from memory
- Decodes what the instruction means (what operation? which registers?)
- Sends control signals to the ALU, registers, and memory
- Manages the sequence of operations

Think of it as the "brain of the brain." The ALU does the math; the control unit
decides *what* math to do and *when*.

### 2.3 Registers

Registers are the fastest storage in the entire computer. They live inside the CPU
itself — no bus trip required. A typical modern CPU has:

| Register Type       | Purpose                                          |
|---------------------|--------------------------------------------------|
| **General purpose** | Hold data being worked on (R0-R15, EAX, etc.)   |
| **Program Counter** | Address of the *next* instruction to fetch (PC)  |
| **Instruction Reg** | Holds the *current* instruction being executed (IR)|
| **Stack Pointer**   | Points to top of the call stack (SP)             |
| **Status/Flags**    | Zero flag, carry flag, overflow, negative        |

The PC and IR deserve special attention — they are the two registers that drive
the fetch-decode-execute cycle.

### How They Connect

```
                    ┌──────────────────────────────┐
                    │         CONTROL UNIT          │
                    │                               │
                    │  ┌────┐        ┌──────────┐  │
  Memory ◄────────► │  │ IR │◄─decode─┤ Control  │  │
     ▲              │  └────┘        │  Logic   │  │
     │              │                └────┬─────┘  │
     │              │  ┌────┐             │        │
     │              │  │ PC │─── fetch ───┘        │
     │              │  └────┘                      │
     │              └──────────┬───────────────────┘
     │                         │ control signals
     │              ┌──────────▼───────────────────┐
     │              │         DATAPATH              │
     │              │                               │
     └──────────────┤  ┌──────────┐    ┌───────┐   │
                    │  │ Registers │───►│  ALU  │   │
                    │  │ R0..R15   │◄───│       │   │
                    │  └──────────┘    └───────┘   │
                    └──────────────────────────────┘
```

The control unit fetches instructions, decodes them, and tells the datapath what
to do. The datapath (registers + ALU) does the actual computation.

---

## 3. The Fetch-Decode-Execute Cycle

This is the heartbeat of every computer. Every program — from "Hello, World" to
a neural network training run — is just this cycle running billions of times
per second.

### Step by Step

```
  ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
  │  FETCH   │────►│ DECODE  │────►│ EXECUTE │────►│  STORE  │────►│ADVANCE  │
  │          │     │         │     │         │     │ RESULT  │     │   PC    │
  └─────────┘     └─────────┘     └─────────┘     └─────────┘     └────┬────┘
       ▲                                                                │
       └────────────────────────────────────────────────────────────────┘
                              (repeat forever)
```

**1. FETCH:** The CPU looks at the Program Counter (PC), which holds the address
of the next instruction. It sends that address to memory over the address bus and
reads the instruction into the Instruction Register (IR).

**2. DECODE:** The control unit examines the bits in the IR. It figures out:
   - What operation? (ADD? LOAD? JUMP?)
   - What operands? (Which registers? What memory address?)

**3. EXECUTE:** The control unit sends signals to the ALU or memory to carry out
the instruction. For an ADD, the ALU adds two register values. For a LOAD, data
is fetched from memory into a register.

**4. STORE RESULT:** The output of the operation is written back — to a register,
to memory, or to a status flag.

**5. ADVANCE PC:** The Program Counter increments to point to the next instruction.
(Unless the instruction was a JUMP or BRANCH, in which case the PC is set to the
jump target.)

### A Concrete Example

Suppose memory contains these instructions (simplified):

```
Address  │ Instruction
─────────┼─────────────────
0x0000   │ LOAD R1, [0x0100]     ; load value from memory address 0x0100 into R1
0x0004   │ LOAD R2, [0x0104]     ; load value from memory address 0x0104 into R2
0x0008   │ ADD  R3, R1, R2       ; R3 = R1 + R2
0x000C   │ STORE R3, [0x0108]    ; write R3 back to memory address 0x0108
```

The CPU cycles through fetch-decode-execute for each instruction in sequence.
Four instructions, four cycles through the loop, and we have added two numbers
from memory and stored the result. That's it. That's computing.

---

## 4. Instruction Set Architecture (ISA)

The ISA is the **contract between hardware and software**. It defines:
- What instructions the CPU understands
- What registers are available
- How memory is addressed
- The binary encoding of every instruction

Software sees only the ISA. Hardware can change underneath (new pipeline, different
cache) as long as it honors the contract. This is one of the most important
abstraction boundaries in all of computing.

### Anatomy of an Instruction

Every instruction has an **opcode** (what to do) and **operands** (what to do it to):

```
  ┌──────────┬──────────┬──────────┬──────────┐
  │  OPCODE  │  DEST    │  SRC 1   │  SRC 2   │
  │  (6 bits)│ (5 bits) │ (5 bits) │ (5 bits) │
  └──────────┴──────────┴──────────┴──────────┘

  Example (RISC-style, 32-bit instruction):
  ADD R3, R1, R2
  opcode = ADD, dest = R3, src1 = R1, src2 = R2
```

### RISC vs. CISC

This is one of the great debates in computer architecture.

| Aspect              | CISC                             | RISC                              |
|---------------------|----------------------------------|-----------------------------------|
| **Philosophy**      | Many complex instructions        | Few simple instructions           |
| **Instruction size**| Variable (1-15 bytes for x86)    | Fixed (4 bytes typically)         |
| **Instructions**    | One instruction can do a lot     | Each instruction does one thing   |
| **Decoding**        | Complex, multi-step              | Simple, fast                      |
| **Registers**       | Fewer (historically)             | Many (32+ general purpose)        |
| **Memory access**   | Many instructions can touch memory| Only LOAD and STORE touch memory |
| **Example**         | x86 (Intel/AMD)                  | ARM, RISC-V, MIPS                |

**CISC** (Complex Instruction Set Computer): x86, the ISA in most desktops and
servers. Instructions like `REP MOVSB` can copy an entire block of memory in a
single instruction. The hardware is complex, but the compiler's job is easier.
x86 has accumulated decades of backwards-compatible instructions — modern chips
decode these into simpler micro-ops internally. CISC on the outside, RISC on
the inside.

**RISC** (Reduced Instruction Set Computer): ARM, RISC-V, MIPS. Each instruction
does one simple thing and takes one clock cycle (ideally). The hardware is simpler,
uses less power, and is easier to pipeline. The compiler must be smarter, breaking
complex operations into sequences of simple instructions.

### The Major ISAs Today

| ISA       | Type | Where you find it                                | Notes                      |
|-----------|------|--------------------------------------------------|----------------------------|
| **x86-64**| CISC | Desktops, laptops, servers (Intel, AMD)          | Dominant in PCs since 1978 |
| **ARM**   | RISC | Phones, tablets, Apple Silicon Macs, AWS Graviton| Low power, high efficiency |
| **RISC-V**| RISC | Embedded, research, growing server/desktop use   | Open-source ISA, no royalties|

### Why ARM Won Mobile

Mobile devices run on batteries. Every watt matters. ARM's RISC design — simpler
instructions, simpler hardware — uses dramatically less power than x86 for
comparable performance. Apple's M-series chips proved ARM can compete with (and
beat) x86 even in laptops and desktops, ending the myth that RISC means slow.

---

## 5. Memory Hierarchy

Here is the fundamental problem: CPUs are fast. Memory is slow. And fast memory
is expensive and physically large (more transistors, more heat).

The solution is a **hierarchy** — small amounts of fast, expensive memory close to
the CPU, backed by progressively larger, slower, cheaper storage.

### The Full Picture

```
             Speed                            Size
          ◄──────────                    ──────────►

  ┌──────────────┐
  │  Registers   │  ~0.3 ns    ~1 KB        $$$$$$
  ├──────────────┤
  │   L1 Cache   │  ~1 ns      ~64 KB       $$$$$
  ├──────────────┤
  │   L2 Cache   │  ~5 ns      ~256 KB-1 MB $$$$
  ├──────────────┤
  │   L3 Cache   │  ~10 ns     ~8-64 MB     $$$
  ├──────────────┤
  │     RAM      │  ~100 ns    ~8-128 GB    $$
  ├──────────────┤
  │     SSD      │  ~100 μs    ~1-8 TB      $
  ├──────────────┤
  │     HDD      │  ~10 ms     ~1-20 TB     ¢
  └──────────────┘
```

### Access Time Comparison Table

| Level       | Access Time | Size (typical) | Cost/GB    | Analogy                      |
|-------------|-------------|----------------|------------|------------------------------|
| Register    | ~0.3 ns     | ~1 KB          | —          | A fact you're holding in mind|
| L1 Cache    | ~1 ns       | 32-64 KB       | ~$$$       | A note on your desk          |
| L2 Cache    | ~5 ns       | 256 KB-1 MB    | ~$$        | A file in your desk drawer   |
| L3 Cache    | ~10 ns      | 8-64 MB        | ~$         | A book on your shelf         |
| RAM (DRAM)  | ~100 ns     | 8-128 GB       | ~$5/GB     | A book in the room next door |
| SSD (NVMe)  | ~10-100 μs  | 1-8 TB         | ~$0.10/GB  | A book in the library        |
| HDD         | ~5-10 ms    | 1-20 TB        | ~$0.02/GB  | A book in a warehouse        |

To put the speed differences in human terms: if a register access takes 1 second,
then RAM takes about 5 minutes, SSD takes about 4 days, and HDD takes about a year.

### Caching: The Key Idea

The hierarchy works because of **locality** — programs don't access memory randomly.
They tend to:

- **Temporal locality:** If you accessed an address recently, you'll likely access it
  again soon. (Think: loop variables, frequently called functions.)
- **Spatial locality:** If you accessed an address, you'll likely access nearby
  addresses soon. (Think: iterating through an array, executing sequential instructions.)

Caches exploit this. When the CPU requests data from memory, the cache stores not
just the requested word but an entire **cache line** (typically 64 bytes). Next time
you need a nearby address, it's already in cache.

### Cache Hits and Misses

```
  CPU requests address 0x1000:

  1. Check L1 cache → MISS
  2. Check L2 cache → MISS
  3. Check L3 cache → HIT! (found it)
  4. Copy data to L2 and L1
  5. Return data to CPU

  Next request for address 0x1004:
  1. Check L1 cache → HIT! (same cache line was loaded)
  2. Return data to CPU immediately
```

A **cache hit** means the data was found in cache — fast. A **cache miss** means
we had to go to a slower level — expensive. Hit rate is everything: a 99% L1 hit
rate sounds great until you realize that 1% of misses (going to RAM at 100ns each)
can dominate your program's runtime.

### Why the Hierarchy Exists

In the 1980s, CPU speed and memory speed were roughly matched. Then CPUs started
getting faster at ~60% per year while memory improved at only ~10% per year. This
"memory wall" meant that by the 2000s, a CPU could execute hundreds of instructions
in the time it took to fetch one piece of data from RAM. Caches bridge this gap.

---

## 6. Buses and I/O

The CPU needs to talk to memory, disks, keyboards, displays, and network cards.
This communication happens over **buses** — shared electrical pathways.

### The Three Buses

```
  ┌───────┐                                    ┌─────────┐
  │       │════ Address Bus (CPU → Memory) ═══►│         │
  │  CPU  │════ Data Bus    (bidirectional) ═══│ Memory  │
  │       │════ Control Bus (read/write/etc)═══│         │
  └───────┘                                    └─────────┘
```

| Bus           | Direction      | Purpose                                    |
|---------------|----------------|--------------------------------------------|
| **Address**   | CPU → Memory   | "Which address do I want to read/write?"   |
| **Data**      | Bidirectional  | The actual data being transferred           |
| **Control**   | Bidirectional  | Read/write signals, clock, interrupt lines  |

The **width** of the address bus determines how much memory the CPU can address.
A 32-bit address bus can address 2^32 = 4 GB. A 64-bit address bus can address
2^64 = 16 exabytes (more than enough for now).

### Memory-Mapped I/O

Here is an elegant trick: instead of having special instructions for I/O devices,
just assign each device a range of memory addresses. Writing to address `0xFFFF0000`
might send a byte to the screen. Reading from `0xFFFF1000` might get the latest
keyboard input. The CPU uses the same LOAD/STORE instructions for both memory and
I/O — simple and uniform.

### Interrupts

Without interrupts, the CPU would have to constantly **poll** every device: "Anything
new? No? How about now? No?" This wastes enormous cycles.

Instead, devices send an **interrupt** — an electrical signal on a special pin that
tells the CPU: "Stop what you're doing, I need attention." The CPU then:

1. Saves its current state (PC, registers) onto the stack
2. Looks up the **interrupt handler** — a function the OS registered for this device
3. Jumps to and executes the handler (e.g., reads the keystroke)
4. Restores the saved state and resumes what it was doing

```
  CPU executing program
       │
       ▼
  ─────────────────────►  Keyboard pressed!
       │                     │ (interrupt signal)
       │ ◄───────────────────┘
       │
  Save state → Run keyboard handler → Restore state
       │
       ▼
  Resume program (as if nothing happened)
```

Interrupts are how your computer stays responsive. Without them, every keystroke
would have to wait for the CPU to finish whatever it was doing and get around to
checking the keyboard.

### DMA (Direct Memory Access)

For large data transfers (reading a file from disk, receiving a network packet),
it would be wasteful for the CPU to copy each byte one at a time. Instead, a
**DMA controller** handles bulk transfers directly between devices and memory.
The CPU just says "copy 4 KB from disk to address 0x5000" and goes back to work.
The DMA controller signals an interrupt when it's done.

---

## 7. Pipelining and Parallelism

A single-cycle CPU wastes time. While the ALU is executing, the fetch circuitry
sits idle. While decoding, the ALU sits idle. We can do better.

### Pipelining

Overlap the stages like an assembly line:

```
  Time →    T1      T2      T3      T4      T5      T6      T7
  ─────────────────────────────────────────────────────────────
  Instr 1: Fetch → Decode → Execute → Store
  Instr 2:          Fetch → Decode  → Execute → Store
  Instr 3:                   Fetch  → Decode  → Execute → Store
  Instr 4:                            Fetch   → Decode  → Execute → Store
```

Without pipelining: 4 instructions x 4 stages = 16 time units.
With pipelining: 4 + 3 = 7 time units. As the pipeline fills, we complete one
instruction per clock cycle — a 4x throughput improvement for a 4-stage pipeline.

Modern CPUs have 14-20+ pipeline stages.

### Pipeline Hazards

Pipelining isn't free. Three things can stall it:

**1. Data Hazard:** One instruction needs the result of the previous one.
```
  ADD R1, R2, R3     ; R1 = R2 + R3
  SUB R4, R1, R5     ; R4 = R1 - R5  ← needs R1, but ADD hasn't finished!
```
Solution: **forwarding** (bypass the result directly from ALU output to ALU input
without waiting for the store stage) or **stalling** (insert a bubble/NOP).

**2. Control Hazard:** Branch instructions. The CPU has fetched the next instruction,
but a branch might redirect execution somewhere else entirely.
```
  CMP R1, R2
  BEQ label          ; branch if equal — did we go to 'label' or not?
  ADD R3, R4, R5     ; already fetched, but maybe we shouldn't execute this
```
Solution: **branch prediction.** The CPU *guesses* which way a branch will go and
speculatively executes that path. Modern predictors are >95% accurate. When wrong,
the pipeline flushes — expensive but rare.

**3. Structural Hazard:** Two instructions need the same hardware resource at the
same time. Solution: duplicate the resource or stall.

### Superscalar Execution

Why have one pipeline when you can have several? A **superscalar** CPU has multiple
execution units and can issue 2, 4, or even 8 instructions per clock cycle — as
long as there are no dependencies between them.

```
  Pipeline A:  Fetch → Decode → Execute → Store
  Pipeline B:  Fetch → Decode → Execute → Store
  Pipeline C:  Fetch → Decode → Execute → Store

  3 independent instructions completing per cycle
```

Modern CPUs also do **out-of-order execution**: if instruction 3 is stuck waiting
for data but instruction 4 is independent, execute 4 first. The hardware reorders
instructions for maximum throughput while maintaining the *illusion* of sequential
execution.

### Multi-Core: Going Wide

Around 2005, clock speeds hit a wall. Pushing past ~4 GHz caused too much heat —
power consumption scales with the *cube* of frequency. So chip designers went in
a different direction: put multiple CPUs (cores) on one chip.

```
  ┌────────────────────────────────────────────┐
  │                   CPU CHIP                  │
  │  ┌────────┐  ┌────────┐  ┌────────┐       │
  │  │ Core 0 │  │ Core 1 │  │ Core 2 │ ...   │
  │  │ (ALU,  │  │ (ALU,  │  │ (ALU,  │       │
  │  │  regs, │  │  regs, │  │  regs, │       │
  │  │ L1/L2) │  │ L1/L2) │  │ L1/L2) │       │
  │  └────┬───┘  └────┬───┘  └────┬───┘       │
  │       └──────┬─────┴──────┬────┘           │
  │         ┌────┴────────────┴────┐           │
  │         │     Shared L3 Cache  │           │
  │         └──────────────────────┘           │
  └────────────────────────────────────────────┘
```

Each core runs its own fetch-decode-execute cycle independently. But writing
software that effectively uses multiple cores is hard — this is the challenge of
**concurrent programming**, which we'll revisit in the OS and programming layers.

---

## 8. The GPU

While CPUs evolved to be fast at *sequential* tasks (deep pipelines, branch
prediction, out-of-order execution), another processor took a completely different
path.

### CPU vs. GPU Philosophy

```
  CPU: Few powerful cores              GPU: Thousands of simple cores

  ┌──────────┐                         ┌─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
  │ ████████ │  Core 0 (complex)       │█│█│█│█│█│█│█│█│█│█│█│█│
  │ ████████ │                         │█│█│█│█│█│█│█│█│█│█│█│█│
  ├──────────┤                         │█│█│█│█│█│█│█│█│█│█│█│█│
  │ ████████ │  Core 1 (complex)       │█│█│█│█│█│█│█│█│█│█│█│█│
  │ ████████ │                         │█│█│█│█│█│█│█│█│█│█│█│█│
  ├──────────┤                         │█│█│█│█│█│█│█│█│█│█│█│█│
  │ ████████ │  Core 2 (complex)       │█│█│█│█│█│█│█│█│█│█│█│█│
  │ ████████ │                         │█│█│█│█│█│█│█│█│█│█│█│█│
  └──────────┘                         └─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
   4-16 cores                           Thousands of cores
   Each very powerful                    Each very simple
   Great at serial work                  Great at parallel work
```

### SIMD: Single Instruction, Multiple Data

A GPU core is simple — no branch prediction, no out-of-order execution. But
thousands of them execute the **same instruction** on **different data** simultaneously.
This is called SIMD.

Consider computing the color of every pixel on a 1920x1080 screen. That's ~2 million
pixels. A CPU would loop through them one (or a few) at a time. A GPU processes
thousands simultaneously.

### Why GPUs Are Perfect for AI

Neural network training is dominated by **matrix multiplication** — and matrix
multiplication is embarrassingly parallel. Each element of the output matrix can
be computed independently.

```
  Matrix A (4096 x 4096)  ×  Matrix B (4096 x 4096)

  = 16 million independent multiply-accumulate operations
  = perfect GPU workload
```

This is why NVIDIA GPUs (and their CUDA programming platform) became the backbone
of modern AI. A high-end GPU can perform tens of teraflops (trillions of
floating-point operations per second) of matrix math.

### The GPU Programming Model

Programming a GPU is different from programming a CPU:

1. **Write a kernel** — a function that runs on each data element
2. **Launch thousands of threads** — the GPU scheduler maps them to cores
3. **Manage memory transfers** — data must be copied from CPU memory to GPU memory
   and back

```
  CPU (host)                    GPU (device)
  ──────────                    ───────────
  Prepare data            →     Copy data to GPU memory
  Launch kernel           →     Thousands of threads execute kernel
  Wait for completion     ←     Copy results back to CPU memory
  Process results
```

**CUDA** (NVIDIA) is the dominant GPU programming framework. **OpenCL** is the
open-standard alternative. Higher-level frameworks like PyTorch and TensorFlow
hide most of this complexity behind Python APIs, but underneath, they're launching
CUDA kernels.

---

## Key Mental Models

- **The fetch-decode-execute cycle is the heartbeat.** Every complex behavior of
  every computer reduces to this simple loop running very, very fast. When in doubt
  about how something works, trace it back to this cycle.

- **Architecture is about trade-offs.** Speed vs. power. Complexity vs. simplicity.
  RISC vs. CISC. More cache vs. more cores. There is no free lunch — every design
  choice sacrifices something.

- **The memory hierarchy is a bet on locality.** The entire cache system is a
  gamble that programs access memory in predictable patterns. When they do (and
  they usually do), performance is excellent. When they don't (random access over
  large datasets), performance collapses.

- **Parallelism is the future (and the present).** Single-core speed has plateaued.
  Performance gains now come from using more cores, wider SIMD, and GPU acceleration.
  Software that can't exploit parallelism leaves most of the hardware idle.

---

## How This Connects

**What it gets from gates:**
Everything in this layer is built from the components in Layer 1. The ALU is made
of adders and logic gates. Registers are flip-flops. The control unit is a state
machine. The buses are wires. Caches are SRAM (cross-coupled inverters). We didn't
introduce any magic — just organization.

**What it gives to the layers above:**
The ISA is the interface that operating systems and compilers target. The OS manages
the memory hierarchy (virtual memory), handles interrupts (device drivers), and
schedules work across cores. Compilers translate high-level code into ISA instructions.
Every piece of software ultimately rests on this hardware foundation.

```
  ┌────────────────────────────────────────┐
  │  Applications (browsers, games, AI)    │
  ├────────────────────────────────────────┤
  │  Programming Languages & Compilers     │
  ├────────────────────────────────────────┤
  │  Operating System                      │
  ├────────────────────────────────────────┤
  │  ► Computer Architecture (you are here)│
  ├────────────────────────────────────────┤
  │  Logic Gates & Boolean Algebra         │
  ├────────────────────────────────────────┤
  │  Transistors & Electronics             │
  └────────────────────────────────────────┘
```

---

## Hands-On

1. **Trace the cycle by hand.** Write a tiny program (5-10 instructions in
   pseudo-assembly: LOAD, STORE, ADD, SUB, JUMP). Walk through the fetch-decode-
   execute cycle for each instruction. Track the PC, IR, and all register values.

2. **Build a CPU in Nand2Tetris.** The Hack computer in the Nand2Tetris course has
   you build a complete CPU from gates. It's the single best exercise for
   understanding this material. If you did Layer 1 with Nand2Tetris, you already
   have the components — now connect them.

3. **Measure cache effects.** Write a program that iterates through arrays of
   increasing size. Plot access time vs. array size. You'll see clear jumps when
   the array overflows L1, L2, and L3 — the memory hierarchy made visible.

4. **Explore your CPU.** On Linux: `lscpu`. On macOS: `sysctl -a | grep cpu`.
   Find the number of cores, cache sizes, and clock speed. Relate each number
   to a concept from this document.

5. **Compare ISAs.** Write a simple function in C (e.g., adding two arrays) and
   compile it for x86 and ARM using Compiler Explorer (godbolt.org). Compare the
   generated assembly. Count instructions. Notice the differences in style.

6. **Branch prediction experiment.** Sort an array, then run a loop with an
   `if (array[i] > threshold)` branch. Time it. Then shuffle the array and time
   it again. The sorted version will be faster — the branch predictor can anticipate
   the pattern in sorted data but not in random data.

---

## Go Deeper

- **Patterson & Hennessy — *Computer Organization and Design*** (the "RISC-V
  Edition" is current). The gold-standard textbook. Start here for depth.

- **Hennessy & Patterson — *Computer Architecture: A Quantitative Approach***.
  The advanced sequel. Focuses on performance, pipelining, and memory hierarchy
  with real engineering trade-offs.

- **Ben Eater's YouTube series** — Building an 8-bit breadboard computer from
  scratch. Seeing physical wires carry actual bits between an ALU, registers,
  and RAM makes everything click. (youtube.com/@BenEater)

- **Nand2Tetris (nand2tetris.org)** — Chapters 1-5 take you from NAND gates
  to a working CPU. The best "build it yourself" path through this material.

- **Charles Petzold — *Code: The Hidden Language of Computer Hardware and
  Software*** — A beautifully written journey from telegraph relays to a
  complete computer. Excellent for intuition.

- **Onur Mutlu's lectures (CMU 18-447)** — Available on YouTube. Deep dives
  into memory hierarchy, branch prediction, and out-of-order execution.

---

*Next: [Operating Systems](../02-systems/operating-systems.md) — How does software manage this hardware to run many programs at once?*
