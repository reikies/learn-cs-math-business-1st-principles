# Corporate Finance — Making Decisions With Money

> Corporate finance is the discipline of making financial decisions — how to fund
> the business, where to invest capital, and how to return value to owners. It
> answers the question every business faces: "Given limited resources and uncertain
> outcomes, what should we do with our money?"

**Prerequisites:** [Accounting](../01-foundations/accounting.md) — you need to read financial statements. [Economics](../01-foundations/economics.md) — time value of money, opportunity cost.
**What it enables:** [Fundraising](fundraising.md), [Financial Modeling](financial-modeling.md), M&A, strategic investment decisions.

---

## Why This Matters

Every business decision is, at its core, a financial decision. Should we hire another engineer? Build a new product? Acquire a competitor? Enter a new market? Raise debt or equity?

Corporate finance provides the framework for answering these questions rationally — not based on gut feeling, but on analysis of risk, return, and the time value of money.

---

## 1. The Time Value of Money

The single most important concept in finance:

**A dollar today is worth more than a dollar tomorrow.**

Why?
- You could invest today's dollar and earn a return
- Inflation erodes purchasing power
- The future is uncertain — a promised future dollar might not arrive

### Present Value and Future Value

```
Future Value = Present Value × (1 + r)^n

Present Value = Future Value / (1 + r)^n
```

Where r = rate of return (or discount rate), n = number of periods.

Example: $1,000 invested at 10% annual return:
- Year 1: $1,100
- Year 5: $1,610
- Year 10: $2,594
- Year 20: $6,727

This is the power of compounding — and it's why starting early (whether investing money or building a business) matters so much.

### Discounted Cash Flow (DCF)

The foundational valuation method. To find the value of something, estimate all the future cash flows it will generate and discount them back to today:

```
Value = CF₁/(1+r)¹ + CF₂/(1+r)² + CF₃/(1+r)³ + ... + CFₙ/(1+r)ⁿ
```

Example: A machine generates $10,000/year for 5 years. Discount rate is 10%.

```
Year 1: $10,000 / 1.10  = $9,091
Year 2: $10,000 / 1.21  = $8,264
Year 3: $10,000 / 1.33  = $7,513
Year 4: $10,000 / 1.46  = $6,830
Year 5: $10,000 / 1.61  = $6,209
                           ──────
Total Present Value:      $37,908
```

The machine is worth $37,908 today — not $50,000 (which is the raw sum of cash flows). The difference is the time value of money.

---

## 2. Valuation — What Is a Business Worth?

### DCF valuation (intrinsic value)

Apply DCF to the entire business: estimate future free cash flows, discount them back.

**Free Cash Flow (FCF)** = Operating Cash Flow - Capital Expenditures

This is the cash the business generates after funding its operations and investments. It's what's available to return to investors.

### Comparable company analysis ("comps")

Value a business by comparing it to similar public companies:

```
Your company's value = Your revenue × (comparable company's EV/Revenue multiple)
```

Common multiples:

| Multiple | What it uses | When to use |
|----------|-------------|-------------|
| EV/Revenue | Enterprise Value / Revenue | Early-stage companies without profit |
| EV/EBITDA | Enterprise Value / EBITDA | Profitable companies |
| P/E | Price / Earnings | Public companies |
| EV/ARR | Enterprise Value / Annual Recurring Revenue | SaaS companies |

Example: Comparable SaaS companies trade at 10x ARR. Your startup has $5M ARR. Implied valuation: $50M.

### Precedent transactions

Value based on what similar companies were actually acquired for. More grounded than comps because it reflects real prices paid, including control premiums.

### The startup valuation problem

Early-stage startups have no revenue, no profits, and no comparable transactions. So how do they get valued at $10M or $50M?

The answer: it's a negotiation, not a calculation. The valuation reflects:
- How much money the company needs
- How much equity the founders want to give up
- Market conditions (cheap money → higher valuations)
- Competitive dynamics (multiple investors interested → higher valuations)
- The team's track record

Pre-revenue startup valuations are closer to art than science. Post-revenue, they become more rigorous.

---

## 3. Capital Structure — Debt vs. Equity

Every business is funded by some combination of **debt** (borrowed money) and **equity** (ownership).

### Debt

Money borrowed that must be repaid with interest.

| Pros | Cons |
|------|------|
| Don't give up ownership | Must be repaid regardless of performance |
| Interest is tax-deductible | Collateral may be required |
| Lender has no control over the business | Too much debt = bankruptcy risk |
| Cheaper than equity (usually) | Fixed obligations reduce flexibility |

### Equity

Selling ownership in exchange for capital.

| Pros | Cons |
|------|------|
| No obligation to repay | Dilutes existing owners |
| Investor shares the risk | Investor may want control (board seat, veto rights) |
| Patient capital | More expensive than debt (investor expects high returns) |
| Smart money (expertise, network) | Permanent dilution |

### The capital structure decision

**Weighted Average Cost of Capital (WACC):**

```
WACC = (E/V × Re) + (D/V × Rd × (1-T))
```

Where E = equity value, D = debt value, V = total value, Re = cost of equity, Rd = cost of debt, T = tax rate.

The optimal capital structure minimizes WACC — the blended cost of capital. In practice:
- Early-stage startups use equity (too risky for debt)
- Mature businesses use a mix (debt is cheaper because of the tax deduction)
- Real estate and infrastructure use heavy debt (stable cash flows support it)
- Tech companies often use little debt (cash flows are volatile)

---

## 4. Investment Decisions — Where to Allocate Capital

### Net Present Value (NPV)

The gold standard for investment decisions:

```
NPV = Present Value of Benefits - Present Value of Costs
```

- NPV > 0 → invest (the project creates value)
- NPV < 0 → don't invest (the project destroys value)
- NPV = 0 → indifferent (project breaks even)

Always choose the project with the highest NPV among your options.

### Internal Rate of Return (IRR)

The discount rate at which NPV = 0. Essentially, the "effective return" of the investment.

- If IRR > your required return → invest
- If IRR < your required return → don't invest

IRR is intuitive ("this project returns 25%") but can be misleading for comparing projects of different sizes or durations. NPV is more reliable.

### Payback period

How long until the investment pays for itself.

- Simple, easy to understand
- Ignores cash flows after payback and the time value of money
- Useful as a quick filter, not as the primary decision tool

---

## 5. Working Capital Management

Working capital = Current Assets - Current Liabilities

It's the cash needed to fund day-to-day operations. Managing it poorly kills companies.

### The cash conversion cycle

```
Cash → Buy Inventory → Sell Inventory → Collect Payment → Cash
        (DIO)           (DSO)
```

**Days Inventory Outstanding (DIO)** — how long inventory sits before selling
**Days Sales Outstanding (DSO)** — how long customers take to pay
**Days Payable Outstanding (DPO)** — how long you take to pay suppliers

```
Cash Conversion Cycle = DIO + DSO - DPO
```

A shorter cycle means you get cash back faster. A negative cycle (like Amazon's) means you collect from customers before paying suppliers — you're funded by your operations.

### Why working capital matters for growth

Growth consumes cash. If you sell more, you need more inventory, more receivables pile up, and more cash is tied up. Many profitable, growing companies go bankrupt because they run out of working capital.

This is the **growth paradox**: the faster you grow, the more cash you need, even if you're profitable.

---

## 6. Risk and Return

### The risk-return trade-off

Higher potential return = higher risk. Always.

If someone offers you "high return, low risk," they're either wrong or lying.

```
Expected Return
│              ╱
│            ╱  Stocks
│          ╱
│    ────╱──── Bonds
│      ╱
│    ╱ Cash / T-Bills
│  ╱
└──────────────── Risk
```

### Types of risk

| Risk type | What it is | How to manage it |
|-----------|-----------|------------------|
| Market risk | The entire market moves against you | Diversification, hedging |
| Credit risk | Someone doesn't pay what they owe | Credit checks, diversified customer base |
| Operational risk | Internal failures (systems, people, processes) | Controls, redundancy, insurance |
| Liquidity risk | Can't convert assets to cash when needed | Cash reserves, credit lines |
| Concentration risk | Too dependent on one customer/product/market | Diversification |
| Currency risk | Exchange rates move against you | Hedging, natural hedges |

### The concept of optionality

An **option** is the right — but not the obligation — to do something in the future. Options have value because they let you wait for more information before committing.

In business:
- **Keeping a small team** preserves the option to pivot
- **Raising a bridge round** buys time (the option to find PMF)
- **Building a platform** creates options for future products
- **Pilot programs** are options to expand if they work

Preserving optionality is especially valuable when uncertainty is high — which is always, in a startup.

---

## 7. Mergers & Acquisitions (M&A)

### Why companies acquire

| Reason | Example |
|--------|---------|
| Acquire technology | Google → DeepMind, Apple → various ML startups |
| Acquire customers | Facebook → Instagram, WhatsApp |
| Acquire talent ("acqui-hire") | Large tech companies acquiring small startups for the team |
| Eliminate competition | Facebook → Instagram (partly) |
| Enter new markets | Disney → 21st Century Fox (streaming content) |
| Achieve scale economies | Mergers in banking, telecom, airlines |

### The M&A process

```
Strategy → Target identification → Valuation → Due diligence →
  Negotiation → Deal structure → Regulatory approval → Integration
```

### Why most acquisitions fail

Studies consistently show that 70-90% of acquisitions fail to create the expected value. Common reasons:

- **Overpaying** — winner's curse in bidding wars
- **Culture clash** — the teams don't work together
- **Integration failure** — combining systems, processes, and people is harder than buying them
- **Wrong thesis** — the strategic rationale was flawed
- **Key people leave** — the talent you acquired walks out the door

---

## Key Takeaways

1. **A dollar today > a dollar tomorrow.** Time value of money is the foundation of all financial decisions.
2. **DCF is the fundamental valuation method.** Estimate future cash flows, discount them back.
3. **NPV is the gold standard for investment decisions.** Positive NPV = create value. Negative = destroy value.
4. **Capital structure is a trade-off.** Debt is cheaper but riskier. Equity is safer but dilutive.
5. **Working capital kills growing companies.** Growth consumes cash. Manage your cash conversion cycle.
6. **Risk and return are inseparable.** Higher expected returns require accepting higher risk.
7. **Most acquisitions fail.** Integration is harder than transaction.

---

## See Also

- [Accounting](../01-foundations/accounting.md) — The financial statements these decisions are based on
- [Fundraising & Venture Capital](fundraising.md) — The equity side of capital structure
- [Financial Modeling & Metrics](financial-modeling.md) — Turning these concepts into spreadsheets
- [Economics](../01-foundations/economics.md) — The theoretical foundation
