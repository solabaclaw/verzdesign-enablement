# 12-Week AI + Shopify Plus Enablement Plan

## Program Overview

**Target:** Verz Design technical team (Singapore)
**Duration:** 12 weeks
**Format:** Self-paced learning + weekly sync sessions + hands-on AI workshops
**Goal:** Build comprehensive Shopify Plus + AI expertise for Southeast Asian enterprise commerce

### Success Metrics

**By end of Week 12:**
- [ ] Team can architect & build AI-enabled Plus stores independently
- [ ] 2+ developers certified in Shopify app development
- [ ] 1 production Shopify Function deployed
- [ ] 1 custom checkout extension live
- [ ] Successfully migrated 1 client to extensible checkout
- [ ] Built 1 custom app for internal use or client
- [ ] Documented 5+ reusable Plus patterns/templates
- [ ] All AI features (Magic, Sidekick, Audiences, semantic search) configured on at least 1 client store
- [ ] AI-powered content generation workflow established (product descriptions, SEO, multilingual)
- [ ] 3+ Shopify Flow AI automations deployed for a SEA client

### Time Commitment
- **Self-study:** 6-8 hours/week
- **Hands-on exercises:** 4-6 hours/week
- **Weekly sync:** 1 hour/week
- **Total:** ~12-15 hours/week per participant

---

## Pre-Implementation: Discovery & AI Readiness Audit (Week 0)

Before the 12-week program begins, conduct a baseline audit for the pilot client store.

### Activities

| **Task** | **Owner** | **Output** |
|----------|----------|-----------|
| Audit current store (theme, apps, integrations) | Verz Tech Lead | Store audit document |
| Review product catalog quality (images, descriptions, tags) | Verz Content | Catalog quality scorecard |
| Analyze current analytics (CVR, AOV, traffic sources) | Verz Analytics | Baseline metrics report |
| Identify top 3 business pain points | Client + Verz PM | Priority alignment doc |
| Document current tech stack (ERP, CRM, POS, marketing tools) | Client IT | Integration map |
| Define success metrics and KPIs | Verz PM + Client | KPI agreement |

### Baseline Metrics to Capture

```
Record current state:
- Conversion rate (overall + by channel)
- Average order value
- Search conversion rate
- Zero-result search percentage
- Content production time (products/week)
- Customer acquisition cost (by channel)
- Customer retention rate (30/60/90 day)
- Revenue per session
- Support ticket volume
- Time to launch new products
```

### Go/No-Go Checklist

```
☐ Shopify Plus plan active
☐ At least 200 products in catalog
☐ Minimum 5,000 monthly sessions (for meaningful AI training)
☐ Product images available (minimum 1 per product)
☐ Client team trained on Shopify Admin basics
☐ Integration requirements documented
☐ Budget and timeline approved
☐ Success metrics agreed
```

---

## Phase 1: Foundation + AI Quick Wins (Weeks 1-4)

### Week 1: Shopify Plus Foundations + AI Overview

#### Learning Objectives
- Understand Plus vs standard Shopify
- Know when to recommend Plus to SEA clients
- Understand Shopify's AI strategy and feature landscape
- Set up development environment with AI features enabled

#### Study Materials
- **Read:** [01-shopify-plus-overview.md](01-shopify-plus-overview.md)
- **Watch:** [Shopify Plus Product Tour](https://www.youtube.com/shopify) (30 min)
- **Explore:** [Shopify Plus website](https://www.shopify.com/plus)
- **Read:** Shopify Editions announcements (AI features)

#### Hands-On Exercise
1. **Create Partner account** (if not already done)
2. **Spin up development store** (Plus plan)
3. **Explore admin AI features:**
   - Shopify Magic — generate 10 product descriptions, test brand voice configuration
   - Sidekick — run 10 analytics queries (top sellers, conversion trends, customer segments)
   - Flow workflows (3 pre-built templates)
   - LaunchPad (create test event)
   - B2B settings (enable company accounts)
4. **Document findings:** What AI features are available? What requires Plus?

#### Deliverable
- **Plus + AI Feature Comparison Matrix** (spreadsheet or doc):
  - Feature list (Magic, Sidekick, Audiences, Flow, Functions, B2B, checkout, etc.)
  - Standard vs Plus availability
  - AI capability per feature
  - Use case examples for SEA clients

#### Resources
- [Shopify Partner Docs](https://shopify.dev/docs)
- [Shopify AI & Magic documentation](https://help.shopify.com/en/manual/shopify-magic)
- [Development Store Setup](https://shopify.dev/docs/apps/tools/development-stores)

---

### Week 2: Checkout Extensibility + AI Content Generation

#### Learning Objectives
- Understand extensible checkout architecture
- Build basic checkout UI extension
- Master Shopify Magic for content generation
- Set up AI-powered SEO metadata workflow

#### Study Materials
- **Read:** [02-checkout-extensibility.md](02-checkout-extensibility.md)
- **Tutorial:** [Build your first checkout extension](https://shopify.dev/docs/apps/checkout/getting-started)
- **Explore:** Shopify Magic brand voice settings

#### Hands-On Exercise

**Part A: Checkout Extensions (same as before)**
Build 3 checkout extensions:
1. **Delivery Instructions Field** — `purchase.checkout.delivery-address.render-after`
2. **Gift Message Option** — conditional on order total >$50
3. **Upsell Banner** — AI-powered product recommendation in checkout

**Part B: AI Content Workshop (NEW)**
1. **Configure brand voice:**
   - Upload 20-30 sample product descriptions from a SEA client
   - Set tone preferences (professional, friendly, luxury — match brand)
   - Define prohibited words/claims
2. **Generate AI content for 50 products:**
   - Product descriptions (English)
   - SEO meta titles and descriptions
   - Alt text for product images
3. **Multilingual content generation:**
   - Generate descriptions in Malay, Thai, or Bahasa Indonesia
   - Have native speakers QA the first 20 translations
4. **Image optimization:**
   - Batch background removal using Shopify Magic
   - Image enhancement for consistency

#### Deliverable
- **GitHub repo:** 3 working checkout extensions
- **Content workflow doc:** Step-by-step AI content generation SOP for SEA markets
- **Before/after:** Content production time comparison (manual vs AI-assisted)

---

### Week 3: Shopify Functions + AI Search & Discovery

#### Learning Objectives
- Understand Functions architecture (Wasm)
- Build discount, payment, and delivery Functions
- Activate and configure AI-powered semantic search
- Set up AI product recommendations

#### Study Materials
- **Read:** [03-shopify-functions.md](03-shopify-functions.md)
- **Tutorial:** [Your first Function](https://shopify.dev/docs/api/functions/getting-started)
- **Explore:** [Search & Discovery app](https://apps.shopify.com/search-and-discovery)

#### Hands-On Exercise

**Part A: Shopify Functions**
Build 3 Functions:
1. **Tiered Discount Function** — 10%/$100+, 15%/$200+, 20%/$500+, VIP +5%
2. **Payment Method Restriction** — B2B: invoice only; International: hide COD; High-risk: manual review
3. **Dynamic Shipping Rates (SEA):**
   - Singapore: Free shipping >S$50
   - Malaysia: Free shipping >RM100
   - Indonesia: Free shipping >IDR 500,000
   - Thailand: Free shipping >฿1,000
   - Philippines: Free shipping >₱2,000
   - Express option: +$10 equivalent (all regions)

**Part B: AI Search & Discovery (NEW)**
1. **Enable AI-powered semantic search:**
   - Configure synonym management (add SEA-specific synonyms)
   - Set up multilingual search (English, Malay, Thai, Bahasa)
   - Configure autocomplete and search filters
2. **Test with 50 common queries** across languages
3. **Set up AI product recommendations:**
   - Homepage: "Recommended for You" / "Trending Now"
   - Product page: "You May Also Like" + "Frequently Bought Together"
   - Cart page: "Complete Your Order"
   - Thank you page: "Customers Also Bought"
4. **Configure Smart Collections:**
   - "Trending Now" (auto-populated by AI)
   - "New Arrivals" (automated)
   - "Best Sellers" (by category)

#### Deliverable
- **GitHub repo:** 3 production-ready Functions with SEA shipping logic
- **Search configuration doc:** Semantic search setup + synonym list for SEA markets
- **Recommendation widget screenshots:** All placements configured and tested

---

### Week 4: Shopify Flow + AI Automation

#### Learning Objectives
- Build complex Flow workflows
- Integrate Shopify AI features into automation
- Use Sidekick for operational efficiency
- Configure Shopify Audiences for SEA ad campaigns

#### Study Materials
- **Read:** [07-flow-automation.md](07-flow-automation.md)
- **Explore:** [Flow Template Library](https://help.shopify.com/en/manual/shopify-flow/templates)

#### Hands-On Exercise

**Build 7 production workflows (enhanced with AI):**

1. **VIP Customer Rewards**
   - Trigger: Customer lifetime spend reaches S$1,000
   - Actions: Tag "VIP", send email with 15% discount, notify sales team

2. **Multi-Region SEA Order Routing**
   - Trigger: Order created
   - SG → Tag "Warehouse-SG"
   - MY/ID/TH → Tag "Warehouse-SEA"
   - PH/VN → Tag "Warehouse-PH"
   - Others → Tag "Warehouse-International"

3. **Low Inventory Alert System**
   - Trigger: Inventory <10 units
   - Actions: Email procurement, Slack alert, tag "low-stock", remove from ads

4. **Fraud Prevention Workflow**
   - Trigger: Fraud risk >0.7 OR order >S$2,000
   - Actions: Tag "manual-review", hold fulfillment, notify team

5. **AI-Powered Win-Back Campaign (NEW)**
   - Trigger: No purchase in 90 days AND customer has 2+ orders
   - Actions: Add to win-back segment, trigger personalized email, generate discount code

6. **Customer Segmentation Automation (NEW)**
   - Trigger: Order created
   - Conditions: Segment by purchase behavior
   - Actions: Tag price-tier (budget/mid/premium), category affinity, region

7. **Product Review Follow-Up (NEW)**
   - Trigger: Order fulfilled
   - Wait: 7 days
   - Action: Send review request with product images + 10% next-purchase incentive

**Sidekick Training Session:**
- Run 20 Sidekick queries relevant to SEA operations
- Document top query templates for the Verz team

**Shopify Audiences Setup (NEW):**
1. Enable Shopify Audiences
2. Connect Meta Ads and Google Ads
3. Generate first audience lists (prospecting, retargeting, category intent)
4. Launch test campaigns: Audiences vs standard targeting
5. Track CPA and ROAS comparison

#### Deliverable
- **Flow documentation:** Setup guide for all 7 workflows
- **Sidekick query library:** 20+ queries tailored for SEA merchants
- **Audiences report:** Initial performance comparison (week 1 data)

---

## Phase 2: Advanced + AI Deep Dives (Weeks 5-8)

### Week 5: B2B Commerce + AI Personalization

#### Learning Objectives
- Set up B2B storefront and catalogs
- Configure AI-powered personalization engine
- Build hybrid D2C + B2B experience for SEA wholesale

#### Study Materials
- **Read:** [05-b2b-on-shopify.md](05-b2b-on-shopify.md)
- **Case study:** B2B implementation examples for APAC

#### Hands-On Exercise

**Part A: B2B Setup**
1. Enable B2B, create test company, set payment terms (Net 30)
2. Create 2 catalogs: "Wholesale" (25% off) + "VIP Wholesale" (35% off)
3. Volume pricing: 10+ units: -10%, 50+ units: -15%
4. Checkout customization: Hide retail payments for B2B, add PO number field
5. Build B2B account dashboard in theme

**Part B: AI Personalization Engine (NEW)**
1. **Configure homepage personalization:**
   - Returning customer → personalized recommendations
   - New visitor → geo-based trending products (detect SG/MY/ID/TH/PH)
   - Referral source → match intent (ad click → show ad product category)
2. **Enable search personalization:** Results ranked by customer profile
3. **Configure email personalization:**
   - Browse abandonment with viewed products
   - Cart abandonment with cart items + AI recommendations
   - Post-purchase with complementary products
4. **Set up customer segments:**
   - Price tier segments (budget/mid/premium)
   - Category affinity segments
   - Regional segments (SG/MY/ID/TH/PH)
   - Lifecycle segments (new/active/at-risk/churned)

#### Deliverable
- **Working B2B store** with AI personalization
- **Personalization configuration doc:** Rules, segments, and expected impact
- **Demo video:** 5-min walkthrough of personalized B2B + D2C journey

---

### Week 6: International Commerce & Markets (SEA Focus)

#### Learning Objectives
- Configure multi-market store for Southeast Asia
- Set up multi-currency and localization with AI translation
- Integrate local SEA payment methods
- Use AI for multilingual content at scale

#### Study Materials
- **Read:** [06-international-commerce.md](06-international-commerce.md)
- **Explore:** [Markets Best Practices](https://help.shopify.com/en/manual/markets)

#### Hands-On Exercise

**Configure 5-market SEA store:**

1. **Singapore** (primary) — SGD, English, PayNow/Stripe/GrabPay
2. **Malaysia** — MYR, English/Malay, GrabPay/Touch 'n Go/Boost
3. **Indonesia** — IDR, Indonesian, GoPay/OVO/DANA/ShopeePay/COD
4. **Thailand** — THB, Thai/English, PromptPay/Rabbit LINE Pay/TrueMoney
5. **Philippines** — PHP, English, GCash/PayMaya/COD

**AI-Enhanced Localization (NEW):**
- Use Shopify Magic to generate product descriptions per market language
- Native speaker QA on first 50 translations per language
- Generate collection descriptions for all markets
- Set up AI-powered search with multilingual synonyms per market

#### Deliverable
- **Multi-market store** (fully configured, 5 SEA markets)
- **AI translation workflow:** Process for scaling multilingual content
- **Market comparison doc:** Pricing, shipping, payment, language per region

---

### Week 7: Headless Commerce & Hydrogen

#### Learning Objectives
- Understand headless architecture
- Build Hydrogen storefront with AI recommendation widgets
- Use Storefront API for AI-powered product discovery
- Deploy to Oxygen hosting

#### Study Materials
- **Read:** [04-headless-hydrogen.md](04-headless-hydrogen.md)
- **Tutorial:** [Build a Hydrogen storefront](https://shopify.dev/docs/custom-storefronts/hydrogen)

#### Hands-On Exercise

**Build Hydrogen storefront with AI features:**

1. **Setup** — scaffold Hydrogen app, connect to dev store
2. **Product Listing Page** — grid, filtering, sorting, pagination
3. **Product Detail Page** — gallery, variants, add-to-cart, **AI recommendations widget**
4. **Cart Experience** — slide-out drawer, quantity adjust, **AI-powered upsells**
5. **Multi-Market Support** — country selector, currency switching, localized content
6. **AI Integration (NEW):**
   - Integrate Storefront API `productRecommendations` query
   - Implement `predictiveSearch` for semantic search
   - Add personalized homepage section based on customer data
7. **Deploy to Oxygen** — custom domain, analytics, Lighthouse >90

#### Deliverable
- **Live Hydrogen store** with AI features (deployed to Oxygen)
- **GitHub repo:** Source code with AI integration documented
- **Performance report:** Lighthouse scores + AI feature load impact

---

### Week 8: App Development + AI Integration

#### Learning Objectives
- Build embedded admin app with AI capabilities
- Use Admin API (GraphQL) for AI-enhanced operations
- Integrate Shopify Magic and Sidekick patterns into custom apps

#### Study Materials
- **Read:** [08-app-development.md](08-app-development.md)
- **Tutorial:** [Your first app](https://shopify.dev/docs/apps/getting-started)

#### Hands-On Exercise

**Build: "Smart Product Manager" app (AI-enhanced)**

**Features:**
1. **Dashboard** — product count, top tags, AI-generated insights
2. **Bulk AI Content Generator (NEW):**
   - Select products → bulk generate descriptions via Shopify Magic
   - Generate SEO metadata in batch
   - Generate multilingual descriptions (English + 1 SEA language)
   - Review/approve workflow before publishing
3. **Smart Tag Rules:**
   - AI-suggested auto-tag rules based on product attributes
   - Price >$100 → "premium"; Vendor = "Nike" → "brand-nike"
   - Apply rules on-demand or via webhook
4. **Analytics Dashboard (NEW):**
   - AI recommendation performance (CTR, conversion by widget)
   - Search analytics (top queries, zero-result rate)
   - Content generation metrics (AI vs manual)

#### Technical Requirements
- Framework: Remix | UI: Polaris | DB: SQLite (dev), PostgreSQL (prod)
- API: GraphQL Admin API | Webhooks: `products/create`, `products/update`

#### Deliverable
- **Working app** (deployed and installable)
- **GitHub repo:** Source code + README
- **Demo video:** 5-min feature walkthrough with AI capabilities

---

## Phase 3: Specialization + AI Mastery (Weeks 9-12)

### Week 9-10: AI Specialization Track (Choose One)

#### Option A: AI-Powered B2B Platform for SEA

**Project:** End-to-end B2B platform with AI features

**Features:**
- Multi-tier AI-suggested pricing (Gold/Silver/Bronze)
- Quote request system with AI pricing recommendations
- AI-powered reorder suggestions based on purchase cycles
- Automated B2B customer segmentation
- Flow: auto-approval workflows based on credit scoring

**Deliverables:**
- Working B2B store with AI features
- Admin app for AI-assisted quote management
- Flow workflows for approval chains
- Documentation + client pitch deck for SEA wholesale market

---

#### Option B: AI-First Headless Mobile Commerce

**Project:** Mobile app + AI-powered headless admin

**Stack:** React Native (iOS/Android) + Next.js admin + Storefront API

**Features:**
- Native mobile shopping with AI recommendations
- AI-powered push notifications (personalized offers based on behavior)
- Semantic search in mobile app
- Custom admin dashboard with AI analytics (recommendation performance, search insights)

**Deliverables:**
- Deployed mobile app (TestFlight/Play Store)
- Admin dashboard with AI metrics
- API integration guide

---

#### Option C: AI-Enhanced Marketplace Platform

**Project:** Multi-vendor marketplace on Shopify Plus

**Features:**
- Vendor registration portal
- AI-powered product categorization and tagging for vendor uploads
- AI recommendation engine across vendors
- Commission tracking with Flow automation
- Automated vendor analytics (AI-generated insights)

**Deliverables:**
- Marketplace app with AI features
- Vendor onboarding flow
- AI-powered product discovery across vendors
- Admin reporting dashboard

---

### Week 11: Optimization, Performance & AI Tuning

#### Learning Objectives
- Optimize theme performance (Core Web Vitals)
- Tune AI features for maximum impact
- Measure and report on AI ROI
- Reduce API call overhead with caching

#### Hands-On Exercise

**Performance Audit:**
1. Run Lighthouse on Week 7 Hydrogen store → target >90
2. Optimize: image lazy loading, code splitting, CDN caching, defer non-critical JS

**AI Performance Review (NEW):**
1. **Search analytics review:**
   - Top zero-result queries → add products or synonyms
   - Low-converting search terms → improve product data
   - High-traffic queries → create dedicated landing pages
2. **Recommendation performance:**
   - CTR by widget placement (PDP, cart, homepage)
   - AOV impact of recommendation widgets
   - A/B test recommendation strategies
3. **Content generation audit:**
   - AI-generated vs manual description conversion rates
   - SEO ranking changes post-AI content
   - Multilingual content quality scores
4. **Audiences performance:**
   - CPA comparison: Audiences vs standard targeting
   - ROAS by audience type
   - Optimization recommendations

#### Deliverable
- **AI Performance Report:**
  - Search: conversion rate, zero-result %, top queries
  - Recommendations: CTR, conversion, AOV lift
  - Content: production time savings, quality metrics
  - Audiences: CPA, ROAS, audience size
- **Optimization playbook:** Checklist for ongoing AI tuning

---

### Week 12: Capstone Project — AI-Powered Plus Store for SEA

#### Project Brief (Simulated)

**Company:** "LUXE Watches" (Singapore-based luxury watch retailer, SEA expansion)

**Requirements:**
- **Multi-market:** SG, MY, TH (3 SEA markets)
- **B2B + D2C:** Wholesale for authorized dealers
- **AI Features (NEW):**
  - Shopify Magic: AI-generated descriptions in English, Malay, Thai
  - Semantic search with multilingual support
  - AI recommendations on all key pages
  - Shopify Audiences campaigns (Meta + Google)
  - Sidekick trained for merchant operations
  - Personalization: geo-based, behavior-based, lifecycle-based
- **Checkout:** Custom engraving field + gift wrap option
- **Functions:** Hide COD for orders >S$5,000
- **Flow:** VIP tagging (spend >S$10k), low inventory alerts, win-back automation, customer segmentation
- **App:** Custom warranty registration portal
- **Performance:** <2s load time, Lighthouse >85

#### Deliverables

1. **Fully configured Plus store:**
   - Theme (Dawn customized or custom)
   - 3 SEA markets with local payments
   - B2B enabled
   - All AI features activated and configured
   - Products (20+ with AI-generated content in 3 languages)

2. **Custom extensions:**
   - 2+ checkout UI extensions
   - 2+ Shopify Functions (SEA shipping + payment logic)
   - 1 theme app extension (warranty registration)

3. **AI configuration:**
   - Semantic search with SEA synonyms
   - Recommendation widgets on all key pages
   - Shopify Audiences with 2+ active campaigns
   - Personalization rules (3+ customer segments)
   - 5+ Flow automations (including AI-enhanced)

4. **Documentation:**
   - **Store setup guide** (20+ pages, includes AI feature setup)
   - **AI playbook** (configuration, tuning, measurement)
   - **Client handover doc** (how to manage store + AI features)

5. **Presentation:**
   - **15-min video demo** (full customer journey + AI features + admin tour)
   - **Pitch deck** (AI + Shopify Plus capabilities for SEA market)
   - **ROI projection** (expected impact of AI features based on benchmarks)

#### Evaluation Criteria
- **Functionality:** All requirements met, AI features working (35%)
- **AI Integration:** Proper configuration, measurable impact (20%)
- **Code quality:** Clean, documented, best practices (15%)
- **Performance:** Lighthouse scores, load times (10%)
- **Documentation:** Comprehensive, client-ready, AI playbook (10%)
- **Presentation:** Professional, compelling AI narrative (10%)

---

## Weekly Sync Sessions

### Format
- **Duration:** 60 minutes
- **Attendees:** Verz Design team + Partner Solutions Engineer
- **Structure:**
  - 10 min: Recap previous week's learnings
  - 20 min: Demo deliverables (team shows work)
  - 20 min: Q&A / troubleshooting
  - 10 min: Preview next week's content

### AI Workshop Sessions (NEW — Biweekly)
- **Duration:** 90 minutes (Weeks 2, 4, 6, 8, 10, 12)
- **Format:** Hands-on, screen-share, build together
- **Topics rotate:** Content generation → Search tuning → Personalization → Audiences → Flow automation → Capstone review

### Office Hours
- **Ad-hoc support:** Slack channel or email
- **Response time:** <24 hours for questions
- **Code reviews:** Submit PRs for feedback

---

## Certification & Recognition

### Shopify Certifications
Upon completion, team members should pursue:
- **Shopify App Development** (official certification)
- **Shopify Theme Development** (official certification)
- **Shopify Plus Foundations** (partner certification)

### AI & Commerce Certifications (NEW)
- **Google AI Essentials** (Coursera) — 10 hours, AI fundamentals
- **AI for Everyone** (Andrew Ng, Coursera) — 6 hours, non-technical AI strategy
- **Google Analytics 4 Certification** — Free, essential for measuring AI impact
- **Meta Blueprint Certification** — For Shopify Audiences campaign optimization

### Verz Design Internal Recognition
- **Plus + AI Developer Badge** (internal recognition program)
- **Showcase AI-powered projects** on company website/portfolio
- **Knowledge sharing:** Present AI learnings to broader team
- **SEA AI case studies:** Document client wins for business development

---

## Post-Enablement Support

### Ongoing Resources
- **Monthly Plus + AI updates webinar** (hosted by PSE)
- **Quarterly deep-dive sessions** (new AI features, beta programs)
- **Private Slack channel** (Verz Design + Shopify PSE)
- **Access to beta programs** (early access to new AI features)

### Success Metrics (6 months post-enablement)
- [ ] 5+ AI-enabled Plus stores launched by Verz Design
- [ ] 3+ custom apps built with AI integration
- [ ] Zero checkout.liquid migrations (all extensible checkout)
- [ ] 10+ reusable Plus + AI components/templates library
- [ ] 2+ AI case studies published (SEA market focus)
- [ ] Measurable client ROI: search CVR +30%, AOV +15%, content production 5x faster
- [ ] Shopify Audiences deployed for 3+ clients with ROAS improvement documented

### 90-Day AI Optimization Roadmap (Post-Enablement)

**Month 1 (Weeks 13-16): Monitor & Adjust**
- Weekly: Review search analytics, recommendation performance, Flow errors
- Adjust personalization rules based on data
- Optimize Audiences campaigns (CPA, ROAS)
- Verz involvement: 4-6 hours/week

**Month 2 (Weeks 17-20): Optimize & Expand**
- A/B test recommendation strategies
- Expand multilingual content (add Vietnamese, Khmer)
- Optimize Flow automations based on edge cases
- Advanced customer segmentation refinement
- Verz involvement: 2-4 hours/week

**Month 3 (Weeks 21-24): Scale & Automate**
- Scale high-ROI AI features across client portfolio
- Automate remaining manual processes
- Prepare seasonal campaign AI playbooks (11.11, 12.12, CNY, Hari Raya)
- Strategic review: Next phase planning
- Verz involvement: 2-3 hours/week

---

## Expected AI Impact Timeline

```
Week 1-4: Foundation + Quick Wins
├── Content production: 80% time reduction (immediate)
├── SEO: Metadata complete (impact in 8-12 weeks)
├── Search: Semantic search active, zero-result rate dropping
└── Customer data: Segmentation begins

Week 5-8: Advanced + AI Deep Dives
├── Search conversion: +30-50% (semantic search)
├── AOV: +10-15% (recommendation widgets)
├── Email conversion: +50-80% (personalization)
└── Ad efficiency: +30-50% ROAS (Audiences)

Week 9-12: Specialization + AI Mastery
├── Team efficiency: +40% (Sidekick + automation)
├── Full AI stack deployed on capstone project
├── Client-ready AI playbooks documented
└── Overall CVR: +50-100% vs baseline

Month 3+: Compounding Returns
├── Organic traffic: +40-60% (SEO compound effect)
├── Customer retention: +25-35% (personalization)
├── CLV: +30-50% (better segmentation + lifecycle marketing)
└── AI features improve as more data flows through
```

---

**Next:** [Resources & References →](10-resources.md)
