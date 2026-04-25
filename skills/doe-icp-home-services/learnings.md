# Learnings — doe-icp-home-services

This file logs every learning from every run. Loaded FIRST by the agent before any evaluation. Rules here OVERRIDE defaults in other reference files.

Last updated: April 16, 2026 (Day 2)

---

## ICP Filter Learnings

### Hard Disqualifiers (discovered Day 1-2)

1. **review_count > 1000 → auto-disqualify.** Businesses at this scale have in-house marketing or agency contracts. Cold emailing them is noise. Evidence: Public Service (1,822), Metro Flow (1,365), Service Squad (1,384), Cody & Sons (2,255), Milestone (18,975) — ALL rejected during human review.

2. **Franchise naming → auto-disqualify.** Patterns: 1-Tom-Plumber, Mr. Rooter, Roto-Rooter, Benjamin Franklin, Z Plumberz, Bluefrog, ARS Rescue, Mike Diamond, One Hour. Local owner can't approve a website change — corporate handles it. Evidence: 1-Tom-Plumber Arlington bounced with `550 5.4.1 Recipient address rejected`.

3. **URL contains /locations/ or /[city]-tx/ → franchise template.** Corporate franchise sites use location subpaths. Evidence: Service Squad (`/locations/fort-worth-tx-plumbing/`), Bacon Plumbing (`/plano-tx/`).

4. **"Home Services" or "Home Solutions" in business_name → auto-disqualify.** Parent brands covering multiple trades. Evidence: Evolution Home Services (Plano) — premium multi-service brand.

### Deprioritization Signals

5. **Premium/tech brand signals → -30 points.** "Next-Gen", "Precision", "Elite", "Professional Grade", dark-mode UI, SaaS-style copy. Evidence: Evolution Home Services.

6. **"Since YYYY" with YYYY < 2005 → -20 points.** 20+ years = iterated through at least one agency website already. Evidence: Classic Plumbing ("Since 1986"), solid mobile UX, nothing to pitch.

7. **Multi-service combos (Plumbing & Heating & Electrical) → -15 points.** Larger operations with in-house marketing. Exception: review_count < 200 may still be a scrappy operator.

### ICP Sweet Spot (positive signals)

- review_count 50-250
- Post-2010 founding (or no year mentioned)
- Owner-operator naming: surname, "& Sons", founder's name, single-trade
- Has a website but it feels template-built, DIY, or neglected
- Rating 4.3-5.0

### 9. NULL review_count from public-DB harvest (Apr 24)

When a lead arrives from the public-database harvest path (TSBPE/TDLR — `source = 'public_db'`), `review_count` and `rating` will be NULL — these businesses aren't necessarily on Google Maps.

**Do not auto-disqualify on missing review data.** Apply this fallback logic:
- `review_count IS NULL` → skip the standard 50-250 sweet-spot check entirely
- Score on: `license_class` (confirms niche), website existence (HTTPS probe passed), name pattern (owner-operator naming = positive signal), `license_owner_first_name` populated (high personalization value)
- A licensed Texas plumber with `license_match: true`, a working website, and an owner first name is a STRONG lead even with no review signal

### 10. Corporate keyword disqualifier (Apr 24)

Even from the license tables, business names containing these keywords AND a license issued within the last 5 years signal a chain/franchise structure where the local manager can't approve a website change:

`Services Inc, Group, Corporate, Holdings, Enterprise, Industries, National, Regional, Franchise`

**Auto-disqualify these regardless of review_count.**

Evidence: TDLR contains "AMERICAN RESIDENTIAL SERVICES" (ARS / Rescue Rooter parent) — looks like a single business in license records, is actually a multi-state corp with thousands of employees. Cold-emailing them is noise.

### 8. Commercial/B2B plumbers — DISQUALIFY (Apr 23)

A plumber/HVAC/electrician business that serves commercial clients (GCs, developers, property managers, hotels, multi-family buildings) is NOT Mike the Plumber. Our pitch ("homeowner backing out and calling the next plumber") doesn't land.

**Signals to detect**:
- Homepage hero shows named commercial buildings/projects (e.g., "The Bowie", "Skyloft")
- "Featured projects" or "Our work" section lists high-rises, hotels, offices
- Services menu mentions "commercial plumbing", "industrial", "construction services", "tenant improvement", "ground-up"
- "Clients" section shows logos of construction/development companies
- About/hero copy uses "GC", "general contractor", "RFQ", "spec", "bid"
- B2B language: "send plans", "request a bid", "commercial inquiry"

**Real evidence**: Biggs Plumbing Austin — 4.9/23 reviews, owner-operator naming, passed all v1 filters. Human Claude-in-Chrome verification revealed hero shows "The Bowie / Skyloft / 70 Rainey" high-rise projects. This is a B2B commercial subcontractor. Our residential Mike-the-Plumber pitch would have read totally wrong to them.

**Implementation**: Add to ICP Filter Agent prompt + detect in Scout v2's Firecrawl markdown scan. If commercial signals detected, deprioritize to 20 points (disqualify unless score >80 from other factors).

---

## Hallucination Learnings

### Never fabricate load times (CRITICAL)
If `pagespeed_mobile` is NULL or 0, do NOT cite any specific seconds value. Day 1: 5 out of 5 briefs invented load times ("13 seconds", "9 seconds", "4-5 seconds"). All fabricated.

### Never fabricate UI bugs (CRITICAL)
If CRO `broken_elements`/`cro_issues` doesn't flag it, don't claim it. Day 1: Rockwater brief claimed "Learn More buttons don't click" — manually verified: they worked fine.

### Never claim a prototype was built
"I sketched a version" / "I built a mockup" — if prospect replies "send it", there's nothing to show. Trust destroyed.

### Never invent percentages or dollar amounts
No "40% of visitors bail" or "$200-800 per missed call". Use directional language: "the next plumber on the list", "a handful of emergency calls a week".

### Signature is hardcoded
Always: — Ola Oluwaseun / Divine Web · divineweb.co. Never [Your Name], never Ayo, never Divine Web Services.

---

## Copy Learnings

### Sav framework works (adopted Day 2)
- Honest positioning ("I'm building out Divine Web") is a pattern disrupt vs corporate tone
- Named case study in Part 2 increases credibility
- Outcome + time + guarantee ("48 hours, don't love it, $0") eliminates risk
- Loom CTA ("2-minute Loom showing what I'd change") is lower friction than any call/meeting ask
- Cliffhanger subject lines ("saw this on your site", "thought you should know") outperform domain-referencing patterns

### Sav framework — refined structure (adopted Apr 25) — CRITICAL OVERRIDE

This SUPERSEDES all prior brief-tone rules. Follow this 5-part structure exactly. Stay creative WITHIN the structure — don't reinvent the structure.

**5-Part Email Body**:

1. **Opening observation** — one specific, true thing about THEIR business (review count + rating, named services visible on the site, copyright year, etc.). No mention of us yet.

2. **Polite self-introduction** — exactly one sentence, framed as the agency, not the individual:
   > "I'm Ola, building Divine — a small web design agency for local trades."

   Variations are fine ("...for local home-service businesses", "for plumbers and HVAC shops in Texas") but always:
   - "Divine" framed as **the agency we're building** (not "I do websites")
   - Introduces Ola once, then refers to Divine
   - Never name-drops past clients in the opener

3. **The problem in their customer's language** — describe the prospect's experience, not a technical audit. "When a homeowner with a leak lands on your site at 9pm on a phone, [specific friction]." Use the CRO findings to ground this in real observed issues.

4. **Offer — explicit 48-hour turnaround + zero-risk guarantee**, in 1-2 sentences:
   > "I can rebuild your homepage in 48 hours focused on [specific outcome — tap-to-call, simple form, etc.]. If it doesn't feel like a clear upgrade, you don't owe me anything."

   The phrase "**48 hours**" must appear. The phrase "**you don't owe me anything**" (or "you don't owe me a thing") must appear.

5. **Loom CTA — exact phrasing**:
   > "Would you mind if I share a 2-minute Loom video showing what I would change?"

   Minor variants OK ("Would it be useful if I shared..." / "Cool if I share..."), but ALWAYS:
   - "2-minute Loom"
   - "showing what I would change" (or "walking through what I'd change")
   - Always a question, never a statement

6. **Signature** — hardcoded, never invented:
   > — Ola Oluwaseun
   > Divine Web · divineweb.co

**Banned in the body**:
- Past client names in the opener ("recently helped Same Day Plumbing…") — save for replies
- Multi-sentence paragraphs about Divine's history, team, or specialty
- "We specialize in…", "our team…", "leveraging…", "synergy"
- Any technical jargon (PageSpeed, responsive, conversion-rate, CRO, SEO)

**Self-test before shipping**:
- Does the brief contain "Divine" exactly once or twice? ✓
- Does it explicitly mention "48 hours"? ✓
- Does it explicitly mention "2-minute Loom"? ✓
- Does it open with THEIR business, not us? ✓
- Is "I'm Ola, building Divine — a [agency framing]" in there? ✓

If any of those fail → rewrite.

### HTTP → HTTPS redirect trap (FIXED in pipeline, still verify)
Scout stores pre-redirect URLs from Apify (e.g., `http://samedayplumbing.pro/`). The site may auto-redirect to HTTPS. Pipeline now uses PageSpeed API's `finalUrl` (post-redirect) to determine `has_ssl` instead of the stored URL. But still verify on mobile before sending — belt and suspenders.

Evidence: Same Day Plumbing showed HTTPS on Safari despite `http://` in database. Don the Plumber LLC same issue — Apify stored `http://`, site redirects to `https://`, connection is secured. Both would have been false "Not Secure" claims.

### Verified observations that shipped
- No SSL (Chrome "Not Secure" warning) — verifiable from data
- Contact form buried at bottom — verifiable on mobile visit
- Chatbot + cart icon competing for attention — verifiable on mobile visit
- No favicon — verifiable in browser tab
- Footer shows old copyright year — verifiable on mobile visit
