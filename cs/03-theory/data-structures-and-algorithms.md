# Data Structures & Algorithms — Organizing Information, Solving Problems

> A computer can execute billions of operations per second. And yet, choose the wrong algorithm and
> it will sit there, churning, until the heat death of the universe. Data structures and algorithms
> are the art of organizing information so that the right answer emerges *fast* — not through raw
> power, but through clever structure. This is where computer science earns the word "science."

**Prerequisites:** [Programming Languages](../02-systems/programming-languages.md) — You need to be able to express code.

**What it enables:** Databases, networking, compilers, AI — every system is built on data structures and algorithms.

---

## 1. Why This Matters

The difference between a good algorithm and a bad one is not a matter of taste. It is the
difference between milliseconds and centuries. Between "runs on a phone" and "can't run on a
supercomputer."

Consider: you have a list of one million names. You need to find one.

| Approach         | Operations     | Time (at 1B ops/sec) |
|------------------|----------------|----------------------|
| Check every name | 1,000,000      | 1 millisecond        |
| Binary search    | 20             | 0.00002 milliseconds |

That is a 50,000x difference — and it only gets worse as data grows. At one billion names,
linear search takes a full second. Binary search takes 30 steps. Thirty.

Efficiency is not premature optimization. It is the difference between *possible* and *impossible*.

Every programmer encounters this eventually: the code is correct, it passes the tests, but it
doesn't finish in time. The fix is almost never "buy a faster computer." The fix is a better
algorithm. That is what this document is about.

---

## 2. Big-O Notation — Measuring Efficiency

Big-O notation answers a single question: **as the input grows, how does the running time grow?**

We don't count exact operations. We care about the *shape* of the growth curve.

### Common Complexity Classes

| Big-O       | Name             | n=10    | n=1,000   | n=1,000,000      | Example                     |
|-------------|------------------|---------|-----------|-------------------|-----------------------------|
| O(1)        | Constant         | 1       | 1         | 1                 | Array index lookup          |
| O(log n)    | Logarithmic      | 3       | 10        | 20                | Binary search               |
| O(n)        | Linear           | 10      | 1,000     | 1,000,000         | Scanning a list             |
| O(n log n)  | Linearithmic     | 33      | 10,000    | 20,000,000        | Merge sort                  |
| O(n^2)      | Quadratic        | 100     | 1,000,000 | 1,000,000,000,000 | Nested loop (bubble sort)   |
| O(2^n)      | Exponential      | 1,024   | 2^1000    | forget it         | Brute-force subsets         |
| O(n!)       | Factorial        | 3.6M    | heat death| heat death        | Brute-force permutations    |

### Growth Rate Visualization

```
ops
 |
 |                                                    * O(n^2)
 |                                               *
 |                                          *
 |                                     *
 |                                *
 |                           *               . O(n log n)
 |                      *           .   .
 |                 *        .  .
 |            *      . .                     --- O(n)
 |       *   .  . -------------------------
 |    * . . ---------
 |  *.---------                              ___ O(log n)
 | .------   ___  ___  ___  ___  ___  ___
 |--- ___                                    === O(1)
 |===================================================
 +----------------------------------------------------> n
```

### Best, Average, and Worst Case

An algorithm doesn't always take the same time. Quicksort is O(n log n) on average but O(n^2) in
the worst case. When we say "O(n log n)" without qualification, we usually mean the worst case or
the average case — context matters.

- **Best case:** Often not useful (any algorithm is fast on trivial input)
- **Average case:** What happens on "typical" input — usually what we care about
- **Worst case:** The guarantee — it will *never* be worse than this

### Space Complexity

Time isn't the only resource. Algorithms also use memory.

- Merge sort: O(n) extra space (needs a temporary array)
- Quicksort: O(log n) extra space (just the call stack)
- In-place algorithms: O(1) extra space — they rearrange the existing data

The tradeoff between time and space is constant. Often you can trade memory for speed
(caching, hash tables) or speed for memory (recomputing instead of storing).

---

## 3. Arrays and Linked Lists — The Two Primitive Strategies

Every data structure is built from two primitives: **contiguous memory** (arrays) and
**linked nodes** (pointers).

### Arrays

Memory laid out in a row. Elements are side-by-side.

```
Index:    0     1     2     3     4
        +-----+-----+-----+-----+-----+
Value:  | 10  | 23  |  7  | 42  | 15  |
        +-----+-----+-----+-----+-----+
Address: 100   104   108   112   116
```

- **Access by index:** O(1) — address = base + (index * element_size)
- **Search (unsorted):** O(n) — must scan
- **Insert/delete at end:** O(1)
- **Insert/delete in middle:** O(n) — must shift elements

### Linked Lists

Each node holds a value and a pointer to the next node.

```
+------+---+    +------+---+    +------+---+    +------+------+
|  10  | *-+--->|  23  | *-+--->|   7  | *-+--->|  42  | null |
+------+---+    +------+---+    +------+---+    +------+------+
```

- **Access by index:** O(n) — must walk from the head
- **Search:** O(n) — must walk
- **Insert/delete at known position:** O(1) — just rewire pointers
- **Insert/delete at head:** O(1)

### When to Use Which

| Need                          | Array | Linked List |
|-------------------------------|-------|-------------|
| Random access by index        | Yes   | No          |
| Frequent insert/delete middle | No    | Yes         |
| Memory efficient              | Yes   | No (pointer overhead) |
| Cache friendly                | Yes   | No (scattered memory) |

In practice, arrays win *most* of the time. Modern CPUs love sequential memory access (cache
lines). Linked lists are theoretically elegant but often slower in practice due to cache misses.

### Dynamic Arrays

What if you don't know the size in advance? Dynamic arrays (Java's `ArrayList`, C++'s `vector`,
Python's `list`) solve this:

1. Allocate an array of some initial capacity (say 8)
2. When it fills up, allocate a new array of **double** the capacity
3. Copy everything over

This sounds expensive, but the doubling strategy means the *amortized* cost of each append is
O(1). You pay O(n) rarely, and each time you pay it, you buy enough room that the next n
operations are free.

```
Append: [1]         capacity 2
Append: [1,2]       capacity 2, full
Append: [1,2,3]     capacity 4 (doubled, copied)
Append: [1,2,3,4]   capacity 4, full
Append: [1,2,3,4,5] capacity 8 (doubled, copied)
...
```

---

## 4. Stacks and Queues — Controlled Access

Sometimes you don't want arbitrary access. You want a *discipline* for adding and removing.

### Stack (LIFO — Last In, First Out)

Think of a stack of plates. You add to the top, you remove from the top.

```
        +-----+
  push  |  C  |  <-- top (pop from here)
        +-----+
        |  B  |
        +-----+
        |  A  |
        +-----+
```

Operations — all O(1):
- `push(x)` — add to top
- `pop()` — remove from top
- `peek()` — look at top without removing

**Where stacks appear everywhere:**
- **Function call stack:** Every function call pushes a frame; returning pops it
- **Undo/Redo:** Each action is pushed; undo pops the last one
- **Expression parsing:** Matching parentheses, evaluating postfix notation
- **DFS traversal:** Explicitly or via recursion (which uses the call stack)

### Queue (FIFO — First In, First Out)

Think of a line at a store. First person in line is first to be served.

```
  enqueue -->  +---+---+---+---+  --> dequeue
               | D | C | B | A |
               +---+---+---+---+
              back              front
```

Operations — all O(1) with proper implementation:
- `enqueue(x)` — add to back
- `dequeue()` — remove from front
- `peek()` — look at front

**Where queues appear:**
- **BFS traversal:** Explore level by level
- **Task scheduling:** Process requests in order
- **Message buffers:** Producer-consumer patterns
- **Print queues, CPU scheduling, network packet handling**

### Deque (Double-Ended Queue)

Insert and remove from *both* ends in O(1). Generalizes both stack and queue. Python's
`collections.deque`, Java's `ArrayDeque`.

### Priority Queue / Heap

A queue where elements come out in *priority order*, not insertion order. Implemented with a
heap (covered in the Trees section). O(log n) insert, O(log n) extract-min/max, O(1) peek.

Use cases: Dijkstra's algorithm, event-driven simulation, task scheduling with priorities,
finding the k largest/smallest elements.

---

## 5. Hash Tables — The Most Important Data Structure

If you could learn only one data structure, learn this one. Hash tables power dictionaries,
sets, caches, databases, symbol tables in compilers — they are *everywhere*.

### The Core Idea

A hash function converts a key into an array index.

```
  key: "alice"  --->  hash("alice") = 7392  --->  7392 % 10 = 2
  key: "bob"    --->  hash("bob")   = 4518  --->  4518 % 10 = 8

  Index:  0    1    2         3    4    5    6    7    8        9
        +----+----+---------+----+----+----+----+----+--------+----+
        |    |    | "alice" |    |    |    |    |    | "bob"  |    |
        +----+----+---------+----+----+----+----+----+--------+----+
```

**Average case:** O(1) lookup, insert, delete. Constant time regardless of size.

### Collisions

What if two keys hash to the same index? This is a *collision*. Two strategies:

**Chaining:** Each slot holds a linked list of entries.

```
  Index 2: ["alice" -> 100] -> ["charlie" -> 300] -> null
  Index 8: ["bob" -> 200] -> null
```

**Open addressing:** If the slot is taken, probe for the next empty slot (linear probing,
quadratic probing, double hashing).

```
  hash("charlie") = 2, but slot 2 is taken
  Try slot 3... empty! Store here.
```

### Load Factor and Resizing

The **load factor** = (number of entries) / (table size). As it grows, collisions increase.

Most implementations resize (double the table, rehash everything) when the load factor exceeds
a threshold — typically 0.75. This keeps operations close to O(1).

### Pseudocode: Simple Hash Table

```
class HashTable:
    buckets = array of empty lists, size = 16
    count = 0

    function put(key, value):
        index = hash(key) % len(buckets)
        for each (k, v) in buckets[index]:
            if k == key:
                update v to value
                return
        append (key, value) to buckets[index]
        count += 1
        if count / len(buckets) > 0.75:
            resize()

    function get(key):
        index = hash(key) % len(buckets)
        for each (k, v) in buckets[index]:
            if k == key:
                return v
        return NOT_FOUND

    function resize():
        old = buckets
        buckets = array of empty lists, size = len(old) * 2
        for each list in old:
            for each (k, v) in list:
                index = hash(k) % len(buckets)
                append (k, v) to buckets[index]
```

### When Hash Tables Fail

- **Worst case O(n):** If every key hashes to the same slot, lookup degrades to scanning a list
- **No ordering:** You cannot iterate in sorted order (use a tree for that)
- **Hash function quality matters:** A bad hash function creates clusters
- **Not cache-friendly for small n:** For tiny datasets, a sorted array may beat a hash table

Despite these caveats, hash tables are the default choice for key-value lookup. When someone
says "use a dictionary," they mean a hash table.

---

## 6. Trees — Hierarchical Structure

Trees model hierarchy: file systems, HTML documents, organizational charts, decision processes.

### Binary Tree

Each node has at most two children.

```
           8
          / \
         3   10
        / \    \
       1   6    14
          / \   /
         4   7 13
```

### Binary Search Tree (BST)

A binary tree with an ordering invariant: **left child < parent < right child**.

This means an **in-order traversal** (left, root, right) produces sorted output:
`1, 3, 4, 6, 7, 8, 10, 13, 14`

| Operation | Average | Worst (degenerate) |
|-----------|---------|--------------------|
| Search    | O(log n)| O(n)               |
| Insert    | O(log n)| O(n)               |
| Delete    | O(log n)| O(n)               |

The worst case happens when the tree degenerates into a linked list (e.g., inserting sorted data).

### Balanced BSTs

To guarantee O(log n), we keep the tree balanced:

- **AVL trees:** Strictly balanced (height difference between subtrees <= 1). Fast lookups,
  slightly slower inserts due to rotations.
- **Red-black trees:** Approximately balanced. Used in Java's `TreeMap`, C++'s `std::map`.
  Slightly faster inserts than AVL, slightly slower lookups.

Both guarantee O(log n) for all operations by performing **rotations** when the tree becomes
unbalanced after an insert or delete.

### B-Trees — Optimized for Disk

A B-tree is a generalization where each node holds *many* keys and has *many* children.

```
              [  17  |  35  ]
             /        |       \
     [3|8|12]    [20|25|30]   [38|42|50]
```

Why? Disk reads are slow but read entire *blocks* at once. A B-tree node is sized to fit one
disk block, so each disk read gives you many keys to compare. This minimizes disk I/O.

**Every major database (PostgreSQL, MySQL, SQLite) uses B-trees or B+ trees for indexing.**

### Heaps

A **heap** is a complete binary tree where every parent is smaller (min-heap) or larger
(max-heap) than its children. Not a search tree — you can only efficiently access the root.

```
Min-heap:        1
               /   \
              3     2
             / \   /
            7   4  5
```

Stored as an array (level by level): `[1, 3, 2, 7, 4, 5]`
- Parent of index i: `(i - 1) / 2`
- Left child: `2i + 1`
- Right child: `2i + 2`

| Operation   | Complexity |
|-------------|------------|
| Find min    | O(1)       |
| Insert      | O(log n)   |
| Extract min | O(log n)   |
| Build heap  | O(n)       |

Heaps implement priority queues. They also enable heap sort (O(n log n), in-place).

### Tries (Prefix Trees)

A tree where each edge represents a character. Used for autocomplete, spell checking,
and IP routing tables.

```
           root
          / | \
         c  d  t
         |  |  |
         a  o  h
        / \  \  |
       r   t  g  e
       |       |
       s       (end)
```

Words stored: "car", "cars", "cat", "dog", "the"

Lookup time: O(length of key), regardless of how many words are stored.

---

## 7. Graphs — Everything is Connected

A graph is a set of **nodes** (vertices) connected by **edges**. Trees are a special case of
graphs (connected, acyclic). Graphs are the most general and flexible data structure.

### Types of Graphs

```
Undirected:       Directed:         Weighted:
  A --- B          A --> B           A --5-- B
  |     |          |     |           |       |
  C --- D          v     v           3       2
                   C --> D           |       |
                                    C --1-- D
```

- **Directed/Undirected:** Are edges one-way or two-way?
- **Weighted/Unweighted:** Do edges have costs?
- **Cyclic/Acyclic:** Can you follow edges and return to where you started?
- **DAG (Directed Acyclic Graph):** Directed, no cycles — models dependencies

### Representation

**Adjacency list** (most common — good for sparse graphs):
```
A: [B, C]
B: [A, D]
C: [A, D]
D: [B, C]
```
Space: O(V + E). Looking up neighbors: O(degree of node).

**Adjacency matrix** (good for dense graphs):
```
    A  B  C  D
A [ 0  1  1  0 ]
B [ 1  0  0  1 ]
C [ 1  0  0  1 ]
D [ 0  1  1  0 ]
```
Space: O(V^2). Looking up "is there an edge from A to D?": O(1).

### Breadth-First Search (BFS)

Explore level by level. Uses a **queue**.

```
function bfs(graph, start):
    queue = [start]
    visited = {start}
    while queue is not empty:
        node = queue.dequeue()
        process(node)
        for each neighbor of node:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.enqueue(neighbor)
```

**Finds shortest path in unweighted graphs.** Time: O(V + E).

### Depth-First Search (DFS)

Explore as deep as possible, then backtrack. Uses a **stack** (or recursion).

```
function dfs(graph, node, visited):
    visited.add(node)
    process(node)
    for each neighbor of node:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

**Used for:** connected components, cycle detection, topological sort. Time: O(V + E).

### Dijkstra's Algorithm — Shortest Path with Weights

Uses a **priority queue**. Greedily picks the closest unvisited node.

```
function dijkstra(graph, source):
    dist = {source: 0, all others: infinity}
    pq = priority queue with (0, source)
    while pq is not empty:
        (d, node) = pq.extract_min()
        if d > dist[node]: continue
        for each (neighbor, weight) in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                pq.insert((new_dist, neighbor))
    return dist
```

Time: O((V + E) log V) with a binary heap.

**Requirement:** No negative edge weights. (Use Bellman-Ford for negative weights.)

### What Graphs Model

Almost everything that involves relationships:
- Social networks (people are nodes, friendships are edges)
- Road maps (intersections are nodes, roads are weighted edges)
- The internet (routers are nodes, connections are edges)
- Dependencies (packages, build steps, course prerequisites — DAGs)
- State machines (states are nodes, transitions are edges)

---

## 8. Sorting Algorithms — Creating Order

Sorting is the most studied problem in computer science, and for good reason: sorted data
unlocks binary search, grouping, deduplication, and efficient merging. Many algorithms use
sorting as a preprocessing step.

### Bubble Sort — O(n^2)

Repeatedly swap adjacent out-of-order elements. Educational only.

```
[5, 3, 8, 1] -> [3, 5, 1, 8] -> [3, 1, 5, 8] -> [1, 3, 5, 8]
```

Terrible performance. Never use in production. But it teaches the concept of invariants:
after each pass, the largest unsorted element "bubbles" to its correct position.

### Merge Sort — O(n log n), Stable

Divide and conquer. Split the array in half, sort each half, merge the sorted halves.

```
         [38, 27, 43, 3, 9, 82, 10]
              /                \
     [38, 27, 43, 3]    [9, 82, 10]
        /       \          /      \
   [38, 27]  [43, 3]   [9, 82]  [10]
    /   \     /   \      /   \     |
  [38] [27] [43]  [3]  [9] [82]  [10]
    \   /     \   /      \   /     |
   [27, 38]  [3, 43]   [9, 82]  [10]
        \       /          \      /
     [3, 27, 38, 43]    [9, 10, 82]
              \                /
       [3, 9, 10, 27, 38, 43, 82]
```

```
function merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr) / 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

function merge(left, right):
    result = []
    i, j = 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

**Stable:** Equal elements preserve their relative order. O(n) extra space.

### Quick Sort — O(n log n) Average, Fastest in Practice

Pick a pivot, partition into elements smaller and larger, recurse.

```
function quicksort(arr, lo, hi):
    if lo >= hi: return
    pivot_index = partition(arr, lo, hi)
    quicksort(arr, lo, pivot_index - 1)
    quicksort(arr, pivot_index + 1, hi)

function partition(arr, lo, hi):
    pivot = arr[hi]
    i = lo
    for j from lo to hi - 1:
        if arr[j] < pivot:
            swap arr[i] and arr[j]
            i += 1
    swap arr[i] and arr[hi]
    return i
```

- **Average case:** O(n log n) — with random pivot selection
- **Worst case:** O(n^2) — when pivot is always the smallest/largest (sorted input with naive pivot)
- **In-place:** Only O(log n) extra space for the call stack

Why fastest in practice? Cache-friendly sequential access, low constant factors, and
randomized pivot selection makes the worst case vanishingly unlikely.

### Counting / Radix Sort — O(n) for Integers

If your data is integers in a known range, you can beat O(n log n):

**Counting sort:** Count occurrences of each value, then reconstruct the array.
Works when the range of values is small relative to n.

**Radix sort:** Sort by each digit, from least significant to most significant,
using a stable sort (like counting sort) for each digit. O(d * n) where d is the
number of digits.

### The Comparison Sort Lower Bound

Any sorting algorithm that works by comparing elements requires at least O(n log n)
comparisons in the worst case. This is a *proven mathematical fact*, not a conjecture.

**Why?** There are n! possible orderings. Each comparison eliminates at most half of the
remaining possibilities. You need at least log2(n!) comparisons, and log2(n!) = O(n log n)
by Stirling's approximation.

Counting and radix sort bypass this by not using comparisons — they exploit the structure
of the keys themselves.

### Summary Table

| Algorithm      | Best      | Average   | Worst     | Space  | Stable? |
|----------------|-----------|-----------|-----------|--------|---------|
| Bubble sort    | O(n)      | O(n^2)    | O(n^2)    | O(1)   | Yes     |
| Merge sort     | O(n log n)| O(n log n)| O(n log n)| O(n)   | Yes     |
| Quick sort     | O(n log n)| O(n log n)| O(n^2)    | O(log n)| No     |
| Counting sort  | O(n + k)  | O(n + k)  | O(n + k)  | O(k)   | Yes     |
| Radix sort     | O(dn)    | O(dn)    | O(dn)    | O(n+k) | Yes     |

---

## 9. Searching — Finding What You Need

### Linear Search — O(n)

Walk through every element. Works on any data. No prerequisites.

```
function linear_search(arr, target):
    for i from 0 to len(arr) - 1:
        if arr[i] == target:
            return i
    return NOT_FOUND
```

### Binary Search — O(log n)

**Requires sorted data.** Repeatedly cut the search space in half.

```
function binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) / 2
        if arr[mid] == target:
            return mid
        else if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return NOT_FOUND
```

Why binary search is magical:

| Items         | Max Steps |
|---------------|-----------|
| 1,000         | 10        |
| 1,000,000     | 20        |
| 1,000,000,000 | 30        |

Every doubling of data adds only **one** more step.

### Binary Search Beyond Arrays

The idea — eliminating half the possibilities — applies far beyond sorted arrays:

- **Finding a bug:** If a test fails, binary search through commits (git bisect)
- **Finding a threshold:** "What is the minimum speed that causes a crash?" Binary search on the answer space
- **Square root:** Binary search between 0 and n for x where x*x = n
- **Optimization:** Binary search on the answer when the function is monotonic

Any time you have a *monotonic predicate* (false, false, ..., false, true, true, ..., true),
you can binary search for the transition point.

---

## 10. Algorithm Design Paradigms

These are the major *strategies* for designing algorithms. Most algorithms you encounter
fall into one of these categories.

### Brute Force

Try every possible solution. Always correct, often too slow.

```
# Find two numbers that sum to target
for i in range(n):
    for j in range(i+1, n):
        if arr[i] + arr[j] == target:
            return (i, j)
```

O(n^2). The starting point before you optimize.

### Divide and Conquer

Split the problem in half, solve each half, combine the results.

**Pattern:**
1. **Divide** the problem into smaller subproblems
2. **Conquer** each subproblem recursively
3. **Combine** the results

**Examples:** Merge sort (divide array, sort halves, merge), binary search (divide search
space, pick half), Karatsuba multiplication (multiply large numbers faster than O(n^2)).

### Greedy

At each step, make the locally optimal choice. Hope it leads to the globally optimal solution.
(It doesn't always, but when it does, greedy algorithms are simple and fast.)

```
# Coin change (greedy works for standard denominations)
# Goal: make 67 cents with fewest coins [25, 10, 5, 1]
function greedy_change(amount, coins):
    coins.sort(descending)
    result = []
    for coin in coins:
        while amount >= coin:
            result.append(coin)
            amount -= coin
    return result
# 25 + 25 + 10 + 5 + 1 + 1 = 67 (6 coins)
```

**Classic greedy algorithms:** Dijkstra's shortest path, Huffman coding (optimal compression),
Kruskal's/Prim's minimum spanning tree, activity selection.

**Danger:** Greedy doesn't always work. For coin denominations [1, 3, 4], making 6: greedy
gives 4+1+1 (3 coins), but optimal is 3+3 (2 coins). You need to *prove* a greedy approach
is correct for your specific problem.

### Dynamic Programming (DP)

Solve problems with **overlapping subproblems** by solving each subproblem once and caching
the result.

**The Fibonacci example makes this crystal clear:**

```
# Naive recursion: O(2^n) — recomputes the same values
function fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# DP (memoization / top-down): O(n)
memo = {}
function fib(n):
    if n in memo: return memo[n]
    if n <= 1: return n
    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]

# DP (tabulation / bottom-up): O(n)
function fib(n):
    table = [0, 1]
    for i from 2 to n:
        table[i] = table[i-1] + table[i-2]
    return table[n]
```

**The call tree without DP (exponential duplication):**
```
                     fib(5)
                    /       \
               fib(4)       fib(3)
              /     \       /     \
          fib(3)  fib(2)  fib(2)  fib(1)
          /   \    / \     / \
      fib(2) fib(1) ...  ...
```

With memoization, each value is computed exactly once: O(n).

**Classic DP problems:** Knapsack, longest common subsequence, edit distance,
shortest paths (Bellman-Ford, Floyd-Warshall), coin change (the correct version).

**How to spot a DP problem:**
1. Can the problem be broken into subproblems?
2. Do subproblems overlap (same subproblem solved multiple times)?
3. Does the problem have *optimal substructure* (optimal solution uses optimal sub-solutions)?

### Backtracking

Systematically try possibilities. If a choice leads to a dead end, *undo* it and try the
next option. A structured form of brute force that prunes invalid branches early.

```
# N-Queens: place N queens on N×N board, no two attacking
function solve(board, col):
    if col == N:
        print(board)  # found a solution
        return
    for row from 0 to N-1:
        if is_safe(board, row, col):
            board[row][col] = QUEEN
            solve(board, col + 1)
            board[row][col] = EMPTY   # backtrack
```

**Examples:** Sudoku solver, maze solving, generating permutations/combinations,
constraint satisfaction problems.

---

## 11. Complexity Theory — The Limits of Computation

Not all problems are created equal. Some are *inherently* hard — not because we haven't
found a good algorithm yet, but because there may be no efficient algorithm.

### P — Polynomial Time

Problems solvable in O(n^k) for some constant k. These are "tractable" — sorting, shortest
path, matrix multiplication, primality testing.

### NP — Nondeterministic Polynomial Time

Problems where a proposed solution can be *verified* in polynomial time, even if *finding*
the solution might be hard.

Example: given a Sudoku puzzle and a filled-in grid, you can quickly check if it is valid.
But finding the solution might require extensive search.

**P is a subset of NP** (if you can solve it fast, you can certainly verify it fast).

### NP-Complete

The hardest problems in NP. Every problem in NP can be reduced to any NP-complete problem.
If you solve *one* NP-complete problem in polynomial time, you solve *all* of them.

**Famous NP-complete problems:**
- **Traveling Salesman (decision version):** Is there a route visiting all cities with total distance < k?
- **Boolean Satisfiability (SAT):** Is there an assignment of variables that makes a formula true?
- **Graph Coloring:** Can you color a graph with k colors such that no adjacent nodes share a color?
- **Knapsack (decision version):** Can you fill a knapsack to value >= k without exceeding weight W?
- **Subset Sum:** Is there a subset of numbers that sums to exactly S?

### P = NP?

The biggest open question in computer science (and mathematics). It asks: if a solution can
be *verified* quickly, can it always be *found* quickly?

Most researchers believe P != NP, but nobody has proven it. There is a $1 million
Millennium Prize for a proof either way.

### What This Means in Practice

When you encounter an NP-complete problem in the real world (and you will — scheduling,
routing, packing, resource allocation):

1. **Recognize it** — "This is NP-complete, there's no known polynomial algorithm"
2. **Don't search for an exact efficient solution** — it almost certainly doesn't exist
3. **Use approximations** — algorithms that find "good enough" solutions quickly
4. **Use heuristics** — greedy approaches, simulated annealing, genetic algorithms
5. **Constrain the problem** — maybe your specific instances are small or have structure

---

## 12. Recursion — The Self-Referential Tool

A recursive function calls itself with a smaller input, building toward a base case.

### The Structure

Every recursive function needs:
1. **Base case:** When to stop (prevents infinite recursion)
2. **Recursive case:** Break the problem down and call yourself

```
function factorial(n):
    if n <= 1: return 1          # base case
    return n * factorial(n - 1)  # recursive case
```

### The Call Stack

Each recursive call adds a frame to the call stack:

```
factorial(4)
  -> 4 * factorial(3)
       -> 3 * factorial(2)
            -> 2 * factorial(1)
                 -> returns 1
            -> returns 2 * 1 = 2
       -> returns 3 * 2 = 6
  -> returns 4 * 6 = 24
```

**Stack overflow** happens when recursion is too deep (e.g., factorial(1000000) in a language
without tail call optimization).

### Where Recursion Shines

- **Tree traversal:** Trees are recursive structures — recursion is the natural fit
  ```
  function inorder(node):
      if node is null: return
      inorder(node.left)
      process(node.value)
      inorder(node.right)
  ```
- **Divide and conquer:** Merge sort, quicksort, binary search
- **Backtracking:** N-queens, Sudoku, maze solving
- **Mathematical definitions:** Fibonacci, factorials, combinatorics

### When Iteration is Better

- **Simple loops:** `sum(1..n)` is cleaner as a for loop
- **Deep recursion:** Risk of stack overflow. Convert to iteration with an explicit stack
- **Performance-critical code:** Function call overhead adds up
- **Tail recursion not supported:** Some languages (Java, Python) don't optimize tail calls

### Tail Recursion

A recursive call is in **tail position** if it is the very last thing the function does —
no computation happens after the recursive call returns.

```
# NOT tail recursive (must multiply after the call returns)
function factorial(n):
    if n <= 1: return 1
    return n * factorial(n - 1)

# Tail recursive (result built up in accumulator)
function factorial(n, acc=1):
    if n <= 1: return acc
    return factorial(n - 1, n * acc)
```

Languages that support **tail call optimization** (Scheme, Haskell, some compilers for C)
convert tail recursion into a loop internally — no stack growth, no overflow. In languages
without this optimization (Python, Java), the distinction is academic.

---

## Key Mental Models

- **Think in terms of trade-offs.** Every data structure and algorithm sacrifices something.
  Arrays sacrifice fast insertion for fast access. Hash tables sacrifice ordering for speed.
  There is no universally best choice — only the best choice for your constraints.

- **Look for the bottleneck.** An algorithm is only as fast as its slowest component. Optimize
  the part that dominates: O(n^2 + n) is just O(n^2). Focus your effort where it matters.

- **Reduce to a known problem.** Most problems are variations of well-studied ones. Can you
  model it as a graph? A shortest path? A sorting problem? Recognition is the first step
  to a solution.

- **Understand the data before choosing the structure.** How big is it? Does it change often?
  What queries do you need? Read-heavy or write-heavy? The answers determine which data
  structure fits.

- **The constant factor matters in practice.** Two O(n log n) algorithms can differ by 10x
  in wall-clock time. Big-O tells you the *shape* of performance. Profiling tells you the
  *reality*.

---

## How This Connects

Everything above flows into the systems built on top:

- **Databases** use B-trees for indexing, hash tables for in-memory lookups, and sort-merge
  joins for query processing
- **Networking** uses graph algorithms for routing (Dijkstra, Bellman-Ford), queues for
  packet buffering, hash tables for connection tracking
- **Compilers** use hash tables for symbol tables, trees for syntax (ASTs), and graph
  algorithms for register allocation and optimization
- **Operating systems** use priority queues for process scheduling, trees for file systems,
  and hash tables for page tables
- **Machine learning** relies on optimization algorithms (gradient descent), matrix operations,
  and tree-based models (decision trees, random forests)
- **Cryptography** depends on number theory algorithms, hash functions, and the assumed
  hardness of NP-like problems

---

## Hands-On

You don't understand a data structure until you have built one. You don't understand an
algorithm until you have implemented it from scratch.

1. **Implement a hash table** from scratch. Handle collisions with chaining. Add resizing.
   Then benchmark: insert 1 million random keys and measure lookup time.

2. **Implement merge sort and quicksort.** Compare their performance on random data, sorted
   data, and reverse-sorted data. Explain what you observe.

3. **Implement a binary search tree** with insert, search, delete, and in-order traversal.
   Then insert sorted data and watch it degrade. Understand why balanced trees exist.

4. **Implement BFS and DFS** on a graph (adjacency list). Use BFS to find shortest paths
   in an unweighted graph. Use DFS to detect cycles.

5. **Solve problems** — not just implement algorithms in isolation:
   - Two Sum (hash table)
   - Valid Parentheses (stack)
   - Merge Intervals (sorting)
   - Number of Islands (BFS/DFS on a grid)
   - Coin Change (dynamic programming)
   - Course Schedule (topological sort)

6. **Analyze complexity** of your solutions. For each one, state the time and space complexity
   and explain why.

---

## Go Deeper

- **CLRS** — *Introduction to Algorithms* (Cormen, Leiserson, Rivest, Stein). The definitive
  textbook. Dense but comprehensive. Use as a reference, not a cover-to-cover read.
- **Skiena** — *The Algorithm Design Manual*. More practical than CLRS. Excellent "war stories"
  showing how algorithms solve real problems. The "Hitchhiker's Guide" catalog of algorithmic
  problems is invaluable.
- **Sedgewick** — *Algorithms* (4th edition, with Wayne). Java-based, beautifully illustrated.
  Companion website with visualizations. Great for building intuition.
- **Competitive programming:** [Codeforces](https://codeforces.com),
  [LeetCode](https://leetcode.com), [Project Euler](https://projecteuler.net).
  Nothing sharpens algorithmic thinking like solving timed problems.
- **Visualizations:** [VisuAlgo](https://visualgo.net) — watch algorithms animate step by step.
  Seeing a red-black tree rebalance is worth a thousand words.

---

*Next: [Databases & Storage](../04-infrastructure/databases.md) — How do we persist and query massive amounts of data?*
