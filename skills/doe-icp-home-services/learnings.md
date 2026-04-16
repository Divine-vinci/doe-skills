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

### HTTP → HTTPS redirect trap
Scout stores pre-redirect URLs (e.g., `http://samedayplumbing.pro/`). The site may auto-redirect to HTTPS. Always verify SSL claim on mobile before claiming "Not Secure". Evidence: Same Day Plumbing showed HTTPS on Safari despite http:// in database.

### Verified observations that shipped
- No SSL (Chrome "Not Secure" warning) — verifiable from data
- Contact form buried at bottom — verifiable on mobile visit
- Chatbot + cart icon competing for attention — verifiable on mobile visit
- No favicon — verifiable in browser tab
- Footer shows old copyright year — verifiable on mobile visit
