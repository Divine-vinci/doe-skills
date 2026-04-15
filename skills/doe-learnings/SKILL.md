---
name: doe-learnings
description: Accumulated learnings from real DOE cold outreach campaigns. MUST be loaded by the ICP Filter Agent and Brief Generation Agent on every run. Contains hard rules derived from observed failures — hallucination traps to avoid, ICP exclusion patterns (review count, franchise, premium-branded, established businesses), verification requirements for human review, and copy patterns proven to work or fail. Updated weekly from the doe_learnings Supabase table. Load this BEFORE any ICP scoring or brief generation to avoid repeating past mistakes.
metadata:
  version: 1.0.0
  source: https://github.com/Divine-vinci/doe-skills
  updated: 2026-04-15
---

# DOE Learnings — Hard-Won Rules From Real Outreach

This skill captures everything DOE has learned from actually running cold outreach campaigns. It exists because the Brief Generation Agent and ICP Filter Agent — left to themselves — will repeat the same mistakes every run: hallucinating load times, qualifying businesses that are too big, writing generic copy, using inconsistent signatures.

**Every rule here was paid for with real session time, real OpenAI tokens, and real near-misses that almost hit a plumber's inbox with false claims.**

Load this skill FIRST before any scoring or brief generation. The rules here OVERRIDE any contradictory defaults in other skills.

## How This Skill Works

The rules below are **non-negotiable constraints**. If a rule conflicts with what an agent is about to output, the agent must adjust its output to comply with the rule — not the other way around.

The learnings are organized into four reference files:

1. **[hallucination-traps.md](references/hallucination-traps.md)** — Never fabricate these. Covers load times, UI bugs, pre-built prototypes, signature placeholders, and name inconsistencies.

2. **[icp-filters.md](references/icp-filters.md)** — Disqualify these patterns. Covers review count thresholds, franchise brands, premium/tech branding, established "since 19XX" businesses, and multi-service parent brands.

3. **[verification-rules.md](references/verification-rules.md)** — Human review protocol. What must be verified against live data before a brief is considered sendable.

4. **[copy-patterns.md](references/copy-patterns.md)** — What worked in real sends (to replicate) and what failed (to avoid).

## Core Directive

**When generating any brief or ICP score, iterate through the rules in each reference file and verify your output does not violate any of them.** If you catch yourself about to violate a rule, rewrite the output.

## How New Learnings Get Added

Every outreach session captures lessons in the `doe_learnings` Supabase table. Each week, the highest-severity pending lessons are synthesized into updates to these reference files and pushed to this repo.

The cycle:

1. **Monday-Thursday** — run campaigns, catch mistakes in human review
2. **Record lessons** in `doe_learnings` table with category + severity + rule
3. **Sunday night** — weekly skill update ritual:
   - Query `v_doe_pending_learnings`
   - Synthesize each pending lesson into a rule in the appropriate reference file
   - Commit + push to `Divine-vinci/doe-skills`
   - Mark lessons as `applied_to_skill = true` in Supabase
4. **Next Monday** — Intel agents load the updated skills and avoid last week's mistakes

## Severity Legend

When you see `**CRITICAL**`, `**HIGH**`, `**MEDIUM**`, or `**LOW**` tags in the reference files:

- **CRITICAL** — If you violate this, a real email gets sent with a provably false claim. This destroys credibility and burns a sending domain. Never violate under any circumstances.
- **HIGH** — If you violate this, the output quality drops significantly. Reply rates collapse. Budget gets wasted on bad leads. Fix before output is returned.
- **MEDIUM** — If you violate this, the output is noticeably worse but not catastrophic. Fix when feasible.
- **LOW** — Minor polish. Apply when convenient.

---

## The Meta-Lesson

The AI is a drafting tool, not a decision maker. Every specific claim in every brief must be traceable to real data in `doe_leads`, `doe_briefs.raw_audit_data`, or `doe_outreach_log`. If the data is NULL, the claim must not be specific. **When you don't have facts, say something generic. Never invent facts.**
