# Copy Patterns — What Works and What Doesn't

This file captures the exact language patterns that worked (or failed) in real DOE outreach. It is updated every week after pulling reply data from `doe_outreach_log`.

As of the first entry (Day 1, April 15 2026), only 2 verified sends have been delivered. Reply data is not yet available. This file will grow rapidly over the 15-day campaign as patterns emerge.

---

## Proven Structure (Day 1 — sent but no reply data yet)

Both Day 1 briefs that passed human review and got sent followed this exact 5-part structure:

### Part 1: Open with a verified compliment

Cite the rating and review count from `doe_leads.rating` and `doe_leads.review_count`. These numbers come from Apify's Google Maps scrape — they are always accurate.

**Examples that shipped**:
- "Saw Royal Flush Plumbing at 4.8 stars with 366 reviews in Fort Worth — reputation is strong."
- "Saw FPP Plumbing sitting at a perfect 5.0 rating across 82 reviews for Frisco / Plano / McKinney — real reputation for a 24/7 emergency service."

**Why this works**: Using the actual rating + review count signals that the sender has looked at real data about THIS specific business. It is verifiable in 5 seconds by the recipient.

---

### Part 2: Bridge with "I looked at your site"

Transition from the compliment to the observation with a phrase that makes it clear the sender personally checked the site.

**Examples that shipped**:
- "Quick thing I noticed: royalflushplumbers.net is still on http:// not https://."
- "Pulled up your site on my phone this morning and noticed something worth flagging..."

**Why this works**: It positions the sender as someone who did real work before emailing. It also pre-frames the observation as helpful, not critical.

---

### Part 3: Specific verified observation

The core of the email. State ONE specific, verifiable thing you observed — with enough detail that the recipient can confirm it in under 10 seconds.

**Examples that shipped**:
- "Chrome shows visitors a 'Not Secure' warning before they even see your phone number." (Royal Flush — verified from `website_url` starting with `http://`)
- "You've got two phone numbers right at the top — a 980 area code (Charlotte, NC) and a 469 area code (Dallas). Both clickable, both dialing out." (FPP Plumbing — verified on mobile site visit)

**Why this works**: Specificity beats generality. "Your site loads slowly" is generic; "your Google Business Profile lists the 980 number as primary, so anyone calling from Google Maps routes through North Carolina" is specific AND shows depth of research.

**Rule**: Only include specific observations that are either (a) directly sourced from `doe_leads` / `doe_briefs` / `raw_audit_data`, or (b) manually verified by the human reviewer.

---

### Part 4: Connect observation to business impact

Explain why the observed issue matters in language Mike understands. Translate technical → money/calls, but without specific percentages or dollar amounts.

**Examples that shipped**:
- "...it spooks them into calling the next guy." (Royal Flush — Not Secure warning)
- "...'which number is the real one?' is exactly the split-second where they bail and call the next plumber on the list." (FPP — dual phone numbers)

**Why this works**: Mike cares about calls. Every observation must connect to "this costs you calls." Directional language ("the next plumber", "they bail", "split-second") feels real. Specific numbers ("60% bounce rate", "$400/hr") feel made up.

**Rule**: Use directional language, not numerical. See `hallucination-traps.md` Rule 5.

---

### Part 5: Single soft CTA + hardcoded signature

End with one low-friction ask, then the exact signature block.

**Examples that shipped**:
- "SSL fix is cheap, usually takes an afternoon. Happy to show you what I mean. Worth a quick call?"
- "Quick fix. Mind if I send over what I'd change?"

**Signature (exact, every time)**:
```
— Ola Oluwaseun
Divine Web · divineweb.co
```

**Rule**: ONE CTA. Not "reply and also book a call and also check my site". Pick the softest ask that still moves the conversation forward.

---

## Subject Line Patterns (Day 1 sample)

Only 2 data points so far. Both used lowercase, curiosity-driven, domain-referencing patterns:

- `quick note on royalflushplumbers.net`
- `quick thing on fppplumbing.com`

**Pattern**: `[filler word] [noun] on [their domain]`

Alternative patterns to test in later sends:
- `question about [business-name]`
- `[rating] stars and your mobile site`
- `noticed something on your site`
- `quick thing about [specific-detail]`

**To avoid**:
- "Website redesign for [business]"
- "Increase your conversions"
- "Free website audit"
- "Limited time offer"
- Anything with TitleCase capitalization
- Anything that sounds like marketing automation

---

## Language to Delete

These phrases appeared in AI-generated drafts during Day 1 review and were killed before sending:

| ❌ Delete | Replace with |
|---|---|
| "I sketched out a cleaner version" | "I have a specific fix in mind" |
| "I put together a mockup" | "I can walk through what I'd change" |
| "I hope this email finds you well" | (delete entirely, open with the compliment) |
| "I've been following your business" | (delete — it's almost always false) |
| "Dear Sir/Madam" | "Hi there," |
| "To whom it may concern" | "Hi there," |
| "I wanted to reach out regarding..." | (delete — just open with the observation) |
| "I think we can help..." | (delete — make them conclude you can help) |
| "Revolutionize your online presence" | (delete — pure jargon) |
| "Leverage cutting-edge technology" | (delete — pure jargon) |
| "Free consultation" | "Worth a quick call?" |
| "Limited time offer" | (delete — creates false urgency) |

---

## Word Count Target

**Target: 150 words ± 30. Max: 200 words.**

Day 1 briefs that shipped were 155 and 200 words. Both felt short enough to read in under 30 seconds on a phone, which is the reader experience that matters.

**Test**: read the email aloud at normal pace. If it takes more than 45 seconds, it's too long.

---

## Tone Calibration

The voice is:

- **Direct** — no throat-clearing openers, no filler
- **Respectful** — never condescending or assumes incompetence
- **Curious** — "I noticed..." not "You have a problem..."
- **Helpful** — framing problems as "you'd probably want to know" not "you're doing this wrong"
- **Human** — uses contractions, sentence fragments, occasional casual phrases ("the next plumber on the list")
- **Low-effort on their side** — every ask is small ("mind if I send over an idea?" not "book a strategy call")

**Anti-patterns**:

- ❌ Pushy: "You need to fix this NOW"
- ❌ Condescending: "You may not realize this but..."
- ❌ Fake urgency: "Before your competitors..."
- ❌ Over-polite: "I hope you don't mind me reaching out, I completely understand if you're busy..."

---

## Post-Reply Data (to be populated)

As replies come in to Day 1 sends, every reply gets logged in `doe_outreach_log` with `reply_sentiment` and `reply_text`. Every Sunday, the copy patterns from positive replies get extracted here as "proven winners" and the patterns from unsubscribes get logged as "proven losers."

**Target**: By Day 15, this file should have at least 3 confirmed winning patterns and 3 confirmed failing patterns, derived from real reply data.

---

## The Reply Loop

The point of this file is to make every week's briefs better than the last week's, using real reply data — not guesses. The Karpathy autoresearch principle applied to cold email:

1. Send baseline copy
2. Measure replies
3. Extract patterns from the winners
4. Update this file
5. Next week, generate copy that follows the winning patterns
6. Repeat

The system gets smarter at writing emails that get replies, not just emails that read well. The only real measure of copy quality is: did a real plumber reply?
