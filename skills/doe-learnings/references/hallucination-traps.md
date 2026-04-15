# Hallucination Traps — Never Fabricate These

Every rule here was caught during real brief review sessions where the Brief Generation Agent invented specific facts that were not in the underlying data. **If you violate any of these rules, a real email will be sent with a provably false claim — the recipient will check, find the claim is wrong, and your credibility is gone forever.**

---

## Rule 1: Never cite specific load times without verified PageSpeed data — **CRITICAL**

**The violation**: Writing "your site takes 13 seconds to load on mobile" or "your homepage loads in 4-5 seconds" or "9 seconds before anything shows" when `doe_leads.pagespeed_mobile` is NULL.

**Why this happens**: The AI has been told in the prompt that load times are a pain point. When it doesn't have a real number, it invents one that sounds plausible.

**The rule**:

- If `doe_leads.pagespeed_mobile` is NULL → **do not cite any specific load time**.
- If `doe_leads.load_time_ms` is NULL → **do not cite any specific seconds value**.
- If both are NULL → use only generic language:
  - ✅ OK: "most plumber sites are slow on mobile"
  - ✅ OK: "mobile performance is probably leaking calls"
  - ❌ NOT OK: "your site took 8 seconds on my phone"
  - ❌ NOT OK: "loads in about 4-5 seconds before anything useful shows"
- If `pagespeed_mobile` is a real integer → you may cite that specific score, but only as-is (e.g., "PageSpeed score of 67"). Never translate score → seconds without the actual `load_time_ms` field.

**Evidence from real failure**: Day 1 session — 5 out of 5 reviewed briefs invented load times. Royal Flush "13 seconds", Public Service "barely loads", Metro Flow "10 seconds", Classic Plumbing "9 seconds", Pure Plumbing "4-5 seconds". Every single one of these was fabricated because `pagespeed_mobile` was NULL across the entire batch.

---

## Rule 2: Never claim specific UI bugs without CRO Agent verification — **CRITICAL**

**The violation**: Writing "your Learn More buttons don't actually click anywhere" or "the Schedule button redirects to Google" or "CTAs bounce to a different site" when no data in `doe_briefs.raw_audit_data` supports the claim.

**Why this happens**: The AI has been trained to find conversion issues, so when no data is available, it invents plausible-sounding UI bugs.

**The rule**:

- Before writing any specific UI bug claim, check `doe_briefs.raw_audit_data` for a matching finding in `broken_elements` or `cro_issues` arrays.
- If the claim is NOT explicitly present in that data, do not include it.
- Generic language about UI patterns is acceptable IF it is true of nearly all plumber sites:
  - ✅ OK: "the homepage isn't optimized for someone panicking about a leak at 2am"
  - ✅ OK: "mobile layout has the info but not in the order an emergency caller needs"
  - ❌ NOT OK: "your Leak Repair and Gas Line Repair Learn More buttons don't click"
  - ❌ NOT OK: "the Schedule An Appointment button redirects to a Google search"

**The exception**: If the claim is directly verifiable by the human reviewer in under 30 seconds AND the human has already confirmed it for this specific brief, include it. Otherwise, do not.

**Evidence from real failure**: Rockwater Plumbing brief claimed "Learn More buttons don't click" — when the user manually checked, the buttons worked fine. Evolution Plumbing brief claimed "CTAs bounce to a different site" — when the user checked, the CTAs worked perfectly and stayed on the domain. Both fabrications.

---

## Rule 3: Signature is hardcoded — never use placeholders or variables — **HIGH**

**The violation**: Outputting `— [Your Name]` or `— {agent_name}` or alternating between "Ola", "Ayo", "Ola Oluwaseun" across different briefs in the same batch.

**The rule**:

The signature block for every brief is **exactly this, always, no exceptions**:

```
— Ola Oluwaseun
Divine Web · divineweb.co
```

Not "Ayo". Not "[Your Name]". Not "Divine Web Services". Not "divineweb.com". Character-for-character the exact 2-line block above, including the em-dash (—) and the middot (·).

**Evidence from real failure**: Day 1 briefs had at least 3 signature variants in the same batch — literal `[Your Name]`, `– Ayo`, `– Ola`. Inconsistency across emails looks like automation and hurts reply rates. The literal `[Your Name]` placeholder would have been an instant-delete if sent.

---

## Rule 4: Never claim a prototype was built when it wasn't — **MEDIUM**

**The violation**: Writing "I sketched out a cleaner version", "I put together a mockup", "I built a quick prototype of your homepage", "Here's a draft I made" when no prototype actually exists.

**Why this matters**: If the recipient replies "send it over," the sender has nothing to show. Trust is destroyed at the worst possible moment — right after the prospect has raised their hand.

**The rule**:

- Do not claim any deliverable was prepared unless the system actually prepared it.
- Replace these phrases with honest equivalents:

| ❌ Dishonest | ✅ Honest |
|---|---|
| "I sketched out a cleaner version" | "I have a specific fix in mind" |
| "I put together a mockup" | "I can walk through what I'd change" |
| "I built a quick prototype" | "I have a quick idea" |
| "Here's a draft I made" | "Mind if I send over an idea?" |
| "I designed a phone-first version" | "There's a specific tweak I'd suggest" |

**Evidence from real failure**: Multiple Day 1 briefs claimed pre-built site redesigns. Not one was actually built. Catastrophic if sent to a prospect who says "show me."

---

## Rule 5: Never invent specific percentages or dollar amounts — **HIGH**

**The violation**: Writing "you're losing 40% of mobile visitors" or "that's $5,000 in missed revenue per month" or "3 out of 4 callers bail" when no data supports the specific number.

**Why this happens**: Specific numbers sound persuasive, so the AI generates them to strengthen claims.

**The rule**:

- Do not invent conversion percentages, bounce rates, visitor counts, or revenue figures.
- If citing an industry stat, either:
  - Reference a known-true ballpark that is generic enough to be defensible ("most people waiting more than 3 seconds leave")
  - Use the actual number from `doe_leads` / `doe_briefs` if it exists
- Business-impact language should be directional, not numerical:

| ❌ Fabricated | ✅ Directional |
|---|---|
| "40% of mobile visitors bail" | "most panicked visitors bail" |
| "$5,000/month in lost bookings" | "a handful of emergency calls a week" |
| "3 out of 4 callers hang up" | "most people calling at 2am want one unambiguous tap" |
| "You're leaking 60% of leads" | "leads are slipping away" |

**Evidence from real failure**: Day 1 briefs repeatedly cited ranges like "$200-$800 per missed call" and "a few $300-$800 jobs a week" — these ranges feel made up because they are. The revised-for-send versions removed all dollar ranges and used "each missed emergency call" or "a handful of bookings a week" instead. Those felt honest and got delivered.

---

## Rule 6: Never invent facts about business operations — **HIGH**

**The violation**: Writing "your dispatcher told me" or "I called and got no answer" or "I've been watching your Google reviews for a while" when none of that happened.

**The rule**:

- Do not claim phone calls, interactions, or observations that did not occur.
- Do not claim familiarity with the business beyond what the data shows.
- Anchor "I pulled up your site" language as the only acceptable observation claim — and only if the human reviewer actually did pull up the site.

---

## The Meta-Rule

**If you do not have the data, write generic language that is true of 90%+ of plumber sites.** Generic observations feel less punchy than specific ones, but they are **sendable**. Specific fabrications are not sendable. A boring email that goes out is infinitely more valuable than a "compelling" email that gets caught in human review and deleted.

## Post-Generation Self-Check

After drafting any brief, run this mental checklist:

1. Did I cite any specific load time? → Check `pagespeed_mobile` is NOT NULL in source data.
2. Did I claim any specific UI bug? → Check `raw_audit_data.cro_issues` or `broken_elements` contains it.
3. Did I use `[Your Name]` or any signature variant other than "Ola Oluwaseun / Divine Web · divineweb.co"? → Fix.
4. Did I claim a prototype exists? → Replace with honest language.
5. Did I invent a percentage or dollar amount? → Replace with directional language.
6. Did I claim operational familiarity ("I called", "your dispatcher", "I've been watching")? → Remove.

**If any answer is "yes, I violated the rule" — rewrite the output before returning.**
