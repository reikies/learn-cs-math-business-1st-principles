# Operations — Turning Chaos Into Repeatable Systems

> A business that depends on heroics doesn't scale. Operations is the discipline
> of building systems so reliable that exceptional results become ordinary. It's the
> difference between a chef who cooks one brilliant meal and McDonald's serving
> 69 million people the same burger every day.

**Prerequisites:** [Product](../03-customers/product.md) — you need something to deliver. [Economics](../01-foundations/economics.md) — supply chains and production.
**What it enables:** [Growth & Scaling](../06-growth/growth-and-scaling.md) — you can't scale what you can't systematize.

---

## Why This Matters

Every startup begins with improvisation. The founders do everything. There are no processes because there's barely a company.

But what works for 5 people breaks at 50. What works for 50 breaks at 500. Operations is what you build so that the company doesn't collapse under its own growth.

Operations is not glamorous. Nobody tweets about their supply chain optimization. But operationally excellent companies — Amazon, Toyota, Costco — systematically outperform competitors who have better branding, better marketing, and sometimes even better products.

---

## 1. Processes — The Building Blocks

A **process** is a repeatable sequence of steps that produces a consistent output.

### Why processes matter

Without processes:
- Every task is done differently each time
- Quality depends on who does it
- Training new people takes forever
- Errors are random and hard to diagnose

With processes:
- Tasks are done the same way every time
- Quality is consistent and predictable
- New people follow documented steps
- Errors are systematic and fixable

### Documenting processes

A good process document answers:
1. **What** — what is the output?
2. **Who** — who is responsible for each step?
3. **When** — what triggers the process? What are the deadlines?
4. **How** — step-by-step instructions
5. **What if** — what to do when things go wrong

### The process maturity ladder

```
Level 1: Ad hoc       — "We wing it every time"
Level 2: Defined      — "We have a documented process"
Level 3: Measured     — "We track how the process performs"
Level 4: Optimized    — "We continuously improve based on data"
Level 5: Automated    — "The system does it without human intervention"
```

Not every process needs to reach Level 5. But every critical process should be at least Level 2.

---

## 2. Systems Thinking — Seeing the Whole

A business is a system — a collection of interconnected parts where changes in one area ripple through others.

### Stocks and flows

Every system can be understood through **stocks** (accumulations) and **flows** (rates of change):

```
                ┌──────────────┐
 Inflow ───────►│    Stock     │────────► Outflow
(new customers) │ (total users)│   (churned customers)
                └──────────────┘
```

- If inflow > outflow, the stock grows (business is growing)
- If outflow > inflow, the stock shrinks (business is dying)
- The *rate* of each flow matters more than the stock at any moment

### Feedback loops

**Positive feedback loops** amplify change (growth or decline):
- More users → more content → attracts more users (social networks)
- More customers → more revenue → more investment → better product → more customers

**Negative feedback loops** stabilize (resist change):
- More customers → longer support wait times → worse experience → fewer customers
- More hiring → more coordination overhead → slower execution → less output per person

Understanding which loops dominate your business tells you where to invest and where to watch for problems.

### Bottlenecks

**The Theory of Constraints** (Eli Goldratt): Every system has one constraint — one bottleneck — that limits the output of the entire system. Improving anything other than the bottleneck is wasted effort.

How to find and fix bottlenecks:
1. **Identify** the constraint (where does work pile up?)
2. **Exploit** it (make sure the bottleneck is never idle)
3. **Subordinate** everything else (adjust other processes to serve the bottleneck)
4. **Elevate** it (invest to increase the bottleneck's capacity)
5. **Repeat** (the constraint has now moved — find the new one)

Example: If your product is great but your sales team can't keep up with leads, sales is the bottleneck. Improving the product further won't grow revenue. Hiring more salespeople will.

---

## 3. Supply Chain and Logistics

For any business that makes or ships physical products:

### The supply chain

```
Raw Materials → Suppliers → Manufacturing → Distribution → Retail → Customer
```

Each link adds cost and time. Supply chain management optimizes the whole chain.

### Key concepts

| Concept | What it is | Why it matters |
|---------|-----------|---------------|
| Lead time | Time from order to delivery | Shorter = more responsive |
| Inventory turnover | How fast inventory sells and is replaced | Higher = less capital tied up |
| Just-in-time (JIT) | Receive materials only when needed | Less inventory cost, but fragile to disruption |
| Safety stock | Buffer inventory for unexpected demand | Insurance against stockouts |
| Supplier diversification | Multiple suppliers for critical inputs | Insurance against supply disruption |

### The bullwhip effect

Small changes in customer demand amplify into large swings upstream in the supply chain. A 10% increase in consumer buying can cause a 40% spike in factory orders because each link in the chain over-reacts.

This is why supply chains are so hard to manage and why companies like Amazon and Walmart invest billions in demand forecasting and inventory management.

---

## 4. Quality Management

### The cost of quality

Quality isn't free — but the cost of *poor* quality is much higher.

| Cost category | Examples |
|--------------|---------|
| Prevention costs | Training, process design, quality planning |
| Appraisal costs | Testing, inspection, QA |
| Internal failure | Rework, scrap, debugging |
| External failure | Returns, refunds, warranty claims, lawsuits, reputation damage |

External failure costs are 10-100x higher than prevention costs. It's always cheaper to build quality in than to fix quality after.

### Continuous improvement (Kaizen)

The philosophy that small, incremental improvements every day compound into massive gains over time. Toyota built the world's most efficient factories not through breakthrough innovations but through millions of tiny improvements over decades.

The cycle:
```
Plan → Do → Check → Act → (repeat forever)
```

1. **Plan** — identify an improvement opportunity
2. **Do** — implement it on a small scale
3. **Check** — measure the results
4. **Act** — if it works, standardize it. If not, learn and try something else.

### The Toyota Production System (TPS) / Lean

The most influential operational philosophy in history. Core principles:

- **Eliminate waste (Muda)** — anything that doesn't add value for the customer is waste. Seven types: overproduction, waiting, transport, over-processing, inventory, motion, defects.
- **Continuous flow** — work should flow smoothly, not sit in batches.
- **Pull, not push** — produce only what's needed, when it's needed (JIT).
- **Built-in quality (Jidoka)** — stop the line when a defect is found. Fix the root cause, not the symptom.
- **Respect for people** — the workers closest to the process know it best. Empower them.

These principles have been adapted far beyond manufacturing — into software (Agile, Kanban), healthcare, and startups (Lean Startup).

---

## 5. Operations for Software Companies

Software companies don't have physical supply chains, but they have operational challenges:

### DevOps and SRE

The operational discipline for keeping software running reliably:

- **CI/CD** — Continuous integration and deployment. Ship small changes frequently.
- **Monitoring & alerting** — Know when things break before users do.
- **Incident management** — When things break, have a process to fix them fast.
- **Post-mortems** — After an incident, figure out why and prevent recurrence.

See [Software Engineering](../../cs/06-engineering/software-engineering.md) for the technical details.

### Customer support operations

- **Tiered support** — L1 handles common issues, L2 handles complex issues, L3 escalates to engineering
- **Knowledge bases** — Self-serve docs reduce support volume
- **SLA management** — Response time guarantees for paying customers
- **Feedback loops** — Support tickets are product feedback. Route them to product teams.

### Onboarding operations

The process of turning a new customer into a successful customer. For enterprise:
- Implementation project management
- Data migration
- Training
- Go-live support
- Success criteria and handoff to ongoing support

Poor onboarding is one of the biggest drivers of early churn.

---

## 6. Automation — When to Systematize

### The automation decision

```
Do you do this task more than once?
    │
    ├── No → Do it manually
    │
    └── Yes → Is it worth the time to automate?
                │
                ├── No (rare, simple) → Document it, do manually
                │
                └── Yes (frequent, complex, error-prone) → Automate it
```

### What to automate first

| High-value automation targets | Why |
|------------------------------|-----|
| Repetitive data entry | Humans are bad at it, computers are perfect |
| Customer onboarding sequences | Consistency matters, scale demands it |
| Invoice generation and billing | Must be accurate, must be timely |
| Report generation | Same format, different data — ideal for automation |
| Lead routing | Speed matters, rules are simple |
| Deployment pipelines | Manual deploys are slow and error-prone |

### What NOT to automate (yet)

- Processes you don't understand well (automate chaos = faster chaos)
- Things that need human judgment (complex negotiations, creative work)
- Processes that change frequently (you'll spend more time maintaining the automation than doing the task)

Rule of thumb: **First do it manually. Then document it. Then automate it.** In that order.

---

## 7. Operational Metrics

| Metric | What it measures | Example |
|--------|-----------------|---------|
| Throughput | Output per unit of time | Orders processed per day |
| Cycle time | Time from start to finish | Days from order to delivery |
| Utilization | % of capacity being used | Server utilization, employee utilization |
| Error rate | % of outputs with defects | Bug rate, return rate |
| Cost per unit | Total cost ÷ units produced | Cost per transaction, per customer served |
| Uptime | % of time the system is operational | 99.9% = 8.76 hours downtime/year |
| NPS / CSAT | Customer satisfaction | Are your operations serving customers well? |

### Operational leverage

The most important concept for scaling:

**Operational leverage** means growing revenue without proportionally growing costs.

- A software company that serves 1,000 customers and 100,000 customers with the same infrastructure has massive operational leverage.
- A consulting firm that needs 10x more consultants to serve 10x more clients has no operational leverage.

This is why software companies are valued so highly. And it's why even non-software companies seek automation and systematization — it creates leverage.

---

## Key Takeaways

1. **Processes are the foundation.** If it's not documented, it's not repeatable. If it's not repeatable, it won't scale.
2. **Find the bottleneck.** The constraint determines the output of the entire system. Fix it first.
3. **Eliminate waste.** Everything that doesn't add customer value is waste. Cut it.
4. **Quality is cheaper than rework.** Prevention costs a fraction of failure costs.
5. **Automate last, not first.** Understand and document the process before automating it.
6. **Operational leverage is the goal.** Grow revenue without proportionally growing costs.

---

## See Also

- [People & Organizations](people-and-organizations.md) — The humans who run the operations
- [Product](../03-customers/product.md) — What operations delivers
- [Growth & Scaling](../06-growth/growth-and-scaling.md) — Scaling requires operational excellence
- [Software Engineering](../../cs/06-engineering/software-engineering.md) — Operations for software systems
