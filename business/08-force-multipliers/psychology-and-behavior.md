# Psychology & Behavioral Science — Why People Do What They Do

> Marketing is applied psychology. UX design is applied psychology. Sales is
> applied psychology. Negotiation is applied psychology. Prompt engineering is
> partly psychology — you're steering a language model's "behavior." If you
> understand why people make decisions, you have leverage over every discipline
> that involves humans. Which is all of them.

**Prerequisites:** None. This is a lens, not a technical stack.
**What it multiplies:** Everything. [Marketing](../03-customers/marketing.md), [Sales](../03-customers/sales.md), [Product](../03-customers/product.md), [Design](design-basics.md), [Writing](writing-and-communication.md), [People & Organizations](../04-operations/people-and-organizations.md).

---

## Why This Matters

People are not rational. They are *predictably* irrational. They make systematic errors in judgment, follow emotional impulses dressed up as logic, and are influenced by context far more than they realize.

This isn't cynicism — it's science. Decades of research in behavioral economics and cognitive psychology have mapped the patterns. Understanding these patterns doesn't mean manipulating people. It means designing products, writing messages, and building organizations that work *with* human nature instead of against it.

---

## 1. Cognitive Biases — The Shortcuts That Shape Decisions

The brain processes millions of inputs per second. To cope, it uses heuristics — mental shortcuts. These shortcuts are usually helpful. When they're not, we call them biases.

### The biases that matter most for builders

| Bias | What it is | Where it shows up |
|------|-----------|------------------|
| **Anchoring** | The first number you hear sets the frame for everything after | Pricing ("Was $200, now $99"), negotiations, salary expectations |
| **Loss aversion** | Losses feel ~2x worse than equivalent gains feel good | Why free trials convert better than discounts. Why people stick with bad products (fear of switching cost). |
| **Social proof** | People follow what others do, especially under uncertainty | Reviews, testimonials, "10,000 teams trust us", GitHub stars |
| **Status quo bias** | People prefer the current state, even when alternatives are better | Why switching costs are such a powerful moat. Why "do nothing" is your biggest competitor. |
| **Confirmation bias** | People seek information that supports what they already believe | Why customer interviews go wrong (they tell you what you want to hear). Why A/B tests get stopped early. |
| **Availability heuristic** | People judge probability by how easily examples come to mind | Why rare, dramatic events (plane crashes) feel more likely than common ones (car accidents) |
| **Sunk cost fallacy** | People continue investing because of what they've already spent, not future returns | Why founders don't pivot. Why companies keep bad employees. Why users stay on platforms they don't like. |
| **Framing effect** | How something is presented changes how it's perceived | "95% survival rate" vs. "5% mortality rate" — same fact, completely different reaction |
| **Dunning-Kruger** | Low skill + low awareness = overconfidence. High skill + high awareness = underconfidence. | Why junior hires seem confident and senior people say "it depends." Why founders overestimate their product. |
| **Halo effect** | Positive impression in one area spills over into others | Beautiful design → perceived as reliable. Famous founder → perceived as smart about everything. |

### Using biases ethically

There's a line between **designing with human psychology** and **exploiting it**:

| Ethical use | Exploitative use |
|-------------|-----------------|
| Anchoring: showing the value before the price | Anchoring: inflating a fake "original price" |
| Social proof: showing genuine testimonials | Social proof: fabricating reviews |
| Loss aversion: reminding users what they'll miss (real value) | Loss aversion: dark patterns that make canceling feel dangerous |
| Default to the best option for the user | Default to the most profitable option for you |

The test: **"Would the user thank me for this design choice if they understood it?"** If yes, it's alignment. If no, it's manipulation.

---

## 2. Decision-Making — How People Actually Choose

### Rational economic model vs. reality

```
Classical economics:
  Person evaluates all options → Assigns utility values → 
  Selects the option with maximum expected utility

What actually happens:
  Person notices 2-3 options → Evaluates them based on 
  emotion and a few salient features → Picks the one that 
  "feels right" → Rationalizes the choice afterward
```

### System 1 vs. System 2 (Kahneman)

| System 1 (Fast) | System 2 (Slow) |
|-----------------|-----------------|
| Automatic, effortless | Deliberate, effortful |
| Intuitive | Analytical |
| Always on | Lazy (engaged only when necessary) |
| Emotional | Logical |
| Pattern-matching | Computing |

**Implications for product and marketing:**
- **Landing pages** target System 1. You have seconds. Use emotion, clarity, and social proof.
- **Enterprise sales** engages System 2. The buyer needs ROI analysis, case studies, and logical arguments.
- **Onboarding** should start with System 1 (simple, intuitive) and gradually introduce System 2 (learning, configuring).
- **Pricing pages** must satisfy both. System 1: "this feels like a good deal." System 2: "the math works out."

### Choice architecture

How you present choices shapes what people choose. This is "nudging" (Thaler & Sunstein).

| Principle | How it works | Example |
|-----------|-------------|---------|
| Defaults | People usually accept the default | Opt-in vs. opt-out changes participation from ~20% to ~90% |
| Reduce options | Too many choices → decision paralysis | 3 pricing tiers, not 7 |
| Order effects | First and last options get more attention | Put your preferred plan in the middle, labeled "Most Popular" |
| Decoy effect | An inferior option makes another option look better | $10/mo basic, $25/mo pro, $27/mo enterprise → $25 looks like a steal |

---

## 3. Motivation — Why People Act (or Don't)

### Intrinsic vs. extrinsic motivation

| Type | What drives it | Effect |
|------|---------------|--------|
| Intrinsic | Curiosity, mastery, purpose, autonomy | Sustainable, self-reinforcing |
| Extrinsic | Money, rewards, punishment, external approval | Effective short-term, can undermine intrinsic motivation |

**The overjustification effect:** Paying people for something they already enjoy doing can *reduce* their motivation. This is why gamification (badges, points) can backfire — it replaces intrinsic satisfaction with extrinsic score-chasing.

### Self-Determination Theory (Deci & Ryan)

Three fundamental human needs that drive engagement:

```
AUTONOMY     → "I have control and choice"
COMPETENCE   → "I'm getting better at this"
RELATEDNESS  → "I'm connected to others"
```

Products that satisfy these needs retain users. Products that undermine them lose users.

| Need | Product design that satisfies it |
|------|--------------------------------|
| Autonomy | Customization, flexible workflows, user control |
| Competence | Progress indicators, skill development, clear feedback |
| Relatedness | Community features, collaboration, shared experiences |

### The Fogg Behavior Model

Behavior happens when three things converge:

```
B = M × A × P

Behavior = Motivation × Ability × Prompt
```

- **Motivation** — How much does the person want to do it?
- **Ability** — How easy is it to do?
- **Prompt** — Is there a trigger at the right moment?

If any one of these is zero, the behavior doesn't happen. This is why:
- High-motivation, low-ability = frustrated users (simplify the product)
- High-ability, low-motivation = need better positioning (solve a real problem)
- High-motivation, high-ability, no prompt = missed opportunity (send the email, show the notification)

---

## 4. Habit Formation — How Products Become Daily Use

### The Hook Model (Nir Eyal)

```
Trigger → Action → Variable Reward → Investment
   ↑                                      │
   └──────────────────────────────────────┘
```

1. **Trigger** — External (notification, email) or internal (boredom, anxiety, curiosity)
2. **Action** — The simplest behavior in anticipation of reward (open app, scroll, search)
3. **Variable reward** — Unpredictable satisfaction (new content, likes, useful results). Variable is key — predictable rewards habituate quickly.
4. **Investment** — User puts something in (data, preferences, followers, content) that makes the product more valuable and creates the next trigger

### Examples

| Product | Trigger | Action | Variable Reward | Investment |
|---------|---------|--------|-----------------|------------|
| Twitter/X | Boredom, FOMO | Open app, scroll | Interesting tweets, engagement on your posts | Followers, tweets, profile |
| Slack | Notification | Check message | New information, social interaction | Message history, channels, integrations |
| Spotify | Mood, routine | Open app, play | Discover new music, personalized playlists | Listening history, saved songs |

### Ethical considerations of habit design

There's a spectrum from **helpful habits** (exercise apps, learning tools) to **addictive patterns** (infinite scroll, slot machine mechanics). Ask:

- Does this habit make the user's life better?
- Can the user easily disengage when they want to?
- Are we optimizing for user value or for engagement metrics?

---

## 5. Trust — The Currency of Business

### How trust is built

```
Trust = (Credibility + Reliability + Intimacy) / Self-Orientation
                                                    — Maister's Trust Equation
```

- **Credibility** — "I believe what you say" (expertise, credentials, track record)
- **Reliability** — "I can count on you" (consistency, follow-through)
- **Intimacy** — "I feel safe with you" (empathy, discretion, vulnerability)
- **Self-orientation** — "You care about yourself more than me" (the denominator — high self-orientation destroys trust)

### Trust in products

| Trust signal | How to build it |
|-------------|----------------|
| Social proof | Real testimonials, case studies, user counts |
| Transparency | Clear pricing, honest limitations, open communication |
| Competence | Product that works. Fast, reliable, no bugs. |
| Risk reversal | Free trials, money-back guarantees, easy cancellation |
| Consistency | Brand promises matched by experience |

### Trust in AI products specifically

AI has a trust deficit. Users don't understand how it works and have been burned by hype.

To build trust in AI:
- **Show your work.** Cite sources, explain reasoning, provide confidence levels.
- **Admit uncertainty.** "I'm not sure about this" builds more trust than false confidence.
- **Be predictable.** Consistent behavior builds trust. Inconsistent outputs erode it.
- **Let users verify.** Don't ask for blind trust. Give them the tools to check.
- **Fail gracefully.** When wrong, be clearly wrong (not subtly wrong). Easy to catch > hard to detect.

---

## 6. Persuasion — The Science of Influence

### Cialdini's Six Principles of Influence

| Principle | How it works | Business application |
|-----------|-------------|---------------------|
| **Reciprocity** | People feel obligated to return favors | Free value (content, tools, trials) creates obligation to reciprocate (buy, refer) |
| **Commitment & consistency** | People want to be consistent with what they've already said/done | Small commitments lead to big ones. Free trial → paid. Survey → demo. |
| **Social proof** | People follow others, especially in uncertainty | Testimonials, user counts, case studies, "most popular" tags |
| **Authority** | People defer to experts and authority figures | Expert endorsements, certifications, published research, media logos |
| **Liking** | People say yes to those they like | Personal brands, storytelling, shared values, humor |
| **Scarcity** | People value what's rare or about to disappear | Limited-time offers, exclusive access, waitlists |

### Persuasion in different contexts

| Context | Primary principles | Why |
|---------|-------------------|-----|
| Landing pages | Social proof, authority, scarcity | Quick decisions, low attention |
| Enterprise sales | Authority, reciprocity, commitment | Slow decisions, multiple stakeholders |
| Fundraising | Social proof, authority, scarcity | Investors follow other investors |
| Hiring | Liking, authority, social proof | Candidates choose culture and credibility |
| Negotiation | Reciprocity, commitment, anchoring | Finding mutual value |

---

## 7. Psychology of Pricing

Pricing is almost entirely psychological. The "right" price has less to do with cost than with perception.

### Key pricing psychology concepts

| Concept | What it means | Example |
|---------|--------------|---------|
| Anchoring | The first price sets the reference point | Show enterprise price first → pro plan feels cheap |
| Charm pricing | $9.99 feels meaningfully less than $10 | Works for consumer. Avoid for enterprise (looks cheap). |
| Price-quality inference | Higher price = higher quality (in the buyer's mind) | Luxury brands. Premium SaaS positioning. |
| Pain of paying | Every payment triggers a small "pain" | Subscriptions reduce pain (one decision, not repeated ones) |
| Bundling | Combining items obscures individual prices | SaaS suites, "unlimited" plans |
| Free | $0 is not just a low price — it's a different psychological category | Free tier removes all friction. But "free" can also signal "not valuable." |
| Decoy pricing | An unattractive option makes another option look better | Small ($5), Medium ($14), Large ($15) → everyone picks Large |

---

## 8. Applied Psychology — Where It All Connects

| Discipline | Key psychological principles at work |
|-----------|--------------------------------------|
| **Product design** | Habit loops, progressive disclosure, feedback, cognitive load reduction |
| **UX/UI** | Gestalt principles (grouping, proximity, similarity), attention, memory limits |
| **Marketing** | Persuasion, framing, social proof, storytelling, emotional appeals |
| **Sales** | Trust equation, reciprocity, commitment, objection handling, mirroring |
| **Pricing** | Anchoring, loss aversion, framing, decoy effect, pain of paying |
| **Hiring** | Halo effect, confirmation bias, structured interviews to reduce bias |
| **Leadership** | Motivation theory, autonomy, mastery, purpose, psychological safety |
| **Negotiation** | Anchoring, BATNA, framing, reciprocity, fairness norms |
| **Prompt engineering** | Framing effects, role-playing, chain-of-thought (steering model "behavior") |

Every time you interact with a human — or design a system that interacts with humans — psychology is operating. The question is whether you're aware of it or not.

---

## Key Takeaways

1. **People are predictably irrational.** Understanding the patterns gives you leverage in every discipline.
2. **System 1 makes most decisions.** Design for emotion and intuition first, logic second.
3. **Defaults are powerful.** How you present choices shapes what people choose.
4. **Habits are built with hooks.** Trigger → action → variable reward → investment → repeat.
5. **Trust is the denominator.** Without trust, no amount of persuasion or design works.
6. **Persuasion is not manipulation.** It's making the right choice easy and clear.
7. **Pricing is psychology.** The "right" price is the one that matches perceived value, not cost.

---

## See Also

- [Product Thinking](../03-customers/product-thinking.md) — User research and understanding what to build
- [Marketing](../03-customers/marketing.md) — Positioning, brand, and persuasive messaging
- [Sales](../03-customers/sales.md) — The psychology of closing deals
- [Design (Basic UI/UX)](design-basics.md) — Applying psychology to interface design
- [Writing & Communication](writing-and-communication.md) — Persuasive writing and framing
- [People & Organizations](../04-operations/people-and-organizations.md) — Motivation, leadership, and organizational behavior
