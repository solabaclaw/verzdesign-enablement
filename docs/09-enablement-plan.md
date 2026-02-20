# 12-Week Shopify Plus Enablement Plan

## Program Overview

**Target:** Verz Design technical team (Singapore)  
**Duration:** 12 weeks  
**Format:** Self-paced learning + weekly sync sessions  
**Goal:** Build comprehensive Shopify Plus expertise across all core capabilities

### Success Metrics

**By end of Week 12:**
- [ ] Team can architect & build Plus stores independently
- [ ] 2+ developers certified in Shopify app development
- [ ] 1 production Shopify Function deployed
- [ ] 1 custom checkout extension live
- [ ] Successfully migrated 1 client to extensible checkout
- [ ] Built 1 custom app for internal use or client
- [ ] Documented 5+ reusable Plus patterns/templates

### Time Commitment
- **Self-study:** 6-8 hours/week
- **Hands-on exercises:** 4-6 hours/week
- **Weekly sync:** 1 hour/week
- **Total:** ~12-15 hours/week per participant

## Program Structure

### Learning Tracks

#### Track A: Foundation (Weeks 1-4)
**For:** Developers new to Shopify or Plus  
**Focus:** Plus features, architecture, Liquid basics

#### Track B: Advanced (Weeks 5-8)
**For:** Experienced Shopify developers  
**Focus:** Checkout extensibility, Functions, app development

#### Track C: Specialization (Weeks 9-12)
**For:** All participants  
**Focus:** Choose 1-2 deep-dive areas (B2B, headless, apps)

---

## Week 1: Shopify Plus Foundations

### Learning Objectives
- Understand Plus vs standard Shopify
- Know when to recommend Plus to clients
- Identify key Plus features and use cases
- Set up development environment

### Study Materials
- **Read:** [01-shopify-plus-overview.md](01-shopify-plus-overview.md)
- **Watch:** [Shopify Plus Product Tour](https://www.youtube.com/shopify) (30 min)
- **Explore:** [Shopify Plus website](https://www.shopify.com/plus)

### Hands-On Exercise
1. **Create Partner account** (if not already done)
2. **Spin up development store** (Plus plan)
3. **Explore admin:**
   - Flow workflows (3 pre-built templates)
   - LaunchPad (create test event)
   - B2B settings (enable company accounts)
4. **Document findings:** What's different from standard Shopify?

### Deliverable
**Plus Feature Comparison Matrix** (spreadsheet or doc):
- Feature list (Flow, Functions, B2B, checkout, etc.)
- Standard vs Plus availability
- Use case examples for APAC clients

### Resources
- [Shopify Partner Docs](https://shopify.dev/docs)
- [Plus Pricing](https://www.shopify.com/plus/pricing)
- [Development Store Setup](https://shopify.dev/docs/apps/tools/development-stores)

---

## Week 2: Checkout Extensibility

### Learning Objectives
- Understand extensible checkout architecture
- Build basic checkout UI extension
- Apply checkout branding via API
- Plan checkout.liquid migration strategy

### Study Materials
- **Read:** [02-checkout-extensibility.md](02-checkout-extensibility.md)
- **Watch:** [Checkout Extensibility Overview](https://www.youtube.com/shopify) (45 min)
- **Tutorial:** [Build your first checkout extension](https://shopify.dev/docs/apps/checkout/getting-started)

### Hands-On Exercise
**Build 3 checkout extensions:**

#### 1. Delivery Instructions Field
- Extension target: `purchase.checkout.delivery-address.render-after`
- Component: TextField (multiline)
- Save to cart attribute `delivery_instructions`

#### 2. Gift Message Option
- Extension target: `purchase.checkout.block.render`
- Conditional: Show only if order total >$50
- Components: Checkbox + TextField

#### 3. Upsell Banner
- Extension target: `purchase.checkout.block.render`
- Show product recommendation based on cart contents
- Add to cart functionality

### Deliverable
- **GitHub repo:** 3 working checkout extensions
- **Video demo:** 2-min walkthrough of extensions in action
- **Documentation:** Setup instructions for each extension

### Resources
- [Checkout UI Extensions API](https://shopify.dev/docs/api/checkout-ui-extensions)
- [Component Reference](https://shopify.dev/docs/api/checkout-ui-extensions/components)
- [Extension Examples](https://github.com/Shopify/checkout-ui-extensions-examples)

---

## Week 3: Shopify Functions

### Learning Objectives
- Understand Functions architecture (Wasm)
- Build discount, payment, and delivery Functions
- Migrate from Script Editor
- Deploy Functions to production

### Study Materials
- **Read:** [03-shopify-functions.md](03-shopify-functions.md)
- **Watch:** [Shopify Functions Deep Dive](https://www.youtube.com/shopify) (60 min)
- **Tutorial:** [Your first Function](https://shopify.dev/docs/api/functions/getting-started)

### Hands-On Exercise
**Build 3 Functions:**

#### 1. Tiered Discount Function
- 10% off orders $100-$200
- 15% off orders $200-$500
- 20% off orders >$500
- VIP customers: +5% extra

#### 2. Payment Method Restriction
- B2B customers: Invoice only (hide credit cards)
- International orders: Hide COD
- High-risk orders: Require manual review (hide all payments)

#### 3. Dynamic Shipping Rates
- Singapore: Free shipping >$50
- Malaysia: Free shipping >$100
- Other APAC: Flat $15
- Express option: +$10 (all regions)

### Deliverable
- **GitHub repo:** 3 production-ready Functions
- **Test cases:** Document 5+ scenarios per Function
- **Migration doc:** Script Editor → Functions conversion guide

### Resources
- [Functions API Reference](https://shopify.dev/docs/api/functions)
- [Function Examples](https://github.com/Shopify/function-examples)
- [Testing Guide](https://shopify.dev/docs/apps/functions/testing)

---

## Week 4: Shopify Flow Automation

### Learning Objectives
- Build complex Flow workflows
- Integrate third-party apps via connectors
- Use HTTP requests for custom integrations
- Optimize workflow performance

### Study Materials
- **Read:** [07-flow-automation.md](07-flow-automation.md)
- **Explore:** [Flow Template Library](https://help.shopify.com/en/manual/shopify-flow/templates)
- **Watch:** [Flow Best Practices](https://www.youtube.com/shopify) (30 min)

### Hands-On Exercise
**Build 5 production workflows:**

#### 1. VIP Customer Rewards
- Trigger: Customer lifetime spend reaches $1,000
- Actions:
  - Tag "VIP"
  - Send email with 15% discount code
  - Notify sales team (Slack/email)

#### 2. Multi-Region Order Routing
- Trigger: Order created
- Conditions: Route by shipping country
  - SG → Tag "Warehouse-SG"
  - MY/ID/TH → Tag "Warehouse-SEA"
  - Others → Tag "Warehouse-International"

#### 3. Low Inventory Alert System
- Trigger: Inventory quantity changed
- Condition: Available <10 units
- Actions:
  - Email procurement team
  - Slack #inventory channel
  - Tag product "low-stock"

#### 4. Fraud Prevention Workflow
- Trigger: Order created
- Condition: Fraud risk score >0.7 OR order total >$2,000
- Actions:
  - Tag "manual-review"
  - Hold fulfillment (via webhook to WMS)
  - Notify fraud team

#### 5. Product Review Request
- Trigger: Order fulfilled
- Wait: 10 days
- Action: Trigger Klaviyo email (review request)

### Deliverable
- **Flow documentation:** Detailed setup guide for each workflow
- **Workflow export:** JSON files for import
- **ROI analysis:** Estimated time saved per workflow

### Resources
- [Flow Documentation](https://help.shopify.com/en/manual/shopify-flow)
- [Connectors Reference](https://shopify.dev/docs/apps/flow/connectors)
- [HTTP Request Examples](https://help.shopify.com/en/manual/shopify-flow/actions/http-request)

---

## Week 5: B2B Commerce

### Learning Objectives
- Set up B2B storefront and catalogs
- Configure company accounts and payment terms
- Build hybrid D2C + B2B experience
- Customize B2B checkout flow

### Study Materials
- **Read:** [05-b2b-on-shopify.md](05-b2b-on-shopify.md)
- **Watch:** [B2B on Shopify Masterclass](https://www.youtube.com/shopify) (90 min)
- **Case study:** [B2B implementation examples](https://www.shopify.com/plus/customers)

### Hands-On Exercise
**Build complete B2B store:**

#### Setup Tasks
1. **Enable B2B:**
   - Settings → Markets → B2B
   - Create company "Acme Corp" (test buyer)
   - Set payment terms: Net 30

2. **Catalog Configuration:**
   - Create 2 catalogs:
     - "Wholesale Catalog" (25% discount)
     - "VIP Wholesale" (35% discount)
   - Assign 20 products to each
   - Set volume pricing (10+ units: -10%, 50+ units: -15%)

3. **Checkout Customization:**
   - Build Function: Hide retail payment methods for B2B
   - Build UI Extension: Add PO number field
   - Flow workflow: Route B2B orders to accounting system

4. **Company Portal:**
   - Theme customization: B2B account dashboard
   - Display: Order history, payment terms, credit limit

### Deliverable
- **Working B2B store** (dev environment)
- **Demo video:** 5-min walkthrough of B2B customer journey
- **Implementation guide:** Steps to set up B2B for client

### Resources
- [B2B Documentation](https://shopify.dev/docs/apps/b2b)
- [Company API](https://shopify.dev/docs/api/admin-graphql/latest/objects/Company)
- [B2B Liquid Objects](https://shopify.dev/docs/themes/liquid/reference/objects/company)

---

## Week 6: International Commerce & Markets

### Learning Objectives
- Configure multi-market store (APAC focus)
- Set up multi-currency and localization
- Understand Markets Pro (DDP)
- Integrate local payment methods

### Study Materials
- **Read:** [06-international-commerce.md](06-international-commerce.md)
- **Watch:** [Shopify Markets Tutorial](https://www.youtube.com/shopify) (60 min)
- **Explore:** [Markets Best Practices](https://help.shopify.com/en/manual/markets)

### Hands-On Exercise
**Configure 5-market APAC store:**

#### Markets to Configure
1. **Singapore** (primary)
   - Domain: example.com
   - Currency: SGD
   - Language: English
   - Payment: PayNow, Stripe, PayPal

2. **Malaysia**
   - Domain: example.com/my
   - Currency: MYR
   - Language: English, Malay
   - Payment: GrabPay, Touch 'n Go

3. **Indonesia**
   - Domain: example.com/id
   - Currency: IDR
   - Language: Indonesian
   - Payment: GoPay, OVO, COD

4. **Thailand**
   - Domain: example.com/th
   - Currency: THB
   - Language: Thai, English
   - Payment: PromptPay, Rabbit LINE Pay

5. **Philippines**
   - Domain: example.com/ph
   - Currency: PHP
   - Language: English
   - Payment: GCash, PayMaya, COD

#### Configuration Tasks
- **Pricing:** Set region-specific pricing (+/-10% from SGD base)
- **Shipping:** Configure rates per market
- **Tax:** Set up GST/VAT per country
- **Localization:** Translate 10 key pages per language
- **Flow:** Create workflow for multi-market order routing

### Deliverable
- **Multi-market store** (fully configured)
- **Market comparison doc:** Pricing, shipping, payment per region
- **Localization checklist:** Translation requirements for clients

### Resources
- [Shopify Markets Guide](https://help.shopify.com/en/manual/markets)
- [Multi-Currency Setup](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency)
- [Markets API](https://shopify.dev/docs/api/admin-graphql/latest/objects/Market)

---

## Week 7: Headless Commerce & Hydrogen

### Learning Objectives
- Understand headless architecture
- Build Hydrogen storefront (Remix-based)
- Use Storefront API effectively
- Deploy to Oxygen hosting

### Study Materials
- **Read:** [04-headless-hydrogen.md](04-headless-hydrogen.md)
- **Watch:** [Hydrogen Framework Overview](https://www.youtube.com/shopify) (75 min)
- **Tutorial:** [Build a Hydrogen storefront](https://shopify.dev/docs/custom-storefronts/hydrogen)

### Hands-On Exercise
**Build Hydrogen storefront:**

#### Phase 1: Setup (Day 1-2)
```bash
npm create @shopify/hydrogen@latest
cd my-hydrogen-store
npm run dev
```

- Configure Storefront API token
- Connect to dev store
- Explore demo template

#### Phase 2: Customization (Day 3-5)
Build these features:
1. **Product Listing Page:**
   - Grid layout (responsive)
   - Filtering (price, type, vendor)
   - Sorting (price, newest, best-selling)
   - Pagination

2. **Product Detail Page:**
   - Image gallery (4+ images)
   - Variant selector (size, color)
   - Add to cart
   - Related products

3. **Cart Experience:**
   - Slide-out cart drawer
   - Quantity adjustment
   - Cart upsells
   - Checkout redirect

4. **Multi-Market Support:**
   - Country selector
   - Currency switching
   - Localized content

#### Phase 3: Deployment (Day 6-7)
- Deploy to Oxygen
- Configure custom domain
- Set up analytics
- Performance optimization (Lighthouse score >90)

### Deliverable
- **Live Hydrogen store** (deployed to Oxygen)
- **GitHub repo:** Source code with documentation
- **Performance report:** Lighthouse scores + optimizations applied

### Resources
- [Hydrogen Docs](https://shopify.dev/docs/custom-storefronts/hydrogen)
- [Storefront API](https://shopify.dev/docs/api/storefront)
- [Oxygen Hosting](https://shopify.dev/docs/custom-storefronts/oxygen)

---

## Week 8: App Development Fundamentals

### Learning Objectives
- Set up Shopify CLI development environment
- Build embedded admin app (Remix)
- Use Admin API (GraphQL)
- Deploy app to production

### Study Materials
- **Read:** [08-app-development.md](08-app-development.md)
- **Watch:** [Build a Shopify App (Full Course)](https://www.youtube.com/shopify) (2 hours)
- **Tutorial:** [Your first app](https://shopify.dev/docs/apps/getting-started)

### Hands-On Exercise
**Build complete admin app:**

#### App: "Bulk Product Tagger"

**Features:**
1. **Dashboard Page:**
   - Display product count
   - Show top 10 tags
   - Quick actions (tag all, untag all)

2. **Bulk Tag Page:**
   - Product table (title, vendor, existing tags)
   - Checkbox selection
   - Bulk add/remove tags
   - Search & filter

3. **Tag Rules Page:**
   - Create auto-tag rules
   - Examples:
     - Price >$100 → tag "premium"
     - Vendor = "Nike" → tag "brand-nike"
     - Type = "Shoes" → tag "footwear"
   - Save rules to database
   - Apply rules on-demand or via webhook

4. **Settings Page:**
   - Configure tag prefixes
   - Set default tags for new products
   - Enable/disable auto-tagging

#### Technical Requirements
- **Framework:** Remix
- **UI:** Polaris components
- **Database:** SQLite (dev), PostgreSQL (prod)
- **API:** GraphQL Admin API
- **Webhooks:** `products/create`, `products/update`
- **Deployment:** Vercel or Render

### Deliverable
- **Working app** (deployed and installable)
- **GitHub repo:** Source code + README
- **Demo video:** 5-min feature walkthrough
- **Documentation:** Setup guide + API reference

### Resources
- [Shopify CLI](https://shopify.dev/docs/apps/tools/cli)
- [Admin API Reference](https://shopify.dev/docs/api/admin-graphql)
- [Polaris Components](https://polaris.shopify.com/components)
- [App Examples](https://github.com/Shopify/shopify-app-examples)

---

## Week 9-10: Specialization Track (Choose One)

### Option A: Advanced B2B Solutions

**Project:** Build end-to-end B2B platform

**Features:**
- Multi-tier pricing (Gold/Silver/Bronze customers)
- Quote request system (admin app)
- Custom order forms (theme extension)
- Account rep assignment (Flow + app)
- Purchase order approval workflow

**Deliverables:**
- Working B2B store with custom features
- Admin app for quote management
- Flow workflows for approval chains
- Documentation + client pitch deck

---

### Option B: Headless & Mobile Commerce

**Project:** Build mobile app + headless admin

**Stack:**
- Mobile: React Native (iOS/Android)
- Admin: Custom dashboard (Next.js)
- APIs: Storefront + Admin GraphQL

**Features:**
- Native mobile shopping experience
- Push notifications (order updates)
- Biometric login
- Custom admin analytics dashboard

**Deliverables:**
- Deployed mobile app (TestFlight/Play Store)
- Admin dashboard (deployed)
- API integration guide

---

### Option C: Marketplace Platform

**Project:** Multi-vendor marketplace on Shopify Plus

**Features:**
- Vendor registration portal (custom app)
- Vendor dashboard (product management)
- Commission tracking (Flow + app)
- Automated payouts (integration with payment provider)
- Vendor analytics

**Deliverables:**
- Marketplace app (multi-tenant)
- Vendor onboarding flow
- Commission calculation engine
- Admin reporting dashboard

---

## Week 11: Optimization & Performance

### Learning Objectives
- Optimize theme performance (Core Web Vitals)
- Reduce API call overhead
- Implement caching strategies
- Monitor and debug production issues

### Study Materials
- **Read:** [Performance Best Practices](https://shopify.dev/docs/themes/best-practices/performance)
- **Watch:** [Shopify Performance Masterclass](https://www.youtube.com/shopify) (60 min)
- **Tool:** [Shopify Theme Inspector](https://shopify.dev/docs/themes/tools/theme-inspector)

### Hands-On Exercise
**Optimize existing project:**

#### Performance Audit
1. **Run Lighthouse** on your Week 7 Hydrogen store
   - Target: >90 Performance score
   - Identify bottlenecks

2. **Optimize:**
   - Image lazy loading
   - Code splitting
   - Reduce bundle size
   - Implement CDN caching
   - Defer non-critical JS

3. **API Optimization:**
   - Batch GraphQL queries
   - Implement query caching
   - Use pagination (avoid fetching all data)
   - Optimize webhook processing

### Deliverable
- **Before/after performance report:**
  - Lighthouse scores
  - Load times (3G/4G/WiFi)
  - API call counts
  - Bundle sizes
- **Optimization guide:** Checklist for future projects

### Resources
- [Web.dev Performance](https://web.dev/performance/)
- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
- [Shopify Performance Docs](https://shopify.dev/docs/themes/best-practices/performance)

---

## Week 12: Capstone Project

### Project: Build Production-Ready Plus Store

**Client Brief (Simulated):**

**Company:** "LUXE Watches" (Singapore-based luxury watch retailer)

**Requirements:**
- **Multi-market:** SG, HK, MY (3 markets)
- **B2B + D2C:** Wholesale for authorized dealers
- **Checkout:** Custom engraving field + gift wrap option
- **Functions:** Hide COD for orders >$5,000
- **Flow:** VIP tagging (spend >$10k), low inventory alerts
- **App:** Custom warranty registration portal
- **Performance:** <2s load time, Lighthouse >85

**Deliverables:**
1. **Fully configured Plus store:**
   - Theme (Dawn customized or custom)
   - Markets setup (3 regions)
   - B2B enabled
   - Products (20+ sample products)

2. **Custom extensions:**
   - 2+ checkout UI extensions
   - 2+ Shopify Functions
   - 1 theme app extension (warranty registration)

3. **Automation:**
   - 3+ Flow workflows
   - Webhook integrations

4. **Documentation:**
   - **Store setup guide** (20+ pages)
   - **Developer documentation** (API reference, architecture)
   - **Client handover doc** (how to manage the store)

5. **Presentation:**
   - **15-min video demo** (full customer journey + admin tour)
   - **Pitch deck** (technical capabilities showcase)

### Evaluation Criteria
- **Functionality:** All requirements met (40%)
- **Code quality:** Clean, documented, best practices (20%)
- **Performance:** Lighthouse scores, load times (15%)
- **Documentation:** Comprehensive, clear, client-ready (15%)
- **Presentation:** Professional, well-organized (10%)

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

### Verz Design Internal Recognition
- **Plus Developer Badge** (internal recognition program)
- **Showcase projects** on company website/portfolio
- **Knowledge sharing:** Present learnings to broader team

---

## Post-Enablement Support

### Ongoing Resources
- **Monthly Plus updates webinar** (hosted by PSE)
- **Quarterly deep-dive sessions** (new features, beta programs)
- **Private Slack channel** (Verz Design + Shopify PSE)
- **Access to beta programs** (early access to new features)

### Success Metrics (6 months post-enablement)
- [ ] 5+ Plus stores launched by Verz Design
- [ ] 3+ custom apps built for clients
- [ ] Zero checkout.liquid migrations (all extensible checkout)
- [ ] 10+ reusable Plus components/templates library
- [ ] 2+ case studies published

---

## Additional Resources

- [Shopify Partner Academy](https://partner-training.shopify.com/)
- [Shopify Dev Discord](https://discord.gg/shopifydevs)
- [Plus Partner Slack](https://shopify-partners.slack.com)
- [GitHub: Shopify Examples](https://github.com/Shopify)

---

**Next:** [Resources & References →](10-resources.md)
