# PRD 2: Unified Project Dashboard — Comparison Matrix
**Author:** Emeri | **Status:** Mockup / Portfolio Artifact | **Date:** March 2026
**Depends on:** PRD 1 — Structured Quote Object

---
## [Demo](https://claude.ai/public/artifacts/45ba05ec-6225-4fb9-98ba-1b8816cb719c)
## Problem Statement

PRD 1 solved the data schema problem: merchants now send structured Quote Objects instead of freeform text. But solving the supply side creates a new consumer-side problem.

**A user who messaged 5 salons now has 5 open chat threads. Price and availability are structured — but still siloed per conversation.** There is no way to compare across salons without manually tabbing between chats.

> *"I have 5 open chats and I'm losing track of who is cheapest and who is available at 2 PM."*

This is a **data aggregation problem**. The Quote Objects exist. The fields are structured. The Comparison Matrix simply surfaces them in one place.

---

## Goal

Build a **Unified Project Dashboard** — a "Project Details" tab view that aggregates all structured quote data into a sortable, filterable comparison table, with clickable slots that either trigger in-app booking or redirect to the merchant's external site, and a direct "→ Chat" deep-link per row.

---

## Users

| Role | Goal | Pain Today |
|---|---|---|
| Consumer (Emeri) | Compare price + availability across all salons in one view | Must context-switch across 5 chats manually |
| Yelp | Increase BCR; keep users in-app | Users drop off or go off-platform to compare |

---

## Data Sources & Parsing Logic

The dashboard pulls from two sources depending on how the merchant responded.

### Source A — Structured Quote Objects (PRD 1)
Merchants who used the Bid Card form have fully structured data. Fields map directly:

| Bid Card Field | Dashboard Column |
|---|---|
| `base_price` + `addons[]` | Price |
| `slots[]` | Availability (clickable chips) |
| `specialist_badge` | Specialty Match |
| `match_reason` | Match Reason |
| `booking_url` (if provided) | Slot click behavior → redirect |

### Source B — Legacy Freeform Text (NLP Auto-Parse)
Merchants who sent unstructured text responses are auto-parsed by Yelp's NLP layer. The system attempts to extract:
- **Price signals:** currency patterns, "starting at", "rates from"
- **Stylist names:** proper nouns in service context
- **Availability:** time expressions, date references
- **Booking method:** URL detection → flag as "external booking"

**Real example — Milvali SF's freeform response:**
> *"We recommend Honey and Mercy with rates starting at $120, or Fiona with rates starting at $135 for women's haircuts. You can book directly through our site."*

**Auto-parsed output:**
- Price: `$120+` (Honey/Mercy), `$135+` (Fiona)
- Stylists: Honey, Mercy, Fiona
- Availability: *(extracted from follow-up or shown as "Confirm with merchant")*
- Booking: external URL → slot clicks redirect to merchant site

**Confidence scoring:** Each auto-parsed field carries a confidence score. Low-confidence extractions are flagged with a "⚠ Confirm with merchant" indicator rather than shown as fact.

**Merchant nudge:** If a merchant's data was auto-parsed (not structured), Yelp sends a nudge: *"Complete your structured quote to increase bookings."*

---

## The Comparison Matrix

### Location
Lives in the **"Project Details"** tab of the existing Yelp Projects view — same left panel consumers already use (see PRD 1 screenshot reference). Activating this tab replaces the current empty/minimal project info with the full matrix.

### Columns

| Column | Description | Notes |
|---|---|---|
| **Salon** | Name + avatar + star rating | Sorted by Match Score by default |
| **Price** | Base price + add-ons | Standardized `$XX` format; shows `$XX+` for "starting from" |
| **Availability** | Clickable time slot chips | Tapping a slot triggers booking action (see below) |
| **Specialty Match** | Badge or stylist names | Structured: badge label. Parsed: stylist names |
| **Match Reason** | Why Yelp surfaced this salon | NLP-generated; builds trust that Yelp "read" the request |
| **→ Chat** | Deep-link to individual chat thread | Always present; opens the chat for that salon |

### Slot Click Behavior (two states)

| Merchant Type | Slot Click Action |
|---|---|
| Uses Yelp booking / Bid Card | In-app "Request to Book" flow (PRD 1 confirm state) |
| External booking site (e.g. CODE Salon, Milvali SF) | Redirect to merchant's website in new tab + log as exit event |

External redirects are tracked as off-platform exits — a key signal for Yelp to prioritize onboarding those merchants to native booking.

### Sort & Filter

**Sort options (single-select):**
- Best Match *(default — composite of specialty match + rating + response time)*
- Price: Low to High
- Price: High to Low
- Soonest Available

**Filter options (multi-select):**
- Availability: time range picker (e.g. "After 2 PM", "This week")
- Price: max budget slider
- Specialty Match: toggle "Specialists only"
- Booking type: "In-app only" (hides external-redirect salons)

### Data States

| State | Display |
|---|---|
| Structured quote received | Full row, all fields populated |
| Freeform parsed (high confidence) | Row populated, subtle "auto-parsed" indicator |
| Freeform parsed (low confidence) | Row with ⚠ on uncertain fields |
| No response yet | Row grayed out: "Awaiting response" |
| External booking only | Slot chips show ↗ icon indicating redirect |

---

## Sample Data (Emeri's Haircut Project)

| Salon | Price | Availability | Specialty Match | Match Reason | → Chat |
|---|---|---|---|---|---|
| Ocean Mirage Studio ★4.8 | $95 + $20 long hair | Tmrw 2PM · Tmrw 4:30PM · Fri 11AM | 🌟 Asian Hair Specialist | 34 reviews mention Asian hair · 28 portfolio photos | → Chat |
| Union Cuts ★5.0 | $95 flat | Tmrw 2PM | ✓ Verified License | 4.8★ for Long Cuts · Licensed cosmetologist | → Chat |
| Milvali SF ★4.5 | $120+ (Honey/Mercy) · $135+ (Fiona) | Tmrw 3PM · Fri 10AM ↗ | Honey · Mercy · Fiona | Auto-parsed: specialists named in response | → Chat |
| Blake Charles Salon ★4.3 | $180 flat | 11:45AM–2:30PM | Generalist | No specialty signals detected | → Chat |
| CODE Salon ★4.4 | Not provided ↗ | Tmrw (visit site) ↗ | — | Redirects to codesalonsf.com | → Chat |

*↗ = external redirect on click*

---

## Success Metrics

| Metric | Hypothesis |
|---|---|
| **Booking Conversion Rate (BCR)** | Primary north star. Matrix view users book at 2x rate vs. chat-only users |
| **Time-to-Book** | Users who use sort/filter book faster than those who don't |
| **Off-platform exit rate** | Baseline: measure % of slot clicks that redirect externally; use to prioritize merchant onboarding |
| **Matrix engagement rate** | % of users with 3+ quotes who open the Project Details tab |
| **Merchant structured quote rate** | % of merchants who send Bid Card vs. freeform; NLP nudge should increase this over time |

---

## What This PRD Demonstrates (PM Portfolio Signal)

1. **Data pipeline thinking** — connects PRD 1's schema fix to a downstream consumer surface
2. **Graceful degradation** — NLP auto-parse means the feature works even before full merchant adoption
3. **Dual booking paths** — respects merchant reality (some use external tools) while tracking exits as product signal
4. **Metrics ladder** — BCR as north star, with supporting leading indicators (engagement rate, structured quote rate)
5. **Sequenced roadmap** — PRD 1 is a prerequisite; this is the natural Phase 2
