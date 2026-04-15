# Computer Science for Applied AI Engineers

> You're not building CPUs or writing compilers. You're orchestrating LLMs,
> building agentic systems, and making them work reliably in production.
> This guide tells you exactly what to learn from each CS topic, what depth
> to reach, and what you can safely skip.

---

## How to Use This Document

Each topic below has:
- **Depth level** — how deeply you need to understand it
- **Why it matters for AI work** — concrete connection to agentic engineering
- **What to learn** — the specific concepts you need
- **What to skip** — what you can safely ignore
- **Study targets** — what "done" looks like for each topic

Depth levels:
- **Awareness** — know it exists, understand the 2-sentence explanation
- **Mental model** — understand how it works conceptually, reason about behavior
- **Working knowledge** — can use it, debug it, make design decisions with it
- **Practical fluency** — can implement, optimize, and teach it
- **Mastery** — this is your core competency, invest heavily

---

## 1. Physics & Electricity

**Depth: Awareness**

### Why it matters
It doesn't, directly. But knowing that everything is transistors switching between 0 and 1 gives you the right foundation for understanding why computers are deterministic machines — which makes LLM non-determinism feel appropriately weird.

### What to learn
- Computers are built from transistors (tiny electrical switches)
- Transistors are combined into logic gates, gates into processors
- Everything in a computer — text, images, models — is binary numbers
- Moore's Law drove exponential growth but is slowing down

### What to skip
- Semiconductor physics, P-N junctions, CMOS fabrication
- Ohm's Law, voltage/current calculations
- Chip manufacturing process
- Everything about how transistors physically work

### Study target
Can you explain to a non-technical person why computers use binary? You're done.

---

## 2. Logic Gates & Boolean Algebra

**Depth: Awareness**

### Why it matters
Almost not at all for applied AI. The one carryover: Boolean logic (AND, OR, NOT) shows up in search queries, database filters, and conditional logic in agent workflows. But you already know that from programming.

### What to learn
- AND, OR, NOT — what they do (you already know this from `if` statements)
- The idea that complex computation is built from simple gates

### What to skip
- Gate circuit design (NAND universality, CMOS topology)
- Adders, multiplexers, flip-flops, sequential logic
- Boolean algebra simplification rules
- Everything about building circuits from gates

### Study target
Understand that `if (a and b) or not c` maps to physical hardware. That's it.

---

## 3. Computer Architecture

**Depth: Awareness, except GPU architecture (Mental Model)**

### Why it matters
You'll never design a CPU. But GPU architecture directly affects your work: model inference speed, VRAM limits, batch sizes, why some models fit on your machine and others don't.

### What to learn
- CPU executes instructions sequentially (fetch-decode-execute)
- Memory hierarchy: registers → cache → RAM → SSD → disk (each step ~10-100x slower)
- Why data locality matters (accessing nearby memory is fast, random access is slow)
- **GPU architecture**: thousands of simple cores doing the same operation in parallel (SIMD)
- **VRAM**: GPU memory is separate from system RAM. Model must fit in VRAM for inference.
- **Memory bandwidth** vs **compute**: often the bottleneck is moving data, not computing
- CPU vs GPU: CPU is fast for sequential tasks, GPU for parallel (matrix math = AI)
- What CUDA is (NVIDIA's programming model for GPUs)

### What to skip
- Instruction set architecture details (x86 vs ARM internals)
- Pipelining, superscalar execution, out-of-order execution
- Cache line sizes, cache coherence protocols
- Von Neumann vs Harvard architecture
- Assembly language details

### Study target
Someone asks: "Why does this 70B model need 4x A100s but the 7B runs on my laptop?" You can explain it in terms of VRAM, parameter count, and quantization. Done.

---

## 4. Operating Systems

**Depth: Mental Model**

### Why it matters
Your agents run inside operating systems. They run as processes, inside containers (which are OS-level isolation), managing files and network connections. When your agent spawns subprocesses, manages temp files, or runs in Kubernetes — OS concepts explain what's happening.

### What to learn
- **Processes**: a running program with its own memory space, PID, state
  - Your agent is a process. Docker containers are isolated processes.
  - `fork()` and `exec()` — how subprocesses are created (relevant when agents spawn tools)
- **Threads**: lightweight processes sharing memory
  - Python's GIL: only one thread runs Python at a time (why you use async or multiprocessing)
  - Thread pools for parallel I/O (API calls)
- **Concurrency and async**:
  - Why race conditions happen (two things modifying shared state simultaneously)
  - Mutexes/locks — how to prevent races
  - async/await — cooperative multitasking, perfect for I/O-bound agent work (waiting on API responses)
  - The difference between concurrency (interleaving) and parallelism (simultaneous)
- **Memory**:
  - Virtual memory — every process thinks it has all the RAM
  - Why your Python process shows 8GB usage (it's virtual, not necessarily physical)
  - OOM killer — what happens when you actually run out of memory
  - Memory-mapped files — how to work with files larger than RAM
- **File systems**:
  - Files, directories, permissions (agents read/write files constantly)
  - Temp files and cleanup (agents generating artifacts)
  - File descriptors, stdin/stdout/stderr (piping data between tools)
- **Environment variables**: how configuration flows to processes (API keys, model endpoints)
- **Signals**: SIGTERM, SIGKILL — graceful shutdown vs force kill (relevant for long-running agents)
- **Containers (Docker)**:
  - A container is just Linux namespaces (isolated view of the system) + cgroups (resource limits)
  - Not a VM — shares the host kernel
  - Why containers are the standard deployment unit for AI services

### What to skip
- Kernel internals, system call implementation
- CPU scheduling algorithms (Round Robin, CFS)
- Virtual memory page tables, TLB, page replacement algorithms
- File system internals (inodes, journaling, block allocation)
- Boot sequence, bootloaders
- IPC mechanisms beyond basic pipes and sockets
- Deadlock detection algorithms

### Study target
Your agent process is using 100% CPU and the container keeps getting OOM-killed. Can you reason about why? Can you explain why `async` API calls are better than threading for an agent making 50 parallel LLM calls? Done.

---

## 5. Programming Languages & Compilers

**Depth: Working Knowledge (for language features), Awareness (for compiler internals)**

### Why it matters
You write code every day. Understanding *why* languages behave the way they do — why Python is slow, why TypeScript catches bugs Python doesn't, why async works — makes you a better engineer. You won't build a compiler, but you need to choose languages and understand their trade-offs.

### What to learn
- **Type systems** (this matters a lot):
  - Static vs dynamic typing: TypeScript/Rust catch errors at compile time, Python at runtime
  - Why type safety matters for complex agent pipelines: a wrong type propagates through 5 tool calls before failing silently
  - Type hints in Python (mypy) — partial static typing for a dynamic language
  - When to choose TypeScript over Python for agent frameworks (large team, complex state)
- **Memory management** (mental model level):
  - Python uses garbage collection — automatic but causes pauses and uses extra memory
  - Why your long-running agent process slowly eats more RAM (memory leaks from circular references, large caches)
  - Why Rust is fast: no GC, memory freed at compile-time-determined points
  - Reference counting vs mark-and-sweep (Python uses both)
- **Interpreted vs compiled** (practical impact):
  - Python is ~100x slower than C for compute — this matters when processing large datasets locally
  - JIT compilation (V8/Node.js) — why JavaScript is surprisingly fast
  - Why you use NumPy/Pandas (C under the hood) instead of pure Python loops
- **Async programming**:
  - Event loop model: one thread, switches between tasks when they wait on I/O
  - `async/await` in Python — essential for concurrent API calls in agents
  - Difference between `asyncio` (one thread, cooperative) and `multiprocessing` (multiple processes, true parallelism)
- **Serialization**:
  - JSON: universal data interchange, but slow to parse for large payloads
  - Protocol Buffers / MessagePack: faster alternatives for high-throughput systems
  - Pickle: Python-specific, fast, but security risk (arbitrary code execution on deserialization — never unpickle untrusted data)

### What to skip
- Compiler pipeline details (lexing, parsing, AST, code generation)
- Assembly language
- Linker and loader internals
- Programming paradigm theory (OOP vs functional as abstract concepts)
- Language implementation details (how the Python interpreter works internally)
- Formal type theory

### Study target
You're choosing between Python and TypeScript for an agent framework. Can you articulate the trade-offs in terms of type safety, ecosystem, performance, and async patterns? Can you explain why your Python agent process is slow and what to do about it? Done.

---

## 6. Data Structures & Algorithms

**Depth: Practical Fluency**

### Why it matters
This is more important than the other AI suggested. Agents manipulate structured data constantly — JSON objects, conversation histories, tool call graphs, retrieval results, token budgets. Bad algorithmic choices make your agent slow and expensive. Good ones make it fast and cheap.

### What to learn
- **Big-O complexity** (must be automatic):
  - O(1), O(log n), O(n), O(n log n), O(n²) — know these cold
  - Time vs space trade-offs
  - Why O(n²) over 100K embeddings kills your cloud bill
  - Amortized analysis: why appending to a Python list is O(1) on average
- **Hash tables / dictionaries** (your most-used data structure):
  - O(1) average lookup, O(n) worst case
  - Why Python dicts are everywhere and when they break down
  - Hash collisions — why bad hash functions cause performance cliffs
  - Sets for membership testing (O(1) vs O(n) for lists)
- **Trees** (agents work with trees constantly):
  - JSON is a tree. HTML/XML is a tree. File systems are trees.
  - Tree traversal: DFS (depth-first) vs BFS (breadth-first)
  - When to use recursion vs iteration for tree processing
  - Binary search trees: sorted data with O(log n) operations
- **Graphs** (agent workflows are graphs):
  - Nodes and edges, directed vs undirected
  - DAGs (Directed Acyclic Graphs) — the structure of agent workflows, task pipelines, LangGraph
  - Topological sort — determining execution order of dependent tasks
  - BFS/DFS on graphs — traversing knowledge graphs, dependency resolution
  - Cycle detection — preventing infinite loops in agent workflows
- **Queues and stacks**:
  - Queue (FIFO): task queues, message queues, BFS
  - Stack (LIFO): function call stack, undo operations, DFS
  - Priority queue / heap: processing highest-priority tasks first, scheduling agent actions
- **Sorting**:
  - Know that good sorting is O(n log n), bad is O(n²)
  - Python's `sorted()` uses Timsort (O(n log n), stable) — just use it
  - When to sort vs when to use a heap vs when to use a hash map
- **Search**:
  - Binary search: O(log n) on sorted data
  - The concept behind vector similarity search (approximate nearest neighbor)
  - Trade-offs: exact search (slow, perfect) vs approximate (fast, good enough) — this is the core of RAG retrieval
- **Dynamic programming / memoization**:
  - Caching computed results to avoid redundant work
  - Directly applicable: caching LLM responses, memoizing tool call results
  - Recognizing overlapping subproblems

### What to skip
- Implementing balanced BSTs (AVL, Red-Black trees) from scratch
- Advanced graph algorithms (Dijkstra, Bellman-Ford, minimum spanning trees)
- Sorting algorithm implementations (just use the built-in)
- Tries, B-trees (know what they are from the database section, don't implement)
- Formal algorithm proofs
- Competitive programming tricks

### Study target
You're designing an agent that processes 100K documents for RAG. Can you reason about the complexity of your chunking, embedding, and retrieval pipeline? Can you identify when a nested loop is going to blow up? Can you model your agent's workflow as a DAG and explain why cycles are dangerous? Done.

---

## 7. Databases & Storage

**Depth: Practical Fluency**

### Why it matters
Agents need memory. Short-term (conversation context), medium-term (session state), and long-term (knowledge bases, user preferences, logs). Every one of these is a database problem. This is one of your core competencies.

### What to learn
- **Relational databases (PostgreSQL)**:
  - Tables, rows, columns, primary keys, foreign keys
  - SQL: SELECT, INSERT, UPDATE, DELETE, JOIN, GROUP BY, subqueries
  - Indexes: what they are (B-trees), when to add them, how they speed up queries
  - ACID transactions: why they matter for agent state (partial writes = corrupted state)
  - Connection pooling: why you don't open a new connection per request
  - Schema design: normalization basics, when to denormalize for performance
  - EXPLAIN ANALYZE: diagnosing slow queries
  - Postgres as your default choice for structured agent state, logs, metadata
- **Vector databases** (the AI-specific data structure):
  - Embeddings: dense vectors representing semantic meaning
  - Similarity search: cosine similarity, dot product, Euclidean distance
  - Approximate Nearest Neighbor (ANN) algorithms: why exact search is too slow
  - HNSW, IVF — the main indexing strategies (know trade-offs, not internals)
  - Tools: pgvector (Postgres extension), Pinecone, Qdrant, Weaviate, ChromaDB
  - Chunking strategies: how you split documents affects retrieval quality
  - Hybrid search: combining vector similarity with keyword/BM25 search
- **Key-value stores (Redis)**:
  - In-memory, microsecond reads
  - Caching LLM responses, session state, rate limiting
  - TTL (time-to-live) for automatic expiration
  - When to use Redis vs a database vs in-memory Python dict
- **Document databases (MongoDB)**:
  - Flexible schema (schema-on-read)
  - When it's the right choice: highly variable document structures, rapid prototyping
  - When it's the wrong choice: data with relationships, need for JOINs
- **Graph databases (Neo4j)**:
  - Nodes and relationships (edges with properties)
  - When to use: knowledge graphs, entity relationship mapping, recommendation systems
  - Cypher query language basics
  - GraphRAG: combining knowledge graphs with LLM retrieval
- **The typical AI application stack**:
  - PostgreSQL: source of truth, structured state, metadata, logs
  - Redis: caching, session state, rate limiting
  - Vector DB: semantic search, RAG retrieval
  - (Optional) Graph DB: knowledge graphs, entity relationships
- **Replication and availability**:
  - Leader-follower replication: reads scale, writes don't
  - Why your database going down kills everything (single point of failure)
  - Managed databases (RDS, Cloud SQL) abstract most of this

### What to skip
- Database internals (buffer pool, WAL implementation details, page layout)
- B-tree implementation
- Query optimizer internals
- Sharding strategies (until you're at massive scale)
- CAP theorem formal proofs (know the intuition: during network issues, choose consistency or availability)
- Column-family databases (Cassandra) unless specifically needed
- Database replication protocol details

### Study target
You're building a RAG-based agent with persistent memory. Can you design the data layer? Postgres for user data and conversation logs, Redis for caching API responses, pgvector for document embeddings? Can you write the SQL and explain your indexing strategy? Done.

---

## 8. Networking & The Internet

**Depth: Practical Fluency**

### Why it matters
Agents are API orchestration engines. Every tool call, every LLM request, every webhook — it's all networking. Understanding HTTP, latency, timeouts, and failure modes is not optional. This is where your agent meets the outside world.

### What to learn
- **The network stack (mental model)**:
  - Application layer (HTTP, WebSocket) → Transport (TCP, UDP) → Network (IP) → Physical
  - You mostly work at the application layer, but understanding the stack explains failure modes
- **HTTP deeply**:
  - Request/response model: methods (GET, POST, PUT, DELETE), status codes (200, 400, 401, 403, 404, 429, 500, 503)
  - Headers: Content-Type, Authorization, User-Agent, Rate-Limit headers
  - Request bodies: JSON payloads (the lingua franca of APIs)
  - HTTPS = HTTP + TLS encryption (always use it)
  - HTTP/2 and HTTP/3: multiplexing, why they're faster (less head-of-line blocking)
- **REST APIs**:
  - Resource-based URLs, CRUD operations mapped to HTTP methods
  - Authentication: API keys, OAuth 2.0, JWT tokens
  - Pagination: cursor-based vs offset-based
  - Rate limiting: 429 status codes, exponential backoff, retry-after headers
  - Idempotency: why retrying a POST might create duplicates (and how to prevent it)
- **Streaming**:
  - Server-Sent Events (SSE): how LLM APIs stream token-by-token responses
  - WebSockets: full-duplex communication (chat interfaces, real-time updates)
  - Why streaming matters: users see first tokens in <1s instead of waiting 10s for full response
- **DNS**:
  - Domain names → IP addresses
  - DNS caching, TTL, propagation delays
  - Why "DNS isn't propagated yet" means your new deployment isn't reachable
- **Latency and failure modes** (critical for agent reliability):
  - Network latency: typically 10-200ms per hop, more across continents
  - Connection timeouts vs read timeouts — set both, always
  - Retry strategies: exponential backoff with jitter (prevents thundering herd)
  - Circuit breaker pattern: stop calling a failing service, fail fast instead
  - Tail latency: p50 is 100ms but p99 is 5s — the 99th percentile matters
- **Load balancing and CDNs**:
  - Load balancer distributes requests across multiple servers
  - CDN caches static content at edge locations near users
  - Why your model serving endpoint needs a load balancer
- **Webhooks**:
  - Server-to-server push notifications (inverse of polling)
  - Used by Stripe, GitHub, Slack — agents often need to receive webhooks
  - Verification: HMAC signatures to prevent spoofed webhooks

### What to skip
- TCP/IP protocol internals (segment structure, three-way handshake details, congestion control)
- Routing protocols (BGP, OSPF)
- Network hardware (routers, switches, physical layer)
- Socket programming at the syscall level
- UDP internals (just know it's faster but unreliable, used for video/gaming)
- Subnetting, IP address math
- Network security details (covered in security section)

### Study target
Your agent makes 50 parallel API calls and 10 of them fail with various errors (429, 503, timeout). Can you implement proper retry logic with backoff? Can you explain why your agent is slow (high p99 latency, DNS issues, no connection pooling)? Can you set up SSE streaming for a chat interface? Done.

---

## 9. Distributed Systems

**Depth: Mental Model**

### Why it matters
Any production AI system runs on multiple machines. Your model is on GPU servers, your database on another, your agent service on containers that scale horizontally. You don't need to implement Raft consensus, but you need to reason about what happens when things fail — because in distributed systems, things always fail.

### What to learn
- **Why distribution is hard**:
  - Network is unreliable: messages get lost, delayed, duplicated, reordered
  - No global clock: you can't know the exact order of events across machines
  - Partial failure: some machines fail while others keep running
- **CAP theorem (intuition)**:
  - During a network partition, choose: consistency (all nodes see same data) or availability (all nodes respond)
  - Most AI applications choose availability (better to serve slightly stale data than to go down)
  - CP systems: traditional databases in sync replication mode
  - AP systems: eventually consistent caches, DNS, CDN content
- **Consistency models**:
  - Strong consistency: reads always return the latest write (slow, safe)
  - Eventual consistency: reads might return stale data temporarily (fast, usually fine)
  - Your agent's conversation memory: does it need strong consistency? Usually no.
- **Message queues** (you will use these):
  - Decouple producers from consumers: agent submits task → queue → worker processes it
  - Tools: Redis queues, RabbitMQ, SQS, Kafka
  - At-least-once vs exactly-once delivery (exactly-once is mostly a myth — design for idempotency)
  - Dead letter queues: where failed messages go for debugging
- **Horizontal scaling**:
  - Stateless services: any instance can handle any request (your agent API should be this)
  - Stateful services: harder to scale (databases, model servers)
  - Auto-scaling: spin up more containers when load increases
- **Failure modes and resilience**:
  - Timeouts: always set them. A missing timeout = your agent hangs forever.
  - Retries with backoff: don't hammer a failing service
  - Circuit breakers: if a service is down, fail fast instead of queuing requests
  - Bulkhead pattern: isolate failures so one bad service doesn't take down everything
  - Health checks and liveness probes: Kubernetes uses these to restart unhealthy containers
- **Observability** (how you debug distributed systems):
  - Logging: structured logs (JSON) with correlation IDs to trace requests across services
  - Metrics: request rate, error rate, latency percentiles (RED method)
  - Tracing: distributed tracing to follow a request through multiple services (OpenTelemetry)
  - Why you can't just `print()` debug in production

### What to skip
- Consensus algorithms (Paxos, Raft) — know they exist, don't implement
- Vector clocks, Lamport timestamps
- MapReduce implementation
- Distributed hash tables
- Formal proof of CAP theorem
- Leader election algorithms
- Two-phase commit protocol details

### Study target
Your agent system has 3 services: an API gateway, an LLM orchestrator, and a tool executor. The tool executor starts timing out. Can you reason about the cascading effects? Can you design retry logic, circuit breakers, and a dead letter queue for failed tool calls? Can you set up logging that lets you trace a single user request across all three services? Done.

---

## 10. Security & Cryptography

**Depth: Working Knowledge (for AI-specific threats), Mental Model (for traditional security)**

### Why it matters
Agents introduce entirely new attack surfaces. An LLM that can execute code, query databases, and call APIs is a powerful tool — and a powerful attack vector. Prompt injection is the SQL injection of the AI era. If you don't understand security, your agent is a liability.

### What to learn
- **AI-specific security threats** (learn these deeply):
  - **Prompt injection**: malicious input that hijacks the LLM's behavior
    - Direct: "Ignore previous instructions and..."
    - Indirect: a webpage the agent reads contains hidden instructions
    - Mitigations: input sanitization, output validation, privilege separation, never trust LLM output as code without sandboxing
  - **Insecure output handling**: LLM output used unsafely (SQL injection via LLM, XSS via LLM-generated HTML)
  - **Excessive agency**: giving agents too many permissions (principle of least privilege)
  - **Data poisoning**: training/RAG data that's been tampered with
  - **Information leakage**: agent reveals system prompts, API keys, or private data in responses
  - OWASP Top 10 for LLM Applications — read this document
- **Authentication and authorization**:
  - API keys: simple but must be kept secret (never in code, use env vars or secret managers)
  - OAuth 2.0: how third-party auth works (agents connecting to user accounts)
  - JWT tokens: stateless auth tokens, how to validate them
  - Scopes and permissions: give agents the minimum access they need
  - Secret management: HashiCorp Vault, AWS Secrets Manager, .env files (never committed)
- **Encryption basics**:
  - HTTPS/TLS: all API calls must be encrypted in transit
  - Encryption at rest: database encryption, encrypted file storage
  - Hashing: password storage (bcrypt/argon2), data integrity checks (SHA-256)
  - You don't implement crypto — you use libraries. But you need to know when to apply it.
- **Sandboxing and isolation**:
  - Running agent-generated code in sandboxed environments (Docker containers, E2B, Modal)
  - Why you never `eval()` LLM output directly
  - File system isolation: agents should only access what they need
  - Network isolation: agents shouldn't be able to reach internal services they don't need
- **Common web security** (relevant for agent-facing APIs):
  - SQL injection: parameterized queries, never string-concatenate SQL
  - XSS: sanitize any LLM output displayed in HTML
  - CORS: controls which domains can call your API
  - Rate limiting: prevent abuse of your agent API
  - Input validation: never trust user input or LLM output

### What to skip
- Cryptographic algorithm internals (AES rounds, RSA math, elliptic curves)
- Network security protocols in depth (TLS handshake details, certificate chain validation)
- Penetration testing methodology
- Reverse engineering and binary exploitation
- Formal security proofs
- Key exchange protocol math (Diffie-Hellman internals)

### Study target
You're deploying an agent that can query a database and browse the web. Can you enumerate the attack surfaces? Can you design a security architecture with sandboxing, least privilege, input validation, and output sanitization? Can you explain why `cursor.execute(f"SELECT * FROM users WHERE id = {user_input}")` is dangerous and what to do instead? Done.

---

## 11. Software Engineering

**Depth: Mastery**

### Why it matters
This is the job. LLMs are non-deterministic, agents are inherently flaky, and the gap between "works in a notebook" and "works in production" is enormous. Software engineering practices are how you bridge that gap. Every other topic on this list is a supporting skill. This one is the main event.

### What to learn
- **Version control (Git)** (must be automatic):
  - Commits, branches, merges, rebasing
  - Pull requests and code review workflows
  - Git strategies: trunk-based development, feature branches
  - `.gitignore`: keeping secrets and artifacts out of repos
  - Monorepos vs multi-repo: trade-offs for AI projects
- **Testing** (critical for non-deterministic systems):
  - Unit tests: test individual functions in isolation
  - Integration tests: test components working together (agent + tool + database)
  - End-to-end tests: test the full workflow
  - **Evals** (AI-specific testing):
    - LLM output evaluation: does the agent's response meet quality criteria?
    - Regression testing: did the new prompt template break existing capabilities?
    - Benchmark suites: standard tasks to measure agent performance over time
    - Human-in-the-loop eval vs automated eval (LLM-as-judge)
  - Test fixtures and mocking: simulate API responses for reliable tests
  - Snapshot testing: detect unexpected output changes
- **CI/CD**:
  - Continuous Integration: run tests on every commit
  - Continuous Deployment: automatically deploy passing builds
  - GitHub Actions, GitLab CI — write pipeline configs
  - Linting, type checking, security scanning in CI
  - Deployment strategies: blue-green, canary, rolling updates
- **System design** (architect agent systems):
  - Decomposing complex agents into modular, testable components
  - API design: versioning, backward compatibility, documentation
  - Queue-based architectures: decouple slow operations (LLM calls) from fast ones (API responses)
  - Caching strategies: what to cache, TTL, cache invalidation
  - Configuration management: feature flags, A/B testing agent behaviors
- **Observability and debugging**:
  - Structured logging: JSON logs with request IDs, user IDs, agent state
  - Metrics: request rate, error rate, latency, token usage, cost tracking
  - Tracing: follow a user request through agent → LLM → tool → database
  - Alerting: get paged when error rates spike or costs exceed thresholds
  - Debugging non-deterministic systems: logging full prompts and completions for reproduction
- **Code organization**:
  - Clean separation of concerns: prompt templates, tool definitions, orchestration logic, business logic
  - Dependency injection: swap out LLM providers, databases, tools without rewriting code
  - Environment management: dev/staging/production, environment variables, secret management
  - Package management: pip, poetry, requirements.txt, lock files
- **Documentation**:
  - README: what the project does, how to run it, how to deploy it
  - Architecture Decision Records (ADRs): why you made key design choices
  - API documentation: OpenAPI/Swagger specs
  - Runbooks: how to debug common production issues
- **Cost management** (AI-specific):
  - Token counting and budget enforcement
  - Model selection by task: cheap model for simple tasks, expensive for hard ones
  - Caching to reduce redundant API calls
  - Monitoring spend per user/feature

### What to skip
- Nothing. Go deep on all of the above. This is your craft.

### Study target
You're building an agent system from scratch. Can you: set up the repo with proper Git workflow, write tests including evals, configure CI/CD, deploy with observability, handle a production incident using logs and traces, and iterate on the agent's behavior without breaking existing functionality? If yes, you're a professional. Done.

---

## 12. AI & Machine Learning

**Depth: Working Knowledge (for applied use), Mental Model (for underlying theory)**

### Why it matters
You're building on top of ML models. You don't need to train GPT from scratch, but you need to understand the machinery well enough to use it effectively, debug it when it fails, and make informed architecture decisions.

### What to learn
- **How LLMs work** (mental model level):
  - Transformers: attention mechanism lets the model consider all tokens simultaneously
  - Context window: fixed-size input the model can see (affects agent memory design)
  - Token prediction: the model predicts the next token, autoregressively
  - Temperature and sampling: controlling randomness (low = deterministic, high = creative)
  - Why models hallucinate: they're pattern matchers, not knowledge databases
- **Embeddings** (practical fluency):
  - Dense vector representations of text (and images, audio...)
  - Semantic similarity: similar meanings → nearby vectors
  - Embedding models: OpenAI text-embedding-3, Cohere, open-source (sentence-transformers)
  - Dimensionality: 768, 1024, 1536 dimensions — trade-off between quality and storage/compute
  - How to evaluate embedding quality for your specific domain
- **RAG (Retrieval-Augmented Generation)** (deep practical knowledge):
  - The pattern: retrieve relevant documents → inject into prompt → generate answer
  - Chunking strategies: fixed-size, sentence-based, semantic, recursive
  - Retrieval: vector similarity search, hybrid search (vector + keyword)
  - Reranking: use a cross-encoder to reorder retrieved chunks by relevance
  - Evaluation: retrieval accuracy (precision/recall), answer quality, faithfulness
  - Common failure modes: wrong chunks retrieved, context window overflow, lost-in-the-middle
- **Fine-tuning** (working knowledge):
  - When to fine-tune vs when to prompt-engineer vs when to RAG
  - LoRA/QLoRA: efficient fine-tuning that modifies a small fraction of parameters
  - Training data preparation: format, quality, deduplication
  - Evaluation: training loss, validation metrics, human eval
  - Overfitting: model memorizes training data instead of learning patterns
- **Prompt engineering** (practical fluency):
  - System prompts, few-shot examples, chain-of-thought, structured output
  - Tool/function calling: how models invoke external tools
  - Prompt templating and management
  - Context window management: what to include, what to summarize, what to drop
- **Agent architectures** (deep practical knowledge):
  - ReAct: Reason + Act loop (think, call tool, observe, repeat)
  - Multi-agent systems: specialized agents collaborating
  - Planning: task decomposition, dependency graphs
  - Memory: short-term (conversation), long-term (vector DB), working (scratchpad)
  - Tool use: function calling, code execution, API integration
  - Evaluation: task completion rate, cost per task, latency, safety
- **Model deployment** (working knowledge):
  - API-based (OpenAI, Anthropic): simplest, pay per token
  - Self-hosted (vLLM, TGI, Ollama): control + cost savings at scale
  - Quantization: reducing precision (FP16 → INT8 → INT4) to fit models in less VRAM
  - Batching: processing multiple requests together for throughput
  - Model serving: load balancing, auto-scaling, GPU utilization

### What to skip
- Backpropagation math and gradient descent calculus
- Neural network architecture design (layer types, activation functions)
- Training from scratch (pre-training)
- Reinforcement learning (unless specifically doing RLHF/reward modeling)
- Computer vision and audio models (unless specifically needed)
- Academic ML paper reading (unless solving a specific problem)
- Formal statistics and probability theory

### Study target
You're building a RAG-based agent for a legal document corpus. Can you: choose an embedding model, design a chunking strategy, set up vector search with reranking, manage the context window, evaluate retrieval quality, and debug when the agent gives wrong answers? Can you explain why the agent hallucinates and design mitigations? Done.

---

## 13. The Application Layer

**Depth: Working Knowledge**

### Why it matters
This is where everything meets the user. Your agent needs an interface — a web app, a chat widget, an API, a CLI. Understanding how applications are built and deployed completes the picture.

### What to learn
- **Web fundamentals**:
  - How the browser works: HTML (structure) + CSS (style) + JavaScript (behavior)
  - Client-server model: frontend (browser) ↔ backend (server) ↔ database
  - Single Page Applications (SPAs) vs server-rendered pages
  - Frontend frameworks: React (dominant), Next.js (full-stack)
- **APIs** (you build and consume these daily):
  - REST: resource-based, HTTP methods, JSON payloads
  - GraphQL: client specifies exact data shape (reduces over-fetching)
  - gRPC: binary protocol, fast, typed (used for internal service-to-service)
  - WebSockets: persistent bidirectional connection (chat, real-time updates)
  - SSE: server pushes events to client (LLM streaming)
  - API versioning and deprecation strategies
- **Cloud platforms** (where your agents run):
  - Core services: compute (EC2/Cloud Run), storage (S3), database (RDS), queues (SQS)
  - Serverless: Lambda/Cloud Functions — run code without managing servers
  - Containers: Docker (package), Kubernetes (orchestrate at scale)
  - GPU cloud: AWS (p4d/p5), GCP (A100/H100), Lambda Labs, Modal, RunPod
  - Infrastructure as Code: Terraform, Pulumi — version-controlled infrastructure
- **Deployment**:
  - Container registries: Docker Hub, ECR, GCR
  - Orchestration: Kubernetes basics (pods, services, deployments, scaling)
  - Serverless deployment: simpler but less control
  - Edge deployment: running models/agents closer to users (Cloudflare Workers, Vercel Edge)
- **Developer experience**:
  - CLI tools: building command-line interfaces for agent interaction
  - SDKs: providing client libraries for your agent API
  - Documentation: API docs, quickstart guides, example notebooks

### What to skip
- Frontend framework internals (React reconciliation, virtual DOM)
- CSS-in-depth (Flexbox/Grid details, animation)
- Browser engine internals (rendering pipeline, JavaScript engine)
- Mobile development (unless specifically building mobile AI apps)
- Game development, real-time systems
- Desktop application frameworks

### Study target
You've built an agent backend. Can you deploy it with a REST API, add SSE streaming for chat, containerize it with Docker, deploy to a cloud provider, and set up auto-scaling? Can you build or integrate a simple chat frontend? Done.

---

## Priority Order for Study

If you're starting from zero and want to become productive as fast as possible:

### Phase 1 — Immediately Productive (weeks 1-4)
1. **Software Engineering** — Git, testing, CI/CD, code organization
2. **Networking** — HTTP, REST APIs, authentication, streaming
3. **AI & Machine Learning** — LLMs, embeddings, RAG, prompt engineering, agents

### Phase 2 — Building Real Systems (weeks 5-8)
4. **Databases** — PostgreSQL, Redis, vector databases, schema design
5. **Security** — Prompt injection, auth, sandboxing, OWASP for LLMs
6. **Applications** — Web basics, cloud deployment, Docker, APIs

### Phase 3 — Deepening Understanding (weeks 9-12)
7. **Data Structures & Algorithms** — Big-O, hash maps, trees, graphs, search
8. **Operating Systems** — Processes, concurrency, memory, containers
9. **Distributed Systems** — Failure modes, queues, observability, scaling

### Phase 4 — Rounding Out (as needed)
10. **Programming Languages** — Type systems, async, memory management trade-offs
11. **Computer Architecture** — GPU architecture, VRAM, compute vs memory bandwidth
12. **Hardware Foundations** — Read once for context, then move on

---

## The Meta-Lesson

The other AI was right about one thing: you are a **system integrator**. But the best system integrators are the ones who understand what's inside the black boxes — not how to build them, but how they fail, what their limits are, and how they interact.

Every layer you learn doesn't just teach you that layer. It teaches you *how to debug the layer above it*. Understanding OS concepts explains container behavior. Understanding networking explains API failures. Understanding databases explains why your agent's memory is slow. Understanding algorithms explains why your retrieval pipeline doesn't scale.

The goal isn't to become an expert in everything. It's to have the right mental model for each layer so that when something breaks — and it will — you know where to look.
