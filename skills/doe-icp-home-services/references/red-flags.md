# Red Flags — Leads to Skip

Not every business with a bad website is a good lead. Disqualify these early to save time and outreach budget.

## Hard Disqualifiers (Always Skip)

### Website Redesigned Within Last 12 Months
**How to detect**: Check Wayback Machine (web.archive.org). Look at the site from 2025 vs now. If it changed significantly, they already invested recently.
**Why skip**: They won't redo it. They're emotionally and financially committed to the current version.

### Business Has "Marketing" or "Digital" in Their Name
**Examples**: "Dallas Marketing Plumbers," "Smith Digital Services"
**Why skip**: They think they know marketing. They'll argue with every suggestion. Not worth it.

### Franchise Locations
**How to detect**: Check the business name — "Roto-Rooter of Dallas," "Mr. Rooter," "Benjamin Franklin Plumbing"
**Why skip**: Decisions are made at corporate HQ, not the local owner. The local manager CAN'T buy a website even if they want to.

### More Than 50 Google Reviews at 4.8+ Stars AND Modern Website
**How to detect**: If pain_score < 30 AND review_count > 50 AND rating >= 4.8
**Why skip**: They're doing fine. Your pitch won't land.

### No Phone Number Anywhere
**How to detect**: No phone in Google listing, no phone on website (if they have one), no phone in Yelp
**Why skip**: They might be out of business. Or worse — they might be a scam business themselves.

### Yelp/Google Listing Marked "Permanently Closed"
**How to detect**: Apify Google Maps scraper returns this flag
**Why skip**: Obviously — they're closed. Dead lead.

### Chain Business With 50+ Locations
**How to detect**: Business name suggests chain, website shows multiple locations
**Why skip**: Same as franchise — decisions are at corporate.

---

## Soft Disqualifiers (Lower Priority, Not Delete)

### Very Low Review Count (< 5 reviews)
**Why lower priority**: Could be brand new business without budget, OR a ghost business that barely operates.
**Action**: Move to bottom of outreach queue, try if you have nothing better.

### No Email Discoverable After MX + Website Scrape
**Why lower priority**: No direct way to reach decision maker.
**Action**: Could try contact form submission, but low conversion.

### Business Name Suggests Part-Time Operator
**Examples**: "John's Handy Services," "Weekend Plumbing"
**Why lower priority**: Often not a real business with a budget — just a guy with tools.
**Action**: Skip unless the website data suggests otherwise.

### Website Uses a Template Obviously From a Local Competitor
**Example**: The site clearly uses a regional web agency's template with their branding in the footer
**Why lower priority**: They have a relationship with another agency. Switching is harder.
**Action**: Try anyway with a strong demo, but don't prioritize.

### Located in Very Small Town
**How to detect**: City has < 20 businesses in your search
**Why lower priority**: Small market, low ticket sizes, price-sensitive.
**Action**: Skip unless you need fill-in leads.

---

## Green Flags (Prioritize These)

### High Pain Score (80+)
**Why**: Website is actively hurting their business. They feel it even if they can't articulate it.

### Active Google Ads (check if ads link to their website)
**How to detect**: Apify can scrape Google Ads presence. Look for "sponsored" results in Google searches.
**Why**: They're SPENDING MONEY on marketing already. Budget confirmed.

### Recent Surge in Reviews
**How to detect**: Compare review count over time via Wayback Machine or Apify's historical data
**Why**: Business is growing, cash flowing, willing to invest.

### 4.3-4.7 Star Rating (Good But Not Perfect)
**Why**: They care about reputation but know they're not perfect. Open to improvement.

### Multiple Phone Numbers Listed
**Why**: Suggests multiple trucks / growing business. Bigger budget.

### Professional Logo / Branded Vehicles in Photos
**Why**: They've already invested in branding — they'll invest in web.

### Listed on HomeAdvisor / Angi
**Why**: They're paying for leads. A good website reduces their cost per lead, which is a compelling ROI pitch.

---

## Priority Scoring Formula

Once you've filtered out hard disqualifiers, score remaining leads:

```javascript
let priority = pain_score;  // 0-100 from Scout

// Boost green flags
if (lead.running_google_ads) priority += 15;
if (lead.review_count >= 20 && lead.rating >= 4.3) priority += 10;
if (lead.rating >= 4.5 && lead.rating < 4.8) priority += 5;
if (lead.has_branded_vehicles) priority += 5;

// Penalty for soft flags
if (lead.review_count < 5) priority -= 10;
if (lead.email_status !== 'valid') priority -= 15;
if (lead.city_population < 50000) priority -= 10;

// Normalize
priority = Math.max(0, Math.min(100, priority));
```

Work leads from highest to lowest priority.

---

## The Daily Disqualification Sweep

Every morning, run a quick check:

1. **Review previous day's replies**: Did anyone ask to be removed? Add them to disqualified list permanently.
2. **Check for "permanently closed"**: Update Google Maps data monthly.
3. **Check for "out of office" replies**: Don't re-send until after their OOO date.
4. **Check for bounces**: If bounced, mark email as invalid, don't retry.

Keep your lead database clean. A polluted database wastes outreach budget and hurts deliverability.
