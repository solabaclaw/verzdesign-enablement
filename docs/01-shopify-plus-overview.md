# Shopify Plus Overview

## What is Shopify Plus?

Shopify Plus is the enterprise commerce platform designed for high-volume merchants and scaling brands. It provides advanced features, dedicated support, and infrastructure that can handle enterprise-level traffic and complexity.

## Plus vs Standard Shopify

### Standard Shopify
- Great for small to medium businesses
- Standard checkout (limited customization)
- Basic automation via third-party apps
- Shared infrastructure resources
- Standard API rate limits

### Shopify Plus
- Enterprise-grade infrastructure (10,000+ orders/min capacity)
- **Checkout Extensibility** - full checkout customization
- **Shopify Functions** - serverless customization layer
- **B2B Commerce** - native wholesale capabilities
- **Shopify Flow** - advanced automation (included)
- **LaunchPad** - automated merchandising & flash sales
- **Higher API rate limits** (4x standard)
- **Expansion stores** - 9 additional stores included
- **Plus Partner & Merchant Success Manager**
- **Script Editor** (legacy, migrating to Functions)

## Key Plus Features (2025-2026)

### 1. Shopify Functions
**Wasm-based serverless customization layer**

- **Discount Functions** - custom pricing logic
- **Payment Customization** - hide/reorder/rename payment methods
- **Delivery Customization** - custom shipping logic
- **Cart & Checkout Validation** - business rules enforcement
- Runs on Shopify infrastructure (no hosting needed)
- Replaces legacy Script Editor
- Performance: <50ms execution time

### 2. Checkout Extensibility
**Modern, customizable checkout**

- **Checkout UI Extensions** - add custom fields, upsells, validations
- **Checkout Branding API** - match brand design system
- **App blocks** - integrate third-party apps directly in checkout
- Migration from checkout.liquid required by August 2025
- Better performance, security, and conversion rates

### 3. B2B Commerce
**Native wholesale built into Shopify Plus**

- Company accounts with multiple buyers
- Custom price lists per customer segment
- Payment terms (net 15, 30, 60, 90)
- Self-serve company registration
- B2B + D2C on same store
- Purchase orders & invoice payments

### 4. Shopify Audiences
**Privacy-first customer targeting**

- Export high-intent audiences to Meta, Google, TikTok
- Privacy-compliant (no PII shared)
- Lookalike audience generation
- Improves ROAS by 2-3x on average

### 5. LaunchPad
**Automated event merchandising**

- Schedule sales, product drops, password removal
- Theme publishing automation
- Inventory & pricing updates
- Perfect for flash sales, Black Friday, product launches

### 6. Shopify Flow
**Visual workflow automation**

- 100+ pre-built templates
- Custom triggers: order, customer, inventory, fraud
- Actions: tag, notify, hide, fulfill, update
- Integrations with 200+ apps
- No-code automation builder

### 7. Script Editor (Legacy)
**Ruby-based customization (being phased out)**

- Line item scripts (discounts)
- Shipping scripts (custom rates)
- Payment scripts (hide methods)
- **Migration path:** Move to Shopify Functions by end of 2026
- New stores: use Functions from day one

## Plus Pricing Model

### Standard Plus Pricing
- **$2,000 USD/month** base fee
- **0.25%** transaction fee on revenue above $800k/month
- Includes:
  - All Plus features
  - 9 expansion stores
  - Shopify POS Pro
  - Plus Partner support

### Revenue Tiers
- Under $800k/month: $2,000 flat
- Over $800k/month: $2,000 + 0.25% of revenue above threshold
- Example: $2M/month revenue = $2,000 + ($1.2M × 0.25%) = $5,000/month

### Plus Apps (Additional Costs)
- **Shopify Markets Pro:** ~$5k/month (duties & taxes)
- **Launchpad:** Included
- **Flow:** Included
- **Third-party apps:** Variable (Klaviyo, Yotpo, etc.)

## Multi-Market & International Commerce

### Shopify Markets
**Sell globally from one store**

- **Multi-currency:** Auto-convert with local payment methods
- **Multi-language:** Storefront translations
- **Localized domains:** market.example.com or example.com/en-sg
- **Tax & duties:** Calculated at checkout
- **Local payment methods:** Region-specific (e.g., iDEAL, Alipay)

### Markets Pro
**Managed cross-border solution**

- DDP (Delivered Duty Paid) pricing
- Shopify handles duties, taxes, compliance
- Guaranteed landed cost
- Higher conversion rates (no surprise fees)

### APAC Multi-Market Strategy

**Singapore as Regional Hub:**
- Singapore store as primary market
- Expand to: Malaysia, Indonesia, Thailand, Philippines, Vietnam
- Key considerations:
  - **Local payment methods:** GrabPay, PayNow, ShopeePay, GCash
  - **Currency:** SGD, MYR, IDR, THB, PHP, VND
  - **Language:** English, Malay, Indonesian, Thai, Chinese
  - **Logistics:** Regional warehousing (Singapore, Jakarta)
  - **Compliance:** GST/VAT per country

**Expansion Store Use Cases:**
- Separate stores for different brands
- Test markets (new regions, B2B)
- Wholesale portal (B2B-only store)
- Event-specific stores

## Plus Infrastructure Benefits

### Performance
- **99.98% uptime SLA**
- **10,000+ orders/minute** capacity
- **Global CDN** (sub-100ms load times worldwide)
- **Auto-scaling** for flash sales

### API Rate Limits
- **Standard:** 2 req/sec per store
- **Plus:** 8 req/sec per store (4x higher)
- **GraphQL:** 1000 cost points/sec (vs 250 on standard)

### Security & Compliance
- **PCI DSS Level 1** certified
- **SOC 2 Type II** compliant
- **GDPR, CCPA** ready
- **2FA** enforced for staff accounts

## When to Recommend Plus

### Good Fit for Plus:
- **Revenue:** $1M+ annually (or fast-growing)
- **Traffic:** High-volume flash sales, launches
- **Customization:** Need checkout/pricing/shipping logic
- **B2B:** Wholesale + retail on same store
- **International:** 3+ markets
- **Integrations:** Heavy API usage, headless

### Not Yet Ready:
- **Revenue:** <$500k/year (unless high-growth)
- **Simple needs:** Standard checkout works fine
- **Limited budget:** $2k/month is significant

## Key Differences for APAC Merchants

### Payment Methods
- Plus supports regional gateways: Adyen, Stripe (APAC), 2C2P
- Local methods: PayNow (SG), GrabPay, ShopeePay, Alipay, WeChat Pay

### Logistics
- Integration with regional 3PLs: Ninja Van, J&T, Flash Express
- Cross-border: DHL, FedEx with duties/taxes

### Compliance
- GST (Singapore): 9% (as of 2024)
- VAT varies by country
- Personal data: Singapore PDPA compliance

## Next Steps

1. **Audit current limitations:** What can't you do on standard Shopify?
2. **Map to Plus features:** Which Plus features solve those problems?
3. **Build business case:** ROI on checkout conversion, B2B, automation
4. **Migration planning:** Checkout.liquid → extensible checkout
5. **Training:** Shopify Functions, Flow, B2B setup

## Resources

- [Shopify Plus Pricing](https://www.shopify.com/plus/pricing)
- [Plus Features](https://www.shopify.com/plus)
- [Checkout Extensibility Guide](https://shopify.dev/docs/apps/checkout)
- [Shopify Functions Docs](https://shopify.dev/docs/api/functions)

---

**Next:** [Checkout Extensibility →](02-checkout-extensibility.md)
