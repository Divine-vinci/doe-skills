# ICP Filters — Who to Disqualify (and Why)

Every rule here was discovered by qualifying a lead through the AI ICP filter, generating a brief, and then catching the mismatch during human review. **Each pattern represents at least one wasted OpenAI call, one wasted Firecrawl page scrape, one wasted Intel agent run, and a near-miss on a broken cold email.**

The ICP Filter Agent must evaluate every lead against these patterns BEFORE applying any pain-point scoring. If a lead matches a disqualification pattern, immediately return `status: 'disqualified'` — do not pass it to Score Lead, do not generate a brief, do not waste tokens.

---

## Pattern 1: Review count > 1000 — **CRITICAL disqualification**

**The rule**: If `doe_leads.review_count > 1000`, the lead is **automatically disqualified**. No exceptions.

**Why**: A plumbing business with 1,000+ Google reviews has been operating at scale long enough to have:
- An in-house marketing coordinator OR
- A contracted marketing agency OR
- A corporate marketing team (if it's a franchise)
- A paid SEO/PPC operation
- A pro-built website that gets updated regularly

Cold emailing them about website improvements is noise. Their marketing person deletes it in 2 seconds. Their site is already good, their SEO is already working (that's how they got to 1000+ reviews), and they're not the bootstrapped owner-operator DOE targets.

**Real evidence**:

| Business | Reviews | What we saw |
|---|---|---|
| Public Service Plumbers | 1,822 | Modern site, visible phone + Request Service CTA, 60-year brand |
| Metro Flow Plumbing | 1,365 | Established Dallas brand, solid site |
| Service Squad | 1,384 | Franchise operation, corporate site |
| Cody & Sons | 2,255 | Multi-generational family business |

All four were AI-qualified as ICP matches. All four were human-rejected in under 3 minutes of site review.

**Implementation note**: The ICP Filter Agent is currently giving these high `icp_fit_score` ratings. The review count hard-stop must execute BEFORE the agent's scoring logic runs.

---

## Pattern 2: Franchise brand naming — **CRITICAL disqualification**

**The rule**: If `doe_leads.business_name` matches any franchise pattern below, **disqualify immediately**.

### Franchise patterns to match (case-insensitive)

**Numbered prefixes**:
- `1-Tom-Plumber`, `1-800-Plumber`, `5-Star`, `24/7 Plumbing`, any `N-Something` pattern

**"Mr./Mrs." prefixes**:
- `Mr. Rooter`, `Mr. Plumber`, `Mr. Appliance`, `Mrs. Plumber`, `Mr. Handyman`

**Regional franchise chains**:
- `Roto-Rooter`, `Benjamin Franklin`, `Rescue Rooter`, `Bluefrog`, `Z Plumberz`, `ARS Rescue`, `Pro Plumber Hero`, `Mike Diamond`, `One Hour`, `Expert Plumbing & Heating`

**Multi-location indicators in URL**:
- `website_url` contains `/[city-name]/` or `/locations/` subpath → likely franchise corporate template
- `website_url` contains `-tx/`, `-arlington/`, `-dallas/`, etc. → franchise landing page structure

**Why**: Franchise sites are built and managed by corporate headquarters. The local owner cannot:
- Change the website
- Update contact info
- Approve a redesign
- Sign a contract with an outside web developer

Even if you reach the local owner, they'll reply "I'd love to but corporate handles the site." The local email address is often a vanity alias that doesn't even route to a real inbox.

**Real evidence**: 1-Tom-Plumber Arlington (Day 1, April 15). Apify scraped `arlington@1tomplumber.com` as the contact email. When sent, Outlook rejected with `550 5.4.1 Recipient address rejected: Access denied`. The mailbox didn't exist. The website was hosted at `1tomplumber.com/ft-worth-tx/` — classic corporate franchise template with location subpaths. Disqualified immediately across all locations after the bounce.

**Expected bounce rate for franchise leads that slip through**: 30-50%.

---

## Pattern 3: Premium / tech-branded sites — **HIGH disqualification**

**The rule**: If the business has premium brand signals, **disqualify or aggressively deprioritize**.

### Signals to detect

**In `business_name` or homepage headline** (from Firecrawl scrape):
- "Next-Gen", "Next Generation", "Smart", "Advanced", "Precision", "Elite", "Professional Grade"
- "Solutions" used corporately (e.g., "Evolution Home Solutions", "Pro Plumbing Solutions")
- "Infrastructure", "Systems", "Technologies"

**In website design signals** (from CRO agent's analysis of `website_markdown`):
- Dark-mode UI with gradient accents
- Multi-service parent brand ("X Home Services" covering plumbing + HVAC + electrical + more)
- Icon grids with 6+ service cards
- Hero copy written in SaaS/product-marketing voice
- Custom iconography (not stock icons)

**Why**: These are brands that already have design systems, premium photography, and six-figure websites. They have product managers, not owner-operators. They are not Mike the Plumber.

**Real evidence**: Evolution Home Services (Plano, Day 1). Tagline: "Next-Gen Plumbing Solutions — Precision engineering meets domestic reliability." Subtitle: "Professional Grade Infrastructure." Dark UI, blue gradient accents, 8+ service cards with custom icons. Site is indistinguishable from a modern SaaS landing page. Cold-emailing them about website improvements would be insulting — their site is better than most tech startups.

---

## Pattern 4: "Since [year < 2005]" established brands — **HIGH deprioritization**

**The rule**: If the business displays "Since YYYY", "YYYY-founded", or an anniversary badge with `YYYY < 2005`, **deprioritize**. Only qualify if the site also shows clear signs of being bad (old copyright, broken UX, no SSL, poor mobile).

**Why**: 20+ years of operation almost guarantees the business has iterated through at least one agency-built website by now. They already know web redesigns exist. Their current site was probably paid for, and they're not in the market for another one.

**Real evidence**: Classic Plumbing (Plano, Day 1). "Your Local Family-Owned Plumber Since 1986." 40-year anniversary badge. 4.7 stars, 270+ reviews. Mobile site: tap-to-call phone number, prominent "Call Us" button, contact form above the fold, HTTPS, 24/7 emergency positioning. Nothing to pitch them on. The site is already doing the job.

**Implementation note**: This is a deprioritization, not a hard disqualify, because a 40-year-old business with a genuinely broken site IS still a lead. But the default assumption should be that old businesses have solid sites.

---

## Pattern 5: Multi-service parent brands — **MEDIUM deprioritization**

**The rule**: If `business_name` or homepage headline shows multi-service offerings (plumbing + HVAC + electrical + roofing), **deprioritize unless `review_count < 200`**.

### Patterns to match

- "Plumbing, Heating & Air"
- "Plumbing & Electrical"
- "Home Services" (when it covers multiple trades)
- "X & Y & Z" service combinations

**Why**: Multi-service brands are almost always larger operations with in-house marketing. They have more revenue to spend on tools and agencies. The bootstrapped single-trade plumber is 5x more likely to be the real ICP.

**Exception**: A small multi-service business with fewer than 200 reviews is probably legitimately a scrappy owner-operator who just offers multiple trades.

**Real evidence**: Observed across Day 1 batches. Single-trade "Acme Plumbing" leads consistently had worse websites than multi-service "Acme Plumbing, Heating & Cooling" leads.

---

## Pattern 6: Home Services parent brands — **HIGH disqualification**

**The rule**: If `business_name` contains "Home Services", "Home Solutions", or similar umbrella terms, **disqualify**.

**Why**: These are almost always parent brands that own multiple service lines — not the owner-operator single-trade plumber DOE targets. They have the budget and scale that makes DOE pointless.

**Real evidence**: Evolution Home Services (Plano) — "Home Services" in the name was a giveaway that this was a premium multi-service parent brand. The actual site confirmed this.

---

## The Real ICP (Positive Signals to Prioritize)

After 6 ICP-miss patterns, here is what the actual DOE sweet-spot lead looks like. **Prioritize** leads with these characteristics:

| Signal | Threshold |
|---|---|
| `review_count` | **50–250** (not too small, not too established) |
| Business founding year | Post-2010 (or no year mentioned at all) |
| Brand pattern | Owner-operator naming: family surname, "& Sons", founder's first name, single-trade focus |
| Independence | Not a franchise, not a multi-service parent brand |
| Web presence signals | Has a website but it feels template-built, DIY, or neglected |
| Badges on their own site | NO "N+ Google Reviews" badges displayed (means they're not obsessing over web presence), NO BBB A+ badges, NO "Since 19XX" anniversary graphics |
| Rating | 4.3–5.0 (shows quality, but many small operators don't have 1000+ reviews) |

**The Day 1 winners that fit this profile**:
- **Royal Flush Plumbing** (Fort Worth): 4.8 stars, 366 reviews, HTTP site (no SSL), personal yahoo email, independent brand
- **FPP Plumbing** (Plano/Frisco/McKinney): 5.0 stars, 82 reviews, acronym branding, visible UX quirks worth mentioning

Both landed in the inbox. Both have verifiable pain points. Both are legitimately owner-operated.

---

## Execution Order for ICP Filter Agent

When scoring a lead, run checks in this order. The first match disqualifies:

1. **Review count > 1000** → disqualify (Pattern 1)
2. **Franchise naming regex match** → disqualify (Pattern 2)
3. **`website_url` contains `/[city-name]/` or `/locations/`** → disqualify (Pattern 2)
4. **Home Services / Home Solutions in business_name** → disqualify (Pattern 6)
5. **Premium/tech brand signals in business_name or homepage headline** → deprioritize score by 30 points (Pattern 3)
6. **"Since YYYY" with YYYY < 2005** → deprioritize score by 20 points (Pattern 4)
7. **Multi-service combinations (X & Y & Z)** → deprioritize score by 15 points (Pattern 5)
8. **None match** → apply normal scoring

If the adjusted `icp_fit_score` after deprioritization falls below 40, mark as `disqualified`.

---

## When In Doubt, Disqualify

**False negatives are cheaper than false positives.** A disqualified-but-actually-good lead costs you $0. A qualified-but-actually-wrong lead costs you OpenAI tokens, Firecrawl credits, human review time, AND risks a fabricated cold email hitting a real inbox.

When any signal is ambiguous, disqualify and move on. There are 5,000+ plumbers in Texas. Spending 5 minutes on a near-miss is not worth it when the next search returns 20 fresh leads.
