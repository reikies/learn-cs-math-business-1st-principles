# Data Literacy & Analytics — Making Numbers Useful

> Not data science — data literacy. You don't need to build models. You need to
> read a chart and know if someone's lying to you. You need to look at a dashboard
> and spot what's actually wrong. You need to know which number matters and which
> is vanity. Every company drowns in data. The people who can make data *useful*
> are the ones who get listened to.

**Prerequisites:** None, though SQL fluency and basic statistics help enormously.
**What it multiplies:** [Financial Modeling](../05-finance/financial-modeling.md), [Marketing](../03-customers/marketing.md), [Product](../03-customers/product.md), [Growth](../06-growth/growth-and-scaling.md) — every decision that should be informed by evidence.

---

## Why This Matters

Data literacy is the gap between "we have data" and "we make better decisions." Most companies have the first. Very few have the second.

The failure mode isn't lack of data — it's drowning in it. Dashboards with 47 metrics. Weekly reports nobody reads. A/B tests that prove whatever you wanted to believe. The data-literate person cuts through this noise and asks: **"What does this actually tell us, and what should we do about it?"**

For someone building AI products, this is especially powerful. "I can make your data useful with AI" is one of the most compelling pitches in business today — but only if you understand what "useful" means from the business side.

---

## 1. The Data Literacy Stack

```
Level 4: Decision-Making
         "Based on this data, we should do X"
              │
Level 3: Interpretation
         "This pattern means..."
              │
Level 2: Analysis
         "Let me slice, compare, and contextualize"
              │
Level 1: Reading
         "I can look at a chart and understand what it shows"
```

Most people stop at Level 1. The value lives at Levels 3 and 4.

---

## 2. Reading Data — The Basics That Most People Get Wrong

### Understanding distributions

Not everything is an average. Averages hide the shape of the data.

```
Company A: Average salary $100K
  → Everyone makes $90K-$110K (tight distribution)

Company B: Average salary $100K
  → Half make $50K, half make $150K (bimodal)

Company C: Average salary $100K
  → Most make $60K, CEO makes $2M (skewed)

Same average. Completely different stories.
```

**When to use what:**
| Measure | When to use it | When it lies |
|---------|---------------|-------------|
| Mean (average) | Symmetric, normal distributions | Skewed data, outliers |
| Median | Skewed distributions, income, prices | When distribution shape matters |
| Mode | Categorical data, "most common" questions | Almost everywhere else |
| Percentiles (p50, p95, p99) | Performance, latency, understanding tails | When the average is "good enough" |

### Reading charts honestly

| Chart type | Good for | Watch out for |
|-----------|----------|--------------|
| Line chart | Trends over time | Truncated y-axis making small changes look dramatic |
| Bar chart | Comparing categories | Unequal intervals, cherry-picked time ranges |
| Pie chart | Almost nothing | Hard to compare slices. Use a bar chart instead. |
| Scatter plot | Correlation between two variables | Correlation ≠ causation |
| Cohort table | Retention, behavior over time | Survivorship bias in later cohorts |

### The questions to ask any chart

1. **What's the y-axis?** (Is it absolute or percentage? Is it truncated?)
2. **What's the time range?** (Cherry-picked to tell a specific story?)
3. **What's missing?** (What data was excluded or not shown?)
4. **Compared to what?** (A number without context means nothing.)
5. **Is this a trend or noise?** (One data point is not a trend.)

---

## 3. The Metrics That Matter in Business

### Unit economics

The numbers that tell you whether a business model works:

| Metric | What it is | Why it matters | Healthy benchmark |
|--------|-----------|---------------|-------------------|
| CAC | Cost to acquire one customer | Are you spending efficiently? | Depends on LTV |
| LTV | Total revenue from a customer over their lifetime | Does each customer justify the acquisition cost? | ≥ 3x CAC |
| LTV/CAC | Return on customer acquisition | Is the business model sustainable? | ≥ 3.0 |
| Payback period | Months to recoup CAC | How fast does the investment return? | < 12-18 months |
| Gross margin | Revenue minus direct costs, as % | How much of each dollar is profit? | > 60% (SaaS), > 30% (services) |

### Growth metrics

| Metric | What it measures | What "good" looks like |
|--------|-----------------|----------------------|
| MRR / ARR | Monthly/annual recurring revenue | Growing consistently |
| MoM growth | Month-over-month revenue growth | 15-20% early stage, 5-10% later |
| Net Revenue Retention (NRR) | Revenue from existing customers vs. prior period | > 100% (ideally > 120%) |
| Churn rate | % of customers lost per period | < 2% monthly (SaaS), < 5% annual (enterprise) |
| Quick ratio | (New + Expansion MRR) / (Churned + Contraction MRR) | > 4x |

### Engagement metrics

| Metric | What it tells you |
|--------|------------------|
| DAU/MAU | How engaged your user base is (> 25% is strong) |
| Session duration | How much time users spend (context-dependent — long isn't always good) |
| Feature adoption | % of users using a specific feature |
| Activation rate | % of signups who reach the "aha moment" |
| NPS | Net Promoter Score — would users recommend you? (> 50 is excellent) |

### The vanity metrics trap

**Vanity metrics** look impressive but don't drive decisions:
- Total registered users (if most are inactive)
- Page views (if nobody converts)
- App downloads (if nobody opens it twice)
- Social media followers (if they don't buy)

**Actionable metrics** drive decisions:
- Active users (DAU/MAU)
- Conversion rate
- Retention by cohort
- Revenue per user

The test: **"If this number changes, do we change what we're doing?"** If no, it's vanity.

---

## 4. Analysis Techniques That Matter

### Cohort analysis

The single most important analysis technique for any product or business.

Instead of looking at all users as one blob, group them by when they joined and track each group separately:

```
           Week 1   Week 2   Week 4   Week 8   Week 12

Jan cohort   100%     60%      40%      30%      25%   ← improving
Feb cohort   100%     65%      45%      35%      30%   ← improving
Mar cohort   100%     70%      55%      45%      40%   ← product is getting better
Apr cohort   100%     55%      35%      20%      10%   ← something broke in April
```

If you aggregate these, you see "average retention is 26%." Boring. The cohort view tells you the product improved Jan-Mar then broke in April. Completely different insight.

### Funnel analysis

Track drop-off at each step of a process:

```
Visited landing page:     10,000  (100%)
Clicked "Sign up":         2,000  (20%)   ← 80% drop-off
Completed registration:    1,200  (12%)   ← 40% drop-off
Reached "aha moment":        600  (6%)    ← 50% drop-off  ← biggest leak
Made first purchase:          300  (3%)    ← 50% drop-off
```

The biggest percentage drop isn't always the biggest opportunity. Look at where the absolute numbers change most and where the drop is fixable.

### Segmentation

Different customer segments behave differently. Blending them hides the truth.

```
Overall conversion rate: 5%

By source:
  Organic search:  8%
  Paid ads:        2%
  Referral:       12%

By company size:
  < 50 employees:  3%
  50-500:          7%
  500+:           15%
```

The aggregate "5%" tells you almost nothing. The segments tell you where to focus.

### A/B testing basics

| Concept | What it means |
|---------|--------------|
| Control vs. variant | The original (A) vs. the change (B) |
| Sample size | You need enough data to be confident. Small samples → unreliable results. |
| Statistical significance | p < 0.05 means there's < 5% chance the result is random noise |
| Practical significance | A 0.1% improvement might be statistically significant but not worth acting on |
| Duration | Run tests for full weeks (behavior changes by day). Don't stop early because it "looks good." |

The most common A/B testing mistake: stopping the test as soon as one variant looks better. Early results are noisy. Commit to a sample size *before* you start.

---

## 5. SQL — The Language of Business Data

SQL is the universal interface for business data. Every major business system (CRM, analytics, finance) stores data in relational databases.

### The queries that answer business questions

```sql
-- Monthly recurring revenue by month
SELECT DATE_TRUNC('month', created_at) AS month,
       SUM(mrr) AS total_mrr
FROM subscriptions
WHERE status = 'active'
GROUP BY 1
ORDER BY 1;

-- Cohort retention (what % of users from each signup month are still active?)
SELECT DATE_TRUNC('month', u.created_at) AS cohort,
       DATE_TRUNC('month', e.event_date) AS active_month,
       COUNT(DISTINCT e.user_id) AS active_users
FROM users u
JOIN events e ON u.id = e.user_id
GROUP BY 1, 2
ORDER BY 1, 2;

-- Top customers by revenue
SELECT customer_name,
       SUM(amount) AS total_revenue,
       COUNT(*) AS num_transactions
FROM payments
WHERE created_at >= CURRENT_DATE - INTERVAL '12 months'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 20;

-- Churn: customers who were active last month but not this month
SELECT u.id, u.email
FROM users u
WHERE u.id IN (
    SELECT user_id FROM events
    WHERE event_date BETWEEN '2026-02-01' AND '2026-02-28'
)
AND u.id NOT IN (
    SELECT user_id FROM events
    WHERE event_date BETWEEN '2026-03-01' AND '2026-03-31'
);
```

SQL fluency means you can answer your own questions without waiting for a data team. In a startup, this is a superpower.

---

## 6. Data Storytelling — Making Numbers Persuasive

Raw data doesn't convince anyone. Stories do. Data storytelling combines the two.

### The structure

```
1. Context    → "Here's the situation and why it matters"
2. Insight    → "Here's what the data shows (the surprising part)"
3. Implication → "Here's what that means for us"
4. Action     → "Here's what we should do about it"
```

**Bad:** "Our churn rate is 8%."
**Good:** "We're losing 8% of customers every month. At our current growth rate, we'll plateau at 12,500 customers — we'll never reach 20K. But the March cohort only churned 3% because we fixed onboarding. If we apply the same fix to all cohorts, we break through the plateau."

The second version has the same data. But it tells a story with stakes, a cause, and a path forward.

### Visualization principles

- **Title = the insight**, not the description. "Revenue is growing 15% MoM" not "Revenue Chart"
- **Annotate the interesting parts.** Call out inflection points, anomalies, key events.
- **Less is more.** One clear chart beats a dashboard of 20.
- **Color with purpose.** Highlight what matters. Gray out what doesn't.

---

## 7. The AI + Data Literacy Combination

This is where things get powerful. If you understand both data literacy and AI:

### What you can pitch

"I can make your data useful" is one of the most compelling statements in business today. Companies sit on massive amounts of data they can't use:
- Customer support logs → AI summarizes patterns, spots issues
- Sales call transcripts → AI identifies what winning reps do differently
- Financial data → AI spots anomalies, generates forecasts
- Documentation → AI makes it searchable and answerable

### What you can build

| Business problem | Data + AI solution |
|-----------------|-------------------|
| "We don't know why customers churn" | Analyze usage data, predict churn risk, surface reasons |
| "Our reports take 3 days to compile" | Automate data aggregation, generate narrative summaries |
| "Sales can't find the right case study" | Semantic search over sales materials |
| "We don't know what's in our contracts" | Extract and structure key terms from legal docs |

The data-literate AI builder knows which of these are valuable problems (high pain, willingness to pay) and which are science projects (cool but nobody cares enough to pay).

---

## Key Takeaways

1. **Data literacy ≠ data science.** You need to read, interpret, and act on data — not build models.
2. **Averages lie.** Look at distributions, cohorts, and segments. The truth is in the shape of the data.
3. **Vanity metrics feel good. Actionable metrics drive decisions.** If a number changing doesn't change your behavior, stop tracking it.
4. **Cohort analysis is the most important technique.** It separates "the product is improving" from "we're just acquiring more users."
5. **SQL is a business skill.** Being able to answer your own questions without waiting for a data team is a superpower.
6. **Data + AI = "I can make your data useful."** One of the most compelling pitches in the market today.

---

## See Also

- [Financial Modeling & Metrics](../05-finance/financial-modeling.md) — Building models and tracking KPIs
- [Product](../03-customers/product.md) — Product metrics (AARRR, North Star, cohort analysis)
- [Growth & Scaling](../06-growth/growth-and-scaling.md) — Growth metrics and what drives them
- [Marketing](../03-customers/marketing.md) — Marketing metrics (CAC, LTV, ROAS, attribution)
- [Writing & Communication](writing-and-communication.md) — Data storytelling as a communication skill
