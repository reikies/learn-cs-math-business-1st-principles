# Operating Systems — The Software That Manages Hardware

> A computer without an operating system is like a brain without a body —
> it can think, but it can't coordinate. The OS is the layer that turns
> raw hardware into something programs can actually use.

**Prerequisites:** [Computer Architecture](../01-hardware/computer-architecture.md) — You need to understand CPU, memory, and I/O.
**What it enables:** Programming languages, applications, everything software.

---

## Why Operating Systems Exist

Imagine a world without an OS. You have a CPU, some RAM, a disk, a keyboard, and a screen. To run a program, you'd need to:
- Know the exact memory addresses to load your code into
- Manually manage every byte of RAM
- Write your own keyboard driver, screen driver, disk driver
- Only run one program at a time
- Restart the machine to switch programs

The OS solves all of this. It provides **abstractions** that make the hardware usable and **sharing** so multiple programs can run simultaneously.

The OS is the first piece of software that runs when you turn on a computer, and the last piece running when you turn it off. Everything else runs on top of it.

---

## 1. The Kernel — The Core of the OS

The **kernel** is the part of the OS that runs in **privileged mode** (also called kernel mode or ring 0). It has direct access to hardware. Everything else runs in **user mode** with restricted access.

```
┌─────────────────────────────────────────┐
│           User Applications             │
│  (browser, editor, games, your code)    │
├─────────────────────────────────────────┤
│           System Libraries              │
│      (libc, Win32 API, Cocoa)           │
├─────────────────────────────────────────┤
│        ═══ System Call Interface ═══     │  ← The boundary
├─────────────────────────────────────────┤
│              The Kernel                 │
│  ┌──────────┬───────────┬───────────┐   │
│  │ Process  │  Memory   │   File    │   │
│  │ Manager  │  Manager  │  System   │   │
│  ├──────────┼───────────┼───────────┤   │
│  │ Device Drivers │ Network Stack   │   │
│  └──────────┴───────────┴───────────┘   │
├─────────────────────────────────────────┤
│              Hardware                   │
│     (CPU, RAM, Disk, Network, GPU)      │
└─────────────────────────────────────────┘
```

### Kernel architectures:

| Type | What it does | Examples | Trade-off |
|------|-------------|----------|-----------|
| **Monolithic** | Everything in one big kernel binary | Linux, older Windows | Fast (no boundary crossings) but one bug can crash everything |
| **Microkernel** | Minimal kernel; drivers run in user space | MINIX, QNX, seL4 | Safer but slower (lots of message passing) |
| **Hybrid** | Mix of both | Windows NT, macOS (XNU) | Pragmatic compromise |

Linux is monolithic but loads **modules** dynamically — you get some flexibility of a microkernel with the speed of a monolith.

---

## 2. Processes and Threads — The Illusion of Parallelism

### What is a process?

A **process** is a running program. It has:
- Its own **address space** (virtual memory — it thinks it has all the RAM)
- A **program counter** (where it is in execution)
- **Open files** and network connections
- A **process ID** (PID)
- A **state**: running, ready, waiting, terminated

When you launch Chrome, your terminal, and Spotify — each is a process.

### What is a thread?

A **thread** is a lightweight unit of execution *within* a process. Multiple threads share the same address space but each has its own:
- Stack (local variables)
- Program counter
- Register state

```
Process (Chrome)
├── Thread 1: UI rendering
├── Thread 2: Network requests
├── Thread 3: JavaScript execution
└── Thread 4: Audio playback
```

### Why threads?

Threads are cheaper to create and switch between than processes (no need to set up a new address space). They share memory, so they can communicate easily. But sharing memory is also dangerous — **race conditions** happen when two threads modify the same data simultaneously.

### Context Switching

The CPU can only run one thread per core at a time. The OS **context switches** between threads rapidly (thousands of times per second), saving and restoring register state each time. This creates the illusion that everything runs simultaneously, even on a single-core machine.

```
Time →
Core: [Thread A][Thread B][Thread A][Thread C][Thread B][Thread A]...
         ↑          ↑          ↑
      context    context    context
      switch     switch     switch
```

Each context switch costs ~1-10 microseconds. It's fast, but not free.

---

## 3. Process Scheduling — Who Gets the CPU?

With many processes wanting the CPU, the **scheduler** decides who runs next. This is one of the most studied problems in OS design.

### Key scheduling algorithms:

| Algorithm | How it works | Pros | Cons |
|-----------|-------------|------|------|
| **FIFO / FCFS** | First come, first served | Simple | Long jobs block short ones |
| **Round Robin** | Each process gets a fixed time slice (quantum), then yields | Fair | Lots of context switches |
| **Priority** | Higher-priority processes go first | Important things run first | Low-priority processes can starve |
| **Multi-Level Feedback Queue** | Multiple priority queues; processes move between them based on behavior | Adaptive, rewards short bursts | Complex to tune |
| **Completely Fair Scheduler (CFS)** | Linux's scheduler; uses a red-black tree to give each process its fair share of CPU time | Fair and efficient | Sophisticated implementation |

### The key trade-offs:
- **Throughput** vs **latency**: Batch systems maximize throughput; interactive systems minimize response time
- **Fairness** vs **priority**: Should every process get equal time, or should some be privileged?
- **CPU-bound** vs **I/O-bound**: Processes that do lots of I/O should be prioritized so they can issue their next I/O request quickly

---

## 4. Virtual Memory — The Illusion of Infinite RAM

### The problem

Your computer has, say, 16 GB of RAM. But you might be running programs that collectively want 40 GB. And each program wants to think it has memory starting at address 0, laid out contiguously.

### The solution: virtual memory

Every process gets its own **virtual address space**. The OS and hardware collaborate to map virtual addresses to physical addresses.

```
Process A sees:              Physical RAM:          Process B sees:
┌──────────────┐             ┌──────────────┐       ┌──────────────┐
│ 0x0000: code │ ──────────► │ 0x5000: A's  │       │ 0x0000: code │
│ 0x1000: data │ ──────┐     │     code     │  ┌──► │ 0x1000: data │
│ 0x2000: heap │       │     │ 0x6000: B's  │  │    │ 0x2000: heap │
│ 0x3000: stack│       └───► │     data     │  │    │ 0x3000: stack│
└──────────────┘             │ 0x7000: A's  │  │    └──────────────┘
                             │     data     │──┘
                             │ 0x8000: ...  │
                             └──────────────┘
```

Both processes think they start at address 0x0000, but the hardware (MMU — Memory Management Unit) translates their virtual addresses to different physical locations.

### Pages and page tables

Memory is divided into fixed-size **pages** (typically 4 KB). The OS maintains a **page table** for each process that maps virtual page numbers to physical frame numbers.

```
Virtual Page 0  →  Physical Frame 42
Virtual Page 1  →  Physical Frame 7
Virtual Page 2  →  [on disk — not in RAM right now]
Virtual Page 3  →  Physical Frame 128
```

### Page faults

When a program accesses a page that isn't in RAM (it's been **swapped** to disk), a **page fault** occurs:
1. CPU traps to the OS
2. OS finds the page on disk
3. OS loads it into a free frame in RAM (possibly evicting another page)
4. OS updates the page table
5. CPU retries the instruction

This is why running out of RAM makes your computer slow — it's constantly swapping pages to and from disk.

### The Translation Lookaside Buffer (TLB)

Looking up the page table for every memory access would be too slow. The **TLB** is a small, fast cache of recent virtual-to-physical translations. TLB hit = fast. TLB miss = walk the page table.

---

## 5. File Systems — Turning Disks Into Folders

### The problem

A disk is a flat array of bytes (or blocks). That's it. No files, no folders, no names.

### The solution: file systems

A file system imposes structure on raw blocks:
- **Files**: named sequences of bytes
- **Directories**: containers that map names to files
- **Metadata**: permissions, timestamps, size, owner

### How it works internally

Most file systems use a structure like this:

```
Disk layout:
┌──────────┬──────────┬──────────┬─────────────────────────┐
│ Boot     │ Super-   │ Inode    │ Data Blocks             │
│ Block    │ block    │ Table    │ (actual file contents)  │
└──────────┴──────────┴──────────┴─────────────────────────┘
```

An **inode** (index node) stores metadata about a file:
- File size
- Permissions (read/write/execute for owner/group/others)
- Timestamps (created, modified, accessed)
- Pointers to data blocks on disk

A **directory** is just a special file that maps names → inode numbers.

```
/home/sky/notes.txt
    │    │     │
    │    │     └── directory entry: "notes.txt" → inode 4827
    │    └── directory entry: "sky" → inode 1205
    └── directory entry: "home" → inode 300
```

### Common file systems:

| File System | Used by | Key feature |
|------------|---------|-------------|
| **ext4** | Linux | Journaling, mature, reliable |
| **APFS** | macOS/iOS | Copy-on-write, encryption, snapshots |
| **NTFS** | Windows | ACLs, journaling, compression |
| **FAT32** | USB drives | Simple, universal compatibility |
| **ZFS** | Servers | Checksums, snapshots, pooled storage |
| **Btrfs** | Linux (newer) | Copy-on-write, snapshots, RAID |

### Journaling

The critical problem: what if power dies mid-write? The file system could be left in an inconsistent state (data written but metadata not updated, or vice versa).

**Journaling** solves this by writing changes to a log (journal) first. On crash recovery, replay the journal to reach a consistent state.

---

## 6. System Calls — The API Between Userland and Kernel

Programs can't touch hardware directly (that would be chaos). Instead, they make **system calls** — formal requests to the kernel.

```
Your program          Kernel
    │                    │
    │  open("file.txt")  │
    │ ─────────────────► │  ← Switches to kernel mode
    │                    │     Validates request
    │                    │     Finds file on disk
    │  returns fd=3      │     Opens file
    │ ◄───────────────── │  ← Switches back to user mode
    │                    │
    │  read(fd=3, buf)   │
    │ ─────────────────► │
    │  returns data      │
    │ ◄───────────────── │
```

### Key system calls (POSIX):

| Category | System calls | What they do |
|----------|-------------|-------------|
| **Files** | open, read, write, close, stat | Access the file system |
| **Processes** | fork, exec, wait, exit, kill | Create and manage processes |
| **Memory** | mmap, brk, munmap | Allocate and manage memory |
| **Network** | socket, bind, listen, accept, connect, send, recv | Network communication |
| **Signals** | signal, sigaction, kill | Asynchronous notifications |
| **I/O** | ioctl, select, poll, epoll | Device control and I/O multiplexing |

**fork()** is the most important Unix system call: it creates a copy of the current process. The original is the parent, the copy is the child. Combined with **exec()** (which replaces the current process with a new program), this is how every program on Unix/Linux/macOS is launched.

```
Shell (PID 100)
    │
    ├── fork() → Child (PID 101)
    │               │
    │               └── exec("firefox") → Firefox (PID 101)
    │
    └── wait() → [waits for child to finish]
```

---

## 7. Concurrency and Synchronization

When multiple threads access shared data, bad things happen:

### The race condition

```
Thread A:                Thread B:
  read balance (100)       read balance (100)
  add 50                   subtract 30
  write balance (150)      write balance (70)    ← WRONG! Should be 120
```

Both threads read the old value before either writes. The result depends on who writes last.

### Solutions:

**Mutex (mutual exclusion):** A lock that only one thread can hold. Others wait.
```
Thread A: lock(mutex) → read/modify/write → unlock(mutex)
Thread B: lock(mutex) → [BLOCKED until A unlocks] → read/modify/write → unlock(mutex)
```

**Semaphore:** A generalized lock with a counter. Allows N threads through simultaneously.

**Condition variables:** Let threads wait for a specific condition to be true.

**Deadlock:** Two threads each hold a lock the other needs. Both wait forever.
```
Thread A: holds Lock1, waiting for Lock2
Thread B: holds Lock2, waiting for Lock1
→ Neither can proceed
```

Deadlock requires four conditions simultaneously: mutual exclusion, hold and wait, no preemption, circular wait. Break any one and deadlock is impossible.

---

## 8. Inter-Process Communication (IPC)

Processes are isolated — they can't read each other's memory. So how do they communicate?

| Mechanism | How it works | When to use |
|-----------|-------------|-------------|
| **Pipes** | One-way byte stream between processes | Simple parent-child communication |
| **Shared memory** | Map the same physical memory into two processes | High-speed data sharing |
| **Message queues** | Kernel-managed queue of messages | Structured communication |
| **Sockets** | Network-style communication, even locally | General purpose, works across machines |
| **Signals** | Asynchronous notifications (SIGINT, SIGKILL) | Simple events (like Ctrl+C) |

When you type `ls | grep ".txt"` in a terminal, the shell creates a **pipe** connecting ls's stdout to grep's stdin. Two processes, communicating through a byte stream.

---

## 9. Security and Protection

The OS is the enforcer of boundaries:

**User mode vs kernel mode:** Hardware enforces that user programs can't execute privileged instructions (like writing to disk directly).

**File permissions (Unix model):**
```
-rwxr-xr--  1  sky  staff  4096  Jan 1 12:00  script.sh
│├──┤├──┤├──┤
│ │    │   │
│ │    │   └── Others: read only
│ │    └── Group: read + execute
│ └── Owner: read + write + execute
└── File type (- = regular file, d = directory)
```

**Process isolation:** Each process has its own virtual address space. Process A cannot read Process B's memory. The hardware (MMU) enforces this.

**The principle of least privilege:** Every program should run with the minimum permissions it needs. This is why you don't run everything as root/admin.

---

## 10. The Boot Process — How It All Starts

When you press the power button:

```
1. Hardware powers on
    │
2. BIOS/UEFI runs from firmware
    │    (initializes hardware, finds boot device)
    │
3. Bootloader loads (GRUB, Windows Boot Manager)
    │    (finds and loads the kernel)
    │
4. Kernel initializes
    │    (sets up memory management, drivers, interrupts)
    │
5. Kernel starts init/systemd (PID 1)
    │    (the first user-space process)
    │
6. Init starts system services
    │    (networking, display manager, login)
    │
7. Login screen appears
    │
8. You log in → shell or desktop environment
```

Every process on the system is a descendant of PID 1.

---

## 11. The Major Operating Systems

| OS | Kernel | Used for | Key insight |
|----|--------|----------|-------------|
| **Linux** | Linux (monolithic) | Servers, Android, embedded, cloud | Open source; runs most of the internet |
| **macOS** | XNU (hybrid Mach + BSD) | Apple desktops/laptops | Unix under the hood with Apple's UI |
| **Windows** | Windows NT (hybrid) | Desktops, enterprise, gaming | Backwards compatibility is sacred |
| **iOS** | XNU (same as macOS) | iPhones, iPads | Sandboxed apps, tight hardware integration |
| **Android** | Linux (modified) | Most smartphones | Linux kernel + Google's framework |
| **FreeBSD** | BSD (monolithic) | Netflix CDN, PlayStation | Clean codebase, permissive license |

Linux dominates servers (~96% of top 1 million websites). Windows dominates desktops (~72%). Android dominates mobile (~71%).

---

## Key Mental Models

1. **The OS is a resource manager:** CPU time, memory, disk space, network access — the OS divides finite resources among competing programs. Every OS feature is ultimately about resource management.

2. **Abstraction hides complexity:** Programs see "files" not "disk blocks," see "virtual memory" not "physical RAM," see "processes" not "context switches." Each abstraction makes programming possible at scale.

3. **Everything is a trade-off:** Monolithic vs microkernel. Throughput vs latency. Safety vs performance. The "best" OS design depends on what you're optimizing for.

4. **Isolation is the foundation of security:** Processes can't touch each other. User mode can't touch kernel mode. Users can't touch each other's files. Every boundary is enforced by hardware and the OS together.

---

## How This Connects

**From below (Layer 2 — Architecture):**
- The OS depends on hardware features: privilege levels (user/kernel mode), MMU (virtual memory), interrupts (I/O), timers (scheduling)
- The ISA defines what instructions the kernel can use
- The memory hierarchy determines how the OS manages caching and swapping

**To above (Layer 4 — Languages):**
- System calls are the API that programming languages use to interact with hardware
- The process model determines how programs are launched and managed
- Virtual memory gives every program its own clean address space to work in
- File systems provide persistent storage for source code, data, and executables

---

## Hands-On

1. **Explore processes:** Open a terminal. Run `ps aux` (Linux/Mac) to see all running processes. Run `top` or `htop` to see CPU and memory usage in real time. Identify which processes are using the most resources.

2. **Trace system calls:** Run `strace ls` (Linux) or `dtruss ls` (Mac) to see every system call a program makes. You'll see open(), read(), write(), close() — the OS API in action.

3. **Fork a process:** Write a C program that calls fork(). Print "parent" in the parent and "child" in the child. See how one process becomes two.

4. **Explore the file system:** Run `stat filename` to see an inode. Run `df -h` to see mounted file systems. Run `mount` to see what's mounted where.

5. **Process tree:** Run `pstree` (Linux) to see the entire process hierarchy from PID 1.

---

## Go Deeper

- **"Operating Systems: Three Easy Pieces" (OSTEP)** — Free online, the best OS textbook. Covers everything here and more.
- **"The Linux Programming Interface"** by Michael Kerrisk — The definitive reference for Linux system calls
- **xv6** — A teaching OS from MIT. Small enough to read the entire source code.
- **Linux From Scratch** — Build an entire Linux system from source. You'll understand everything.
- **Ben Eater's OS videos** — Visual explanations of boot process, interrupts, etc.

---

*Next: [Programming Languages & Compilers](programming-languages.md) — How do human thoughts become machine instructions?*
