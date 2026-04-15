# Verification Rules — Human Review Protocol

The Brief Generation Agent produces **drafts, not final copy**. Every draft must be verified by a human before it is sent. This file defines what must be checked, in what order, and what constitutes a blocking vs non-blocking issue.

These rules were developed during the Day 1 review session where 10+ fabricated claims were caught before any bad emails went out. The human review step is the single highest-ROI step in the entire DOE pipeline.

---

## The Pre-Send Checklist

Before any brief is sent, the human reviewer must complete every check below. If any check fails and cannot be fixed in place, the brief is discarded and a replacement is pulled from the queue.

### Check 1: Signature is exact — **BLOCKING**

**Verify**:
- Signature reads exactly: `— Ola Oluwaseun\nDivine Web · divineweb.co`
- No `[Your Name]` placeholder
- Not "Ayo" or any other name
- Not "Divine Web Services" (must be "Divine Web")
- Em-dash (—) at the start of the signature, not hyphen (-)
- Middot (·) between "Divine Web" and "divineweb.co", not pipe (|) or hyphen

**If this check fails**: Fix in place. Never send with a wrong signature.

---

### Check 2: Subject line exists and is curiosity-driven — **BLOCKING**

**Verify**:
- Subject line is present (not empty)
- Subject line is under 60 characters
- Subject line is lowercase or sentence case (not Title Case — feels corporate)
- Subject line does NOT contain: "Website redesign", "Increase conversions", "Free audit", "Limited time", "Offer", "ROI", "Guaranteed"
- Subject line ideally references the recipient's business directly

**Good subject line patterns**:
- `quick note on [domain]`
- `question about [business-name]`
- `noticed something on your site`
- `[N] reviews and your mobile site`
- `quick thing on [domain]`

**If this check fails**: Write a subject line manually. Most Brief Agent outputs currently have empty subject lines — this is a known bug. Generate manually until the agent is fixed.

---

### Check 3: Every specific technical claim is verified against real data — **BLOCKING**

**Verify each of the following claim types. If any are present, cross-check:**

| Claim type | Verification source |
|---|---|
| "Your site takes X seconds to load" | `doe_leads.pagespeed_mobile` and `load_time_ms` must be real integers |
| "PageSpeed score of X" | `doe_leads.pagespeed_mobile` must match exactly |
| "Not Secure warning in Chrome" | `doe_leads.website_url` must start with `http://` (not `https://`) |
| "Button X doesn't click / redirects to Y" | `doe_briefs.raw_audit_data.broken_elements` or `cro_issues` must contain this finding OR human must have manually verified |
| "Your contact form doesn't work" | Human must have manually tested the form |
| "No click-to-call on mobile" | Human must have manually checked mobile site |
| "4.8 stars with 366 reviews" | `doe_leads.rating` and `doe_leads.review_count` must match exactly |

**If this check fails**: The specific claim must be either removed or rewritten with generic language. Do not send a brief with an unverifiable specific claim.

---

### Check 4: Business identity matches between `email` and brief body — **BLOCKING**

**Verify**:
- The business name in the brief body matches the business name in `doe_leads.business_name` for that `email` address
- The city/region mentioned matches `doe_leads.city`
- No mixing of two different businesses in one email (yes, this has happened — copy/paste error during personalization)

**Evidence from real failure**: Day 1 — user almost sent an email addressed to `rfplumber@yahoo.com` (Royal Flush Plumbing) with body content about 1-Tom-Plumber Arlington. Caught at the last second during pre-send review.

**If this check fails**: Discard the draft. Rewrite from scratch using the correct lead's data.

---

### Check 5: ICP fit — does this look like a real Mike the Plumber lead? — **BLOCKING**

**Open the live website. Spend 2-3 minutes looking at it on mobile.** Answer:

1. Does this look like a pro-built site? (If yes → likely ICP miss, skip)
2. Does it have a modern, premium feel? (If yes → likely ICP miss, skip)
3. Is there a prominent "Since [year < 2005]" badge? (If yes → likely ICP miss, skip)
4. Are there multiple service categories (plumbing + HVAC + electrical)? (If yes → likely multi-service brand, deprioritize)
5. Is the phone number prominent and tap-to-call on mobile? (If yes → at least one claimed pain point may be invalid)
6. Is the site actually bad in some verifiable way?

**If the live site contradicts claims in the brief**: The brief is wrong and must be rewritten OR the lead must be skipped.

**Evidence from real failures on Day 1**:
- Public Service Plumbers — site was excellent, nothing to pitch. Skipped.
- Evolution Home Services — premium "Next-Gen" brand. Skipped.
- Classic Plumbing — 40-year family business with solid mobile UX. Skipped.
- FPP Plumbing — site had real quirks (two phone numbers with different area codes). Sendable.
- Royal Flush Plumbing — verifiably on HTTP, no SSL. Sendable.

---

### Check 6: Dollar amounts and percentages are generic, not fabricated — **HIGH**

**Verify**:
- No specific dollar ranges ("$200-$800 per call") unless they trace to real data
- No specific percentages ("40% of visitors bail") unless they trace to real data
- Directional language is used instead ("each missed emergency call", "a handful of bookings a week", "most panicked visitors")

**If this check fails**: Replace specific numbers with directional language before sending.

---

### Check 7: Single clear CTA — **MEDIUM**

**Verify**:
- Only ONE call to action in the email
- Soft asks preferred: "Mind if I send over a quick idea?" or "Worth a quick call?"
- No multi-CTA combinations ("Schedule a call AND let me send you an idea AND check out my portfolio")
- No hard asks on cold emails ("Book a 30-minute strategy session")

**If this check fails**: Remove all but one CTA. Pick the softest one.

---

## The 5-Minute Site Visit Protocol

For every lead about to receive a brief, the human reviewer must spend 2-3 minutes on the live website before sending. Budget this into the morning block — it is non-negotiable.

**What to check on mobile** (not desktop — the ICP opens emails on their phone):

1. **First fold**: What do you see in the first 2 seconds?
2. **Phone number visibility**: Is there a tap-to-call number above the fold?
3. **CTA labels**: Click the main CTA. Does it go where the label implies?
4. **Contact form**: If a form exists, does it submit successfully? (Test with a fake submission if needed.)
5. **Load feel**: Does the page feel fast or slow? (Subjective — don't claim specific seconds from this.)
6. **Visual quality**: Does it look DIY/template, agency-built, or premium?
7. **Footer**: Copyright year? Last updated? "Powered by [agency]"?

**What you verify here → what you can claim in the email**. If you haven't verified it, the brief cannot claim it.

---

## When to Discard vs Fix vs Send

| Situation | Action |
|---|---|
| Brief has fabricated load time, no real data available | Rewrite with generic language ("most plumber sites are slow on mobile"), then send |
| Brief has fabricated UI bug, human visits site and confirms bug is real | Send as-is |
| Brief has fabricated UI bug, human visits site and confirms bug is NOT real | Rewrite claim or discard brief |
| Brief is for a business that turns out to be premium-branded or franchise | Discard, disqualify lead in `doe_leads`, pull replacement |
| Brief signature has `[Your Name]` | Fix in place, send |
| Brief has two CTAs | Remove the harder one, send |
| Brief is addressed to the wrong business | Discard, rewrite from scratch |
| Brief is solid and every claim verifies | Send immediately |

---

## The Rule of "When In Doubt, Don't Send"

A cold email that doesn't go out is free. A cold email that goes out with a false claim burns your sending domain's reputation, trains the recipient to ignore you, and potentially gets you marked as spam.

**If you're not confident in a claim, either remove it or don't send the brief.** Tomorrow's lead queue will have better options.

---

## Capturing New Lessons

Every time you catch a hallucination or ICP miss in review, immediately add it to `doe_learnings` table:

```sql
INSERT INTO doe_learnings (category, severity, lesson, rule, evidence, source_lead_id)
VALUES (
  '<hallucination|icp_filter|copy_pattern|verification>',
  '<critical|high|medium|low>',
  '<short imperative: what to do differently>',
  '<exact rule to bake into skill file>',
  '<what triggered this lesson>',
  '<lead_id from doe_leads>'
);
```

Then during the weekly skill update ritual, these new lessons get synthesized into updates to `hallucination-traps.md`, `icp-filters.md`, or `copy-patterns.md`.

**The goal**: every week, the Brief Agent gets smarter without any manual prompt engineering. The skill files are the learning substrate.
