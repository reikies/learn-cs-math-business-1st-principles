# Product — Building What People Actually Want

> The graveyard of startups is full of beautifully engineered products that nobody
> wanted. Product is the discipline of figuring out *what* to build, *for whom*,
> and *why* — before you spend months building the wrong thing. It's the bridge
> between strategy and execution.

**Prerequisites:** [Competitive Strategy](../02-strategy/competitive-strategy.md) — you need to know where you're playing. [Economics](../01-foundations/economics.md) — you need to understand demand.
**What it enables:** [Marketing](marketing.md), [Sales](sales.md), [Growth](../06-growth/growth-and-scaling.md) — everything downstream depends on having a product worth selling.

---

## Why This Matters

The number one cause of startup failure isn't running out of money — it's building something nobody wants. CB Insights consistently finds that "no market need" is the top reason startups die.

Product thinking is the antidote. It's a systematic approach to understanding customer problems, generating solutions, and iterating until you've built something people can't live without.

---

## 1. Customer Discovery — Find the Problem First

The biggest mistake in product development: starting with a solution.

"We should build an app that..." Wrong.
"Customers struggle with..." Right.

### The Mom Test

Rob Fitzpatrick's framework for talking to potential customers without getting misleading feedback. The rules:

1. **Talk about their life, not your idea.** "Tell me about the last time you tried to [do X]" instead of "Would you use a product that does Y?"
2. **Ask about the past, not the future.** "What did you do?" not "What would you do?"
3. **Talk less, listen more.** You're there to learn, not to pitch.
4. **Get specific.** "How much time did that take?" not "Was it hard?"
5. **Follow the money.** "Have you paid for a solution?" "How much?"

Bad questions:
- "Would you use this?" (They'll say yes to be polite.)
- "What would you pay?" (They'll guess wrong.)
- "Do you think this is a good idea?" (Your mom thinks everything is a good idea.)

Good questions:
- "What's the hardest part about [doing X]?"
- "Tell me about the last time that happened."
- "What solutions have you tried? What was wrong with them?"
- "If you could wave a magic wand, what would change?"

### Jobs to Be Done (JTBD)

People don't buy products. They "hire" products to do a job.

**The milkshake example** (from Clayton Christensen): McDonald's wanted to sell more milkshakes. They studied demographics, ran focus groups, made the shakes better. Sales didn't budge. Then they watched *when* people bought milkshakes. Most buyers were morning commuters. The "job" wasn't "enjoy a milkshake" — it was "make my boring commute interesting and keep me full until lunch." The competition wasn't other milkshakes — it was bagels, bananas, and boredom.

The JTBD framework:

```
When I'm _____________ [situation],
I want to _____________ [motivation],
so I can _____________ [expected outcome].
```

Example: "When I'm managing a remote team, I want to see who's working on what without interrupting them, so I can make decisions without waiting for a meeting."

This reframes your product around the customer's world, not your technology.

---

## 2. Product-Market Fit — The Only Thing That Matters

Marc Andreessen: "Product-market fit means being in a good market with a product that can satisfy that market."

You know you have it when:
- Customers are pulling the product from you (not you pushing it on them)
- Word-of-mouth is driving growth
- Usage grows faster than you can keep up
- Customers complain when it's down (they care)

You know you don't have it when:
- You have to convince people why they need it
- Customers sign up but don't come back
- Growth requires constant marketing spend
- Feedback is polite but not enthusiastic

### The Sean Ellis test

Ask users: **"How would you feel if you could no longer use this product?"**

- If 40%+ say "very disappointed" — you have product-market fit
- If less than 40% — you don't

Before PMF, your *only* job is to find it. Don't scale. Don't optimize. Don't hire. Find PMF first. Everything else is a waste of money without it.

### The PMF pyramid

```
                    ╱╲
                   ╱  ╲     UX
                  ╱    ╲    (usability, delight)
                 ╱──────╲
                ╱        ╲   Feature Set
               ╱          ╲  (functionality that serves the underserved needs)
              ╱────────────╲
             ╱              ╲  Value Proposition
            ╱                ╲ (how you address the needs differently/better)
           ╱──────────────────╲
          ╱                    ╲  Underserved Needs
         ╱                      ╲ (specific unmet needs of target customer)
        ╱────────────────────────╲
       ╱                          ╲  Target Customer
      ╱                            ╲ (who has the problem you're solving)
     ╱──────────────────────────────╲
```

Each layer must be right before the layer above it matters.

---

## 3. The MVP — Minimum Viable Product

The MVP is the smallest thing you can build to test your most important assumption.

### What an MVP is NOT
- A crappy version of your full product
- A prototype with no real users
- A pitch deck
- Something you're embarrassed by (despite what Reid Hoffman says — if it doesn't work at all, it proves nothing)

### What an MVP IS
- A test of your riskiest hypothesis
- Something real users can actually use
- Small enough to build fast, functional enough to get honest feedback

### Types of MVPs

| Type | What it is | When to use it |
|------|-----------|----------------|
| **Landing page** | Describe the product, collect signups | Testing demand before building anything |
| **Wizard of Oz** | Looks automated to users, is actually manual behind the scenes | Testing the experience before building the tech |
| **Concierge** | Manually deliver the service to a few customers | Learning the workflow before automating |
| **Single-feature** | Build one core feature, nothing else | Testing if the core value proposition works |
| **Fake door** | Add a button for a feature that doesn't exist yet, measure clicks | Testing demand for a specific feature |

### The Build-Measure-Learn loop

```
        IDEAS
       ╱      ╲
    Build      Learn
     ╱            ╲
 PRODUCT ──── DATA
    Measure
```

1. **Build** the smallest possible experiment
2. **Measure** what actually happened (not what you hoped would happen)
3. **Learn** whether your hypothesis was right or wrong
4. **Repeat** — pivot or persevere

Speed through this loop is the primary competitive advantage of a startup.

---

## 4. Prioritization — What to Build Next

You will always have more ideas than capacity. Prioritization is the core discipline of product management.

### The RICE framework

Score each potential feature:

```
RICE Score = (Reach × Impact × Confidence) / Effort
```

- **Reach:** How many customers will this affect?
- **Impact:** How much will it affect them? (3 = massive, 0.25 = minimal)
- **Confidence:** How sure are you about the above? (100%, 80%, 50%)
- **Effort:** Person-months to build

### The 2x2 matrix

Simple but effective:

```
                    High Impact
                        │
         Do these  ─────┼───── Do these FIRST
         SECOND         │
                        │
    ────────────────────┼──────────────────────
                        │
         Don't do  ─────┼───── Do these if
         these          │      time permits
                        │
                    Low Impact
              High Effort     Low Effort
```

### ICE scoring

```
ICE = Impact × Confidence × Ease
```

Similar to RICE but simpler. Each dimension scored 1-10. Quick and dirty, good for fast decisions.

### The one question that clarifies everything

For any feature request, ask: **"If we build this, will it help us reach product-market fit (or deepen it)?"**

If yes, consider it. If no, it's a distraction.

---

## 5. User Experience (UX) Principles

### Don't make me think

Steve Krug's principle: every page, every interaction should be self-evident. If users have to think about how to use your product, you've already lost.

### Progressive disclosure

Don't show everything at once. Show the basics first, reveal complexity as needed. Gmail's compose window is simple — the advanced formatting options are hidden until you need them.

### The 80/20 of UX

80% of users use 20% of features. Design for the 80%. Don't clutter the primary experience with power-user features.

### Feedback loops

Every user action should produce visible feedback:
- Button clicked → something visibly changes
- Form submitted → confirmation message
- Error occurred → clear explanation of what went wrong and how to fix it

Silence is the worst UX. Users should never wonder "did that work?"

---

## 6. Product Metrics

What you measure shapes what you build.

### The pirate metrics (AARRR)

Dave McClure's framework:

| Stage | Question | Example Metric |
|-------|----------|---------------|
| **A**cquisition | How do users find you? | Visitors, signups |
| **A**ctivation | Do they have a great first experience? | Completed onboarding, "aha moment" |
| **R**etention | Do they come back? | DAU/MAU, week-over-week retention |
| **R**evenue | Do they pay? | Conversion rate, ARPU |
| **R**eferral | Do they tell others? | NPS, referral rate, viral coefficient |

### The North Star Metric

One metric that best captures the core value your product delivers:

| Company | North Star Metric |
|---------|-------------------|
| Airbnb | Nights booked |
| Slack | Messages sent |
| Spotify | Time spent listening |
| Uber | Rides completed |
| Facebook | Daily active users |

The North Star should correlate with both customer value and business revenue. "Revenue" alone is usually not a good North Star — it's an output, not a driver.

### Cohort analysis

Don't look at aggregate metrics — they hide decay. Instead, group users by when they signed up (cohorts) and track each cohort's behavior over time.

```
        Week 1    Week 2    Week 4    Week 8    Week 12
Jan cohort  100%    60%       40%       30%       25%
Feb cohort  100%    65%       45%       35%       30%
Mar cohort  100%    70%       55%       45%       40%
```

If newer cohorts retain better than older ones, your product is improving. If retention is flat across cohorts, you have a leaky bucket.

---

## 7. Product-Led Growth (PLG)

A strategy where the product itself is the primary driver of acquisition, expansion, and retention.

### How PLG works

```
User discovers product → Free trial/freemium → Uses product → Gets value →
  → Invites colleagues → They get value → Team upgrades to paid → Expands usage
```

The product replaces (or supplements) the traditional sales team.

### PLG characteristics

- **Low friction onboarding** — sign up and get value in minutes, not weeks
- **Self-serve** — users can buy without talking to sales
- **Viral loops** — usage naturally spreads (documents shared, team invites)
- **Usage-based expansion** — revenue grows as usage grows

### Examples

| Company | PLG mechanism |
|---------|--------------|
| Slack | Teams adopt bottom-up. IT buys when enough teams are on it. |
| Dropbox | Share a folder → recipient needs Dropbox → new user |
| Zoom | Join a meeting → "this is easy" → host your own meetings |
| Figma | Share a design → comment on it → start designing |
| Notion | Templates shared publicly → people sign up to use them |

PLG doesn't mean no sales team — it means the sales team focuses on expansion and enterprise deals while the product handles initial adoption.

---

## Key Takeaways

1. **Start with the problem, not the solution.** Customer discovery before product development.
2. **Product-market fit is the only thing that matters** before you scale.
3. **The MVP is a test, not a product.** Build the smallest thing that validates your riskiest assumption.
4. **Prioritize ruthlessly.** Say no to most things. Build what moves the needle.
5. **Measure what matters.** Retention is the most important metric. Everything else is vanity without it.
6. **The product is the growth engine** in PLG companies. Reduce friction relentlessly.

---

## See Also

- [Product Thinking](product-thinking.md) — The "should this exist?" mindset that precedes the process
- [Competitive Strategy](../02-strategy/competitive-strategy.md) — The strategic context that shapes product decisions
- [Marketing & Distribution](marketing-and-distribution.md) — Getting people to know your product exists
- [Marketing](marketing.md) — How you communicate the product's value
- [Sales](sales.md) — How you convert product interest into revenue
- [The Startup Journey](../07-entrepreneurship/the-startup-journey.md) — How product evolves through the startup lifecycle
