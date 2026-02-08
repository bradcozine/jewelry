
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