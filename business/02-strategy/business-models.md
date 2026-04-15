# Business Models — How Companies Capture Value

> A business model answers the most fundamental question in commerce: how do you
> make money? You can build the greatest product in the world, but without a model
> for capturing value, you have a project, not a business. The model is the machine
> that turns value creation into value capture.

**Prerequisites:** [Economics](../01-foundations/economics.md) — supply, demand, and pricing. [Competitive Strategy](competitive-strategy.md) — positioning determines which models work.
**What it enables:** [Financial Modeling](../05-finance/financial-modeling.md), [Fundraising](../05-finance/fundraising.md), [Growth](../06-growth/growth-and-scaling.md).

---

## Why This Matters

Instagram had 30 million users and no revenue when Facebook acquired it for $1 billion. Was it a business? Not yet — it was a product with massive distribution and no business model.

Twitter had a business model (advertising) but struggled for years to make it profitable. The model existed, but it didn't work well enough.

Stripe built a business model (take a percentage of every transaction) that is so aligned with its customers' success that it barely needs a sales team.

The business model is not an afterthought — it's the core design decision that shapes everything else: what you build, who you sell to, how you grow, and whether you survive.

---

## 1. The Business Model Canvas

A simple framework for mapping any business model. Nine building blocks:

```
┌─────────────┬──────────────┬──────────────┬──────────────┬─────────────┐
│             │              │              │              │             │
│   Key       │   Key        │   Value      │  Customer    │  Customer   │
│  Partners   │ Activities   │ Proposition  │ Relationships│  Segments   │
│             │              │              │              │             │
│             ├──────────────┤              ├──────────────┤             │
│             │              │              │              │             │
│             │   Key        │              │   Channels   │             │
│             │ Resources    │              │              │             │
│             │              │              │              │             │
├─────────────┴──────────────┴──────┬───────┴──────────────┴─────────────┤
│                                   │                                    │
│         Cost Structure            │         Revenue Streams            │
│                                   │                                    │
└───────────────────────────────────┴────────────────────────────────────┘
```

| Block | Question it answers |
|-------|-------------------|
| Customer Segments | Who are you creating value for? |
| Value Proposition | What problem do you solve? Why should they care? |
| Channels | How do you reach customers? |
| Customer Relationships | How do you acquire, retain, and grow customers? |
| Revenue Streams | How do you make money? |
| Key Resources | What assets do you need? |
| Key Activities | What must you do exceptionally well? |
| Key Partners | Who do you need to work with? |
| Cost Structure | What are your biggest costs? |

The canvas is useful for mapping existing businesses and for designing new ones. The real insight comes from understanding how the blocks connect — how your value proposition determines your customer segments, which determine your channels, which determine your cost structure.

---

## 2. Revenue Model Patterns

### Subscription / SaaS

Recurring payments for ongoing access.

```
Revenue = Number of subscribers × Price per period
```

- **Examples:** Netflix, Spotify, Salesforce, Adobe Creative Cloud
- **Strengths:** Predictable revenue, high LTV, compounding growth
- **Weaknesses:** Churn is existential — even 5% monthly churn means you replace half your customers yearly
- **Key metric:** Net Revenue Retention (NRR). Above 100% means existing customers spend more over time, even without new customers.

### Transaction fee / Take rate

Take a percentage of each transaction you facilitate.

```
Revenue = Transaction volume × Take rate (%)
```

- **Examples:** Stripe (2.9% + $0.30), Airbnb (~14%), App Store (15-30%), Uber (~25%)
- **Strengths:** Scales with customer success. Zero revenue = zero usage (aligned incentives).
- **Weaknesses:** You need massive volume. Take rate under competitive pressure.
- **Key metric:** Gross Merchandise Volume (GMV) and take rate.

### Advertising

Attention is the product. Users get the service free. Advertisers pay for access to users.

```
Revenue = Impressions × CPM (cost per thousand) or Clicks × CPC
```

- **Examples:** Google, Facebook/Meta, YouTube, newspapers
- **Strengths:** Massive scale potential. Low friction for users.
- **Weaknesses:** Requires enormous audiences. Privacy concerns. You're selling your users.
- **Key metric:** Revenue per user (ARPU), DAU/MAU ratio.

### Licensing

Sell the right to use your technology/IP for a fee.

```
Revenue = Number of licenses × Price per license
```

- **Examples:** Microsoft (Windows, Office — historically), ARM (chip designs), Qualcomm (patents)
- **Strengths:** High margins. IP is expensive to create but cheap to reproduce.
- **Weaknesses:** Piracy risk. One-time revenue unless you sell upgrades.

### Freemium

Give the basic product away. Charge for premium features.

```
Free tier → captures users → some % convert to paid → revenue
```

- **Examples:** Dropbox, Slack, Zoom, LinkedIn
- **Strengths:** Low friction acquisition. The free product is your marketing.
- **Weaknesses:** Conversion rates are typically 2-5%. Most users never pay. You subsidize free users with paying ones.
- **Key metric:** Free-to-paid conversion rate, time to conversion.

### Marketplace

Connect buyers and sellers. Take a cut of the transaction.

```
Revenue = Transactions × Commission rate
```

- **Examples:** eBay, Amazon Marketplace, Etsy, Uber, Airbnb
- **Strengths:** Network effects create moats. Asset-light (you don't own inventory).
- **Weaknesses:** The cold-start problem (need both sides simultaneously). Disintermediation risk (buyers and sellers cut you out).
- **Key metric:** Liquidity (% of listings that result in transactions).

### Hardware + consumables (Razor and blade)

Sell the hardware cheap (or at cost), make money on recurring consumables.

```
Revenue = (Hardware × low margin) + (Consumables × high margin × frequency)
```

- **Examples:** Printers + ink, Keurig + pods, game consoles + games, Tesla + Supercharging
- **Strengths:** Lock-in. Recurring revenue after initial sale.
- **Weaknesses:** If someone makes compatible consumables, your model breaks.

### Enterprise / custom pricing

High-touch sales with negotiated contracts, usually annual.

```
Revenue = ACV (Annual Contract Value) × Number of customers
```

- **Examples:** Palantir, Snowflake, large Salesforce deals
- **Strengths:** High ACV, long contracts, deep integration (switching costs).
- **Weaknesses:** Long sales cycles, high CAC, customer concentration risk.

---

## 3. Pricing Strategy

Pricing is one of the most powerful and least understood levers in business.

### Cost-plus pricing
Add a markup to your costs: `Price = Cost + Margin`

Simple but flawed. It ignores willingness to pay. If your cost is $5 and customers would pay $50, you're leaving $45 on the table. If your cost is $5 and customers would only pay $3, no markup saves you.

### Value-based pricing
Price based on the **value delivered** to the customer, not your cost.

If your software saves a company $100,000/year, charging $20,000/year is a no-brainer for them — and vastly more profitable for you than cost-plus.

This is the correct approach for most B2B products.

### Competitive pricing
Price relative to alternatives. Useful in commoditized markets.

### Price discrimination (tiered pricing)
Charge different customers different prices based on willingness to pay.

```
Starter:     $29/month   (individuals, small teams)
Pro:         $99/month   (growing companies)
Enterprise:  Custom      (large organizations with budget)
```

This captures more total value by serving multiple segments. The enterprise customer pays more because they get more value (and have more budget). The individual pays less but still contributes revenue you'd otherwise miss.

### The psychology of pricing

| Tactic | How it works |
|--------|-------------|
| Anchoring | Show the expensive option first. Everything after feels cheaper. |
| Decoy pricing | Add a third option that makes the target option look like a deal. |
| Ending in 9 | $99 feels significantly cheaper than $100 (irrational, but proven). |
| Free trial | Let them experience value before asking for money. Loss aversion keeps them. |
| Annual discount | Offer 2 months free on annual billing. You get cash upfront + lower churn. |

---

## 4. Unit Economics — The Math of Survival

Two numbers determine whether your business model works:

### Customer Lifetime Value (LTV)

Total revenue from an average customer over their entire relationship with you.

```
LTV = Average Revenue Per User (ARPU) × Average Customer Lifetime

Or for subscriptions:
LTV = ARPU / Churn Rate
```

If a customer pays $100/month and stays for 30 months, LTV = $3,000.

### Customer Acquisition Cost (CAC)

Total cost to acquire one customer.

```
CAC = Total Sales & Marketing Spend / Number of New Customers
```

### The golden ratio

```
LTV / CAC ≥ 3
```

If LTV/CAC < 1, you lose money on every customer. If it's between 1 and 3, you're probably not profitable after accounting for overhead. Above 3 is healthy. Above 5, you should be spending *more* on acquisition — you're leaving growth on the table.

### CAC payback period

How many months until you recoup the cost of acquiring a customer.

```
CAC Payback = CAC / Monthly ARPU
```

For venture-backed startups, under 12 months is good. Under 6 months is great. Over 18 months means you need a lot of capital to grow.

---

## 5. Monetization Timing

When you introduce revenue matters as much as how.

### Monetize early
- **Pros:** Validates willingness to pay. Focuses you on paying customers. Generates revenue.
- **Cons:** Limits growth. May turn away users who would convert later.
- **Best for:** B2B products, tools with clear ROI, niche markets.

### Monetize late (grow first)
- **Pros:** Maximizes growth. Builds network effects. Captures market share.
- **Cons:** Requires funding. May train users to expect free. No validation of willingness to pay.
- **Best for:** Social networks, marketplaces, products with strong network effects.

### The danger of "we'll figure out monetization later"

This works if you have network effects (Facebook, Instagram, WhatsApp). It fails if you don't. For most products, if customers won't pay early, they won't pay later. Free users and paying users are often different populations, not the same population at different times.

---

## 6. Business Model Innovation

Sometimes the innovation isn't the product — it's the business model.

| Company | Product innovation? | Business model innovation? |
|---------|--------------------|-----------------------------|
| Dollar Shave Club | No (same razors) | Yes (subscription, D2C) |
| Salesforce | Moderate | Yes (cloud SaaS vs. on-premise licenses) |
| Rolls-Royce (jet engines) | No | Yes ("power by the hour" — charge per flight hour, not per engine) |
| Spotify | Moderate | Yes (subscription streaming vs. per-song purchase) |
| AWS | Yes | Yes (pay-as-you-go cloud vs. buy-your-own-servers) |
| Michelin Fleet Solutions | No (same tires) | Yes (charge per kilometer, not per tire) |

Business model innovation is often more disruptive than product innovation because incumbents can copy features but can't easily change how they make money.

---

## 7. Common Business Model Mistakes

### Confusing revenue with value
Revenue is not value. If you charge $1M/year but your customers would pay $10M, your pricing is broken. If you charge $1M/year and deliver $500K in value, your churn will be brutal.

### Ignoring the cost side
A business model with $100 LTV and $200 CAC is a machine for destroying money, no matter how fast it grows.

### Wrong model for the market
Selling a $10/month subscription to Fortune 500 companies? They'll never take you seriously. Selling $50,000 annual contracts to individual developers? They'll never pay.

### Premature complexity
Don't launch with five pricing tiers, add-ons, and usage-based billing. Start with one plan. Add complexity when you have data showing you need it.

---

## Key Takeaways

1. **The business model is not an afterthought.** It's a core design decision that shapes everything.
2. **Subscriptions win** when you can sustain low churn — predictable, compounding revenue.
3. **Price on value, not cost.** What you charge should reflect what the customer gets, not what you spend.
4. **LTV/CAC ≥ 3** is the minimum for a healthy business. Below that, growth kills you.
5. **Business model innovation** can be more disruptive than product innovation.
6. **Start simple.** One price, one plan. Add complexity only when data demands it.

---

## See Also

- [Competitive Strategy](competitive-strategy.md) — How strategy determines which models work
- [Sales](../03-customers/sales.md) — How you actually execute the model
- [Financial Modeling & Metrics](../05-finance/financial-modeling.md) — How to measure and forecast your model's performance
- [The Startup Journey](../07-entrepreneurship/the-startup-journey.md) — How business models evolve during the startup lifecycle
