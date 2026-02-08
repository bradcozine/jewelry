# Noble Metal Studios: Unified Infrastructure Blueprint v5

**Completeness estimate:** 78% (key architecture and phased plan are defined, but issue decomposition, operational runbooks, and live system validation still need deeper detail before implementation).

> This revision extends v4 by:
> - Adding an explicit Issue Decomposition with sub-issues and agent ownership mapping.
> - Defining AI agent types, infrastructure overview, and business structure.
> - Introducing agent personas as “users,” with task tagging guidance.
> - Noting current limits in accessing the GitHub Issues tracker from this environment.

---

## 0. Access Notes & Assumptions

- **GitHub Issues access:** The local environment does not include a GitHub remote or the `gh` CLI, so the open-issue list is not directly visible here. The decomposition below is **derived from the latest proposal’s scope (v4)** and should be reconciled against the actual Issues tab as soon as access is available.
- **Action required:** Replace or merge the derived items below with the actual open issues once you can view the tracker.

---

## 1. Business Context & Founder Capabilities

*Unchanged from v4.* Key advantages:
- Enterprise integration engineering background (Pacific Bell, UCSF, Summit Equities, Kaiser Permanente ESB/IVR)
- Current role: 3D jewelry modeling (MatrixGold) in a gold/silver/diamond manufacturing shop
- Inventory access at discount, existing CAD model library, iJewel.design batch photography
- CAD models provide metal volumes and diamond sizes for accurate BOM cost estimates

---

## 2. Architecture Summary

The core architecture from v2–v4 remains, with **digital-first sales** plus a physical commerce pipeline:

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

**Primary platforms:** CGTrader, Cults3D, Printables, Etsy (Digital), Shopify Digital.

### 3.4 AI Digital Sales Agent (Summary)

Automates multi-market listings by scanning the MatrixGold library, generating metadata and renders, pricing against comps, and publishing to marketplaces. Produces a weekly report on sales, winners, and price recommendations.

### 3.5 Revenue Projections (Conservative)

| Scenario | Models Listed | Avg Price | Monthly Sales | Monthly Revenue |
|---|---|---|---|---|
| **Month 1–3** | 20–50 | $50 | 5–10 | $250–500 |
| **Month 4–6** | 50–100 | $60 | 10–25 | $600–1,500 |
| **Month 7–12** | 100–200 | $75 | 25–50 | $1,875–3,750 |

---

## 4. Funding Opportunities (Summary)

- **Small business grants:** SBIR/STTR, SBA Microloans, Kiva, Grants.gov
- **Arts/culture grants:** NEA Art Works, Americans for the Arts, Kansas Creative Arts Commission, CERF+
- **Impact funding:** Echoing Green, Halcyon, Ashoka, B Corp
- **Industry-specific:** Jewelers of America, Women’s Jewelry Association, Metalsmith awards
- **Tech credits:** Google/Microsoft/AWS/NVIDIA startup programs

---

## 5. Updated Implementation Phases (Condensed)

**Phase 0 (Weeks 1–4):** Digital-first launch, 20–50 models listed, digital sales agent, cloud credits, Kiva, bookkeeping setup.

**Phase 1 (Months 1–3):** Physical storefront setup, SKU selection, photography, BOM pricing, Shopify + Next.js + CMS, AI content/QC/pricing, first artists onboarded.

**Phase 2 (Months 4–6):** Configurator, production automation, returns, search, CRM automation, reviews/loyalty, digital catalog 100+.

**Phase 3 (Months 7–10):** Artist marketplace, commission tracking, forecasting, impact reporting, SBIR application.

**Phase 4 (Months 11+):** Optimization & expansion.

---

## 6. Issue and Sub-Issue Analysis

> **Note:** Derived from proposal scope due to missing access to the GitHub Issues tracker. Replace with real issue IDs/titles when available.

### Issue #1: Digital Sales Launch (Marketplace Listings)
- **Sub-issue:** Model inventory audit and selection (20–50 assets) — Identify viable models, check file formats, confirm render-ready assets. **Owner agent:** Catalog Ops Agent.
- **Sub-issue:** Listing metadata templates — Define title, description, tag structures per marketplace. **Owner agent:** Content Agent.
- **Sub-issue:** Pricing model v1 — Create complexity-based pricing and comp-scan baseline. **Owner agent:** Pricing Analyst Agent.
- **Sub-issue:** Batch upload workflow — Manual or scripted uploads per marketplace with consistency checks. **Owner agent:** Ops Agent.
- **Sub-issue:** Performance reporting — Weekly sales/click reports and iteration loop. **Owner agent:** Analytics Agent.

### Issue #2: Digital Sales Agent (Automation)
- **Sub-issue:** Marketplace API capability survey — Verify APIs, rate limits, and fallback methods. **Owner agent:** Research Agent.
- **Sub-issue:** Metadata generation pipeline — Generate titles/descriptions/tags and QA checks. **Owner agent:** Content Agent.
- **Sub-issue:** Pricing engine integration — Plug pricing rules into listings. **Owner agent:** Pricing Analyst Agent.
- **Sub-issue:** Render automation hook — Connect iJewel batch renders into asset pipeline. **Owner agent:** Pipeline Engineer Agent.
- **Sub-issue:** Reporting dashboard — Revenue, top sellers, conversion, pricing recommendations. **Owner agent:** Analytics Agent.

### Issue #3: Physical Commerce Setup (Shopify + Headless)
- **Sub-issue:** SKU shortlist (18–30) — Select Day 1 lineup and validate BOM pricing. **Owner agent:** Product Planning Agent.
- **Sub-issue:** Shopify setup — Tax, shipping, product taxonomy, metafields. **Owner agent:** Commerce Ops Agent.
- **Sub-issue:** Next.js storefront — Implement headless storefront with checkout. **Owner agent:** Frontend Agent.
- **Sub-issue:** CMS content models — Artist profiles, editorial, catalog metadata. **Owner agent:** Content Systems Agent.
- **Sub-issue:** Email flows — Klaviyo onboarding, abandoned cart, post-purchase. **Owner agent:** Lifecycle Marketing Agent.

### Issue #4: AI Content & Pricing Stack
- **Sub-issue:** Content templates for products — Style guide, tone, SEO schema. **Owner agent:** Content Agent.
- **Sub-issue:** Pricing rules engine — Inputs: metal volume, stones, labor, margin. **Owner agent:** Pricing Analyst Agent.
- **Sub-issue:** QC documentation — AI-driven QA checklist with outputs. **Owner agent:** QA Agent.

### Issue #5: Funding & Grants Pipeline
- **Sub-issue:** Grant eligibility matrix — Map eligibility, cycles, and requirements. **Owner agent:** Research Agent.
- **Sub-issue:** Application calendar — Timeline and prep checklist. **Owner agent:** Ops Agent.
- **Sub-issue:** Impact metrics draft — Define KPIs (artists supported, income, diversity). **Owner agent:** Impact Analyst Agent.

### Issue #6: Artist Program & Marketplace
- **Sub-issue:** Artist onboarding workflow — Contract, portfolio intake, tooling setup. **Owner agent:** Program Manager Agent.
- **Sub-issue:** Commission tracking — Ledger model and payout automation. **Owner agent:** Finance Ops Agent.
- **Sub-issue:** Artist visibility & features — Profiles, stories, collections. **Owner agent:** Content Agent.

---

## 7. AI Agent Types and Infrastructure

### Agent Types

| Agent Type | Responsibilities | Inputs | Outputs | Tools/Permissions |
|---|---|---|---|---|
| **Research Agent** | Market research, API capability checks, grant scans | Web sources, API docs | Reports, recommendations | Web access, doc ingestion |
| **Content Agent** | Listings, product copy, artist profiles | Asset data, tone guide | Descriptions, tags, SEO | LLM access, CMS write |
| **Pricing Analyst Agent** | Pricing rules and comps | BOM data, comps | Price recommendations | Sheets/DB, AI assist |
| **Pipeline Engineer Agent** | Automation, integrations | Asset library, APIs | ETL jobs, agent flows | Render, queues, storage |
| **Commerce Ops Agent** | Shopify setup, tax, shipping | Shopify admin | Configured storefront | Admin access |
| **Frontend Agent** | Headless storefront | Design, product data | Next.js UI | Git, hosting |
| **Analytics Agent** | Reporting & dashboards | Sales, traffic logs | KPIs, weekly reports | BI tools, DB read |
| **Program Manager Agent** | Artist onboarding | Contracts, schedules | Onboarding plan | Docs, CRM |
| **Finance Ops Agent** | Commission tracking, payouts | Sales ledger | Payout files, ledgers | Accounting tools |
| **Lifecycle Marketing Agent** | Email flows | CRM segments | Email sequences | ESP access |
| **QA Agent** | QC docs, compliance checks | Product data | QA checklists | Docs, check tools |
| **Impact Analyst Agent** | Impact reporting | Program data | Impact metrics | BI tools |

### AI Infrastructure Overview

- **Core services:** Shopify (physical), digital marketplaces (digital), Next.js storefront, Sanity CMS.
- **Agent runtime:** Node.js (Render) or serverless functions for scheduling & workflows.
- **Data layer:** Lightweight DB or spreadsheets for product metadata, pricing, and listing state.
- **File storage:** Cloudflare R2 or similar for CAD assets and renders.
- **Analytics:** Marketplace exports + Shopify data into a single reporting table.
- **Security & access:** Minimal-permission service accounts; avoid exposing proprietary CAD files.

---

## 8. Agentic Business Structure

- **Product & Merchandising:** Product Planning Agent defines Day 1 lineup, coordinates with Pricing Analyst and Content Agent.
- **Engineering & Automation:** Pipeline Engineer Agent owns the digital sales agent and integrations.
- **Commerce Operations:** Commerce Ops Agent handles Shopify config and compliance.
- **Marketing & Growth:** Content Agent + Lifecycle Marketing Agent drive listings, SEO, and email flows.
- **Artist Program:** Program Manager Agent coordinates onboarding, while Impact Analyst Agent tracks outcomes.
- **Finance & Admin:** Finance Ops Agent manages payouts and ledger accuracy.

**Coordination flow:**
- Research Agent → Product Planning & Pricing (market inputs)
- Pricing Analyst → Content Agent & Commerce Ops (prices + product copy)
- Pipeline Engineer → Analytics Agent (logs/metrics)
- Program Manager → Impact Analyst (program KPIs)

**KPIs:**
- Digital sales revenue, listing conversion rate, time-to-list, CAC, artist payout timeliness, customer repeat rate.

---

## 9. Users / Agent Personas and Task Tagging

> “Users” are agent personas who interact with the system and are assigned tasks in issues.

- **Research Agent** — cares about accuracy and market fit; tags: #research, #grants, #marketplaces.
- **Content Agent** — cares about listings and narrative; tags: #copywriting, #seo, #cms.
- **Pricing Analyst Agent** — cares about margin and comps; tags: #pricing, #bom, #margin.
- **Pipeline Engineer Agent** — cares about automation reliability; tags: #automation, #integrations.
- **Commerce Ops Agent** — cares about platform correctness; tags: #shopify, #tax, #shipping.
- **Frontend Agent** — cares about UX and performance; tags: #frontend, #headless.
- **Analytics Agent** — cares about KPIs and insights; tags: #analytics, #dashboard.
- **Program Manager Agent** — cares about artist experience; tags: #artist-program, #onboarding.
- **Finance Ops Agent** — cares about ledger accuracy; tags: #payouts, #accounting.
- **Lifecycle Marketing Agent** — cares about retention; tags: #email, #lifecycle.
- **QA Agent** — cares about product correctness; tags: #qa, #compliance.
- **Impact Analyst Agent** — cares about mission outcomes; tags: #impact, #reporting.

---

## 10. Key Differences from v4

1. Added explicit Issue Decomposition with sub-issues and ownership.
2. Formalized AI agent types, infrastructure, and business structure.
3. Defined “users” as agent personas with tagging guidance.
4. Added completeness estimate and access caveat for GitHub Issues.

---

*Next action:* reconcile the Issue Decomposition with the live GitHub Issues tracker and update issue IDs/titles accordingly.
