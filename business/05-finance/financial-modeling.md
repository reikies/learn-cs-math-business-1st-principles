# Financial Modeling & Metrics — Measuring and Predicting the Business

> A financial model is a simplified representation of your business in numbers.
> It forces you to articulate every assumption — how many customers you'll acquire,
> what they'll pay, how long they'll stay, what it costs to serve them — and see
> whether the math works. It's not about predicting the future. It's about
> understanding the present well enough to make better decisions.

**Prerequisites:** [Accounting](../01-foundations/accounting.md) — financial statements, revenue recognition. [Corporate Finance](corporate-finance.md) — valuation, time value of money. [Business Models](../02-strategy/business-models.md) — revenue model patterns.
**What it enables:** Fundraising (investors expect models), strategic planning, budgeting, scenario analysis.

---

## Why This Matters

"We'll figure out the numbers later" is a recipe for running out of money.

A financial model isn't a prediction — it's a thinking tool. Building one forces you to answer hard questions:
- How many customers do we need to break even?
- Can we afford to hire five more people?
- How long will our runway last at this burn rate?
- What happens if growth slows by 50%?

The model doesn't need to be right. It needs to be useful. A model that's wrong but internally consistent tells you *which assumptions matter most* — which is more valuable than a prediction.

---

## 1. The Three-Statement Model

The foundation of all financial modeling. Three interconnected financial statements projected forward:

```
┌────────────────────┐
│  Income Statement   │  (Revenue - Costs = Profit)
│  (P&L)              │
└────────┬───────────┘
         │ Net Income flows down
         ▼
┌────────────────────┐
│  Balance Sheet      │  (Assets = Liabilities + Equity)
│                     │
└────────┬───────────┘
         │ Changes in balance sheet items
         ▼
┌────────────────────┐
│  Cash Flow Statement│  (Where cash comes from and goes)
│                     │
└────────────────────┘
```

Each statement feeds the others:
- Net income from the P&L flows into retained earnings on the balance sheet
- Changes in balance sheet items (receivables, payables, inventory) flow into the cash flow statement
- Cash flow changes update the cash position on the balance sheet

---

## 2. Building a SaaS Model

SaaS (Software as a Service) is the most common model for tech startups. Here's how to model it.

### Revenue drivers

```
MRR (Monthly Recurring Revenue) =
    Existing MRR
  + New MRR (from new customers)
  + Expansion MRR (from upsells/upgrades)
  - Churned MRR (from lost customers)
  - Contraction MRR (from downgrades)
```

ARR (Annual Recurring Revenue) = MRR × 12

### The SaaS model structure

```
INPUTS (your assumptions)
├── New customers per month
├── Average contract value (ACV)
├── Monthly churn rate
├── Expansion rate (net revenue retention)
├── CAC (customer acquisition cost)
├── Gross margin
├── Headcount plan and salaries
└── Other operating expenses

CALCULATIONS
├── Revenue build-up (cohort-by-cohort)
├── COGS (hosting, support)
├── Gross profit
├── Operating expenses (S&M, R&D, G&A)
├── Operating income / loss
├── Cash burn / generation
└── Runway

OUTPUTS
├── Monthly and annual P&L
├── Cash flow projection
├── Key metrics (LTV, CAC, LTV/CAC, Rule of 40, etc.)
└── Scenario comparisons
```

### Cohort-based modeling

The most accurate way to model a subscription business. Track each month's new customers as a cohort and model their behavior over time:

```
         Month 1   Month 2   Month 3   Month 4   Month 5
Jan cohort  100      95        90        86        82
Feb cohort   -      120       114       108       103
Mar cohort   -       -        150       142       135
Apr cohort   -       -         -        130       124
May cohort   -       -         -         -        140

Total MRR   100     215       354       466       584
(× ACV)
```

Each cohort shrinks by the churn rate each month. This automatically captures the compounding effect of retention.

---

## 3. Unit Economics — The Building Blocks

### Customer Lifetime Value (LTV)

```
Simple LTV = ARPU / Monthly Churn Rate

Example: $100/month ARPU, 3% monthly churn
LTV = $100 / 0.03 = $3,333
```

More precise (with gross margin):
```
LTV = (ARPU × Gross Margin) / Monthly Churn Rate
LTV = ($100 × 0.80) / 0.03 = $2,667
```

### Customer Acquisition Cost (CAC)

```
CAC = Total Sales & Marketing Spend / New Customers Acquired

Example: $300K in S&M, 100 new customers
CAC = $300,000 / 100 = $3,000
```

### The key ratios

| Metric | Formula | Healthy benchmark |
|--------|---------|-------------------|
| LTV/CAC | LTV ÷ CAC | > 3x |
| CAC Payback | CAC ÷ (Monthly ARPU × Gross Margin) | < 12 months |
| Net Revenue Retention (NRR) | (Starting MRR + Expansion - Churn - Contraction) / Starting MRR | > 100% (ideally > 120%) |
| Gross margin | (Revenue - COGS) / Revenue | > 70% for SaaS |
| Rule of 40 | Revenue growth rate + profit margin | > 40% |

### The magic of NRR > 100%

If your Net Revenue Retention is above 100%, existing customers spend *more* over time. This means you grow even without acquiring new customers:

```
Year 1: 100 customers × $1,000 ACV = $100K ARR
Year 2: Same 100 customers × $1,200 (120% NRR) = $120K ARR
Year 3: Same 100 customers × $1,440 (120% NRR) = $144K ARR
```

New customer acquisition stacks on top. This is why high-NRR SaaS companies are valued so highly.

---

## 4. Burn Rate and Runway

### Burn rate

```
Gross Burn = Total monthly cash outflow
Net Burn = Total monthly cash outflow - Total monthly cash inflow
```

Example: You spend $200K/month and earn $50K/month.
- Gross burn: $200K
- Net burn: $150K

### Runway

```
Runway = Cash in bank / Net Burn Rate

Example: $3M cash / $150K net burn = 20 months of runway
```

### The "default alive or default dead" test (Paul Graham)

Given your current:
- Revenue growth rate
- Expense growth rate
- Cash in the bank

Will you reach profitability before running out of money?

If yes → "default alive" (you survive even without raising more money)
If no → "default dead" (you must raise or cut costs to survive)

Every founder should know which category they're in.

---

## 5. Key Metrics by Business Type

### SaaS metrics

| Metric | What it tells you | Target |
|--------|-------------------|--------|
| ARR | Total annualized recurring revenue | Growth rate matters more than absolute |
| MRR growth | Month-over-month revenue growth | 10-20% m/m early, 5-10% later |
| Churn (logo) | % of customers lost | < 5% annually (enterprise), < 7% monthly (SMB) |
| Churn (revenue) | % of revenue lost | < 5% annually |
| NRR | Revenue expansion from existing customers | > 110% |
| Gross margin | Revenue after direct costs | > 70% |
| CAC payback | Months to recoup acquisition cost | < 18 months |
| LTV/CAC | Customer value vs. acquisition cost | > 3x |

### Marketplace metrics

| Metric | What it tells you |
|--------|-------------------|
| GMV | Gross Merchandise Value — total transaction volume |
| Take rate | Revenue as % of GMV |
| Liquidity | % of listings that sell |
| Supply/demand balance | Ratio of sellers to buyers |
| Repeat rate | % of users who transact again |

### E-commerce metrics

| Metric | What it tells you |
|--------|-------------------|
| AOV | Average Order Value |
| Conversion rate | % of visitors who buy |
| Cart abandonment | % who start but don't finish checkout |
| Repeat purchase rate | % of customers who buy again |
| Contribution margin | Revenue minus variable costs per order |

---

## 6. Scenario Analysis

A good model isn't one forecast — it's multiple scenarios.

### The three scenarios

| Scenario | What it assumes | What it's for |
|----------|----------------|---------------|
| Base case | Your best estimate | Default planning |
| Upside case | Things go better than expected | Understanding the opportunity |
| Downside case | Things go worse than expected | Stress testing survival |

### Sensitivity analysis

Which inputs matter most? Change one variable at a time and see how the output changes:

```
                    Impact on 3-year revenue
                    ─────────────────────────
Churn rate          ████████████████████  (very high impact)
New customer growth ███████████████████   (very high impact)
ACV                 █████████████         (high impact)
CAC                 ████████              (moderate impact)
Gross margin        ██████                (moderate impact)
OpEx growth         ████                  (lower impact)
```

If small changes in churn dramatically change your outcome, churn is where you should focus. This is the real value of modeling — not predicting the future, but understanding which levers matter.

---

## 7. Dashboards and Reporting

### What to track (and how often)

| Frequency | Metrics |
|-----------|---------|
| Daily | Cash balance, new signups, active users |
| Weekly | Revenue, pipeline, churn events, support tickets |
| Monthly | Full P&L, burn rate, runway, unit economics, cohort analysis |
| Quarterly | Board deck: ARR, growth rate, NRR, CAC, runway, headcount plan |

### The board deck

What investors and board members want to see quarterly:

1. **Highlights and lowlights** — what went well, what didn't
2. **Key metrics** — ARR, growth, churn, NRR, burn, runway
3. **P&L** — actual vs. plan
4. **Product update** — what shipped, what's next
5. **Team** — headcount, key hires, open roles
6. **Ask** — what help do you need from the board?

### Common modeling mistakes

| Mistake | Why it's wrong |
|---------|---------------|
| Hockey stick revenue with flat costs | Revenue requires investment. Costs grow with revenue. |
| 100% year-over-year growth sustained for 5 years | Almost no company sustains this. Growth rates decay. |
| No churn in the model | Every business has churn. Ignoring it is dishonest. |
| "Conservative" assumptions that aren't | If your "conservative" case still shows 10x growth, it's not conservative. |
| Too much precision | Don't model to the dollar in year 5. Use ranges. |
| Bottom-up revenue, top-down costs | If revenue is built customer by customer, costs should be too. |

---

## 8. The Model as Communication Tool

Your model isn't just for you. It communicates your thinking to:

- **Investors** — "We've thought rigorously about the business"
- **Board members** — "Here's our plan and how we're tracking against it"
- **Team leads** — "Here's your budget and targets"
- **Yourself** — "Here's what has to be true for us to survive"

The best models are simple enough that a non-financial person can follow the logic but rigorous enough that a CFO can't poke holes in it.

---

## Key Takeaways

1. **A model is a thinking tool, not a crystal ball.** It's about understanding which assumptions matter.
2. **Build cohort-based models for subscriptions.** They accurately capture retention dynamics.
3. **LTV/CAC ≥ 3 and CAC payback < 18 months.** These are the minimum thresholds for a viable business.
4. **NRR > 100% is magic.** Existing customers growing means you win even without new customers.
5. **Know your burn rate and runway.** Always. "Default alive or default dead" is the most important question.
6. **Run scenarios.** Base, upside, and downside. The downside is where you learn the most.
7. **Sensitivity analysis reveals what matters.** Focus your effort on the inputs with the biggest impact.

---

## See Also

- [Accounting](../01-foundations/accounting.md) — The financial statements these models project
- [Corporate Finance](corporate-finance.md) — Valuation and investment decision frameworks
- [Business Models](../02-strategy/business-models.md) — The revenue patterns you're modeling
- [Fundraising](fundraising.md) — Investors will scrutinize your model
