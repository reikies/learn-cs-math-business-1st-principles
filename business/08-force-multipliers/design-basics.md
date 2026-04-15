# Design (Basic UI/UX) — Enough Taste to Not Build Ugly, Confusing Things

> You don't need to be a designer. You need enough design literacy to not ship
> products that confuse people, bury the value, or look like they were built in
> 2008. AI products that look good get adopted faster, period. Design is the
> difference between a demo that impresses and a product that gets used.

**Prerequisites:** [Product Thinking](../03-customers/product-thinking.md) — knowing *what* to build comes before knowing how it should look and feel.
**What it multiplies:** [Product](../03-customers/product.md), [Marketing](../03-customers/marketing.md), [Sales](../03-customers/sales.md) — every surface where your product meets a human.

---

## Why This Matters

Users form opinions about your product in milliseconds. Before they read a word, before they click anything, they've already decided whether it looks trustworthy, modern, and competent — or janky and amateur.

This isn't about aesthetics for aesthetics' sake. It's about removing friction. Bad design makes people think. Good design makes them act. Every moment a user spends figuring out your interface is a moment they're not getting value from your product.

For AI products especially, design is critical. AI outputs are inherently uncertain — users need to trust the system. Trust is built through clear, clean, predictable interfaces. A powerful model behind a confusing UI is a failed product.

---

## 1. Information Hierarchy — The Most Important Design Concept

The single most valuable design skill: knowing what's important and making it visually obvious.

### Visual hierarchy principles

```
Most important
      │
      ▼
┌─────────────────────────────────────┐
│  LARGE, BOLD, HIGH CONTRAST         │  ← Primary: the thing you want
│                                     │     users to see/do first
├─────────────────────────────────────┤
│  Medium size, medium weight         │  ← Secondary: supporting info
│                                     │     they'll need next
├─────────────────────────────────────┤
│  Small, light, low contrast         │  ← Tertiary: details, metadata,
│                                     │     advanced options
└─────────────────────────────────────┘
      │
      ▼
Least important
```

**The tools of hierarchy:**
| Tool | How it creates hierarchy |
|------|------------------------|
| Size | Bigger = more important |
| Weight | Bolder = more important |
| Color | Higher contrast = more attention. Use sparingly. |
| Spacing | More whitespace around something = more importance |
| Position | Top-left gets seen first (in LTR languages) |
| Grouping | Related items near each other; unrelated items separated |

### The squint test

Squint at your interface until you can't read the text. Can you still tell what's most important? If everything blurs into sameness, your hierarchy is broken.

---

## 2. Layout & Spacing — Why Things Feel "Off"

Most amateur designs fail not because of bad colors or fonts, but because of inconsistent spacing.

### The spacing system

Pick a base unit (8px is standard) and use multiples of it everywhere:

```
8px  — tight spacing (between related elements)
16px — default spacing (between elements in a group)
24px — medium spacing (between groups)
32px — large spacing (between sections)
48px — extra large (major section breaks)
```

Consistency matters more than the specific numbers. When spacing is consistent, things feel organized. When it's random, things feel messy — even if you can't articulate why.

### Alignment

Every element on screen should be aligned to something. Invisible grids create order.

```
Bad:                          Good:
┌──────────────────┐          ┌──────────────────┐
│ Title             │          │ Title             │
│    Some text here │          │ Some text here    │
│ Button      │          │ Button            │
│       Another item│          │ Another item      │
└──────────────────┘          └──────────────────┘
```

Left-align most things. Center-align headlines sparingly. Right-align numbers in tables. Never mix alignments without reason.

### Whitespace

The most common mistake non-designers make: not enough whitespace. Whitespace isn't empty — it's breathing room. It separates ideas, creates focus, and reduces cognitive load.

When in doubt, add more space.

---

## 3. Typography — Fewer Decisions, Better Results

### The rules that cover 90% of cases

1. **Use one font family.** Two at most (one for headings, one for body). Zero decorative fonts.
2. **Use a system font or a well-known font.** Inter, SF Pro, Roboto for UI. Not Comic Sans. Not Papyrus.
3. **Create hierarchy with size and weight, not different fonts.**
4. **Body text: 16px minimum on web.** Smaller is unreadable on most screens.
5. **Line height: 1.5 for body text, 1.2 for headings.** Tight line height makes text feel cramped.
6. **Line length: 45-75 characters.** Longer lines are hard to read. This is why content is centered on a max-width container.

### A simple type scale

```
Heading 1:   32px, bold
Heading 2:   24px, bold
Heading 3:   20px, semibold
Body:        16px, regular
Small/meta:  14px, regular
Caption:     12px, regular (use sparingly)
```

You don't need more than this for most products.

---

## 4. Color — Less Is More

### The 60-30-10 rule

- **60%** — neutral/background color (white, light gray, dark gray)
- **30%** — secondary color (used for surfaces, cards, sections)
- **10%** — accent color (buttons, links, highlights, CTAs)

### A simple approach to color

1. Start with a single accent color (blue is safe; it conveys trust).
2. Use gray for everything else (text, borders, backgrounds).
3. Add semantic colors only as needed: green for success, red for errors, yellow for warnings.
4. Don't use color as the *only* signal. (See: accessibility.)

### Dark mode

If you ship dark mode, don't just invert colors. Dark backgrounds need:
- Slightly muted text (not pure white on pure black — it vibrates)
- Reduced contrast for secondary elements
- Adjusted shadow/elevation (shadows don't work the same on dark backgrounds)

---

## 5. User Flows — How People Move Through Your Product

### The golden rule: one primary action per screen

Every screen should have one obvious thing the user should do. If everything is equally prominent, nothing stands out.

```
Bad: Screen with 5 buttons, 3 forms, and 2 navigation menus
     → User doesn't know where to start

Good: Screen with 1 primary action, 2 secondary options, and a clear path
     → User knows exactly what to do
```

### Progressive disclosure

Don't show everything at once. Reveal complexity as the user needs it.

```
Level 1: The basics (what 80% of users need)
    │
    └── "Advanced settings" →
        Level 2: More options (what 15% need)
            │
            └── "Custom configuration" →
                Level 3: Full power (what 5% need)
```

Gmail does this well. Compose is simple. Formatting options are hidden until clicked. Scheduling and confidential mode are tucked in a menu.

### Common flow patterns

| Pattern | When to use it | Example |
|---------|---------------|---------|
| Wizard (stepped) | Multi-step processes where order matters | Onboarding, checkout |
| Tab navigation | Parallel views of related content | Settings categories |
| Modal / dialog | Focused task that interrupts the flow | Confirm delete, quick edit |
| Inline editing | Low-friction changes | Rename a file, edit a field |
| Search + filter | Large collections of items | Product catalogs, admin panels |

---

## 6. Design for AI Products

AI products have unique design challenges because the output is non-deterministic and the user's mental model is unfamiliar.

### The trust problem

Users don't trust AI by default. Your UI must build trust:

| Design choice | Builds trust | Erodes trust |
|--------------|-------------|-------------|
| Show sources | "Based on these 3 documents" | "Here's the answer" (no provenance) |
| Show confidence | "High confidence" / "I'm not sure about this" | Presenting uncertain results as facts |
| Allow correction | "Was this helpful? Edit this answer" | No feedback mechanism |
| Be transparent | "I used your last 30 days of data" | Black box |
| Fail gracefully | "I couldn't find enough information. Try..." | Error or hallucinated answer |

### Streaming and latency

AI is slow compared to traditional software. Design for it:
- **Stream responses** — show tokens as they arrive, not a loading spinner for 10 seconds
- **Progressive rendering** — show structure first, fill details as they arrive
- **Skeleton screens** — show the shape of the result immediately
- **Explain the wait** — "Analyzing 200 documents..." is better than a spinner

### The AI interaction spectrum

```
Fully automated          Copilot/assistant         Human-in-the-loop
     │                        │                          │
AI does everything    AI suggests, human      Human does the work,
No human input        approves/edits          AI assists on request
     │                        │                          │
 High risk if wrong    Sweet spot for most     Low efficiency gain
 High efficiency gain   products today          Low risk
```

Most AI products today should be in the middle — copilot mode. Show the AI's work, let the human approve, edit, or reject.

### Error states matter more in AI

Traditional software has predictable errors. AI has unpredictable failures — hallucinations, irrelevant responses, confident wrong answers.

Design for these:
- **"I don't know" is a valid response.** Better than a hallucinated answer.
- **Confidence indicators.** Not all outputs are created equal.
- **Easy escape hatches.** If the AI is wrong, make it trivial to correct or dismiss.
- **Feedback loops.** Thumbs up/down, edit capability, "report issue" — these make the product better *and* build trust.

---

## 7. Tools That Lower the Bar

You don't need to learn Photoshop. Modern tools make basic design accessible:

| Tool | What it's for | Learning curve |
|------|-------------|---------------|
| Figma | UI design, prototyping, collaboration | Medium (worth learning) |
| v0 (Vercel) | AI-generated UI components from prompts | Low |
| Tailwind CSS | Utility-first CSS framework with built-in design decisions | Low-Medium |
| Shadcn/ui | Pre-built component library | Low |
| Linear / Notion | Templates with strong default design | Very low |
| Coolors | Color palette generation | Minimal |
| Untitled UI | Free Figma UI kit with common patterns | Low |

The key insight: **you don't need to design from scratch.** Use design systems, component libraries, and templates. Your job is to compose them well, not create them.

---

## 8. The Design Checklist for Non-Designers

Before shipping any interface, run through this:

```
□ Is there one clear primary action on each screen?
□ Can users get to value in < 3 clicks from entry?
□ Is the text hierarchy clear? (squint test)
□ Is spacing consistent? (8px grid)
□ Are interactive elements obvious? (buttons look like buttons)
□ Do error states have clear messages and recovery paths?
□ Does it work on mobile? (if applicable)
□ Is there sufficient contrast for readability?
□ Did I remove everything that isn't necessary?
```

The last question is the most important. The best design is often what you take away, not what you add.

---

## Key Takeaways

1. **You don't need to be a designer.** You need enough taste to not build confusing things.
2. **Information hierarchy is the #1 skill.** Make the important things visually obvious.
3. **Spacing and alignment create "professional."** Consistent spacing is the difference between amateur and polished.
4. **One primary action per screen.** If everything is important, nothing is.
5. **AI products need trust-building design.** Show sources, show confidence, allow correction, fail gracefully.
6. **Use existing design systems.** Don't design from scratch. Compose well-designed components.
7. **When in doubt, remove something.** The best interface is the one with the least in it.

---

## See Also

- [Product Thinking](../03-customers/product-thinking.md) — What to build before how it looks
- [Product](../03-customers/product.md) — UX principles and product metrics
- [Psychology & Behavioral Science](psychology-and-behavior.md) — Why users behave the way they do
- [Writing & Communication](writing-and-communication.md) — Product copy as a design element
