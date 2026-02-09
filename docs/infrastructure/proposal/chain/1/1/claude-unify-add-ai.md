# Noble Metal Studios: Unified Infrastructure Blueprint — Commerce, Content, and AI-Driven Operations

> *Best-of-both-worlds synthesis of the two prior infrastructure plans, extended with practical AI integration points for running the business.*

---

## 1. Business Context & Strategic Goals

- **Brand:** Noble Metal Studios
- **Sales Channels:** Shopify (commerce engine) + custom headless website (brand storytelling, editorial, portfolio, artist features, custom design intake) + optional POS for trunk shows and studio sales
- **Product Model:** Configurable jewelry (metals, gems, modular components), custom design projects, and featured artist collections
- **Mission Allocation:** 5–10% of profits funds programs for emerging jewelry artists (impact program)
- **Core Differentiators:** Deep product configurability, artisan-first brand narrative, AI-augmented operations from day one

---

## 2. High-Level Architecture

Shopify is the **system of record** for products, inventory, pricing, tax, and checkout. The custom website is a **headless front-end and content hub**. An **AI services layer** sits alongside both, augmenting every major workflow.

### Architecture Diagram (Logical)

```
┌──────────────────────────────────────────────────────────────────────┐
│                        CUSTOMERS / CHANNELS                         │
│   Web (Headless)  ·  Shopify Storefront  ·  POS  ·  Social/Ads     │
└────────────┬─────────────────┬──────────────────┬────────────────────┘
             │                 │                  │
       Storefront API      Checkout API       POS SDK
             │                 │                  │
┌────────────▼─────────────────▼──────────────────▼────────────────────┐
│                     SHOPIFY COMMERCE CORE                            │
│  Products · Variants · Inventory · Pricing · Tax · Shipping · CRM   │
│  Draft Orders · Gift Cards · Discounts · Shopify Flow               │
└────────────┬────────────────────────────────────┬────────────────────┘
             │                                    │
     ┌───────▼────────┐                  ┌────────▼────────┐
     │  HEADLESS WEB  │                  │   CMS (Sanity   │
     │  (Next.js on   │◄────────────────►│   / Contentful) │
     │   Vercel)      │                  └────────┬────────┘
     └───────┬────────┘                           │
             │                           Artist profiles, editorial,
             │                           program impact pages
             │
┌────────────▼─────────────────────────────────────────────────────────┐
│                    OPERATIONS & BACK-OFFICE                          │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │  PIM / BOM   │  │   CRM &      │  │  Production  │               │
│  │  (Katana /   │  │   Pipeline   │  │  Tracking &  │               │
│  │   Cin7)      │  │  (HubSpot /  │  │  QC          │               │
│  │              │  │   Airtable)  │  │              │               │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘               │
│         │                 │                 │                        │
│  ┌──────▼─────────────────▼─────────────────▼───────┐               │
│  │            AI SERVICES LAYER                      │               │
│  │  Recommendations · Pricing · Forecasting · QC    │               │
│  │  Chatbot · Content Gen · Artist Matching          │               │
│  └──────────────────────────────────────────────────┘               │
│                                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │  Shipping    │  │  Email/SMS   │  │  Analytics    │               │
│  │  (ShipStation│  │  (Klaviyo)   │  │  (GA4 + BI)  │               │
│  │   / Shippo)  │  │              │  │              │               │
│  └──────────────┘  └──────────────┘  └──────────────┘               │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 3. Infrastructure Components

### 3A — Commerce Core (Shopify)

| Capability | Detail |
|---|---|
| **Plan** | Shopify Advanced or Plus (multi-location inventory, Shopify Flow, B2B/wholesale ready) |
| **Payments** | Shopify Payments (+ alternative gateways for specific regions) |
| **POS** | Shopify POS for trunk shows, studio pop-ups |
| **Products & Variants** | Metal type, gemstone, size, finishing — modeled via standard variants + line item properties for custom selections |
| **Metafields** | Metal weight, stone grade, carat, source, artist ID, customization flags |
| **Collections** | By artist, metal type, gemstone, custom design showcase |
| **Inventory Locations** | Studio, warehouse, consignment |
| **Shipping Profiles** | By category (rings vs. necklaces vs. oversized) |
| **Tax** | Shopify Tax + Avalara for complex jurisdictions |
| **Discounts & Gift Cards** | Native Shopify |
| **Shopify Markets** | Multi-currency / international selling |

### 3B — Headless Website Stack

| Component | Recommendation |
|---|---|
| **Frontend** | Next.js (App Router) *or* Hydrogen + Remix |
| **Hosting** | Vercel (for Next.js) *or* Shopify Oxygen (for Hydrogen) |
| **DNS / CDN** | Cloudflare or AWS Route 53 |
| **CMS** | Sanity (first choice), Contentful, or Prismic — headless, structured content |
| **Content Types** | Artist profiles, editorial lookbooks, custom design case studies, program impact reports |
| **Auth** | Shopify customer account tokens for wishlists, order history, custom quote tracking |
| **Forms** | Native Next.js forms or Tally/Typeform → CRM integration |

### 3C — PIM, Inventory & Manufacturing

| Capability | Detail |
|---|---|
| **PIM / ERP** | Katana (production-focused) or Cin7/Acumatica (scaling) + QuickBooks for accounting |
| **BOM / Manufacturing Layer** | Track raw materials, component assembly, labor per configuration (metal weight, gemstone size/grade, setting type) |
| **Pricing Engine** | Rules combining metal spot price (live feed), gemstone tier, labor hours, overhead, and target margin |
| **Supplier Integration** | Metals & gemstone procurement portals; lead-time tracking; QC status per lot |
| **Configurator** | Bold Product Options, Zepto, or custom configurator writing selections into Shopify line item properties |
| **Configurator Rules** | Gem-metal compatibility constraints, rare-stone inventory locks, min/max sizing |
| **Inventory States** | In stock → Reserved → In production → QC → Ready to ship |

### 3D — CRM & Custom Design Pipeline

| Stage | Tooling |
|---|---|
| **Intake** | Custom intake form on website (reference images, budget, timeline, material preferences) |
| **CRM** | HubSpot or Airtable — design request pipeline |
| **Quoting** | Shopify Draft Orders or dedicated quoting tool |
| **Approval** | Drafts → customer review → revision loop → sign-off checkpoint |
| **Production** | Stage pipeline: **Design → Approval → Sourcing → Production → QC → Shipping** with SLA timers per stage |
| **QC** | Photo documentation at key milestones; checklist per product type |
| **Communication** | Automated email/SMS templates at each stage via Klaviyo |

### 3E — Artist Program Operations

| Capability | Detail |
|---|---|
| **Onboarding** | Vetting criteria, contracts (IP rights, revenue splits, timelines), digital onboarding workflow in CRM |
| **Portfolio & Marketplace** | Dedicated CMS-driven artist collections with biography, portfolio, medium, story |
| **Royalty / Commission** | Per-artist sales tracking; configurable commission tiers and payout schedules |
| **Program Budgeting** | Separate ledger: 5–10% of profits allocated; quarterly reporting |
| **KPIs** | Artists onboarded, funds allocated, grant outcomes, collection sales performance |
| **Collection Standards** | Photography specs, product description templates, storytelling guidelines |

### 3F — Logistics & Operations

| Capability | Recommendation |
|---|---|
| **Shipping** | ShipStation or Shippo |
| **Returns / Repairs** | Loop or AfterShip Returns; repair intake workflow |
| **Compliance** | PCI via Shopify; GDPR/CCPA privacy; hallmarking/metal standards; FTC jewelry guides |

### 3G — Marketing, Email, Loyalty & Reviews

| Capability | Recommendation |
|---|---|
| **Email / SMS** | Klaviyo |
| **Loyalty / Referral** | Smile.io or LoyaltyLion |
| **Reviews** | Yotpo or Loox |
| **Analytics** | GA4 + Shopify Analytics + Meta Pixel |
| **Data Warehouse** | Optional — BigQuery or a lightweight BI tool for advanced reporting |

---

## 4. Design Documents To Create

### 4A — Technical Design Docs

1. **System Architecture Document** — Diagram of all systems, API integration points, and data flows (products, inventory, orders, customer data).
2. **Data Model & SKU Strategy** — How metals, gems, sizes, and components map to Shopify variants; combinatorial SKU generation rules.
3. **BOM & Pricing Model** — Inputs (spot prices, labor, overhead), margin targets, dynamic pricing formulas.
4. **Inventory & Production Flow** — Raw materials → component assembly → finished goods; SLA definitions per stage.
5. **Custom Design Workflow** — Intake → design → quote → approval → production → QC → delivery.
6. **Artist Collaboration Workflow** — Onboarding, product submission, review, revenue share mechanics.
7. **Security & Compliance Plan** — Access levels, PII handling, GDPR/CCPA, cookie consent, Shopify API scoping.
8. **AI Integration Architecture** — Which models power which workflows, data pipelines, feedback loops (see §6).

### 4B — Experience Design Docs

1. **Brand & UI Style Guide** — Typography, color palette, imagery direction, photography standards.
2. **Customer Journey Maps** — Three paths: ready-to-buy · custom design · artist collection discovery.
3. **Content Strategy** — Editorial calendar, artisan spotlights, behind-the-scenes, SEO plan.
4. **Impact Program Documentation** — How the earnings component is allocated, public-facing storytelling.

### 4C — Business / Operational Docs

1. **Impact Program Charter** — Mission, funding percentage, reporting cadence, grant criteria.
2. **Artist Partnership Agreements** — IP rights, revenue splits, exclusivity terms, timelines.
3. **Standard Operating Procedures** — QC checklists, production milestones, customer communication scripts.
4. **Revenue Allocation Policy** — % dedicated to artist programs with quarterly true-up.

---

## 5. Foundational Configurations

### Shopify

- Product types & tags: standardized naming (metal, gemstone, collection, artist)
- Variants: metal type, gemstone, size, finishing
- Metafields: metal weight, stone grade, carat weight, source, artist ID
- Collections: by artist, metal, gemstone, "Custom Design Showcase"
- Inventory locations: Studio, Warehouse, Consignment
- Shipping & tax profiles configured per category and region
- Shopify Flow automations: low-stock alerts, order routing, review requests

### Website

- Storefront API keys provisioned
- CMS content models defined (artist profile, editorial, portfolio piece, impact report)
- GA4 + Meta Pixel installed
- Customer auth via Shopify customer tokens
- Custom intake forms wired to CRM

### Configurator & Inventory Rules

- Gem-metal compatibility matrix enforced
- Rare-stone inventory locks (auto-reserve on configuration)
- Dynamic pricing recalculated on each configuration change
- Line item properties stored for every custom selection

---

## 6. AI Integration — Where It Makes Sense and How It Looks

AI is not bolted on as an afterthought; it is woven into the operational fabric so that a small team can run a sophisticated jewelry business at scale. Below are the highest-impact AI integration points, grouped by business function.

### 6A — Customer-Facing AI

#### 6A-1. AI Design Concierge (Chatbot + Guided Intake)

**What it does:** A conversational assistant on the website guides customers through custom design requests — asking about occasion, style preferences, metal/gemstone affinity, budget — and produces a structured brief that flows directly into the CRM pipeline.

**How it looks:**
- A chat widget on the "Custom Design" page powered by a fine-tuned LLM (e.g., GPT-4-class model via API).
- The assistant references the product catalog (metals, gems, past designs) so answers are grounded.
- Output: a structured JSON brief auto-created as a HubSpot/Airtable record with images, preferences, and budget.
- Escalation: hands off to a human jeweler when complexity exceeds threshold.

**Value:** Converts browsers into qualified design leads 24/7; reduces manual intake time by 60–80%.

#### 6A-2. Visual Product Configurator with AI Rendering

**What it does:** Customers select metal, gemstone, setting, and size; an AI rendering engine produces a photorealistic preview of **their** configuration in real time.

**How it looks:**
- Built into the headless storefront's configurator page.
- Uses a fine-tuned image generation model or 3D rendering pipeline (e.g., a diffusion model trained on studio product photography, or a deterministic 3D engine with AI-enhanced texture/lighting).
- Fallback: composite photography (layer-based) for configurations where AI rendering isn't trained yet.

**Value:** Dramatically increases conversion on high-value custom pieces; reduces "what will it look like?" friction.

#### 6A-3. Personalized Recommendations

**What it does:** A recommendation engine on the storefront suggests products based on browsing history, past purchases, wishlist items, and similar-customer behavior.

**How it looks:**
- Lightweight ML model (collaborative filtering + content-based) running on product metadata + anonymized behavior data.
- Surfaces in "You Might Also Love" carousels, email campaigns (Klaviyo dynamic sections), and post-purchase follow-ups.
- Data source: Shopify customer events + GA4 behavior.

**Value:** Increases average order value (AOV) and repeat purchase rate.

#### 6A-4. AI-Powered Search & Discovery

**What it does:** Semantic search on the storefront — customers type natural language queries ("rose gold ring with sapphire under $2,000") and get relevant results even if exact keywords don't match product titles.

**How it looks:**
- Vector embeddings of product catalog (title, description, metafields, tags) stored in a vector DB (Pinecone, Weaviate, or Supabase pgvector).
- Query → embedding → nearest-neighbor retrieval → ranked results displayed on the search page.

**Value:** Better discovery for long-tail, high-intent queries.

---

### 6B — Operations & Back-Office AI

#### 6B-1. Dynamic Pricing Engine

**What it does:** Automatically adjusts prices based on real-time metal spot prices, gemstone market rates, demand signals, and target margin rules.

**How it looks:**
- A scheduled service (daily or intra-day) pulls spot prices from a metals API (e.g., Kitco, Metals-API).
- Applies BOM cost calculations + margin rules → updates Shopify variant prices via Admin API.
- Dashboard shows margin health per SKU, alerts on margin compression.
- Optional ML layer: price elasticity modeling to optimize margin vs. conversion.

**Value:** Protects margins in volatile precious metals markets; eliminates manual repricing.

#### 6B-2. Demand Forecasting & Inventory Optimization

**What it does:** Predicts future demand per SKU/category/material to optimize purchasing and production scheduling.

**How it looks:**
- Time-series forecasting model (Prophet, or a simple gradient-boosted model) trained on historical sales, seasonality, and marketing calendar.
- Outputs: reorder recommendations, optimal safety-stock levels, supplier PO suggestions.
- Integrated into PIM/ERP dashboard (Katana or custom).

**Value:** Reduces overstock of slow-moving gems; prevents stockouts on popular metals.

#### 6B-3. AI Quality Control (Computer Vision)

**What it does:** Automates first-pass quality inspection by analyzing production photos against standards.

**How it looks:**
- At the QC stage, the jeweler takes standardized photos (macro shots against a controlled background).
- A vision model (fine-tuned classification/object-detection model) checks for: surface defects, stone alignment, prong integrity, finish consistency.
- Flags anomalies for human review; auto-approves pieces that pass.
- Over time, learns from human override decisions.

**Value:** Faster QC throughput; consistent quality standards even with multiple jewelers.

#### 6B-4. Automated Content Generation

**What it does:** Generates product descriptions, SEO copy, artist feature drafts, social media captions, and email copy from structured product/artist data.

**How it looks:**
- LLM (GPT-4-class) with prompt templates tuned to brand voice.
- Input: metafield data (metal, gem, weight, artist bio, design story).
- Output: product description, meta title/description, Instagram caption, Klaviyo email snippet.
- Human review before publish (editor-in-the-loop).

**Value:** Cuts content production time by 70%; maintains consistent brand voice across hundreds of SKUs.

#### 6B-5. Customer Service AI (Support Agent)

**What it does:** Handles tier-1 support queries — order status, shipping ETA, return initiation, sizing guides, care instructions — with escalation to humans for complex issues.

**How it looks:**
- AI agent with access to Shopify order data (read-only), FAQ knowledge base, and shipping tracker APIs.
- Deployed as a chat widget and email responder.
- Uses tool-calling / function-calling to look up specific order details and provide precise answers.
- Escalation triggers: custom design questions, complaints, refund requests above threshold.

**Value:** 24/7 support coverage; frees the team to focus on high-touch custom design consultations.

---

### 6C — Artist Program AI

#### 6C-1. Artist Discovery & Matching

**What it does:** Identifies emerging jewelry artists whose style complements the Noble Metal Studios brand by scanning portfolios on Instagram, Behance, and application submissions.

**How it looks:**
- Periodic scraping + embedding of artist portfolios.
- Style-matching model compares new artist work against the NMS brand aesthetic (trained on existing catalog imagery + curated mood boards).
- Surfaced as a ranked shortlist in the artist-program dashboard.

**Value:** Scales artist scouting without proportionally scaling staff.

#### 6C-2. Program Impact Analytics

**What it does:** Automatically compiles impact metrics — funds disbursed, artists supported, collection performance, customer engagement with artist stories — into quarterly reports.

**How it looks:**
- Data pipeline pulling from Shopify (sales by artist collection), CRM (artists onboarded), and finance ledger (program spend).
- LLM-generated narrative summary with charts (auto-drafted, human-reviewed).
- Published to the CMS impact page and shared with stakeholders.

**Value:** Professional impact reporting with minimal manual effort; strengthens brand credibility.

---

### 6D — AI Infrastructure Requirements

| Component | Recommendation |
|---|---|
| **LLM API** | OpenAI API (GPT-4o / GPT-4.1) or Anthropic Claude — for chatbot, content gen, reporting narratives |
| **Vector Database** | Pinecone, Weaviate, or Supabase pgvector — for semantic search and artist matching |
| **ML Platform** | Vertex AI, SageMaker, or simple Python services on Railway/Render — for forecasting, pricing, QC models |
| **Image Generation** | Fine-tuned diffusion model or 3D pipeline — for visual configurator |
| **Orchestration** | LangChain / LangGraph or custom function-calling pipelines — for multi-step AI agents |
| **Monitoring** | LangSmith, Helicone, or custom logging — for LLM cost tracking, latency, quality scoring |
| **Data Pipeline** | Shopify webhooks → event bus (e.g., Inngest, Temporal) → data warehouse (BigQuery) → ML models |
| **Human-in-the-Loop** | Every customer-facing AI output has an escalation path; every generated content piece has an editor review step |

---

## 7. Implementation Phases

### Phase 1: Core Commerce + Brand Story + AI Foundations (Months 1–3)

- Launch Shopify storefront (Advanced plan) with product taxonomy, metafields, basic variants
- Deploy headless website (Next.js on Vercel) with CMS (Sanity) for brand story and artist profiles
- Build custom design intake form → CRM pipeline
- Manual production tracking (Airtable/spreadsheet)
- **AI:** Deploy AI Design Concierge chatbot on intake page; set up automated content generation for product descriptions; implement basic personalized recommendations

### Phase 2: Configurator + Inventory + Operations AI (Months 4–6)

- Implement product configurator with dynamic pricing (metal spot price integration)
- Add PIM/BOM tracking (Katana) for precise inventory and procurement
- Integrate shipping (ShipStation), returns, and Klaviyo email flows
- **AI:** Launch dynamic pricing engine; deploy demand forecasting model; begin AI QC pilot with production photo analysis; add semantic search to storefront

### Phase 3: Artist Marketplace + Program + Scale AI (Months 7–10)

- Artist onboarding workflow with partnership agreements
- Commission tracking and program budgeting ledger
- Expand content marketing around artist stories
- **AI:** Deploy artist discovery/matching system; automate impact reporting; deploy AI customer service agent; launch visual configurator with AI rendering; refine all models based on accumulated data

### Phase 4: Optimization + Wholesale/B2B (Months 11+)

- Price elasticity modeling for margin optimization
- B2B/wholesale channel via Shopify Plus features
- International expansion with Shopify Markets
- Advanced customer segmentation and lifetime value modeling
- **AI:** Full closed-loop feedback on all AI systems; continuous model improvement

---

## 8. Recommended App & Tool Stack Summary

| Function | Primary Recommendation | Alternative |
|---|---|---|
| Commerce | Shopify Advanced / Plus | — |
| Frontend | Next.js on Vercel | Hydrogen + Remix on Oxygen |
| CMS | Sanity | Contentful, Prismic |
| DNS/CDN | Cloudflare | Route 53 |
| PIM/ERP | Katana + QuickBooks | Cin7, Acumatica |
| Product Configurator | Bold Product Options | Zepto, custom-built |
| CRM | HubSpot | Airtable |
| Shipping | ShipStation | Shippo |
| Returns | Loop | AfterShip Returns |
| Email/SMS | Klaviyo | — |
| Loyalty | Smile.io | LoyaltyLion |
| Reviews | Yotpo | Loox |
| Analytics | GA4 + Shopify Analytics | — |
| BI / Data Warehouse | BigQuery | Metabase + Postgres |
| LLM API | OpenAI (GPT-4o) | Anthropic Claude |
| Vector DB | Pinecone | Weaviate, Supabase pgvector |
| ML Platform | Vertex AI | SageMaker, Railway |
| AI Orchestration | LangChain / LangGraph | Custom function-calling |
| AI Monitoring | LangSmith | Helicone |

---

## 9. Next Steps Checklist

- [ ] Choose Shopify plan + headless stack (Next.js vs. Hydrogen)
- [ ] Build SKU/variant strategy and gem-metal compatibility matrix
- [ ] Select CMS and define content models
- [ ] Select and configure product configurator app
- [ ] Draft system architecture document with integration map
- [ ] Build custom design intake form and wire to CRM
- [ ] Document BOM and pricing rules; integrate metals spot-price API
- [ ] Draft artist partnership agreement and program charter
- [ ] Define production pipeline stages with SLA targets
- [ ] Set up Klaviyo flows for order lifecycle and custom design stages
- [ ] Provision LLM API keys; build AI design concierge MVP
- [ ] Build product description generation pipeline (LLM + metafields)
- [ ] Set up data warehouse for analytics and ML training data
- [ ] Pilot AI QC with initial training set of production photos
- [ ] Launch MVP and begin iterating

---

*This document synthesizes both prior infrastructure plans, takes the strongest elements of each, and layers in AI as a first-class operational capability — not a future nice-to-have, but a foundational advantage for a small team running a premium jewelry brand.*
