# Copy Patterns — What Works and What Doesn't

Updated: April 16, 2026 (Day 2). Based on 4 verified sends (Day 1: Royal Flush, FPP | Day 2: Ardent, Same Day) and the Sav/Nate Herk cold email framework.

---

## The 5-Part Structure (v2 — updated Day 2)

### Part 1: Verified compliment

Cite exact rating + review_count from lead data.

**Examples that shipped**:
- "Saw Ardent Plumbing sitting at 4.8 stars with 121 reviews in Plano — that's hard to earn."
- "Saw Same Day Plumbing sitting at 4.6 stars with 18 reviews in Irving — solid reputation for emergency work."

---

### Part 2: Honest positioning with named case study

Introduce yourself as a peer building something, not a faceless agency. Reference a real business by name.

**Examples that shipped**:
- "I'm building out Divine Web — recently helped Same Day Plumbing in Irving tighten up their mobile site and conversion path."
- "I'm building out Divine Web — recently helped a Fort Worth plumber fix their SSL and mobile conversion path."

**Why this works**: Pattern disrupt. Most cold emails sound corporate. Saying "I'm building out" signals honesty and sincerity — a real person, not an automation. The named case study provides proof without bragging.

**Rule**: Rotate case study names as real clients accumulate. Never reference a business you haven't actually worked with or sent to.

---

### Part 3: One verified observation

State ONE specific, verifiable thing — traceable to real data or manually confirmed during mobile site visit.

**Examples that shipped**:
- "Chrome is showing a 'Not Secure' warning before visitors even see your phone number." (Ardent — http:// URL)
- "The contact form is buried at the very bottom — past your gallery, blogs, and a shopping cart icon." (Ardent — manually verified)
- "There's no site icon in the browser tab, and the footer still shows last year." (Same Day — manually verified)

**Rule**: Only include observations sourced from (a) `doe_leads`/`doe_briefs` data, (b) CRO `broken_elements`/`cro_issues`, or (c) human mobile site visit. See `hallucination-traps.md`.

---

### Part 4: Offer with outcome + time + guarantee

Frame as: desired outcome + specific timeframe + risk reversal. This is the Sav zero-risk framework.

**Examples that shipped**:
- "I can turn this into a clean, phone-first site that gets more visitors to call you — 48 hours, and if you don't love it, you don't owe me a thing."
- "A simple phone-first tweak — tap-to-call above the fold, contact form front and center — would turn more of those visitors into actual calls."

**Why this works**: Outcome (more calls) + Time (48 hours) + Catch (don't love it = $0) eliminates risk. Strangers respond to cold emails when they perceive zero downside.

---

### Part 5: Loom CTA + hardcoded signature

Close with ONE ask: a 2-minute Loom video walkthrough. No other CTA.

**The line**:
> "Mind if I walk you through a 2-minute Loom showing what I'd change?"

**Acceptable variants**:
- "Mind if I record a quick 2-minute Loom walking you through what I'd change?"
- "Want me to send over a 2-minute Loom walking you through it?"

**Forbidden CTAs** (do not use):
- "Worth a quick call?"
- "Mind if I send over a quick idea?"
- "Book a strategy call"
- "Reply if interested"

**Signature (exact, every time)**:
```
— Ola Oluwaseun
Divine Web · divineweb.co
```

---

## Subject Line Patterns (v2 — cliffhanger/internal style)

Subject lines should sound like they're from a colleague, team member, or someone who knows the business — not a cold email.

**Patterns that work**:
- `saw this on your site`
- `thought you should know`
- `[first-name] - quick one`
- `is [business-name] still taking on work?`
- `re: [business-name]`

**Day 1 patterns (still valid but lower priority)**:
- `quick note on [domain]`
- `quick thing on [domain]`

**To avoid**:
- "Website redesign for [business]"
- "Free website audit"
- Anything with Title Case
- Anything that sounds like marketing automation

---

## Language to Delete

| Delete | Replace with |
|---|---|
| "I sketched out a cleaner version" | (use Loom CTA instead) |
| "I put together a mockup" | (use Loom CTA instead) |
| "I hope this email finds you well" | (delete — open with compliment) |
| "Dear Sir/Madam" | (delete — no greeting needed, just open) |
| "I wanted to reach out regarding..." | (delete — just open with the observation) |
| "We are a leading agency" | "I'm building out Divine Web" |
| "Revolutionize your online presence" | (delete — pure jargon) |
| "Free consultation" | Loom CTA |
| "Worth a quick call?" | Loom CTA |

---

## Tone: Honest Peer

- **Direct** — no filler, no throat-clearing
- **Honest** — "I'm building out Divine Web" not "We are leaders in..."
- **Human** — contractions, fragments, casual phrases ("the next plumber on the list")
- **Humble** — slight vulnerability is a pattern disrupt vs. corporate cold email
- **Zero-pressure** — the Loom ask is tiny, the guarantee removes risk

---

## Metrics to Track (from Sav framework)

- **<2% reply rate** = lead list or deliverability problem
- **2-5% reply rate** = copy or offer problem
- **5-10% reply rate** = golden zone
- **>10% reply rate** = exceptional

---

## Post-Reply Data (to be populated)

Every reply logged in `doe_outreach_log`. Every Sunday, winning patterns extracted here, losing patterns logged.

**Target**: By Day 15, at least 3 confirmed winning patterns and 3 confirmed failing patterns from real reply data.

---

## The Reply Loop

1. Send baseline copy
2. Measure replies
3. Extract patterns from winners
4. Update this file
5. Next week's briefs follow winning patterns
6. Repeat
