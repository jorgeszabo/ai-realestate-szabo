# AI Real Estate Analyst — Realtor Edition
# Skill: realestate-realtor
# Command: /realestate realtor <address> [buyer|seller|listing]
# Version: 1.0 — Houston Metro Focus
# Built for: Alejandra Parra Szabo, licensed Realtor, Houston TX

---

## PURPOSE

Produce a client-ready Realtor report focused on what matters to
buyers, sellers, and listing appointments — not investor metrics.
This skill runs three parallel agents (comps, neighborhood, market)
and synthesizes them into a single professional output.

Not for: cap rates, cash flow, BRRRR analysis, rental yield.
Use /realestate invest or /realestate rental for those.

---

## TRIGGER

```
/realestate realtor <address>
/realestate realtor <address> buyer
/realestate realtor <address> seller
/realestate realtor <address> listing
```

If no mode specified, auto-detect from context or default to buyer.

---

## HOUSTON PRE-CHECK

Run before any other analysis. This fires for all Houston Metro
properties (Harris, Fort Bend, Montgomery, Brazoria, Galveston,
Liberty, Waller, Chambers counties).

1. **Flood zone:** Search FEMA FIRM map + Harris County Flood
   Control District (harriscountyfcd.org) for Harvey 2017 /
   Imelda 2019 flood history. Flag any flood history as RED FLAG.
2. **MUD district:** Look up all tax entities at the relevant
   county appraisal district. Identify MUD district and rate.
   County CADs: Harris→hcad.org | Fort Bend→fbcad.org |
   Montgomery→mcad-tx.org | Brazoria→brazoriacad.org |
   Galveston→galvestoncad.org | Liberty→libertycad.com |
   Waller→wallercad.org | Chambers→chamberscad.org
3. **Homestead status:** Check if currently homestead-exempt.
   Flag for buyers: "Taxes shown reflect prior owner's homestead
   exemption. Your taxes as a new owner may differ."
4. **Subdivision identification:** Note the exact subdivision name
   — required for accurate comps in Houston market.

---

## DATA GATHERING

Run these searches in parallel:

### Comps Agent
- Search HAR.com first for subdivision comps (not radius-based)
- Pull 5-8 closed sales in same subdivision, last 6 months
- If fewer than 3 subdivision comps, expand to adjacent subdivisions
- Note: two homes 0.3 miles apart in different subdivisions are
  not valid comps in Houston
- Extract: address, sold price, list price, $/sqft, beds/baths,
  sqft, DOM, sold date, list-to-sale ratio

### Neighborhood Agent
- School district and specific campus ratings (TexasSchoolGuide.com)
- Do NOT use Walk Score — Houston is car-dependent by design
- Highway access: proximity to Beltway 8, Grand Pkwy, I-10,
  I-45, I-69, I-610
- Master-planned community status and amenities
- Employment corridor proximity: Energy Corridor, Galleria,
  Medical Center, Downtown, NASA/JSC, Port of Houston
- Recent development and growth trajectory
- Crime index (search "[subdivision] crime rate")

### Market Agent
- Current active listings in same subdivision/zip
- Days on market trend (last 30/60/90 days)
- List-to-sale price ratio in the area
- Months of inventory
- Year-over-year price trend
- Seasonal context (Houston market timing)

---

## OUTPUT FORMAT

### BUYER MODE: `/realestate realtor <address> buyer`

```
================================================================
  BUYER'S PROPERTY REPORT
  [Address]
  Prepared by AI Real Estate Analyst | [Date]
  Data source: HAR MLS + public records
================================================================

  PROPERTY OVERVIEW
  Price:        [list price or estimate]
  Type:         [SFR/Condo/Townhome] | [Beds]/[Baths] | [Sqft]
  Year Built:   [year] | Subdivision: [name]
  School Dist:  [district] | Campus ratings: [elem/mid/high]

----------------------------------------------------------------
  IS THIS PROPERTY FAIRLY PRICED?
----------------------------------------------------------------
  List Price:       $[X]
  Price per sqft:   $[X] vs subdivision avg $[X]/sqft
  Estimated Value:  $[low]–$[high] based on [N] comps
  Assessment:       [Priced fairly / Priced above comps /
                    Priced below comps — opportunity]

  RECENT COMPS IN [SUBDIVISION]:
  [Address] | [sqft] | Sold $[X] ($[X]/sqft) | [DOM] days | [date]
  [Address] | [sqft] | Sold $[X] ($[X]/sqft) | [DOM] days | [date]
  [Address] | [sqft] | Sold $[X] ($[X]/sqft) | [DOM] days | [date]
  [show up to 5 comps]

----------------------------------------------------------------
  OFFER STRATEGY
----------------------------------------------------------------
  Market temperature:  [Hot/Warm/Cool] — [context sentence]
  Avg DOM in area:     [X] days
  List-to-sale ratio:  [X]% (sellers receiving [above/at/below] asking)
  Competition level:   [Low/Moderate/High] — [active listings count]

  Suggested offer range: $[X]–$[X]
  Reasoning: [1-2 sentences on why, based on comps and market temp]

  [If hot market]: Multiple offers likely. Consider escalation
  clause up to $[X]. Limit contingencies where possible.
  [If cool market]: Room to negotiate. Start at $[X] with
  inspection and financing contingencies.

----------------------------------------------------------------
  NEIGHBORHOOD
----------------------------------------------------------------
  Schools:      [District] — [Elementary] [rating]/10,
                [Middle] [rating]/10, [High] [rating]/10
  Commute:      [X] min to [nearest employment corridor]
                [Highway access: Beltway 8 / Grand Pkwy / I-X]
  Community:    [Master-planned / Established neighborhood /
                Urban / Suburban]
  Amenities:    [key community features]
  Growth:       [appreciation trend, development context]

----------------------------------------------------------------
  TRUE COST OF OWNERSHIP
  (Houston-specific — what buyers often miss)
----------------------------------------------------------------
  Est. mortgage (20% down, 30yr):  $[X]/mo
  Property taxes ([County], [X]%): $[X]/mo
  ⚠ MUD district taxes:            [CONFIRMED $X/mo OR UNCONFIRMED
                                    — verify at [CAD URL]]
  Homeowners insurance:            $[X]/mo (est. $[X]/yr)
  Flood insurance:                 [Required / Recommended /
                                    Not in flood zone] — $[X]/mo
  Wind/hail insurance:             $[X]/mo (separate policy, TX)
  HOA/POA fees:                    $[X]/mo
  ─────────────────────────────────────────────────────────────
  Est. total monthly cost:         $[X]/mo

  ⚠ HOUSTON BUYER NOTES:
  • Taxes shown may reflect prior owner's homestead exemption.
    Your taxes as a new owner may be higher until you file.
  • File for homestead exemption after closing to reduce future
    taxes and lock in the 10%/year assessment cap.
  • Texas has no state income tax — factor into rent vs buy math.

----------------------------------------------------------------
  RED FLAGS / DUE DILIGENCE CHECKLIST
----------------------------------------------------------------
  [ ] Flood zone verified at msc.fema.gov
  [ ] Harvey 2017 / Imelda 2019 history checked
  [ ] MUD district and rate confirmed at [CAD URL]
  [ ] HOA documents reviewed (deed restrictions, STR rules)
  [ ] Foundation inspection (pier-and-beam areas of Houston)
  [ ] AC system age (Houston heat is demanding on HVAC)
  [ ] Roof age and condition (hail and hurricane exposure)
  [ ] [Any property-specific flags found during analysis]

  [🔴 RED FLAG: if flood history found — add specific detail here]

================================================================
  DISCLAIMER: AI-generated report for educational purposes.
  Not a substitute for a licensed appraiser or inspector.
  Verify all data with HAR MLS and licensed professionals.
  Alejandra Parra Szabo | Licensed Realtor | Houston TX
================================================================
```

---

### SELLER MODE: `/realestate realtor <address> seller`

```
================================================================
  SELLER'S PRICING REPORT
  [Address]
  Prepared by AI Real Estate Analyst | [Date]
================================================================

  SUBJECT PROPERTY
  [Address] | [Beds]/[Baths] | [Sqft] | Built [year]
  Subdivision: [name] | County: [county]

----------------------------------------------------------------
  PRICING RECOMMENDATION
----------------------------------------------------------------
  Suggested list price:    $[X]–$[X]
  Estimated sale price:    $[X]–$[X]
  Price per sqft:          $[X] (subdivision avg: $[X]/sqft)

  Pricing rationale:
  [2-3 sentences explaining the recommendation based on comps,
  market temperature, and property-specific factors]

----------------------------------------------------------------
  COMPARABLE SALES (same subdivision, last 6 months)
----------------------------------------------------------------
  Address          Sqft   List $    Sold $    $/sqft  DOM  Date
  [comp 1]         [X]    $[X]      $[X]      $[X]    [X]  [date]
  [comp 2]         [X]    $[X]      $[X]      $[X]    [X]  [date]
  [comp 3]         [X]    $[X]      $[X]      $[X]    [X]  [date]
  [comp 4]         [X]    $[X]      $[X]      $[X]    [X]  [date]
  [comp 5]         [X]    $[X]      $[X]      $[X]    [X]  [date]

  Avg sold price:   $[X] | Avg $/sqft: $[X] | Avg DOM: [X] days
  Avg list-to-sale: [X]%

----------------------------------------------------------------
  MARKET CONDITIONS
----------------------------------------------------------------
  Market temperature:   [Hot/Warm/Cool]
  Active competition:   [X] similar listings currently active
  Months of inventory:  [X] months ([seller's/balanced/buyer's] market)
  Avg DOM (area):       [X] days
  Price trend (YoY):    [+/-X%]

  [If hot]: Seller's market. Price at the top of the range.
  Consider reviewing offers on a set date to create competition.
  [If cool]: Buyer's market. Price competitively from day one.
  Homes that sit go stale — correct pricing now saves time.

----------------------------------------------------------------
  POSITIONING STRATEGY
----------------------------------------------------------------
  Strengths to highlight:
  • [School district quality]
  • [Community/MPC features]
  • [Highway access / commute advantage]
  • [Property-specific strengths from data]

  Pricing adjustments vs comps:
  [+/- factors that differentiate this property from comps]

  Recommended list date context:
  [Seasonal timing note for Houston market]

----------------------------------------------------------------
  WHAT BUYERS WILL ASK
----------------------------------------------------------------
  Flood zone:      [Zone X / Zone AE / Verify at msc.fema.gov]
  Flood history:   [No flags found / RED FLAG: flooded in Harvey]
  MUD taxes:       [Confirmed $X/yr / Unconfirmed — verify at CAD]
  HOA fees:        $[X]/mo
  School district: [District name] — [ratings]

================================================================
  DISCLAIMER: AI-generated pricing analysis. Not an appraisal.
  Verify comps with HAR MLS. Consult with Alejandra for final
  pricing strategy and market expertise.
  Alejandra Parra Szabo | Licensed Realtor | Houston TX
================================================================
```

---

### LISTING APPOINTMENT MODE: `/realestate realtor <address> listing`

```
================================================================
  LISTING APPOINTMENT PREP
  [Address]
  Prepared by AI Real Estate Analyst | [Date]
================================================================

  ONE-MINUTE PROPERTY BRIEF
  [Address] | [Beds]/[Baths]/[Sqft] | Built [year]
  Subdivision: [name] | [School district]
  Est. value: $[X]–$[X] | Suggested list: $[X]

----------------------------------------------------------------
  MARKET SNAPSHOT (lead with these in the appointment)
----------------------------------------------------------------
  • [X] homes sold in [subdivision] in the last 6 months
  • Average sold price: $[X] ($[X]/sqft)
  • Average days on market: [X] days
  • List-to-sale ratio: [X]% — sellers are getting [above/at/below] asking
  • Current competition: [X] active listings in the area
  • Market direction: [prices up/stable/softening] [X]% YoY

----------------------------------------------------------------
  COMP SUMMARY (for CMA conversation)
----------------------------------------------------------------
  [comp 1]: [address] — [sqft] — Sold $[X] — [DOM] days — [date]
  [comp 2]: [address] — [sqft] — Sold $[X] — [DOM] days — [date]
  [comp 3]: [address] — [sqft] — Sold $[X] — [DOM] days — [date]
  [show 3-5 most relevant comps]

  Subject property at $[X]/sqft positions it [above/at/below]
  recent comps — [one sentence rationale].

----------------------------------------------------------------
  NEIGHBORHOOD SELLING POINTS
----------------------------------------------------------------
  Schools:    [District] — [Elementary] [rating], [High] [rating]
  Location:   [X] min to [nearest major employment area]
              [Highway: Beltway/Grand Pkwy/I-X access]
  Community:  [MPC name and key amenities if applicable]
  Trend:      [Growth/stability/appreciation context]

----------------------------------------------------------------
  QUESTIONS THE SELLER WILL ASK — YOUR ANSWERS
----------------------------------------------------------------
  "What should we list at?"
  → $[X]–$[X] based on [N] comps. I recommend $[X] to [rationale].

  "How long will it take to sell?"
  → At this price point, similar homes are selling in [X] days.
  [Context on current market conditions.]

  "Should we do repairs/updates first?"
  → [Based on comp analysis and market temp: brief recommendation]

  "Will buyers ask about flood?"
  → [Flood zone status and honest answer based on Pre-Check]

  "What about the MUD taxes?"
  → Total taxes including MUD are approximately $[X]/year at
  current assessment. Buyers should verify at [CAD URL].

----------------------------------------------------------------
  MARKETING ANGLE
----------------------------------------------------------------
  Lead headline: [suggested listing headline based on strengths]

  Top 3 selling points for this property:
  1. [school district or community name — strongest buyer draw]
  2. [location/commute/highway access]
  3. [property-specific feature or value relative to comps]

  Buyer profile most likely to purchase:
  [families with school-age kids / commuters to X corridor /
  move-up buyers / downsizers — based on property and area]

================================================================
  DISCLAIMER: AI-generated listing prep. Verify all comp data
  with HAR MLS before presenting to clients.
  Alejandra Parra Szabo | Licensed Realtor | Houston TX
================================================================
```

---

## OUTPUT ROUTING

Save reports to:
- Buyer reports → Google Drive / [client name] folder
- Seller reports → Google Drive / [address] listing folder
- Listing prep → Notion Active Pipeline / [client] record

All reports require Alejandra's review before sharing with clients.
Never send directly to clients without Alejandra's approval.

---

## HOUSTON-SPECIFIC NOTES FOR ALL MODES

**Texas property taxes:**
Houston Metro rates by county (combined all entities):
Harris: 2.0–2.8% | Fort Bend: 2.0–2.6% | Montgomery: 1.8–2.5%
Brazoria: 2.0–2.5% | Galveston: 2.0–2.5%
Always add MUD district separately — adds $0.50–$1.50/$100 valuation.

**Insurance — always show three line items:**
1. Homeowners: 0.75–1.25% of value/year
2. Flood: $800–$3,000/year (NFIP or private — separate policy)
3. Wind/hail: $500–$1,500/year (separate policy in Harris County)

**HAR.com is the authoritative source.**
Use HAR for all comp data. Zillow/Redfin lag Houston data by
30-90 days and misrepresent values by 5-15%.

**Subdivision comps only.**
Never use radius-based comps. Same subdivision = valid comp.
Different subdivision = not a valid comp, even if 0.3 miles away.

**Walk Score is meaningless in Houston.**
Use school ratings, highway access, and employment proximity instead.
