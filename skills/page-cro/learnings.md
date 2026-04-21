# Learnings — Page CRO

This file logs every learning from real production runs. Loaded FIRST by the agent before any evaluation. Rules here OVERRIDE defaults in other reference files.

Last updated: April 21, 2026

---

## CRITICAL: You Cannot Detect Visual Bugs From Markdown

You analyze Firecrawl markdown output, NOT the live rendered site. This means you CANNOT reliably detect:
- Broken buttons or links (they look identical to working ones in markdown)
- Overlapping sections (CSS rendering issue, invisible in text)
- Design quality or visual aesthetics
- Whether elements are clickable or functional
- Missing favicon or browser tab icons

**Real failures**:
- Ron's Irving Plumbing — CRO claimed "testimonials section shows the word 'Button' instead of a real button." Human verified: there IS no testimonial section at all.
- Rockwater Plumbing — CRO claimed "Learn More buttons don't click." Human verified: buttons worked fine.

**Rule**: Only report boolean presence/absence facts. Does a form exist? Does a CTA exist? Is a phone number present? What services are listed? What tech stack? What copyright year? NOTHING about visual rendering, broken elements, or design quality.

## Reliable Signals (safe to report)

- has_contact_form: true/false — detectable from form HTML in markdown
- has_cta: true/false — detectable from button/link text
- has_click_to_call: true/false — detectable from tel: links in markdown
- services_listed: extractable from heading/list text
- tech_stack: sometimes detectable from meta tags, "Powered by" text
- copyright_year: detectable from footer text

## Unreliable Signals (DO NOT report)

- broken_elements — REMOVED from schema, never report
- cro_issues — REMOVED from schema, never report
- design_score — REMOVED from schema, never report
- Any claim about visual appearance, layout, or rendering
