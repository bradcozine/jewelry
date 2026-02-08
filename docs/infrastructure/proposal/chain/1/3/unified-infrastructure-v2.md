# Noble Metal Studios: Unified Infrastructure Blueprint v2

## Commerce, Content, and AI-First Operations for a Solo Founder

> This document synthesizes two prior infrastructure proposals (Codex A and B), takes the strongest elements of each, and rebuilds the AI layer from the ground up — with concrete architectures, data flows, cost estimates, and an honest assessment of what makes sense at solo-founder scale.

---

## 1. Business Context

- **Brand:** Noble Metal Studios
- **Founder model:** Solo operator (1–2 people) — AI replaces headcount, not augments a team
- **Sales channels:** Shopify (commerce engine) + custom headless website (brand, editorial, portfolio, custom design intake) + optional POS for trunk shows
- **Product model:** Configurable jewelry (metals × gems × components), custom design commissions, featured artist collections
- **Mission allocation:** 5–10% of profits funds programs for emerging jewelry artists
- **Core bet:** A single founder, armed with the right AI tooling, can run a premium jewelry brand that looks and operates like a 15-person company

---

## 2. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           CUSTOMERS                                     │
│    Headless Website  ·  Shopify Storefront  ·  POS  ·  Social/Ads      │
└──────────┬──────────────────┬──────────────────┬────────────────────────┘
           │                  │                  │
     Storefront API      Checkout API       POS SDK
           │                  │                  │
┌──────────▼──────────────────▼──────────────────▼────────────────────────┐
│                      SHOPIFY COMMERCE CORE                              │
│   Products · Variants · Inventory · Pricing · Tax · Shipping · CRM     │
│   Draft Orders · Gift Cards · Discounts · Shopify Flow                  │
└──────────┬──────────────────────────────────────┬───────────────────────┘
           │                                      │
   ┌───────▼─────────┐                   ┌────────▼────────┐
   │  HEADLESS WEB   │                   │   CMS (Sanity)  │
   │  Next.js on     │◄─────────────────►│                 │
   │  Vercel         │                   │  Artist profiles │
   └───────┬─────────┘                   │  Editorial       │
           │                             │  Impact reports  │
           │                             └────────┬────────┘
           │                                      │
┌──────────▼──────────────────────────────────────▼───────────────────────┐
│                                                                         │
│                    ┌──────────────────────────────┐                      │
│                    │    🤖 AI SERVICES LAYER      │                      │
│                    │                              │                      │
│                    │  Pricing Engine (4a)         │                      │
│                    │  Demand Forecasting (4b)     │                      │
│                    │  QC Documentation (4c)       │                      │
│                    │  Content Pipeline (4d)       │                      │
│                    │  Design Concierge (4e)       │                      │
│                    │  Semantic Search (4e)        │                      │
│                    └──────────┬───────────────────┘                      │
│                               │                                         │
│  ┌────────────┐  ┌────────────▼──┐  ┌──────────────┐  ┌─────────────┐  │
│  │ PIM / BOM  │  │  CRM &       │  │  Production  │  │  Shipping   │  │
│  │ (Katana)   │  │  Pipeline    │  │  Tracking &  │  │ (ShipStn)   │  │
│  │            │  │  (HubSpot /  │  │  QC          │  │             │  │
│  │            │  │   Airtable)  │  │              │  │             │  │
│  └────────────┘  └──────────────┘  └──────────────┘  └─────────────┘  │
│                                                                         │
│  ┌────────────┐  ┌──────────────┐  ┌──────────────┐                    │
│  │ Email/SMS  │  │  Analytics   │  │  Accounting  │                    │
│  │ (Klaviyo)  │  │  (GA4)       │  │  (QuickBooks)│                    │
│  └────────────┘  └──────────────┘  └──────────────┘                    │
│                                                                         │
│                       OPERATIONS & BACK-OFFICE                          │
└─────────────────────────────────────────────────────────────────────────┘
```

**Key principle:** The AI Services Layer is not a separate system — it is a collection of small, focused services that sit between data sources (Shopify, CMS, metals APIs) and outputs (price updates, content drafts, alerts). Each service is a single script or serverless function, not a monolith.

---

## 3. Infrastructure Components

### 3A — Commerce Core (Shopify)

| Capability | Configuration |
|---|---|
| **Plan** | Shopify Advanced (multi-location inventory, Shopify Flow, international markets) — upgrade to Plus only if B2B/wholesale volume justifies it |
| **Payments** | Shopify Payments; add alternative gateway only if selling in regions Shopify Payments doesn't cover |
| **POS** | Shopify POS Lite for trunk shows / studio sales |
| **Product Types** | Base designs, limited-edition collections, custom design placeholders, artist collection pieces |
| **Variants** | Metal type (gold 14k/18k, sterling, platinum, etc.) × gemstone × size × finishing |
| **Metafields** | `metal_weight_g`, `stone_grade`, `stone_carat`, `stone_source`, `artist_id`, `bom_cost`, `custom_config_json` |
| **Collections** | By artist, by metal, by gemstone, by price tier, "Custom Design Showcase", "New Arrivals", seasonal |
| **Tags** | Standardized: `metal:gold-14k`, `stone:sapphire`, `artist:jane-doe`, `collection:spring-2026` |
| **Inventory Locations** | Studio, Warehouse, Consignment-at-gallery |
| **Shipping Profiles** | By category: rings/earrings (small box), necklaces/bracelets (medium), custom/oversized (large); insured shipping for items >$500 |
| **Tax** | Shopify Tax; add Avalara only if selling in complex multi-state/international jurisdictions |
| **Shopify Flow** | Automations: low-stock alerts, order routing by location, review request after delivery, tag high-value customers |
| **Draft Orders** | For custom design invoicing and wholesale quotes |
| **Gift Cards** | Enabled — good revenue for jewelry |

### 3B — Headless Website

| Component | Choice | Rationale |
|---|---|---|
| **Framework** | Next.js (App Router) | Largest ecosystem, best Vercel integration, strong Shopify Storefront API support |
| **Hosting** | Vercel | Zero-config deploys, edge functions, image optimization, analytics |
| **DNS / CDN** | Cloudflare | Free tier is generous; DDoS protection, SSL, caching |
| **CMS** | Sanity | Real-time collaboration, structured content, generous free tier, excellent Next.js integration |
| **Auth** | Shopify Customer Account API | Wishlists, order history, custom design quote tracking — no separate auth system needed |
| **Forms** | Native Next.js Server Actions → CRM | No third-party form tool needed; fewer dependencies |

**CMS Content Models:**
- `artist` — name, bio, medium, portrait, portfolio images, social links, collection reference
- `editorial` — title, body (portable text), featured images, related products, publish date
- `designCaseStudy` — client brief, process photos, final images, materials used, artist credit
- `impactReport` — quarter, funds disbursed, artists supported, outcomes, narrative
- `page` — flexible page builder for about, FAQ, program info

### 3C — PIM, Inventory & Manufacturing

| Capability | Detail |
|---|---|
| **PIM / ERP** | Katana (manufacturing-focused: BOMs, production orders, material tracking) + QuickBooks Online for accounting |
| **BOM Structure** | Base design → components (setting, band, clasp) → materials (metal weight in grams, gemstone spec) → labor (hours × rate) |
| **Pricing Formula** | `(metal_weight_g × spot_price_per_g × markup) + gemstone_cost + labor_hours × labor_rate + overhead_pct + target_margin_pct` |
| **Spot Price Feed** | Metals-API.com or GoldAPI.io — hourly/daily rates for gold, silver, platinum, palladium |
| **Supplier Management** | Tracked in Katana: preferred suppliers per material, lead times, minimum order quantities, QC notes |
| **Configurator** | Bold Product Options on Shopify — stores selections as line item properties; configurator rules enforce gem-metal compatibility and rare-stone inventory locks |
| **Inventory States** | `available` → `reserved` (in cart/configurator) → `committed` (order placed) → `in_production` → `qc` → `ready_to_ship` → `shipped` |

### 3D — CRM & Custom Design Pipeline

| Stage | How It Works |
|---|---|
| **Intake** | Customer fills out form on website (occasion, style, metal/gem preferences, budget range, reference images, timeline) — submitted via Next.js Server Action |
| **AI Qualification** | AI Design Concierge (see §4e) pre-qualifies the lead: validates budget vs. feasibility, asks clarifying questions via email, creates a structured brief |
| **CRM Record** | Brief lands in HubSpot (or Airtable) as a deal with structured fields; auto-assigned to "New Design Requests" pipeline |
| **Quoting** | Founder reviews brief → creates Shopify Draft Order with itemized pricing (materials + labor + margin) → sends to customer |
| **Approval** | Customer approves quote → draft order converted to paid order → production begins |
| **Production Tracking** | Pipeline stages with SLA timers: **Design (5 days)** → **Customer Approval (3 days)** → **Sourcing (7 days)** → **Production (10 days)** → **QC (2 days)** → **Shipping (2 days)** |
| **QC** | AI-assisted documentation (see §4c): founder photographs piece → AI generates QC report → stored with order |
| **Communication** | Klaviyo automated emails at each stage transition: "Your design is in production," "QC complete — here's a preview photo," etc. |

### 3E — Artist Program Operations

| Capability | Detail |
|---|---|
| **Onboarding** | Application form on website → founder reviews portfolio → partnership agreement (template in CMS) → artist profile created in Sanity → products listed in Shopify under artist collection |
| **Partnership Agreements** | IP rights (artist retains ownership, NMS gets exclusive retail license for term), revenue split (e.g., 60/40 or 70/30 favoring artist), exclusivity window, quality standards, termination terms |
| **Revenue Tracking** | Shopify order data filtered by `artist_id` metafield → monthly sales report per artist → payout via Stripe Connect or manual transfer |
| **Commission Tiers** | Base: 30% to artist · Established: 35% · Featured: 40% — tiers based on sales volume and tenure |
| **Program Fund** | 5–10% of net profits deposited into a separate ledger; quarterly allocation to grants, scholarships, or equipment for emerging artists |
| **KPIs** | Artists onboarded (cumulative), active artist collections, revenue per artist, program funds allocated vs. deployed, impact stories published |
| **Collection Standards** | Photography spec sheet (lighting, angles, background), product description template, artist story template — all documented in Sanity |

### 3F — Logistics

| Capability | Choice |
|---|---|
| **Shipping** | ShipStation — rate shopping, label printing, tracking sync to Shopify |
| **Insurance** | Shipsurance or built-in carrier insurance for items >$500 |
| **Returns** | AfterShip Returns — self-service portal, reason tracking, restocking workflow |
| **Repairs** | Custom intake form (similar to design intake) → repair pipeline in CRM |

### 3G — Marketing, Email, Loyalty

| Capability | Choice |
|---|---|
| **Email / SMS** | Klaviyo — flows for welcome, abandoned cart, post-purchase, review request, win-back, custom design stage updates |
| **Reviews** | Yotpo (free tier) — photo reviews, trust badges |
| **Loyalty** | Smile.io (free tier to start) — points on purchases, referral program |
| **Analytics** | GA4 + Shopify Analytics + Meta Pixel — no data warehouse needed until Phase 3+ |

---

## 4. AI Operations Architecture — Deep Dive

This is the core differentiator of this plan. Each subsystem below includes: what it does, exactly how it works, the data flow, technology choices, estimated monthly cost, and an honest assessment of when it becomes valuable.

### 4a. Dynamic Pricing Engine

**What it does:** Automatically recalculates and updates product prices based on live precious metal spot prices, ensuring margins stay healthy without manual repricing.

**Data Flow:**

```
┌─────────────────┐     ┌──────────────────────┐     ┌──────────────────┐
│  Metals-API.com │────►│  Pricing Service     │────►│  Shopify Admin   │
│  (spot prices   │     │  (Node.js on Render) │     │  API (bulk       │
│   gold, silver, │     │                      │     │   variant price  │
│   platinum)     │     │  1. Fetch spot prices│     │   update)        │
│                 │     │  2. Load BOM per SKU │     │                  │
│  Runs: daily    │     │  3. Apply formula    │     │  Updates: daily  │
│  at 6am EST     │     │  4. Compare to       │     │  or on-demand    │
│                 │     │     current prices   │     │                  │
└─────────────────┘     │  5. Update if delta  │     └──────────────────┘
                        │     > threshold (2%) │
                        │  6. Log changes      │
                        │  7. Alert if margin  │──────► Slack / Email
                        │     < floor (40%)    │        alert
                        └──────────────────────┘
```

**Pricing Formula (per variant):**

```
material_cost = metal_weight_g × (spot_price_per_troy_oz / 31.1035) × metal_purity_factor
gemstone_cost = looked up from gemstone tier table (grade × carat × cut)
labor_cost    = estimated_hours × hourly_labor_rate
overhead      = (material_cost + gemstone_cost + labor_cost) × overhead_pct
subtotal      = material_cost + gemstone_cost + labor_cost + overhead
retail_price  = subtotal / (1 - target_margin_pct)
```

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Spot price API | Metals-API.com (free tier: 50 req/mo; paid: $14/mo for 1,000 req/mo) | $0–14/mo |
| Pricing service | Node.js script on Render (cron job) | $0–7/mo (free tier covers a cron service) |
| BOM data source | Katana API or a simple JSON/spreadsheet synced to the service | $0 |
| Shopify updates | Shopify Admin API (GraphQL `productVariantsBulkUpdate`) | $0 (included in Shopify plan) |
| Alerts | Slack webhook or email via Resend | $0 |

**Estimated total: $0–21/month**

**When it matters:** Immediately. Gold price moves 1–3% on a normal day. On a $2,000 ring with $800 in gold, a 3% swing is $24 in margin erosion per piece. Automating this from day one protects margins.

**Configuration:**
- `price_update_threshold`: only push updates when calculated price differs by >2% from current (avoids churning Shopify)
- `margin_floor`: alert when any SKU's margin drops below 40%
- `manual_override_flag`: metafield on a variant to exclude it from auto-pricing (e.g., sale items)

---

### 4b. Demand Forecasting & Inventory Optimization

**What it does:** Predicts when to reorder materials and which products will sell, so you don't tie up cash in slow-moving inventory or run out of popular materials.

**Honest assessment:** With <6 months of sales data, statistical forecasting models (Prophet, ARIMA) have nothing meaningful to train on. This section defines a **two-phase approach**: rules-based reorder points first, then ML forecasting once you have a year of data.

**Phase 1 — Rules-Based (Months 1–9)**

```
safety_stock     = avg_weekly_units_sold × supplier_lead_time_weeks × 1.5
reorder_point    = safety_stock + (avg_weekly_units_sold × review_cycle_weeks)
reorder_quantity = economic_order_quantity or supplier_minimum, whichever is greater
```

- Tracked in a spreadsheet or Katana's built-in reorder alerts
- Weekly review: founder scans a dashboard showing stock levels vs. reorder points
- Alerts via Shopify Flow when inventory drops below reorder point

**Phase 2 — Statistical Forecasting (Month 10+)**

```
┌──────────────┐     ┌────────────────────┐     ┌────────────────────┐
│ Shopify       │────►│  Weekly Python     │────►│  Reorder Report    │
│ Orders API    │     │  Script            │     │  (email digest)    │
│ (historical   │     │                    │     │                    │
│  order data)  │     │  1. Pull 12mo      │     │  - SKUs to reorder │
│               │     │     order history  │     │  - Predicted demand│
│               │     │  2. Prophet or     │     │    next 30 days    │
│               │     │     exp. smoothing │     │  - Suggested qty   │
│               │     │  3. Factor in      │     │  - Supplier lead   │
│               │     │     seasonality    │     │    time warnings   │
│               │     │     (Valentine's,  │     │                    │
│               │     │      holidays)     │     └────────────────────┘
│               │     │  4. Generate       │
│               │     │     reorder report │
└──────────────┘     └────────────────────┘
```

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Data source | Shopify Admin API (orders endpoint) | $0 |
| Forecasting | Python script (Prophet library) on Render cron or a Jupyter notebook you run manually | $0–7/mo |
| Output | Email digest via Resend or a Notion/Airtable dashboard | $0 |

**Estimated total: $0–7/month**

**When it matters:** Phase 1 (rules-based) from day one. Phase 2 forecasting only becomes useful after 9–12 months of sales data with enough volume to detect patterns.

---

### 4c. AI-Assisted Quality Control Documentation

**What it does:** Turns your QC step from "I looked at it and it's fine" into a structured, documented, AI-verified inspection — creating a quality record for every piece and catching issues you might miss when tired or rushed.

**Honest assessment:** Full computer-vision QC (training a custom defect-detection model) requires thousands of labeled images and is overkill at low volume. Instead, this uses a **vision-capable LLM** (GPT-4o or Claude) as a second pair of eyes, comparing production photos against a checklist.

**How It Works:**

```
┌──────────────────┐     ┌────────────────────────────┐     ┌───────────────┐
│  Founder takes   │────►│  QC Service                │────►│  QC Report    │
│  standardized    │     │  (Node.js on Render)       │     │  (stored with │
│  photos (3 angles│     │                            │     │   order in    │
│  + macro detail) │     │  1. Receives photos +      │     │   Airtable /  │
│                  │     │     order metadata         │     │   Katana)     │
│  Upload via      │     │  2. Sends to vision LLM   │     │               │
│  simple web form │     │     with checklist prompt  │     │  Fields:      │
│  or mobile app   │     │  3. LLM evaluates:        │     │  - Overall:   │
│                  │     │     □ Stone alignment      │     │    PASS/FLAG  │
│                  │     │     □ Prong integrity      │     │  - Per-item   │
│                  │     │     □ Finish consistency   │     │    scores     │
│                  │     │     □ Hallmark clarity     │     │  - Notes      │
│                  │     │     □ Symmetry             │     │  - Photos     │
│                  │     │     □ Matches design spec  │     │               │
│                  │     │  4. Returns structured     │     │  If FLAG:     │
│                  │     │     JSON report            │     │  → Slack alert│
│                  │     │  5. Auto-stores with order │     │    to founder │
│                  │     └────────────────────────────┘     └───────────────┘
```

**Photo Station Spec:**
- Consistent lighting: daylight-balanced LED ring light
- 3 required angles: top-down, 45°, side profile
- 1 macro shot: detail of stone setting / clasp / hallmark
- Neutral gray background
- Camera: phone is fine if consistent (same phone, same distance, same lighting)

**LLM Prompt Structure (simplified):**
```
You are a jewelry quality control inspector. Evaluate these photos of a
{product_type} against the following checklist. For each criterion, rate
PASS, FLAG (minor concern), or FAIL (defect requiring rework).

Product: {name}
Design spec: {description, expected materials, reference_photo_url}
Checklist:
1. Stone alignment — centered, level, secure in setting
2. Prong integrity — all prongs uniform, no gaps, properly burnished
3. Surface finish — consistent polish/matte per spec, no scratches
4. Hallmark — legible, correctly placed
5. Symmetry — band width uniform, balanced proportions
6. Overall match to design spec

Return JSON: { criteria: [...], overall: "PASS"|"FLAG"|"FAIL", notes: "..." }
```

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Vision LLM | OpenAI GPT-4o (vision) or Anthropic Claude (vision) | ~$0.01–0.05 per QC check |
| QC service | Node.js on Render (triggered by form submission) | $0–7/mo |
| Photo upload | Simple web form (part of internal tools on the headless site) | $0 |
| Storage | Cloudflare R2 or S3 for photos | $0–5/mo |
| Report storage | Airtable record linked to order, or Katana production order | $0 |

**Estimated total: $5–15/month** (at ~100 pieces/month)

**When it matters:** From first production piece. Even at low volume, having documented QC records builds credibility, catches mistakes, and creates training data for a future custom vision model.

---

### 4d. Content Generation Pipeline

**What it does:** Generates product descriptions, social media content, email copy, and artist feature articles from structured data — letting the founder focus on editing and approving rather than writing from scratch.

**This is the highest-ROI AI investment for a solo founder.** Content is the biggest time sink that doesn't require hands-on craftsmanship. Every hour saved on writing is an hour available for design and production.

**Sub-pipelines:**

#### 4d-1. Product Descriptions

```
┌──────────────────┐     ┌────────────────────────┐     ┌──────────────────┐
│ Shopify metafields│────►│  Content Service       │────►│  Sanity CMS      │
│ (metal, gem,     │     │  (Node.js on Render)   │     │  (draft status)  │
│  weight, artist, │     │                        │     │                  │
│  dimensions)     │     │  Prompt template:      │     │  Founder reviews │
│                  │     │  brand voice guide +   │     │  → approves →    │
│                  │     │  metafield data →      │     │  publishes       │
│                  │     │  LLM generates:        │     │                  │
│                  │     │  • Product description  │     │  Also pushes to  │
│                  │     │    (150-250 words)     │     │  Shopify product │
│                  │     │  • SEO meta title      │     │  description via │
│                  │     │    (≤60 chars)         │     │  Admin API       │
│                  │     │  • SEO meta description│     │                  │
│                  │     │    (≤155 chars)        │     │                  │
│                  │     │  • 5 search keywords   │     │                  │
│                  │     └────────────────────────┘     └──────────────────┘
```

**Brand voice guide** (stored as a system prompt):
> Noble Metal Studios writes in a warm, confident, artisan voice. Descriptions evoke the sensory — the weight of metal, the play of light in a stone. Avoid corporate jargon and hard sells. Speak to someone who appreciates craftsmanship and wants to know the story behind the piece. Keep sentences short. Lead with the emotional, close with the technical (dimensions, materials, care).

**Batch mode:** When launching a new collection (10–30 pieces), the founder triggers a batch run: all metafields → all descriptions generated → all land in CMS as drafts → founder reviews and tweaks in one sitting.

#### 4d-2. Social Media Content

```
┌──────────────────┐     ┌────────────────────────┐     ┌──────────────────┐
│ Weekly triggers:  │────►│  Content Service       │────►│  Content Calendar│
│ • New products   │     │                        │     │  (Notion or      │
│ • Artist stories │     │  LLM generates:        │     │   Google Sheet)  │
│ • Behind-scenes  │     │  • 5 Instagram captions│     │                  │
│   prompts        │     │  • 3 Pinterest descs   │     │  Founder picks   │
│ • Seasonal themes│     │  • 2 email subject     │     │  favorites →     │
│ • Trending       │     │    line options        │     │  schedules in    │
│   hashtags       │     │  • Suggested posting   │     │  Later / native  │
│                  │     │    schedule            │     │  platform tools  │
└──────────────────┘     └────────────────────────┘     └──────────────────┘
```

#### 4d-3. Email Copy (Klaviyo Flows)

AI generates the initial draft copy for every Klaviyo flow:
- Welcome series (3 emails)
- Abandoned cart (2 emails)
- Post-purchase thank you + care instructions
- Review request
- Custom design stage updates (6 templates: received → designing → approved → sourcing → production → shipped)
- Win-back (60-day, 90-day)
- Artist spotlight monthly newsletter

Founder edits once, then flows run automatically. AI regenerates quarterly for freshness.

#### 4d-4. Artist Feature Articles

```
Sanity artist profile data → LLM generates:
  • 600-word feature article (bio, process, inspiration, collection preview)
  • Pull quotes for social
  • Email newsletter spotlight block
→ All land as CMS drafts for founder review
```

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| LLM API | OpenAI GPT-4o (best price/quality for content) | ~$20–50/mo depending on volume |
| Content service | Node.js on Render (same service as QC — different endpoints) | $0–7/mo (shared) |
| Prompt templates | Stored as markdown files in repo — version controlled, iterable | $0 |
| Brand voice guide | System prompt, refined over first month based on founder edits | $0 |
| Output destinations | Sanity CMS (drafts) + Shopify Admin API (product descriptions) + Notion (social calendar) | $0 |

**Estimated total: $20–57/month**

**When it matters:** Immediately. This is the single biggest time-saver. Writing product descriptions, social captions, and email flows for a jewelry brand is 10–20 hours/week without AI. With this pipeline, it's 2–3 hours/week of reviewing and approving.

---

### 4e. Customer-Facing AI (Phase 2+)

These are valuable but secondary to operations and content. Deploy after the business has revenue and the founder has bandwidth.

#### 4e-1. AI Design Concierge

**What it does:** A conversational assistant on the custom design page that guides customers through describing what they want, collects structured data, and creates a qualified lead in the CRM — 24/7, without the founder being online.

**How it works:**
- Chat widget on the "Custom Design" page (Vercel AI SDK + OpenAI)
- **Grounded** in the product catalog (metals available, gemstone options, price ranges, typical timelines) via a system prompt with catalog data
- Conversation flow:
  1. "What's the occasion?"
  2. "Do you have a style in mind?" (shows examples from the portfolio)
  3. "Metal preference?" (explains options, price implications)
  4. "Gemstone?" (explains grading, sources, price tiers)
  5. "What's your budget range?"
  6. "Any reference images?" (file upload)
  7. "Timeline — when do you need it by?"
- Output: structured JSON brief → auto-created as an Airtable/HubSpot record
- **Escalation rules:** If the customer asks for something outside the catalog's capabilities, or expresses frustration, or the budget is >$5,000 — hand off to the founder with a summary

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Chat UI | Vercel AI SDK (streaming) on the Next.js site | $0 |
| LLM | OpenAI GPT-4o (function calling for structured output) | ~$10–30/mo |
| Catalog grounding | Product catalog JSON refreshed daily from Shopify | $0 |
| CRM integration | Webhook to HubSpot/Airtable on conversation complete | $0 |

**Estimated total: $10–30/month**

**When to deploy:** Phase 2 (Month 4+), once custom design intake is proven manually and you understand what questions customers actually ask.

#### 4e-2. Semantic Search

**What it does:** Lets customers search the storefront with natural language: "rose gold ring with sapphire under $2,000" returns relevant results even if no product title contains those exact words.

**How it works:**
- Product catalog (title + description + metafields + tags) → generate embeddings via OpenAI `text-embedding-3-small`
- Store in Supabase pgvector (free tier covers a small catalog)
- Search query → embed → nearest-neighbor retrieval → return ranked product IDs → render on search results page
- Re-embed on product create/update (Shopify webhook)

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Embeddings | OpenAI `text-embedding-3-small` ($0.00002/1K tokens) | <$1/mo for a small catalog |
| Vector store | Supabase pgvector (free tier: 500MB) | $0 |
| Search endpoint | Next.js API route | $0 |

**Estimated total: ~$1/month**

**When to deploy:** Phase 2–3. Low cost, high UX value, but only matters once you have 50+ products.

#### 4e-3. Personalized Recommendations

**Phase 1:** Use Shopify's native "You may also like" (free, zero effort).

**Phase 2+ (Month 10+):** Once you have meaningful order history, build a lightweight collaborative filtering model:
- Input: anonymized order co-occurrence data (customers who bought X also bought Y)
- Output: "Recommended for you" carousel on PDP and homepage
- Implementation: Python script generating a recommendation matrix, stored as a JSON file, consumed by the storefront at build time

**Defer until you have the data to make it work.**

#### 4e-4. Visual Product Configurator (AI Rendering)

**Phase 3+ aspiration.** Generating photorealistic renders of custom configurations requires either:
- A fine-tuned image model trained on hundreds of studio product photos, or
- A 3D rendering pipeline with AI-enhanced materials/lighting

Both require significant investment. **Park this until the business has revenue, a large photo library, and proven demand for the configurator.**

In the meantime: use composite photography (layered product shots in different metals/stones) or simple mockups for custom quotes.

---

### 4f. AI Infrastructure Summary

**All AI services are simple, independent scripts — not a platform.**

```
┌──────────────────────────────────────────────────────┐
│              AI SERVICES (all on Render.com)          │
│                                                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │
│  │ pricing.js  │  │ content.js  │  │ qc.js       │  │
│  │ (cron daily)│  │ (on-demand  │  │ (on form    │  │
│  │             │  │  + batch)   │  │  submit)    │  │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  │
│         │                │                │          │
│         ▼                ▼                ▼          │
│  ┌─────────────────────────────────────────────┐     │
│  │         Shared utilities                    │     │
│  │  • OpenAI SDK (GPT-4o)                      │     │
│  │  • Shopify Admin API client                 │     │
│  │  • Sanity client                            │     │
│  │  • Slack webhook                            │     │
│  │  • Simple JSON logging                      │     │
│  └─────────────────────────────────────────────┘     │
│                                                      │
│  ┌─────────────┐  ┌─────────────┐                    │
│  │ concierge.js│  │ search.js   │   (Phase 2)        │
│  │ (Vercel AI  │  │ (pgvector   │                    │
│  │  SDK on     │  │  on         │                    │
│  │  Next.js)   │  │  Supabase)  │                    │
│  └─────────────┘  └─────────────┘                    │
└──────────────────────────────────────────────────────┘
```

**Monthly AI Cost Budget:**

| Service | Phase 1 Cost | Phase 2 Cost | Phase 3 Cost |
|---|---|---|---|
| Dynamic Pricing (4a) | $0–21 | $14–21 | $14–21 |
| Demand Forecasting (4b) | $0 (rules-based) | $0–7 | $7 |
| QC Documentation (4c) | $5–15 | $10–20 | $15–30 |
| Content Pipeline (4d) | $20–57 | $30–60 | $40–80 |
| Design Concierge (4e-1) | — | $10–30 | $20–40 |
| Semantic Search (4e-2) | — | $1 | $1 |
| **Total** | **$25–93** | **$65–139** | **$97–179** |

**Target: under $200/month total AI spend.** This replaces what would otherwise be 20–30 hours/week of manual work — effectively a part-time employee at $0.20/hour.

**LLM Provider Strategy:**
- **Primary: OpenAI GPT-4o** — best price/performance for content generation, function calling, and vision. Strong structured output (JSON mode).
- **Fallback: Anthropic Claude** — for tasks where you want a different "voice" or if OpenAI has an outage. Also strong for long-form editorial content.
- **Embeddings: OpenAI `text-embedding-3-small`** — cheapest option for semantic search; no reason to use anything else at this scale.
- **No open-source models needed** at this volume. The API costs are trivially small. Self-hosting adds complexity with zero cost savings until you're making thousands of API calls per day.

**Monitoring (Keep it simple):**
- Each service logs to a JSON file: timestamp, service name, input summary, output summary, tokens used, cost, latency
- Weekly: founder glances at a simple cost dashboard (spreadsheet pulling from logs)
- No LangSmith, no Helicone — unnecessary overhead at this scale. Add observability tooling only if AI call volume exceeds 1,000/month.

---

## 5. Solo Founder Operating Model

This is how one person actually runs this business, day to day, with AI handling the operational surface area.

### Daily (~1 hour of admin)

| Task | Who Does It | How |
|---|---|---|
| Check pricing alerts | Founder reviews | Slack notification if any SKU margin dropped below floor; pricing engine already updated prices overnight |
| Review QC reports | Founder reviews | For pieces completed yesterday — AI-generated QC report with photos; founder confirms or flags for rework |
| Approve content drafts | Founder reviews | Quick scan of any AI-generated product descriptions or social posts in CMS draft queue |
| Respond to high-priority customer messages | Founder handles | AI concierge (Phase 2) pre-qualifies custom design leads; founder responds to qualified leads only |
| Check order status | Founder glances | Shopify mobile app — new orders, fulfillment queue |

### Weekly (~2 hours of admin)

| Task | Who Does It | How |
|---|---|---|
| Review & schedule social content | Founder curates | AI generates 5–7 posts; founder picks best 3–4, edits captions, schedules |
| Review demand/inventory digest | Founder reviews | Email digest: stock levels, reorder alerts, upcoming supplier lead time deadlines |
| Process artist applications | Founder decides | Review any new artist applications; AI provides portfolio match score (Phase 3) |
| Custom design consultations | Founder handles | High-touch, human-only — video calls with clients, sketch reviews, material selection |
| Financial review | Founder reviews | QuickBooks dashboard: revenue, costs, margins, program fund balance |

### Monthly (~3 hours of admin)

| Task | Who Does It | How |
|---|---|---|
| Update pricing rules | Founder adjusts | Review margin performance; adjust labor rates, overhead %, or target margins if needed |
| Content strategy | Founder directs | Set themes for next month's content; AI generates the calendar |
| Artist payouts | Founder approves | Review per-artist sales report → process payouts |
| Program fund allocation | Founder decides | Review program fund balance → allocate to grants/scholarships |
| AI performance review | Founder reviews | Glance at AI cost log; review any flagged QC items or content that needed heavy editing; adjust prompts if needed |

### Quarterly

| Task | Who Does It | How |
|---|---|---|
| Impact report | AI drafts + founder reviews | AI compiles metrics from Shopify + CRM + finance → generates narrative report → founder edits → publishes to CMS |
| Strategy review | Founder | Review business performance, adjust product mix, plan new collections, evaluate artist program |

### What the founder NEVER does (AI handles entirely)

- Repricing products based on metal spot prices
- Writing first drafts of product descriptions
- Generating SEO metadata
- Creating QC documentation
- Sending lifecycle emails (handled by Klaviyo flows with AI-written copy)
- Answering "where is my order?" questions (Shopify order status page + Klaviyo shipping updates)

### What the founder ALWAYS does (human judgment required)

- Final design decisions on custom pieces
- Artist selection and relationship management
- Quality approval on flagged pieces
- Brand voice calibration (editing AI drafts to sound right)
- Program fund allocation decisions
- Pricing strategy (AI executes, founder sets the rules)

---

## 6. Design Documents To Create

### Technical

1. **System Architecture Document** — diagram of all systems with API integration points and data flows
2. **Data Model & SKU Strategy** — metals × gems × sizes → Shopify variant mapping; combinatorial rules
3. **BOM & Pricing Model** — formula spec, input sources, update frequency, margin rules
4. **Inventory & Production Flow** — material states, production pipeline stages, SLA definitions
5. **Custom Design Workflow** — intake → qualify → quote → approve → produce → QC → ship
6. **Artist Collaboration Workflow** — application → review → agreement → onboard → list → payout
7. **Security & Compliance** — API key management, PII handling, GDPR/CCPA, cookie consent
8. **AI Services Specification** — per-service: prompt templates, input/output schemas, error handling, fallback behavior

### Experience

1. **Brand & UI Style Guide** — typography, colors, imagery, photography standards
2. **Customer Journey Maps** — three paths: ready-to-buy · custom design · artist collection
3. **Content Strategy & Calendar** — editorial themes, posting cadence, SEO targets
4. **Impact Program Public Page** — how the story is told to customers

### Business / Operational

1. **Impact Program Charter** — mission, funding %, reporting cadence, grant criteria
2. **Artist Partnership Agreement Template** — IP, revenue split, exclusivity, quality, termination
3. **SOPs** — QC checklist, production milestones, customer communication scripts
4. **Revenue Allocation Policy** — % to program, quarterly true-up procedure

---

## 7. Implementation Phases

### Phase 1: Commerce + Content AI (Months 1–3)

**Goal:** Live storefront, first sales, AI writing all content.

- [ ] Shopify Advanced: product taxonomy, metafields, variants, collections, shipping/tax
- [ ] Next.js headless site on Vercel: homepage, about, shop (Storefront API), custom design intake form
- [ ] Sanity CMS: artist profiles, editorial, content models
- [ ] Klaviyo: welcome flow, abandoned cart, post-purchase, order stage updates
- [ ] **AI — Dynamic Pricing Engine** deployed (daily spot price → price updates)
- [ ] **AI — Content Pipeline** deployed (product descriptions, SEO, social captions, email copy)
- [ ] **AI — QC Documentation** deployed (photo upload → AI inspection report)
- [ ] Rules-based inventory reorder points in spreadsheet/Katana
- [ ] Manual custom design pipeline in Airtable/HubSpot
- [ ] Artist program: first 1–2 artists onboarded manually

### Phase 2: Configurator + Pipeline Automation (Months 4–6)

**Goal:** Product configurator live, custom design pipeline semi-automated, AI concierge deployed.

- [ ] Bold Product Options: configurator with gem-metal rules, dynamic pricing tied to pricing engine
- [ ] Katana: full BOM tracking, production orders, supplier management
- [ ] ShipStation: shipping integration, label automation
- [ ] AfterShip Returns: self-service return portal
- [ ] **AI — Design Concierge** deployed on custom design page
- [ ] **AI — Semantic Search** deployed on storefront (pgvector on Supabase)
- [ ] Custom design pipeline: concierge → CRM → draft order → production tracking → Klaviyo stage emails
- [ ] Yotpo reviews + Smile.io loyalty (free tiers)

### Phase 3: Artist Marketplace + Scale (Months 7–10)

**Goal:** Artist program fully operational, AI handles most routine decisions.

- [ ] Artist onboarding workflow: application form → portfolio review → agreement → CMS profile → Shopify collection
- [ ] Commission tracking: per-artist sales reports, automated payout calculations
- [ ] Program fund ledger: quarterly allocation, impact reporting
- [ ] **AI — Demand Forecasting** deployed (Prophet model, once 9+ months of data)
- [ ] **AI — Impact Report Generator** (quarterly: metrics → narrative → CMS draft)
- [ ] GA4 + Shopify Analytics dashboards refined
- [ ] Content strategy matures: AI generates weekly content calendar, founder curates

### Phase 4: Optimization + Expansion (Months 11+)

- [ ] Price elasticity analysis (which products can sustain higher margins?)
- [ ] Customer segmentation and LTV modeling
- [ ] Shopify Markets for international expansion
- [ ] B2B/wholesale via Shopify Plus (if volume warrants upgrade)
- [ ] Visual product configurator exploration (AI rendering or 3D pipeline)
- [ ] Personalized recommendations (collaborative filtering on order data)

---

## 8. Tool & Service Stack Summary

| Function | Choice | Monthly Cost (Phase 1) |
|---|---|---|
| Commerce | Shopify Advanced | $399 |
| Frontend | Next.js on Vercel | $0–20 |
| CMS | Sanity (free tier → Growth) | $0–99 |
| DNS / CDN | Cloudflare (free tier) | $0 |
| PIM / ERP | Katana Starter | $99 |
| Accounting | QuickBooks Online | $30 |
| Configurator | Bold Product Options | $50 |
| CRM | HubSpot Free / Airtable Plus | $0–20 |
| Shipping | ShipStation Starter | $25 |
| Returns | AfterShip Returns (free tier) | $0 |
| Email / SMS | Klaviyo (free tier → paid) | $0–45 |
| Reviews | Yotpo (free tier) | $0 |
| Loyalty | Smile.io (free tier) | $0 |
| AI Services Hosting | Render (cron + web services) | $0–14 |
| LLM API | OpenAI GPT-4o | $20–50 |
| Metals API | Metals-API.com | $0–14 |
| Vector DB | Supabase (free tier) | $0 |
| Photo Storage | Cloudflare R2 | $0–5 |
| **Total** | | **~$623–870/mo** |

Domain and SSL excluded (one-time / annual costs).

---

## 9. Next Steps

1. Choose Shopify plan and set up store skeleton (product types, metafields, collections)
2. Scaffold Next.js project on Vercel with Storefront API connection
3. Set up Sanity workspace with content models
4. Build the pricing engine service (spot price API → BOM calc → Shopify price update)
5. Build the content generation service (metafields → LLM → CMS draft)
6. Create the QC photo upload form and AI inspection service
7. Write the brand voice guide and first batch of prompt templates
8. Design custom design intake form and wire to CRM
9. Draft artist partnership agreement and program charter
10. Configure Klaviyo flows with AI-generated email copy
11. Build the first product configurator rules (gem-metal compatibility)
12. Launch MVP and start selling

---

*A solo founder with the right AI tooling doesn't need a team — they need a system. This document is that system.*
