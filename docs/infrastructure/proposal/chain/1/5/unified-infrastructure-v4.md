# Noble Metal Studios: Unified Infrastructure Blueprint v4

> This revision extends v3 by adding:
> - **Digital design sales** as an early, zero-inventory revenue stream
> - **AI agent for digital asset marketplace management** (CGTrader, Cults3D, Printables)
> - **Funding opportunities** aligned with the artist development mission
> - Reorganized phases to prioritize digital-first revenue on Day 1

---

## 1. Business Context & Founder Capabilities

*Unchanged from v3.* Key advantages:
- Enterprise integration engineering background (Pacific Bell, UCSF, Summit Equities, Kaiser Permanente ESB/IVR)
- Current role: 3D jewelry modeling (MatrixGold) in a gold/silver/diamond manufacturing shop
- Inventory access at discount, existing CAD model library, iJewel.design batch photography
- CAD models provide metal volumes and diamond sizes for accurate BOM cost estimates

---

## 2. Architecture Summary

The core architecture from v2/v3 remains. This revision adds a **Digital Sales Channel** alongside the physical commerce pipeline:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           CUSTOMERS                                     │
│  Headless Website · Shopify · POS · Social · CGTrader · Cults3D        │
└──────┬──────────────────┬─────────────────┬──────────────┬─────────────┘
       │                  │                 │              │
  Storefront API     Checkout API      POS SDK    Marketplace APIs
       │                  │                 │              │
┌──────▼──────────────────▼─────────────────▼──────────────▼─────────────┐
│                      COMMERCE CORE                                      │
│                                                                         │
│  ┌─────────────────────────┐    ┌──────────────────────────────────┐    │
│  │   SHOPIFY (Physical)    │    │  DIGITAL SALES AGENT             │    │
│  │   Products · Inventory  │    │  3D Model Marketplace Manager    │    │
│  │   Pricing · Tax · Ship  │    │  CGTrader · Cults3D · Printables │    │
│  └─────────────────────────┘    │  Pricing · Listing · Analytics   │    │
│                                  └──────────────────────────────────┘    │
│                                                                         │
│                    ┌──────────────────────────────┐                      │
│                    │    AI SERVICES LAYER          │                      │
│                    │  Pricing · Content · QC       │                      │
│                    │  Digital Listing Agent         │                      │
│                    │  Forecasting · Concierge       │                      │
│                    └──────────────────────────────┘                      │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Digital Design Sales — Early Revenue Stream

### 3.1 Why Digital First

- **Zero inventory risk** — no materials, no shipping, no returns
- **Kansas does not charge sales tax on digital products** — simplifies Day 1 tax compliance
- **Existing asset library** — the shop's MatrixGold models are ready to list
- **Passive income** — once listed, designs sell without production effort
- **Market validation** — digital sales signal which designs have demand before committing to physical production

### 3.2 Digital Product Types

| Product Type | Format | Target Market | Price Range |
|---|---|---|---|
| **Ready-to-cast 3D models** | .STL, .3DM, .OBJ | Jewelers, manufacturers | $25–150 |
| **MatrixGold project files** | .M3D, .3DM | Advanced jewelers with MatrixGold | $50–300 |
| **Parametric/configurable designs** | .M3D with parameters | Jewelers wanting customizable templates | $100–500 |
| **Render-ready scene files** | .BLEND, .C4D, .MAX | 3D artists, marketers | $15–75 |
| **Design collections/bundles** | Mixed | Studios, schools | $200–1,000 |

### 3.3 Marketplace Strategy

**Primary platforms:**

| Platform | Audience | Commission | Notes |
|---|---|---|---|
| **CGTrader** | Professional jewelers, 3D artists | 20–30% | Largest 3D marketplace; strong jewelry category |
| **Cults3D** | Makers, jewelers, hobbyists | 20% | Good European audience |
| **Printables** | 3D printing community | Free to list | Lower price point but high volume |
| **Etsy (Digital)** | Craft/jewelry makers | 6.5% + fees | Already familiar to jewelry buyers |
| **Own website (Shopify Digital)** | Direct customers | 0% commission | Highest margin, needs traffic |

### 3.4 AI Digital Sales Agent

**What it does:** An automated agent that manages the digital asset catalog across multiple marketplaces — listing, pricing, updating, and reporting.

**Workflow:**

```
┌──────────────────┐     ┌────────────────────────┐     ┌──────────────────┐
│  MatrixGold      │────►│  Digital Sales Agent    │────►│  Marketplaces    │
│  Model Library   │     │  (Node.js on Render)    │     │                  │
│                  │     │                          │     │  CGTrader API    │
│  • .3DM files    │     │  1. Catalog scan:        │     │  Cults3D API     │
│  • Volume data   │     │     new/updated models   │     │  Etsy API        │
│  • Diamond specs │     │  2. Generate metadata:   │     │  Shopify Digital  │
│  • Renders       │     │     title, description,  │     │                  │
│                  │     │     tags, category        │     │  Updates:        │
│                  │     │  3. Price calculation:    │     │  • Create listing│
│                  │     │     complexity + market   │     │  • Update price  │
│                  │     │     comps + file format   │     │  • Sync inventory│
│                  │     │  4. Generate preview      │     │  • Pull analytics│
│                  │     │     renders (iJewel)       │     │                  │
│                  │     │  5. Push to marketplaces  │     └──────────────────┘
│                  │     │  6. Track sales + report  │
│                  │     └────────────────────────────┘
│                  │                    │
└──────────────────┘                    ▼
                              ┌──────────────────┐
                              │  Weekly Report    │
                              │  • Revenue/platform│
                              │  • Best sellers    │
                              │  • Price rec's     │
                              │  • New model queue │
                              └──────────────────┘
```

**AI-generated listing content:**
- Title optimized per platform (SEO keywords: "3D jewelry model", "ready to cast", "engagement ring STL")
- Description: design specs (metal volume, stone count/sizes, dimensions), compatible software, printing notes
- Tags: metal type, jewelry category, style, technique
- Pricing: based on model complexity (polygon count, parametric features), comparable listings, format value

**Technology:**

| Component | Choice | Cost |
|---|---|---|
| Marketplace APIs | CGTrader API, Etsy API, Cults3D (manual or scraping if no API) | $0 |
| Listing agent | Node.js on Render (scheduled + on-demand) | $0–7/mo |
| Content generation | OpenAI GPT-4o for descriptions | Shared with content pipeline |
| Preview renders | iJewel.design (existing tool) | $0 (already available) |
| File storage | Cloudflare R2 | $0–5/mo |

**Est. cost: $0–12/month** (largely shared with existing AI services)

### 3.5 Revenue Projections (Conservative)

| Scenario | Models Listed | Avg Price | Monthly Sales | Monthly Revenue |
|---|---|---|---|---|
| **Month 1–3** | 20–50 | $50 | 5–10 | $250–500 |
| **Month 4–6** | 50–100 | $60 | 10–25 | $600–1,500 |
| **Month 7–12** | 100–200 | $75 | 25–50 | $1,875–3,750 |

These are net-of-commission estimates. Digital revenue requires almost no ongoing effort once listed — the AI agent handles updates and optimization.

---

## 4. Funding Opportunities

Given the altruistic artist-development mission, Noble Metal Studios qualifies for several funding categories:

### 4.1 Small Business Grants

| Program | Amount | Eligibility | Notes |
|---|---|---|---|
| **SBIR/STTR** (SBA) | $50K–$250K+ | Tech-driven small business | AI-powered commerce angle could qualify Phase I |
| **SBA Microloans** | Up to $50K | Any small business | Low interest, good for equipment/inventory |
| **Kiva** | Up to $15K | Any entrepreneur | 0% interest crowd-funded microloans |
| **Grants.gov** | Varies | Search by category | Federal grants database |

### 4.2 Arts & Culture Grants

| Program | Amount | Eligibility | Notes |
|---|---|---|---|
| **NEA Art Works** | $10K–$100K | Orgs supporting artists | Artist development program aligns well |
| **Americans for the Arts** | Varies | Arts organizations | Directory of 4,000+ grant programs |
| **Kansas Creative Arts Industries Commission** | Varies | KS-based arts businesses | State-level arts funding |
| **Craft Emergency Relief Fund (CERF+)** | Varies | Craft artists | Emergency and development funding |

### 4.3 Social Enterprise / Impact Funding

| Program | Amount | Eligibility | Notes |
|---|---|---|---|
| **Echoing Green Fellowship** | $80K | Social entrepreneurs | Highly competitive but perfect mission fit |
| **Halcyon Incubator** | Fellowship + support | Social enterprises | DC-based but accepts remote |
| **Ashoka Fellowship** | Stipend + network | Social entrepreneurs | For "system-changing" social ventures |
| **B Corp certification** | N/A | Mission-driven businesses | Not funding, but unlocks impact investor networks |

### 4.4 Jewelry Industry Specific

| Program | Amount | Eligibility | Notes |
|---|---|---|---|
| **Jewelers of America (JA) scholarships** | Varies | Emerging jewelers | Could fund artist program participants |
| **Women's Jewelry Association grants** | Varies | Women in jewelry | If applicable to founder or artists |
| **Metalsmith Magazine awards** | Portfolio recognition | Jewelers/metalsmiths | Visibility + credibility for artist program |

### 4.5 Tech / AI Innovation

| Program | Amount | Eligibility | Notes |
|---|---|---|---|
| **Google for Startups** | Credits + mentorship | AI-driven startups | Cloud credits could offset AI service costs |
| **Microsoft for Startups** | $150K Azure credits | Startups < 7 years | Significant cloud savings |
| **AWS Activate** | $10K–$100K credits | Startups | Hosting cost reduction |
| **NVIDIA Inception** | GPU credits + support | AI startups | For AI rendering / visual configurator work |

### 4.6 Funding Strategy

**Immediate (Month 1):**
- Apply to Kiva (0% interest microloan, $15K)
- Register on Grants.gov and Kansas Creative Arts Commission
- Apply for Google for Startups / AWS Activate / Microsoft for Startups cloud credits

**Short-term (Months 2–4):**
- NEA Art Works application (next cycle)
- CERF+ for artist program participants
- B Corp assessment to understand certification path

**Medium-term (Months 6–12):**
- Echoing Green Fellowship (annual cycle)
- SBIR Phase I (AI-powered commerce innovation)
- Document impact metrics for all future grant applications

---

## 5. Updated Implementation Phases

### Phase 0: Digital-First Launch (Weeks 1–4)

**Goal:** Revenue from Day 1 with zero inventory risk.

- [ ] LLC formation, EIN, business bank account
- [ ] List 20–50 3D models on CGTrader + Cults3D + Etsy Digital
- [ ] Build AI digital sales agent (listing + pricing automation)
- [ ] Apply for cloud credits (Google/AWS/Microsoft for Startups)
- [ ] Apply for Kiva microloan
- [ ] Register on Grants.gov, Kansas Creative Arts Commission
- [ ] Set up QuickBooks for bookkeeping

### Phase 1: Physical Commerce Launch (Months 1–3)

**Goal:** Live storefront, first physical sales, AI writing all content.

- [ ] AI-assisted product line selection → Day 1 lineup (18–30 physical SKUs)
- [ ] Product photography via iJewel.design
- [ ] BOM + pricing from MatrixGold volume data
- [ ] Shopify Advanced: taxonomy, metafields, variants, collections, shipping/tax
- [ ] Next.js headless site on Vercel
- [ ] Sanity CMS: artist profiles, editorial, content models
- [ ] Klaviyo: email flows
- [ ] AI Dynamic Pricing Engine deployed
- [ ] AI Content Pipeline deployed
- [ ] AI QC Documentation deployed
- [ ] Rules-based inventory reorder points
- [ ] Manual custom design pipeline in CRM
- [ ] Artist program: onboard first 1–2 artists
- [ ] Sales tax permit (physical goods only — digital already tax-free in KS)
- [ ] NEA Art Works application submitted

### Phase 2: Configurator + Pipeline Automation (Months 4–6)

*Unchanged from v3:*
- [ ] Bold Product Options configurator
- [ ] Katana BOM tracking
- [ ] ShipStation + AfterShip Returns
- [ ] AI Design Concierge
- [ ] AI Semantic Search
- [ ] Custom design pipeline automation
- [ ] Yotpo + Smile.io
- [ ] Digital sales agent expanded to 100+ models
- [ ] Echoing Green Fellowship application

### Phase 3: Artist Marketplace + Scale (Months 7–10)

*Unchanged from v3, plus:*
- [ ] Artist onboarding workflow
- [ ] Commission tracking
- [ ] Program fund ledger
- [ ] AI Demand Forecasting (Prophet)
- [ ] AI Impact Report Generator
- [ ] Analytics dashboards refined
- [ ] SBIR Phase I application (AI commerce innovation)
- [ ] Document impact metrics for grant applications

### Phase 4: Optimization + Expansion (Months 11+)

*Unchanged from v3.*

---

## 6. Updated Cost Summary

| Function | Monthly Cost (Phase 0) | Monthly Cost (Phase 1) |
|---|---|---|
| Digital marketplace listings | $0 | $0 |
| AI Digital Sales Agent | $0–7 | $7 |
| Commerce (Shopify) | — | $399 |
| Frontend (Vercel) | — | $0–20 |
| CMS (Sanity) | — | $0–99 |
| All other services | — | See v2 table |
| **New total Phase 0** | **$0–7** | — |
| **New total Phase 1** | — | **~$630–880** |

Phase 0 is essentially free — the only cost is the AI agent hosting, which fits in Render's free tier.

---

## 7. Key Differences from v3

1. **Phase 0 added** — digital-first revenue before any physical inventory costs
2. **Digital Sales Agent** — new AI service for managing 3D model marketplace listings
3. **Kansas tax advantage** — no sales tax on digital goods simplifies Day 1 compliance
4. **Funding roadmap** — structured grant/loan/credit application timeline
5. **Revenue before expenses** — digital sales can fund early physical commerce costs

---

*Digital designs are the lowest-friction path to Day 1 revenue. The existing MatrixGold library is an untapped asset. List it, price it, let the AI agent manage it — and use the revenue to fund the physical commerce launch.*
