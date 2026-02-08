# Noble Metal Studios: Shopify + Custom Website Infrastructure, Design Docs, and Configurations

## 1. Business Context & Goals
- **Brand:** Noble Metal Studios
- **Sales channels:** Shopify (primary commerce engine) + custom website (brand storytelling, editorial, portfolio, artist features, custom design intake)
- **Product model:** configurable jewelry (metals, gems, components), custom design projects, and featured artist collections
- **Mission allocation:** portion of earnings funds programs for emerging artists (impact program)

## 2. High-Level Architecture (Omnichannel)
**Core idea:** Shopify is the system of record for products, inventory, pricing, tax, and checkout. Your custom website acts as a headless front-end plus a content hub.

### Components
- **Shopify Store** (commerce engine)
  - Products, variants, inventory, pricing, discounts
  - Shopify Checkout, taxes, shipping, gift cards
  - Customer accounts, orders, returns
- **Custom Website** (headless frontend)
  - Uses Shopify Storefront API for product/collections/cart
  - Uses CMS for content (artist features, editorial, program pages)
  - Handles custom design intake forms + CRM pipeline
- **CMS** (content & editorial)
  - Artist profiles, collections, lookbooks, program impact updates
- **PIM + Inventory Rules**
  - Manages component SKUs, metals, gemstones, sizes, finishing
  - Maps combinatorial SKUs to Shopify variants
- **Custom Design Workflow**
  - Intake form → CRM → quote → invoice → production order
- **Analytics & BI**
  - Shopify Analytics + GA4 + customer segmentation
- **Community & Impact Program**
  - Artist onboarding, grants/scholarships, program reporting

## 3. Infrastructure Components (What You Need)
### A) Commerce Core
- **Shopify Advanced or Plus** for robust multi-location inventory, Shopify Flow, and B2B/wholesale later.
- **Shopify Payments** (or alternative gateway if needed for specific regions).
- **Shopify POS** (optional) for trunk shows or studio sales.

### B) Headless Website Stack
- **Frontend:** Next.js (recommended) or Hydrogen + Remix
- **Hosting:** Vercel, Netlify, or Shopify Oxygen (if Hydrogen)
- **Domains & DNS:** Cloudflare or Route53

### C) Content & CMS
- **Headless CMS:** Sanity, Contentful, or Prismic
- **Content types:** Artist profiles, editorial stories, program updates, custom design portfolio

### D) Inventory, PIM, and Configuration Logic
- **Inventory + PIM:** Options include:
  - Shopify + app-based product configurator
  - ERP/PIM: Cin7, Katana, or Acumatica (if scaling)
- **Configurator:**
  - For metal/gem/component options, use a Shopify product configurator app or a custom configurator storing selections in line item properties.

### E) CRM & Custom Design Pipeline
- **CRM:** HubSpot or Airtable for design requests and artist partnerships
- **Forms:** Typeform, Tally, or native Next.js forms → CRM
- **Quotes & invoices:** Shopify Draft Orders or third-party quoting tools

### F) Logistics & Operations
- **Shipping:** ShipStation or Shippo
- **Returns:** Loop or AfterShip Returns
- **Tax & compliance:** Shopify Tax + Avalara (if needed)

### G) Marketing, Email, and Loyalty
- **Email/SMS:** Klaviyo
- **Loyalty/Referral:** Smile.io or LoyaltyLion
- **Reviews:** Yotpo or Loox

## 4. Design Documents You Should Create
### A) Technical Design Docs
1. **System Architecture Document**
   - Diagram showing Shopify, CMS, website, PIM, CRM, analytics
   - API integration points and data flow
2. **Data Model & SKU Strategy**
   - How metals, gems, sizes, and components map to variants
3. **Inventory & Production Flow**
   - Raw materials → component assembly → finished goods
4. **Custom Design Workflow**
   - Intake → design → quote → approval → production → delivery
5. **Artist Collaboration Workflow**
   - Artist onboarding, product submission, revenue share
6. **Security & Compliance**
   - Access levels, PII data handling, GDPR/CCPA

### B) Experience Design Docs
1. **Brand & UI Style Guide**
   - Typography, colors, imagery style
2. **Customer Journey Maps**
   - Ready-to-buy vs. custom design vs. artist collections
3. **Content Strategy**
   - Editorial features, artisan spotlight, behind-the-scenes
4. **Impact Program Documentation**
   - How the earnings component is allocated

### C) Business/Operational Docs
1. **Program Charter (Impact)**
   - Mission, funding percentage, reporting cadence
2. **Artist Partnership Agreements**
   - IP rights, revenue splits, timelines
3. **SOPs for custom work**
   - Quality control, production milestones

## 5. Foundational Configurations (Shopify + Website)
### Shopify Configurations
- **Product Types:** Base designs, limited collections, custom design placeholders
- **Variants:** Metal type, gemstone, size, finishing
- **Tags & Metafields:**
  - Materials: metal, gemstone, carat, source
  - Artist attribution: artist name, bio, collection
  - Customization fields
- **Inventory Locations:** Studio, warehouse, consignment
- **Shipping Profiles:** by category (rings vs necklaces)
- **Tax Profiles:** by region
- **Discounts & Gift Cards**

### Website Configurations
- **Storefront API keys**
- **CMS content models**
- **Analytics (GA4, Meta Pixel)**
- **Customer accounts:** Shopify customer tokens

### Customization & Inventory Rules
- **Configurator rules**
  - Certain gems only valid with specific metals
  - Limited inventory of rare stones
- **Line item properties** for custom selections
- **Dynamic pricing rules** for premium metals/gems

## 6. Recommended App Stack
- **Product configurator:** Bold Product Options, Zepto, or Shopify-native custom options
- **Inventory/ERP:** Katana + QuickBooks
- **Email/SMS:** Klaviyo
- **Reviews:** Yotpo or Loox
- **Analytics:** GA4 + Shopify Analytics

## 7. Impact Program (Earnings Allocation)
- **Program fund model:** e.g., 5–10% of profits
- **Tracking:** separate ledger + quarterly reporting
- **Storytelling:** dedicated CMS section + annual report

## 8. Next Steps
1. Choose Shopify plan + headless stack
2. Build the SKU/variant strategy for metals & gems
3. Select CMS + configurator app
4. Draft the design docs above
5. Build custom design intake workflow
6. Launch MVP

---

If you want, I can also draft: system architecture diagrams, a detailed SKU/variant matrix, or a full project plan.
