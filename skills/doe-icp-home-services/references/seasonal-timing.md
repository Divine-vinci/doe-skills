# Seasonal Timing & Send Windows

When to outreach which niches, and when Mike is most likely to read your email.

## Seasonal Niche Priorities

| Season | Best Niches | Why |
|--------|------------|-----|
| **Spring (Mar-May)** | HVAC, Landscaping, Roofing | AC season starting, spring projects, people getting quotes |
| **Summer (Jun-Aug)** | Plumbing, HVAC | Emergency calls spike (burst pipes in heat, broken AC), businesses are flush with cash |
| **Fall (Sep-Nov)** | HVAC, Plumbing, Roofing | Heating season prep, winterization, pre-winter roof checks |
| **Winter (Dec-Feb)** | All niches (slower season) | Business owners have MORE time to think about marketing — but ALSO tighter budgets after holidays |

### April 2026 Context (Current)

We're in early spring. This is **perfect timing for plumbers and HVAC**:
- HVAC: Pre-cooling season, quote requests starting
- Plumbers: Steady demand, businesses have cash from winter work
- Landscapers: Season just starting, marketing pitch lands well

---

## Best Day/Time to Send

### The Optimal Window

**Tuesday, Wednesday, Thursday** — arrive in his inbox at **6-8 PM local time**

Why this window:
- **Not Monday** — Mike is overwhelmed after the weekend, email gets buried
- **Not Friday** — he's thinking about the weekend, not business decisions
- **Not weekends** — he's with family, won't respond
- **6-8 PM arrival** — he's home, relaxed, checking email on his phone before dinner or after
- **Not morning** — he's on his way to jobs, won't read carefully
- **Not mid-day** — he's on job sites, phone in pocket, can't focus

### Send Logic (Automated)

```
IF day in [Tue, Wed, Thu]:
  IF local_time between 5pm-8pm:
    SEND NOW
  ELSE:
    SCHEDULE FOR next 5pm-8pm window
ELSE:
  SCHEDULE FOR next Tuesday at 6pm local time
```

### Time Zone Matters

Dallas is CST. If sending from the VPS (UTC), the math is:
- 6 PM CST = 11 PM UTC
- 8 PM CST = 1 AM UTC (next day)

Schedule sends in UTC to hit CST evening window.

---

## Frequency Rules

### Per Lead

- **Email 1**: Initial outreach
- **Email 2**: +48 hours (no reply)
- **Email 3**: +96 hours (no reply) — breakup email
- **Stop after 3.** Never send a 4th.

### Per Day (Volume Limits)

- **Month 1 (single domain)**: Max 30 emails/day
- **Month 2 (two domains)**: Max 60 emails/day (30 per domain)
- **Never exceed 50/day per inbox** — kills deliverability

### Batching Within the Window

If sending 30 emails in a 3-hour window (5-8 PM):
- 10 emails per hour = 1 every 6 minutes
- Add random 3-8 minute delays between sends
- Avoids spam filter pattern detection

---

## Seasonal Messaging Hooks

Tailor the opener to the season for extra relevance:

### Spring

- "With AC season coming, are you getting more calls than you can handle?"
- "I noticed your site doesn't mention spring maintenance specials..."

### Summer

- "July is peak emergency call season for plumbers — is your site ready?"
- "Google searches for 'emergency AC repair' are up 40% this week in Dallas..."

### Fall

- "Winterization season starts in 6 weeks — your site should be ranking for it now"
- "Pre-winter heating checks are huge revenue for HVAC — but your site doesn't have a booking form..."

### Winter

- "Slow season is the perfect time to upgrade your website for spring rush"
- "Your competitors are updating their sites right now — get ahead of them"

---

## Holidays & Dead Zones (DO NOT SEND)

- **Thanksgiving week** (Monday before through Sunday after)
- **Christmas week** (Dec 22 - Jan 2)
- **Fourth of July week**
- **Memorial Day weekend**
- **Labor Day weekend**

Sending during these times:
- Low open rates (everyone's with family)
- High spam-mark rates (people are annoyed)
- Messages buried when they return to inbox

Better strategy: Pause outreach for 5-7 days, resume with fresh leads.

---

## Monitoring Timing Performance

Track in your dashboard:

| Metric | Good | Bad |
|--------|------|-----|
| Open rate by day | Tue/Wed/Thu 30-40% | Mon/Fri < 20% |
| Open rate by hour | 6-9 PM 35%+ | 2-5 AM < 10% |
| Reply rate by day | Wed/Thu highest | Weekends lowest |

If you see patterns breaking these expectations (e.g., Sunday morning is converting well), trust the data over the rule.
