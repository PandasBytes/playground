# PRD: Yelp Structured Quote Object
**Author:** Emeri | **Status:** Mockup / Portfolio Artifact | **Date:** March 2026

---
## [Demo](https://claude.ai/public/artifacts/eb543e74-f7bf-4e4e-9d59-504a63a5f173)
## Problem Statement

Yelp's "Request a Quote" feature suffers from a **leaky bucket problem**: high-intent users enter the funnel but drop off because merchant responses are unstructured free-text. Price and availability are buried in paragraphs, impossible to compare across salons, and often redirect users off-platform entirely.

**Current state:** Consumer sends request → merchant responds with a wall of text → user must manually extract price + time + call/visit external site → booking happens off-Yelp or not at all.

**Root cause:** This is a **data schema problem**. Price and time are trapped in strings. Without structured fields, Yelp cannot compare, sort, or act on them programmatically.

---

## Goal

Replace freeform merchant chat responses with a **Structured Quote Object ("Bid Card")** — a merchant-filled form that renders as a scannable, actionable card in the consumer's chat thread.

**Out of scope (this iteration):** Comparison Matrix / Project Dashboard, payment/deposit flow, CV-based specialist matching backend.

---

## Users

| Role | Goal | Pain Today |
|---|---|---|
| Consumer (Emeri) | Find price + availability, book without leaving app | Gets vague texts, redirected to external sites |
| Merchant (Salon) | Close leads quickly, stand out on relevant attributes | No way to signal expertise; "trash leads" waste time |

---

## The Two Screens

### Screen 1 — Merchant: "Bid Card" Form
When a merchant receives a quote request, instead of a free-text reply box, they see a **structured response template**.

**Required fields:**
| Field | Type | Notes |
|---|---|---|
| Base Price | Currency input | e.g. $95 |
| Add-ons | Optional text + price | e.g. "+$20 long hair" |
| Earliest Available Slot | DateTime picker | Shows as formatted slot to consumer |
| Additional Slots | Optional (up to 2 more) | Consumer can pick preferred time |
| Note to customer | Short text (140 char max) | Optional personal touch |

**Specialist Badge (system-generated, merchant confirms):**
- Yelp pre-populates a badge based on NLP on reviews + CV on portfolio photos
- Merchant sees: *"We're showing you to this user because of your Asian Hair expertise — confirm to display this badge on your quote"*
- Merchant can confirm or dismiss

**CTA:** "Send Quote" → submits the structured object into the chat thread

---

### Screen 2 — Consumer: Quote Card in Chat
Instead of a text bubble, the merchant's reply renders as a **Bid Card component** inline in the chat.

**Card displays:**
- Salon name + avatar + star rating
- Specialist badge (if confirmed by merchant) with match reason
- Base price (large, prominent)
- Add-ons (secondary)
- Available time slots as **selectable chips** (tap to select)
- Optional merchant note
- CTA: **"Request to Book"** button (primary, Yelp red)

**Inbox list view:**
- Each conversation shows a summary pill: price + soonest slot (e.g. `$95 · 2PM Tomorrow`)
- Badge indicator if specialist match exists

---

## Interaction Flow (Mockup States)

```
[Merchant Side]
Quote Request Received
    → Merchant opens chat
    → Sees "Send a Quote" prompt (not free-text box)
    → Fills out Bid Card form
    → Confirms specialist badge (optional)
    → Hits "Send Quote"

[Consumer Side]
Chat inbox
    → See summary pills per salon (price + soonest slot)
    → Tap into Ocean Mirage Studio thread
    → See Bid Card rendered in chat (replaces wall of text)
    → Select a time slot chip
    → Hit "Request to Book"
    → Confirmation state
```

---

## Success Metrics

| Metric | Hypothesis |
|---|---|
| **Booking Conversion Rate (BCR)** | Primary. Target 2x vs. freeform chat baseline |
| **Time-to-Quote** | Merchants respond faster when form is structured (less writer's block) |
| **Off-platform exits** | Reduce redirects to external booking sites by 50%+ |
| **Lead quality score** | Merchants rate leads higher when consumer context is visible |

---

## Design Principles (Yelp-Branded)
- Stay within Yelp's existing visual system: red `#D32323`, SF Pro / system fonts, star ratings in red
- Quote Card should feel like a **native Yelp component**, not a third-party widget
- Merchant form should feel like filling out a Yelp business profile field — familiar, not foreign
- Mobile-first layout (most Yelp usage is mobile)

---

## What This Mockup Demonstrates (PM Portfolio Signal)

1. **Systems thinking** — reframes a UX problem as a data schema problem
2. **Two-sided marketplace awareness** — solves for both merchant and consumer simultaneously
3. **Ruthless scoping** — defers Comparison Matrix, payments, and backend ML to later iterations
4. **Metrics-first** — BCR as north star, not engagement or DAU
