# Programming Languages & Compilers — How Human Thoughts Become Machine Instructions

> The CPU speaks one language: machine code. Humans think in another: natural language.
> Programming languages are the bridge — each one a different trade-off between
> what's easy for humans to say and what's possible for machines to do.

**Prerequisites:** [Operating Systems](operating-systems.md) — You need processes, memory, and system calls.
**What it enables:** Data structures, algorithms, databases, web apps — all software is written in a language.

---

## Why Programming Languages Exist

The CPU executes instructions like:
```
10110000 01100001    (move the value 97 into register AL)
```

This is **machine code**. It's what the hardware understands. It's also completely unreadable by humans. You could write an entire operating system in machine code — people did, in the 1940s — but it's agonizing, error-prone, and impossible to maintain.

Programming languages exist to let humans express computation in terms they can reason about, and then mechanically translate those expressions into machine code.

The history of programming languages is a history of **rising abstraction**:

```
Machine code    →  "raw binary, talks directly to hardware"
    ↓
Assembly        →  "human-readable machine code, 1-to-1 with instructions"
    ↓
C / Fortran     →  "portable, structured, compiles to fast native code"
    ↓
Java / C#       →  "managed memory, runs on a virtual machine"
    ↓
Python / JS     →  "interpreted, dynamic, prioritize developer speed"
    ↓
Rust            →  "safe AND fast — the best of both worlds, with trade-offs"
```

Each step up trades some performance for some human convenience. The art is choosing the right level for the task.

---

## 1. Assembly Language — Human-Readable Machine Code

Assembly replaces binary opcodes with mnemonics:

```asm
; Machine code: 10110000 01100001
; Assembly:
mov al, 97      ; Move the value 97 into register AL

; A simple loop that adds numbers 1 to 10:
    mov ecx, 10    ; counter = 10
    mov eax, 0     ; sum = 0
loop:
    add eax, ecx   ; sum += counter
    dec ecx        ; counter--
    jnz loop       ; if counter != 0, jump to loop
    ; eax now holds 55
```

Assembly is **1-to-1 with machine code** — each assembly instruction maps to exactly one machine instruction. An **assembler** does the translation (it's trivial — just a lookup table).

### Why assembly still matters:
- Understanding how CPUs actually work
- Performance-critical inner loops (rare today)
- Bootloaders and OS kernels (some parts must be assembly)
- Reverse engineering and security research
- Compiler output (understanding what your C/Rust compiles to)

### Why assembly isn't enough:
- Not portable — x86 assembly won't run on ARM
- No variables, functions, types — just registers and addresses
- Extremely tedious for complex programs
- No protection against bugs — you can overwrite anything

---

## 2. Compiled Languages — The Translation Model

A **compiler** reads an entire program in a high-level language and translates it to machine code (or assembly) before execution.

```
Source code (.c)  →  [Compiler]  →  Machine code (binary)  →  [CPU executes]
```

### The Compilation Pipeline

Every compiler does roughly these steps:

```
Source code
    │
    ▼
┌──────────────────┐
│ 1. LEXING         │  Break source into tokens: "int", "x", "=", "5", ";"
│    (tokenization) │
└──────────────────┘
    │
    ▼
┌──────────────────┐
│ 2. PARSING        │  Build an Abstract Syntax Tree (AST)
│                   │  Checks grammar: is "int x = 5;" valid syntax?
└──────────────────┘
    │
    ▼
┌──────────────────┐
│ 3. SEMANTIC       │  Type checking: can you assign 5 to an int? Yes.
│    ANALYSIS       │  Name resolution: is x declared? What scope?
└──────────────────┘
    │
    ▼
┌──────────────────┐
│ 4. OPTIMIZATION   │  Dead code elimination, constant folding,
│    (IR level)     │  loop unrolling, inlining...
└──────────────────┘
    │
    ▼
┌──────────────────┐
│ 5. CODE           │  Generate assembly/machine code for the target
│    GENERATION     │  architecture (x86, ARM, RISC-V...)
└──────────────────┘
    │
    ▼
┌──────────────────┐
│ 6. LINKING        │  Combine multiple object files + libraries
│                   │  into a single executable
└──────────────────┘
    │
    ▼
Executable binary
```

### Example: How `int x = a + b * c;` compiles

**Lexing:** `int` `x` `=` `a` `+` `b` `*` `c` `;`

**Parsing (AST):**
```
    =
   / \
  x   +
     / \
    a   *
       / \
      b   c
```

Note: the tree respects operator precedence — `*` binds tighter than `+`.

**Code generation (x86):**
```asm
mov eax, [b]       ; load b
imul eax, [c]      ; eax = b * c
add eax, [a]       ; eax = a + b*c
mov [x], eax       ; store result in x
```

### Major compiled languages:

| Language | Year | Key idea | Used for |
|----------|------|----------|----------|
| **C** | 1972 | Portable assembly. Manual memory. | OS kernels, embedded, databases |
| **C++** | 1979 | C + objects + templates + RAII | Games, browsers, finance, systems |
| **Rust** | 2010 | Memory safety without garbage collection (ownership) | Systems, WebAssembly, safety-critical |
| **Go** | 2009 | Simple, fast compilation, built-in concurrency | Cloud infrastructure, servers |
| **Swift** | 2014 | Safe, fast, modern syntax | iOS/macOS apps |

---

## 3. Interpreted Languages — Translate on the Fly

An **interpreter** reads and executes code line by line (or statement by statement) at runtime. No separate compilation step.

```
Source code (.py)  →  [Interpreter reads + executes line by line]
```

### Why interpret?
- **Faster development cycle** — no compile step, just run
- **Dynamic features** — eval(), runtime type modification, duck typing
- **Portability** — the interpreter handles platform differences

### Why it's slower:
The interpreter must read, parse, and decide what to do with each statement every time it runs. A compiler does all that work once, ahead of time.

### The hybrid approach: bytecode compilation

Most "interpreted" languages actually compile to **bytecode** first — an intermediate form that's faster to interpret:

```
Python source  →  [Compiler]  →  Bytecode (.pyc)  →  [Virtual Machine interprets bytecode]
Java source    →  [javac]     →  Bytecode (.class) →  [JVM interprets/JIT-compiles bytecode]
```

### Just-In-Time (JIT) compilation

The best of both worlds: start interpreting, but when a piece of code runs many times (a "hot path"), compile it to native machine code on the fly.

```
JavaScript source → [V8 engine interprets] → [hot function detected] → [JIT compiles to native] → [runs at native speed]
```

This is why modern JavaScript (V8/SpiderMonkey) is surprisingly fast — it JIT-compiles hot paths.

### Major interpreted/JIT languages:

| Language | Year | Key idea | Used for |
|----------|------|----------|----------|
| **Python** | 1991 | Readable, batteries included | ML/AI, scripting, web, science |
| **JavaScript** | 1995 | The language of the browser | Web (front + back), serverless |
| **Ruby** | 1995 | Developer happiness, elegant syntax | Web (Rails), scripting |
| **Lua** | 1993 | Tiny, embeddable | Game scripting, embedded systems |
| **PHP** | 1995 | Easy web server-side scripting | Web (WordPress, ~77% of websites) |

---

## 4. Type Systems — The Safety/Flexibility Trade-Off

A **type** is a category that tells the compiler/interpreter what operations are valid on a value.

### Static vs Dynamic typing:

| | Static typing | Dynamic typing |
|---|---|---|
| **When checked** | At compile time | At runtime |
| **Declaration** | Must declare types (or infer them) | No type declarations |
| **Errors** | Caught before running | Caught when that line executes |
| **Examples** | C, Java, Rust, Go, TypeScript | Python, JavaScript, Ruby |
| **Trade-off** | More upfront work, fewer runtime surprises | Faster to write, more runtime errors |

```python
# Python (dynamic): this runs fine until the bad line executes
x = "hello"
y = x + 5      # TypeError at runtime! Can't add string and int.
```

```java
// Java (static): this won't compile at all
String x = "hello";
int y = x + 5;    // Compile error! Type mismatch.
```

### Strong vs Weak typing:

**Strong** (Python, Rust): The language prevents implicit type coercions. `"5" + 3` is an error.
**Weak** (C, JavaScript): The language silently converts types. `"5" + 3` might give `"53"` (JS) or something undefined (C).

### Type inference:

Modern static languages can often figure out types without you declaring them:
```rust
let x = 42;           // Rust infers: x is i32
let y = "hello";      // Rust infers: y is &str
let z = x + 1;        // Rust infers: z is i32
```

You get static safety with dynamic-feeling ergonomics.

---

## 5. Memory Management — The Biggest Decision

Every program needs memory. The question is: who manages it?

### Manual memory management (C, C++)

You allocate and free memory explicitly:
```c
int* arr = malloc(100 * sizeof(int));   // allocate
// ... use arr ...
free(arr);                                // free — YOUR responsibility
```

**Dangers:**
- **Memory leak:** Forget to free → memory grows forever
- **Use after free:** Access memory after freeing → undefined behavior, security vulnerabilities
- **Double free:** Free the same memory twice → corruption
- **Buffer overflow:** Write past the end of an array → corruption, exploits

Manual memory is fast (no overhead) but dangerous. Most security vulnerabilities in history come from memory bugs in C/C++.

### Garbage collection (Java, Python, Go, JavaScript)

A runtime system (the **garbage collector** or GC) automatically tracks which memory is still in use and frees the rest.

```java
Object obj = new Object();  // allocate
// ... use obj ...
// When obj is no longer reachable, the GC frees it automatically
```

**How GC works (simplified):**
1. Start from "roots" (global variables, stack variables)
2. Trace every object reachable from roots (mark)
3. Free everything not marked (sweep)

**Trade-off:** No manual memory bugs, but GC pauses can cause latency spikes. The GC also uses extra memory and CPU.

### Ownership (Rust)

Rust's innovation: **compile-time memory management with no garbage collector.**

Rules:
1. Every value has exactly one **owner**
2. When the owner goes out of scope, the value is **dropped** (freed)
3. You can have either ONE mutable reference OR any number of immutable references — never both

```rust
fn main() {
    let s1 = String::from("hello");   // s1 owns the string
    let s2 = s1;                       // ownership MOVES to s2. s1 is now invalid.
    // println!("{}", s1);             // COMPILE ERROR: s1 no longer owns the data
    println!("{}", s2);                // OK
}   // s2 goes out of scope, string is freed. No GC needed.
```

The compiler enforces all of this at compile time. If it compiles, it's memory-safe. No runtime cost.

---

## 6. Programming Paradigms — Ways of Thinking

A **paradigm** is a style of organizing code. Most languages support multiple paradigms.

### Imperative — Tell the machine what to do, step by step

```python
total = 0
for num in numbers:
    if num > 0:
        total += num
```

"Set total to 0. For each number, if it's positive, add it to total."

### Functional — Describe what the result is, using functions

```python
total = sum(filter(lambda n: n > 0, numbers))
```

"The total is the sum of the positive numbers." No mutable state, no step-by-step.

Key ideas: pure functions (no side effects), immutability, higher-order functions (functions that take/return functions), recursion over loops.

### Object-Oriented — Model the world as interacting objects

```python
class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

Key ideas: encapsulation (hide internal state), inheritance (share behavior), polymorphism (same interface, different behavior).

### Declarative — Describe what you want, not how to get it

```sql
SELECT name FROM users WHERE age > 21;
```

"Give me names of users over 21." You don't say how to scan the table or use an index — the database figures that out.

HTML, CSS, SQL, and configuration languages are all declarative.

### The reality:

Most modern programming is **multi-paradigm.** Python mixes imperative + OOP + functional. Rust is imperative + functional. JavaScript is everything at once.

---

## 7. The Linker and the Loader

Two often-overlooked pieces of the puzzle:

### The Linker

Programs use code from multiple files and libraries. The **linker** combines them:

```
main.o  ─┐
utils.o  ─┼──► [Linker] ──► executable
libc.a  ─┘
```

**Static linking:** Library code is copied into your executable. Bigger binary, but self-contained.
**Dynamic linking:** Your executable references a shared library (.so / .dll / .dylib). Smaller binary, but needs the library at runtime.

### The Loader

When you run a program, the OS **loader**:
1. Reads the executable format (ELF on Linux, Mach-O on macOS, PE on Windows)
2. Maps code and data into virtual memory
3. Loads dynamic libraries
4. Resolves symbol addresses
5. Jumps to the entry point (usually `_start`, which calls `main()`)

---

## 8. The Runtime

Some languages come with a **runtime** — code that runs alongside your program:

| Language | Runtime includes |
|----------|-----------------|
| **C** | Almost nothing — just startup/shutdown code |
| **Go** | Goroutine scheduler, garbage collector, network poller |
| **Java** | JVM: bytecode interpreter, JIT compiler, GC, class loader |
| **Python** | CPython interpreter, GC, import system, type system |
| **Rust** | Minimal — no GC, no runtime scheduler |

A heavier runtime = more features out of the box, but more overhead and less control.

---

## Key Mental Models

1. **Every language is a trade-off:** There is no "best" language. C gives you control but demands discipline. Python gives you speed-of-development but takes runtime performance. Rust gives you safety + speed but demands a learning investment. Choose based on what you're building.

2. **Compilation is just translation:** A compiler is a translator from one language to another. There's nothing magic about it — it's pattern matching, tree manipulation, and code generation. You could write one.

3. **The abstraction tax:** Every layer of abstraction (high-level language, GC, virtual machine) costs something at runtime. Usually it's worth it. Sometimes it isn't. Knowing what's underneath lets you make informed choices.

4. **Types are the first line of defense:** A good type system catches bugs before your code runs. The stronger the type system, the more bugs are impossible. Rust's ownership model makes entire categories of memory bugs unrepresentable.

---

## How This Connects

**From below (Layer 3 — Operating Systems):**
- System calls are the primitives that languages wrap (open, read, write, fork)
- Virtual memory gives every program its own clean address space
- The loader (part of the OS) starts your compiled programs
- The process model determines how concurrent programs behave

**To above (Layer 5 — Data Structures & Algorithms):**
- Languages provide the notation for expressing algorithms
- Memory management determines which data structures are practical
- Type systems constrain and enable data structure designs
- The choice of language affects constant factors in algorithm performance

---

## Hands-On

1. **Read assembly output:** Write a simple C function, compile with `gcc -S -O2 function.c`, and read the generated assembly. See how the compiler translates your code.

2. **Build a tiny interpreter:** Write a calculator that parses and evaluates `"3 + 4 * 2"`. You'll implement lexing, parsing, and evaluation — the core of any language.

3. **Compare memory models:** Write the same linked list in C (malloc/free), Python (automatic GC), and Rust (ownership). See how each language handles memory differently.

4. **Explore bytecode:** In Python, run `import dis; dis.dis(your_function)` to see the bytecode your function compiles to. In Java, use `javap -c ClassName`.

5. **Type system exploration:** Try to write the same "wrong" code in Python, JavaScript, Java, and Rust. See which languages catch which errors, and when (compile time vs runtime).

---

## Go Deeper

- **"Crafting Interpreters"** by Robert Nystrom — Build a complete language from scratch (free online)
- **"The Rust Programming Language"** (The Rust Book) — Best introduction to ownership and modern systems programming
- **"Structure and Interpretation of Computer Programs" (SICP)** — The classic on computation and language design
- **"Compilers: Principles, Techniques, and Tools" (The Dragon Book)** — The definitive compiler textbook
- **Nand2Tetris Part 2** — Build a compiler for a high-level language

---

*Next: [Data Structures & Algorithms](../03-theory/data-structures-and-algorithms.md) — How do we organize information and solve problems efficiently?*
