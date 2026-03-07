# Yelp — Quote Flow Redesign
**PM Portfolio Case Study · Emeri Z. · March 2026**

[Demo 1 Structured Quotes](https://claude.ai/public/artifacts/45ba05ec-6225-4fb9-98ba-1b8816cb719c)

[Demo 2 Quote Comparison](https://claude.ai/public/artifacts/eb543e74-f7bf-4e4e-9d59-504a63a5f173)

**For full walkthrough (WIP):** [Yelp Quote Request Enhancements](https://docs.google.com/presentation/d/1O6vqkzqjAslaHLz_IwwlLA8ckOK2TnhjbLTSVw5Mlfw/edit?usp=sharing)


Morning of Fri March 6, I wanted to book a haircut in San Francisco. I used Yelp's "Request a Quote" feature, messaged 5 salons, and the experience wasn't delightful — one sent me to their website, one gave me a wall of text with prices buried inside, and none of them gave me a way to actually book without leaving the app.

Honestly I couldn't stop thinking about how bad the experience was. That evening I spent a few hours musing over how I'd improve the experience; this project captures said musings.

The root cause isn't a UX problem. Price and availability are trapped in freeform text strings. With yelp we can't compare, sort, or act on them.

This project redesigns the flow in two phases:

**Phase 1 — Structured Quote Object:** Merchants fill out a Bid Card (price, add-ons, time slots) instead of typing a free reply. Consumers see a clean Quote Card in chat with tappable slots and a direct "Request to Book" button.

**Phase 2 — Comparison Matrix:** All quotes aggregated into one sortable, filterable table under "Project Details" — so you're not tabbing between 5 chats trying to remember who was cheapest.

## Files
| File | What it is | Add'l Links|
|---|---| ---|
| `yelp-quote-object-prd.md` | PRD 1: Structured Quote Object | [Demo 1](https://claude.ai/public/artifacts/45ba05ec-6225-4fb9-98ba-1b8816cb719c)|
| `yelp-quote-mockup.html` | Interactive mockup — merchant form + consumer chat |  [Demo 1](https://claude.ai/public/artifacts/45ba05ec-6225-4fb9-98ba-1b8816cb719c)|
| `yelp-comparison-matrix-prd.md` | PRD 2: Comparison Matrix |[Demo 2](https://claude.ai/public/artifacts/eb543e74-f7bf-4e4e-9d59-504a63a5f173)|
| `yelp-comparison-matrix.html` | Interactive mockup — sortable/filterable quote table | [Demo 2](https://claude.ai/public/artifacts/eb543e74-f7bf-4e4e-9d59-504a63a5f173)|

## North Star Metric
Booking Conversion Rate (BCR). The hypothesis: structured quotes + a comparison view drive 2x more bookings than the current freeform chat.
