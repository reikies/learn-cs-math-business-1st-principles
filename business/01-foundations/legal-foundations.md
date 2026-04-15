# Legal Foundations — The Rules of the Game

> The law is not a theoretical concern you can deal with later. It defines what
> your business *is* — its structure, its obligations, its rights, and its
> boundaries. Every contract you sign, every employee you hire, every product
> you ship operates within a legal framework. Ignorance of it doesn't protect
> you — it destroys you.

**Prerequisites:** [Economics](economics.md) helps you understand *why* these rules exist. [Accounting](accounting.md) helps you understand the financial implications.
**What it enables:** Starting a company, raising money, hiring people, protecting your work, staying out of court.

---

## Why This Matters

Founders love building and hate paperwork. This is understandable. It's also dangerous.

Legal mistakes made early — wrong entity structure, missing co-founder agreement, sloppy IP assignment — compound over time and can kill deals, fundraises, or entire companies years later. A $500 legal decision at incorporation can become a $5M problem at acquisition.

You don't need to be a lawyer. You need to know enough to (a) avoid obvious traps, (b) ask the right questions, and (c) know when to call a lawyer.

---

## 1. Business Entities — What *Is* Your Company?

The first legal decision you make. The entity type determines your taxes, liability, fundraising ability, and governance.

### Sole Proprietorship

You *are* the business. No legal separation.

- **Liability:** Unlimited personal liability. If the business gets sued, your house is at stake.
- **Taxes:** Business income is your personal income.
- **Fundraising:** Can't sell equity. Investors won't touch this.
- **Use case:** Freelancers, side projects. Never for a startup.

### Partnership (General & Limited)

Two or more people sharing ownership. A general partnership gives all partners unlimited liability. A limited partnership (LP) has general partners (who manage, unlimited liability) and limited partners (who invest, limited liability).

- **Use case:** Professional firms (law, accounting), venture capital fund structures (the VC fund is usually an LP).

### Limited Liability Company (LLC)

A hybrid. Liability is limited (like a corporation), but taxation is flexible (can elect to be taxed as a pass-through or a corporation).

- **Liability:** Limited to what you invested. Personal assets are protected (with exceptions).
- **Taxes:** Default is pass-through (profits taxed on members' personal returns). Can elect corporate taxation.
- **Fundraising:** Possible but awkward. VCs generally don't invest in LLCs.
- **Use case:** Small businesses, real estate, consulting firms. Not ideal for VC-backed startups.

### C-Corporation (C-Corp)

The standard structure for any company that plans to raise venture capital, go public, or be acquired.

- **Liability:** Limited.
- **Taxes:** Double taxation — the corporation pays corporate tax on profits, then shareholders pay personal tax on dividends. Sounds bad, but in practice startups don't pay dividends — they reinvest.
- **Fundraising:** Preferred by VCs. Clean equity structure, well-understood governance.
- **Use case:** Startups, growth companies, public companies. Delaware C-Corp is the default for VC-backed companies.

### S-Corporation (S-Corp)

Like a C-Corp but with pass-through taxation (no double taxation). Restrictions: max 100 shareholders, one class of stock, US shareholders only.

- **Use case:** Small businesses that want corporate liability protection without double taxation. Can't work for VC-backed startups (VCs need preferred stock = multiple share classes).

### Why Delaware?

Most US startups incorporate in Delaware regardless of where they're located. Why?

- **Predictable case law** — Delaware's Court of Chancery has 200+ years of corporate law. Disputes get resolved by expert judges, not juries.
- **Investor familiarity** — VCs, lawyers, and acquirers all know Delaware law.
- **Flexibility** — Delaware law is the most corporation-friendly in the US.

---

## 2. Co-Founder Agreements

The most important document most founders never write. When the company is two friends in a garage, everything feels simple. When the company is worth $50M and one founder wants out, you'll wish you had this in writing.

### What it should cover

- **Equity split** — who owns what, and *why*
- **Vesting schedule** — standard is 4-year vesting with a 1-year cliff
- **Roles and responsibilities** — who does what
- **Decision-making** — how do you resolve disagreements
- **IP assignment** — everything created belongs to the company, not to individuals
- **Departure terms** — what happens if someone leaves or is fired
- **Non-compete / non-solicit** — restrictions on competing after departure (enforceability varies by state)

### Vesting

**Vesting** means you earn your equity over time. You don't get it all on day one.

Standard: **4 years, 1-year cliff, monthly thereafter.**

```
Year 0-1: Nothing vested (the "cliff")
Month 12: 25% vests all at once (you passed the cliff)
Month 13-48: ~2.08% vests each month
Month 48: 100% vested
```

Why the cliff? If a co-founder leaves after 3 months, they shouldn't walk away with 25% of the company for 90 days of work. The cliff protects everyone.

---

## 3. Contracts — The Backbone of Business

A contract is a legally enforceable agreement. Almost every business relationship is defined by one.

### Essential elements of a valid contract

1. **Offer** — one party proposes terms
2. **Acceptance** — the other party agrees
3. **Consideration** — each side gives something of value (money, services, goods)
4. **Capacity** — both parties are legally able to enter contracts (adults, sound mind)
5. **Legality** — the subject matter must be legal

### Contracts you'll encounter constantly

| Contract | What it does |
|----------|-------------|
| NDA (Non-Disclosure Agreement) | Protects confidential information shared between parties |
| Employment agreement | Terms of hiring — role, compensation, IP assignment, non-compete |
| Independent contractor agreement | Same, but for contractors (important distinction — see below) |
| Terms of Service (ToS) | The contract between your product and your users |
| SaaS agreement | Enterprise version of ToS — negotiated, not click-through |
| Vendor/supplier agreement | Terms with your suppliers |
| Partnership/JV agreement | Terms when collaborating with another company |
| LOI (Letter of Intent) | Non-binding (usually) outline of a deal before the real contract |

### The employee vs. contractor distinction

Misclassifying employees as contractors is one of the most common (and expensive) legal mistakes.

| Factor | Employee | Contractor |
|--------|----------|------------|
| Control | You control *how* they work | You control *what* they deliver |
| Tools | You provide tools/equipment | They use their own |
| Schedule | You set it | They set it |
| Exclusivity | Usually exclusive | Usually multiple clients |
| Benefits | You owe benefits, taxes | They handle their own |
| Duration | Ongoing | Project-based |

Misclassification risk: back taxes, penalties, benefits owed. States like California are aggressive about enforcement.

---

## 4. Intellectual Property — Owning What You Create

IP is often the most valuable asset a startup has. Protect it or lose it.

### The four types of IP

| Type | What it protects | Duration | How to get it |
|------|-----------------|----------|---------------|
| **Patent** | Inventions, processes, methods | 20 years | File with patent office (expensive, slow) |
| **Trademark** | Brand names, logos, slogans | Indefinite (if maintained) | Use it + register for stronger protection |
| **Copyright** | Creative works (code, text, art, music) | Life + 70 years (or 95 for corporations) | Automatic upon creation |
| **Trade secret** | Confidential business info (formulas, algorithms) | Indefinite (if kept secret) | Keep it secret + NDAs |

### IP for startups — what actually matters

1. **IP assignment** — Make sure all founders and employees sign agreements assigning their work to the company. If a developer writes code on a personal laptop and never signs an IP assignment, they might argue they own it.

2. **Copyright in code** — Your source code is automatically copyrighted. Open-source licenses are copyright licenses that grant specific permissions.

3. **Trademarks** — Register your company name and product names. It's cheap insurance against someone else using your name.

4. **Patents** — Expensive and slow. Valuable in some industries (biotech, hardware). In software, patents are controversial and often defensive ("we have patents so others can't sue us").

5. **Trade secrets** — Your proprietary algorithms, customer lists, pricing strategies. Protected by NDAs and access controls, not registration. Once leaked, protection is lost forever.

---

## 5. Equity, Cap Tables, and Securities Law

When you issue shares, you're issuing **securities**, which are regulated by federal and state law.

### The cap table

A **capitalization table** tracks who owns what percentage of the company. At founding, it's simple:

```
Founder A    5,000,000 shares    50%
Founder B    5,000,000 shares    50%
─────────────────────────────────────
Total       10,000,000 shares   100%
```

After a seed round with a VC:

```
Founder A    5,000,000 shares    40%
Founder B    5,000,000 shares    40%
Seed VC      2,500,000 shares    20%  (preferred stock)
─────────────────────────────────────
Total       12,500,000 shares   100%
```

Each round **dilutes** existing shareholders. This is normal and expected — you're trading percentage ownership for growth capital.

### Common vs. Preferred stock

| Feature | Common stock | Preferred stock |
|---------|-------------|----------------|
| Who holds it | Founders, employees | Investors |
| Voting rights | Usually yes | Usually yes |
| Dividends | None (typically) | Sometimes |
| Liquidation preference | Last in line | First in line |
| Conversion | N/A | Can convert to common |

Liquidation preference is the big one. If the company sells for $10M and the VC invested $5M with a 1x liquidation preference, the VC gets their $5M back first. The remaining $5M is split among common shareholders.

### Securities law basics

You can't just sell shares to anyone. Securities issuance is heavily regulated (SEC in the US). Key exemptions startups use:

- **Regulation D (Rule 506)** — sell to accredited investors without registering with the SEC
- **83(b) election** — founders file this within 30 days of receiving unvested shares to avoid a massive tax bill later. Missing this deadline is one of the most expensive mistakes a founder can make.

---

## 6. Employment Law

Hiring people creates legal obligations.

### At-will employment
In most US states, employment is "at-will" — either party can end it at any time, for any (legal) reason. But "any legal reason" has important exceptions:

- Can't fire someone for discriminatory reasons (race, gender, age, disability, religion)
- Can't fire someone for whistleblowing or filing a complaint
- Can't fire someone in violation of an employment contract

### Key employment laws (US)

| Law | What it does |
|-----|-------------|
| Title VII (Civil Rights Act) | Prohibits discrimination based on race, color, religion, sex, national origin |
| ADA | Requires reasonable accommodations for disabilities |
| FLSA | Sets minimum wage, overtime rules, exempt vs. non-exempt classification |
| FMLA | Provides unpaid leave for medical/family reasons |
| WARN Act | Requires 60-day notice for mass layoffs (100+ employees) |

### Stock options for employees

The standard way startups compensate employees with equity:

- **Stock options** give the right to *buy* shares at a fixed price (the **strike price** or **exercise price**)
- Strike price is set at Fair Market Value (FMV) at time of grant
- If the company grows, shares become worth more than the strike price — the difference is the employee's upside
- **ISO (Incentive Stock Options)** — tax-advantaged, for employees only
- **NSO (Non-Qualified Stock Options)** — less tax-advantaged, available to anyone

---

## 7. Regulatory Compliance

Depending on your industry, you may face significant regulation:

| Domain | Key regulations |
|--------|----------------|
| Finance / Fintech | SEC, FINRA, state money transmitter licenses, SOX |
| Healthcare | HIPAA (patient data), FDA (devices/drugs) |
| Data / Privacy | GDPR (EU), CCPA (California), SOC 2 (security audits) |
| Food & Beverage | FDA, state health departments |
| Education | FERPA (student data) |
| Employment | EEOC, OSHA, wage & hour laws |

### Privacy law — increasingly important

**GDPR** (EU) and **CCPA** (California) have changed how every tech company handles data:

- Users must consent to data collection
- Users can request their data be deleted
- Data breaches must be disclosed
- Non-compliance carries massive fines (GDPR: up to 4% of global revenue)

If you have users in the EU, GDPR applies to you regardless of where your company is based.

---

## 8. Dispute Resolution

When things go wrong, you have options:

| Method | Speed | Cost | Privacy | Binding? |
|--------|-------|------|---------|----------|
| Negotiation | Fast | Low | Private | If agreed |
| Mediation | Moderate | Moderate | Private | If agreed |
| Arbitration | Moderate | Moderate-High | Private | Yes (usually) |
| Litigation (court) | Slow | High | Public | Yes |

Most commercial contracts include an **arbitration clause** — disputes go to arbitration, not court. This is cheaper, faster, and private, but it also means you waive your right to a jury trial.

---

## Key Takeaways

1. **Entity structure matters from day one.** Delaware C-Corp for VC-backed startups. LLC for small businesses. Get it right early.
2. **Co-founder agreements are non-negotiable.** Vesting protects everyone. Write it down before you build anything.
3. **IP must be assigned to the company.** If founders and employees don't sign IP assignment agreements, the company may not own its own product.
4. **Securities law is real.** Issue equity incorrectly and you face SEC violations. File your 83(b) election.
5. **Employment law has teeth.** Misclassifying workers, missing payroll taxes, or discriminatory practices can destroy a company.
6. **Privacy regulation is expanding.** GDPR and CCPA are the floor, not the ceiling.

---

## See Also

- [Economics](economics.md) — Why these rules exist (market failures, externalities, property rights)
- [Fundraising & Venture Capital](../05-finance/fundraising.md) — The financial mechanics these legal structures enable
- [People & Organizations](../04-operations/people-and-organizations.md) — The human side of employment law
