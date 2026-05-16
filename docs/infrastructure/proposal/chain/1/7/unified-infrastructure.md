# Noble Metal Studios — Infrastructure & Launch Plan

**Chain 1, link 7.** Self-contained. Supersedes `chain/1/6` (`unified-infrastructure-v5`): that document's strategy is absorbed here; its implementation plan is set aside as over-built. This link is intended to **close Chain 1** — the next step is not another proposal, it is a real project.

> **The reset.** v2–v5 were six rounds of AI iteration. Every round *added* — sections, agents, tooling, structure. None subtracted. The result was a "78%-complete" enterprise architecture for a business that should open for roughly $0. This document is the subtraction pass: the same strategy, a fraction of the apparatus.

---

## 1. The business

Noble Metal Studios — a jewelry business with two revenue surfaces:
- **Digital** — 3D/CAD jewelry models sold as downloads.
- **Physical** — finished jewelry pieces.

## 2. Founder advantages (the real moat)

- A career enterprise-integration engineer — fluent in systems and automation.
- Works in 3D jewelry modeling (MatrixGold) inside a gold/silver/diamond manufacturing shop.
- An existing CAD model library, discounted inventory, batch photography.
- Based in Kansas — no sales tax on digital products.

The library already exists. The first product does not need to be *made* — it needs to be *listed*.

## 3. The principle

**Earn on platforms that already exist. Build infrastructure with revenue, not against it.**

Every tool, subscription, and line of custom code must be paid for by a pain it removes or a dollar it brings in. Until then, it is not added.

## 4. Phases — gated by revenue, not the calendar

### Phase 0 — First dollars (now, ≈ $0)
- List existing 3D/CAD models on **CGTrader** and **Cults3D**.
- List finished pieces on **Etsy**.
- No infrastructure, no subscriptions, no risk. Validates which designs have demand; produces the first real revenue and data.
- *Exit when:* digital sales are consistent and it's clear which pieces move.

### Phase 1 — Physical commerce backbone (when Phase 0 earns)
- **Shopify Basic ($39/mo)** — not Advanced. Etsy + Shopify carry sales.
- Bookkeeping (QuickBooks or similar).
- Pricing from a CAD-derived BOM (metal volume + stones + labor + margin) — a spreadsheet first; a script only when the spreadsheet hurts.
- *Exit when:* physical orders flow and Shopify covers its own cost.

### Phase 2 — The self-hosted storefront comes up to speed (parallel, unhurried)
- The static **Astro storefront** (`jewelry-store`) — git-managed markdown catalog, Stripe checkout, deployed via Coolify to a Hostinger VPS.
- Matures alongside Etsy/Shopify; becomes primary **only when it is genuinely better** for the business — never on a schedule.
- *Exit when:* the self-hosted store outperforms the marketplaces for a real segment.

### Phase 3 — Scale what's proven
- Automation, configurator, artist program, and the rest — each added only when a specific, present pain pays for it.

## 5. Who actually does the work

Not a twelve-agent organization. There are:
- **Brad** — design, final judgment, relationships, the irreplaceable human calls.
- **Claude** — the collaborator: builds, lists, scripts, drafts, maintains.
- **A few small scripts** — BOM pricing, listing helpers — named for what they are.

That is the whole org chart. It scales by adding scripts and skills, not personas.

## 6. Tooling — current and deferred

- **Now:** free marketplace accounts, a spreadsheet, git.
- **Phase 1:** Shopify Basic, bookkeeping.
- **Deferred until a pain pays for them:** ERP (Katana), email automation (Klaviyo), configurator, semantic search, CRM, reviews/loyalty, demand forecasting. Each is a real tool with a real cost — none is a launch requirement.

## 7. What this closes

Chain 1 was an exploration, and it has run its course. The next step is not `chain/1/8`. It is to restructure this into a **real project** — issues for Phase 0's concrete tasks, a proper repo, and execution. This document is the brief for that.
