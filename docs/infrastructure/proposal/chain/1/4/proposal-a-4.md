# Noble Metal Studios: Unified Infrastructure Blueprint v3

> This revision extends the prior unified infrastructure plan (v2) by adding:
> - AI-assisted product line selection, including a Day 1 lineup.
> - Business legal structure, tax filings, and property lease considerations.
> - The founder’s background and in-shop manufacturing capabilities (MatrixGold, iJewel.design, inventory access).

---

## 1. Business Context & Founder Capabilities

**Founder experience:**
- Former enterprise integration engineer with service-bus and large-scale integration work (Pacific Bell, UCSF Medical Center hospital merger data, Summit Equities market-timing integration, Kaiser Permanente ESB team and IVR prescription refill system).
- Current role: 3D jewelry modeling in a small shop that manufactures gold/silver pieces with diamonds for high-end jewelers.

**Operational advantages:**
- **Inventory access:** Ability to purchase shop inventory at a discount to seed online sales.
- **Design pipeline:** MatrixGold for CAD, access to existing model library for rapid catalog expansion.
- **Photo pipeline:** iJewel.design to batch-generate product photos.
- **Cost modeling:** Existing CAD models provide metal volumes and diamond sizes → accurate BOM cost estimates.

These strengths shift the plan from a theoretical headless stack to a practical, cost-aware manufacturing + ecommerce system.

---

## 2. Architecture Summary (Same Core, Founder-Optimized)

**System of record:** Shopify for products, inventory, pricing, tax, checkout.
**Brand & storytelling:** Headless website (Next.js) with CMS (Sanity).
**Operations:** Katana (BOM + production tracking) + QuickBooks.
**AI services:** Small, targeted scripts (pricing, content, QC, forecasting).

The core architecture from v2 remains, but the “Day 1” plan is now grounded in the shop’s existing manufacturing pipeline and inventory access.

---

## 3. AI-Assisted Product Line Selection (With Day 1 Lineup)

### 3.1 Product Line Selection Engine (AI + Rules)

**Purpose:** Select a sellable, margin-safe lineup using data from:
- Existing shop inventory
- CAD model library (MatrixGold)
- Historical sales in-store (if available)
- Online market demand signals (search trends, competitor pricing, Etsy/Shopify benchmarks)

**Data inputs:**
- **Inventory list:** SKU, metal, stone, cost, retail price, margin
- **CAD library:** design type, metal weight (g), stone count/size
- **Market signals:** scraped pricing ranges, keyword volume, seasonal demand
- **Production constraints:** lead time, labor hours, complexity rating

**AI outputs:**
- Ranked list of candidate products by **margin + demand + production feasibility**
- Suggested pricing bands
- Recommended variants (metal/stone combinations that balance margin and appeal)

### 3.2 Day 1 Product Lineup (Minimum Viable Catalog)

**Goal:** Launch with a curated lineup that is easy to produce, photogenic, and margin-stable.

**Recommended Day 1 lineup: 18–30 SKUs**

1. **Core Staples (8–12 SKUs)**
   - Simple gold/silver bands (plain, matte, or lightly textured)
   - Solitaire or 3-stone rings (lab-grown + natural options)
   - Minimal stud earrings (diamond or gemstone accents)

2. **Signature Pieces (6–8 SKUs)**
   - 2–3 “hero” rings with distinct settings
   - 1–2 pendants with recognizable motif (Noble Metal Studios branding)
   - 1–2 bracelets or cuffs with light customization

3. **Artist Spotlight (4–6 SKUs)**
   - 1–2 pieces from each of 2–3 artists (curated entry-level pieces)
   - Focus on high visual impact and storytelling

4. **Custom Design Placeholders (2–4 SKUs)**
   - “Design Your Own” starter items with intake form
   - These act as lead generators, not inventory-heavy products

**Why this lineup works:**
- Minimal complexity for launch while still showing breadth.
- Balances ready-to-ship inventory with custom intake.
- Leverages existing shop capabilities to avoid expensive new production lines.

### 3.3 AI Workflow for Lineup Management

- **Monthly:** AI re-scores SKUs based on sales velocity and margin.
- **Quarterly:** AI recommends product rotation (swap low performers, test 2–3 new designs).
- **Seasonal:** Holiday or event-based collections (Valentine’s, Mother’s Day) are auto-proposed by trend analysis.

---

## 4. Legal Structure, Tax Filings, and Property Leases

### 4.1 Business Legal Structure

**Recommended:** LLC (default) with the option to elect S-Corp later for tax optimization.

**Initial steps:**
- File **Articles of Organization** with the state.
- Draft an **Operating Agreement** (even single-member).
- Obtain **EIN** from the IRS.
- Open a **business bank account** and separate finances.

**Why:** Protects personal assets and establishes credibility with vendors and artists.

### 4.2 Tax & Compliance Checklist

**Federal/State:**
- EIN (IRS)
- State business registration
- State sales tax permit (if required)

**Shopify / ecommerce compliance:**
- Configure sales tax collection (Shopify Tax or Avalara if complex).
- Maintain **resale certificates** for wholesale purchasing.
- Track **COGS** for precious metals and gemstones.
- Maintain **FTC jewelry compliance** (metal hallmarking, gemstone disclosure).

**Quarterly:**
- Estimated tax payments (federal + state).
- Reconcile inventory, precious metal usage, and cost accounting.

### 4.3 Property Leases / Studio Space

**If leasing:**
- Confirm zoning allows jewelry manufacturing and retail.
- Verify insurance requirements (liability, property, equipment, inventory coverage).
- Negotiate build-out allowances if workspace requires secure storage or ventilation.
- Assess lease terms around **security** (safe, alarm systems) and **after-hours access**.

**If operating in existing shop:**
- Document internal agreements on inventory access, pricing discounts, and use of equipment.
- Clarify IP ownership for designs created on shared equipment.

---

## 5. Updated Implementation Phases (Grounded in Day 1 Reality)

### Phase 1: Day 1 Launch (Months 0–2)
- Choose 18–30 SKUs using AI scoring + inventory feasibility.
- Generate product photos via iJewel.design.
- Use MatrixGold library to build BOM and pricing.
- Launch Shopify storefront + basic headless site.
- Configure core legal/tax setup (LLC, EIN, sales tax permit).

### Phase 2: Operations + AI Expansion (Months 3–6)
- Add dynamic pricing engine (metal spot prices).
- Deploy AI content pipeline (descriptions, SEO, social).
- Implement inventory tracking (Katana or lightweight alternative).
- Introduce custom design intake + pipeline automation.

### Phase 3: Scale + Artist Program (Months 7–10)
- Expand artist partnerships and collections.
- Deploy AI forecasting (once meaningful data exists).
- Launch quarterly impact reports and program funding.

---

## 6. Updated “Next Steps” Checklist

1. Confirm LLC formation and tax registrations.
2. Build Day 1 product list (18–30 SKUs) using AI + inventory reality check.
3. Generate pricing models from CAD volume/weight data.
4. Produce product photos with iJewel.design in batches.
5. Configure Shopify catalog (metafields, collections, pricing rules).
6. Launch headless website + CMS (story + artist profiles).
7. Deploy content AI pipeline to avoid manual writing overload.

---

**This version keeps the deep AI architecture from v2, but grounds it in your real-world manufacturing access, inventory advantages, and business launch realities.**
