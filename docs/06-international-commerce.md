# International Commerce on Shopify

## Overview

Shopify **Markets** enables merchants to sell globally from a single store with localized pricing, languages, domains, and payment methods per region.

**Key benefit:** One store, multiple countries—no need for separate stores per market.

## What is Shopify Markets?

**Markets** is Shopify's native multi-market solution for international commerce.

### Core Features
- **Multi-currency:** Sell in local currencies (USD, SGD, EUR, etc.)
- **Multi-language:** Translate storefront into regional languages
- **Localized domains:** market.example.com or example.com/en-sg
- **Market-specific pricing:** Adjust prices per region
- **Local payment methods:** Region-specific gateways (Alipay, iDEAL, etc.)
- **Tax & duties:** Calculate duties/taxes at checkout
- **Inventory by market:** Allocate stock per region

### Markets vs Expansion Stores

| Feature | Markets | Expansion Stores |
|---------|---------|------------------|
| Setup | Configure markets in one store | Create separate stores |
| Inventory | Shared inventory | Separate inventory |
| Admin | Single admin | Multiple admins |
| Cost | Included in Plus | 9 included in Plus |
| Use case | Same brand, multiple regions | Different brands, separate catalogs |

**Recommendation:** Use Markets for multi-region expansion (same brand). Use expansion stores for separate brands or distinct catalogs.

## Setting Up Markets

### 1. Enable Markets
**Admin → Settings → Markets**

- Click "Add market"
- Select countries (e.g., "Southeast Asia")
- Add: Singapore, Malaysia, Indonesia, Thailand, Philippines, Vietnam
- Save

### 2. Configure Market Settings

**For each market, configure:**

#### a) Domains & Subfolders
**Options:**
- **Subfolder:** example.com/en-sg (Singapore), example.com/en-my (Malaysia)
- **Subdomain:** sg.example.com, my.example.com
- **Top-level domain:** example.sg, example.com.my

**Recommendation for SEA:** Use subfolders (easier SSL, better SEO consolidation).

#### b) Languages
**Set primary language per market:**
- Singapore: English
- Malaysia: English + Malay
- Indonesia: Indonesian
- Thailand: Thai
- Vietnam: Vietnamese

**Implementation:**
- Use Shopify's native translation feature or third-party apps (Langify, Weglot)
- Translate: product titles, descriptions, collections, pages, checkout

#### c) Currency
**Set local currency per market:**
- Singapore: SGD
- Malaysia: MYR
- Indonesia: IDR
- Thailand: THB
- Philippines: PHP
- Vietnam: VND

**Pricing options:**
- **Auto-convert:** Use exchange rates to convert from base currency
- **Manual pricing:** Set fixed prices per market (recommended for margin control)

**Example:**
```
Product: T-Shirt
- US market: $50 USD
- Singapore: $68 SGD (not just conversion, adjusted for local pricing)
- Malaysia: RM 220 MYR
- Indonesia: Rp 750,000 IDR
```

#### d) Payment Methods
**Enable region-specific gateways:**

| Market | Payment Methods |
|--------|----------------|
| Singapore | Credit card, PayNow, GrabPay, Atome (BNPL) |
| Malaysia | Credit card, FPX, GrabPay, ShopeePay |
| Indonesia | Credit card, GoPay, OVO, DANA, Alfamart (cash) |
| Thailand | Credit card, PromptPay, Rabbit LINE Pay, TrueMoney |
| Philippines | Credit card, GCash, PayMaya, 7-Eleven (cash) |
| Vietnam | Credit card, MoMo, ZaloPay |

**Setup:** Use payment gateway that supports regional methods:
- **Stripe:** Supports most SEA methods
- **Adyen:** Enterprise solution, all methods
- **2C2P:** SEA-focused gateway

#### e) Shipping
**Configure shipping per market:**

- **Domestic (Singapore):** Local carriers (Ninja Van, J&T, Flash)
- **Regional (SEA):** Regional carriers (DHL eCommerce, Aramex)
- **International (rest of world):** DHL, FedEx, UPS

**Rates:**
- Set per market (e.g., free shipping in SG for >$50, Malaysia >$100)
- Use **carrier-calculated rates** for accuracy
- Offer **local pickup** in key markets

### 3. Tax Configuration

**Automatic tax calculation** per market:

| Market | Tax Rate | Notes |
|--------|----------|-------|
| Singapore | 9% GST | Charged on all sales |
| Malaysia | 0% SST | Varies by product category (6-10%) |
| Indonesia | 11% VAT | Required for local sales |
| Thailand | 7% VAT | E-commerce requires registration if >฿1.8M revenue |
| Philippines | 12% VAT | Required if registered |
| Vietnam | 10% VAT | E-commerce law requires local entity |

**Shopify Markets Tax:**
- Enable in Settings → Taxes and duties
- Automatically calculates based on buyer location
- Files tax returns (with Shopify Tax, paid add-on)

## Markets Pro (Advanced)

**Markets Pro** is Shopify's managed cross-border solution for duties & taxes.

### What is Markets Pro?
- **DDP (Delivered Duty Paid):** Shopify collects duties/taxes at checkout
- **Guaranteed landed cost:** No surprise fees for customers
- **Compliance:** Shopify handles tax registration, filing, remittance
- **Higher conversion:** 20-30% increase (customers see total price upfront)

### Cost
- **~$5,000 USD/month** base fee
- Commission on cross-border sales (~3-5%)

### When to Use Markets Pro
- **High cross-border volume:** $1M+ in international sales
- **Complex markets:** EU (VAT), Australia (GST)
- **Premium brand:** Want seamless customer experience
- **Risk aversion:** Don't want to handle tax compliance

### Markets Pro vs Standard Markets

| Feature | Standard Markets | Markets Pro |
|---------|------------------|-------------|
| Currency conversion | ✅ | ✅ |
| Language translation | ✅ | ✅ |
| Duties & taxes at checkout | Manual | ✅ Automated (DDP) |
| Tax registration | Merchant handles | ✅ Shopify handles |
| Landed cost guarantee | ❌ | ✅ |
| Cost | Included | ~$5k/month + commission |

## APAC Multi-Market Strategy

### Singapore as Regional Hub

**Why Singapore?**
- **Strategic location:** Central to SEA
- **Infrastructure:** World-class logistics, internet, banking
- **Business-friendly:** Easy company setup, low tax (17% corporate)
- **IP protection:** Strong legal framework
- **Talent pool:** English-speaking, skilled workforce
- **Free trade:** Multiple FTAs (ASEAN, US, EU, China)

**Hub-and-spoke model:**
- **Hub:** Singapore (headquarters, primary inventory)
- **Spokes:** Malaysia, Indonesia, Thailand, Philippines, Vietnam

### Expansion Prioritization

**Phase 1: Core SEA (Year 1)**
- **Singapore:** Home market
- **Malaysia:** Close proximity, similar culture, English-friendly
- **Indonesia:** Largest population (270M), huge market

**Phase 2: Secondary SEA (Year 2)**
- **Thailand:** 70M population, growing e-commerce
- **Philippines:** 110M population, high English proficiency
- **Vietnam:** 100M population, rapidly growing middle class

**Phase 3: Greater APAC (Year 3+)**
- **Australia:** Mature e-commerce, high spending power
- **Hong Kong:** Gateway to China
- **Japan:** High-value market, requires localization
- **South Korea:** Advanced e-commerce infrastructure

### Market-Specific Considerations

#### Singapore
- **Currency:** SGD (stable)
- **Language:** English, Mandarin, Malay, Tamil
- **Payment:** Credit cards dominant, PayNow growing, Atome/Grab BNPL
- **Shipping:** Fast (1-2 days), Ninja Van/J&T/Flash popular
- **Behavior:** High smartphone penetration, price-sensitive despite wealth

#### Malaysia
- **Currency:** MYR (volatile vs USD)
- **Language:** Malay, English, Mandarin
- **Payment:** FPX (online banking), GrabPay, ShopeePay, credit card
- **Shipping:** 2-5 days, logistics infrastructure good in KL/Penang
- **Behavior:** Strong Islamic market (halal important), social commerce (Facebook, Instagram)

#### Indonesia
- **Currency:** IDR (weak vs USD, large numbers)
- **Language:** Indonesian (Bahasa Indonesia)
- **Payment:** E-wallets dominant (GoPay, OVO, DANA), credit card penetration low, cash-on-delivery (COD) expected
- **Shipping:** Challenging (archipelago), 5-10 days, J&T/Ninja Van/Sicepat
- **Behavior:** Social commerce huge (WhatsApp, Instagram), influencer-driven

#### Thailand
- **Currency:** THB (stable)
- **Language:** Thai (low English proficiency)
- **Payment:** PromptPay (bank transfer), LINE Pay, TrueMoney, credit cards
- **Shipping:** 2-5 days, Flash Express/Kerry Express/Thailand Post
- **Behavior:** LINE app dominant, mobile-first, visual content (videos)

#### Philippines
- **Currency:** PHP (moderate volatility)
- **Language:** English, Tagalog
- **Payment:** GCash, PayMaya, 7-Eleven cash (credit card penetration low)
- **Shipping:** Island logistics complex, 5-10 days, LBC/J&T
- **Behavior:** High social media usage (Facebook #1), remittance-driven economy

#### Vietnam
- **Currency:** VND (weak vs USD)
- **Language:** Vietnamese (low English proficiency)
- **Payment:** MoMo, ZaloPay, bank transfer, COD common
- **Shipping:** 3-7 days, improving logistics (Ninja Van, J&T, ViettelPost)
- **Behavior:** Fast-growing middle class, mobile commerce, TikTok-driven

### Local Payment Methods Setup

**Stripe (recommended for most):**
- Supports: PayNow (SG), FPX (MY), GrabPay (SG/MY/PH/TH), Alipay, WeChat Pay
- Easy setup via Shopify admin
- Single integration for multiple methods

**Adyen (enterprise):**
- All SEA payment methods
- Better for high-volume (>$10M/year)
- More complex setup

**Regional gateways:**
- **2C2P:** SEA-focused, all local methods, good support
- **iPay88:** Malaysia-focused
- **Xendit:** Indonesia-focused

**Setup:**
1. Admin → Settings → Payments
2. Add provider (Stripe/Adyen)
3. Enable regional payment methods per market
4. Test in each market

### Localization Best Practices

#### 1. Pricing Strategy
**Cost-plus vs market-based:**
- **Cost-plus:** Base price + shipping + duties + margin → Not recommended (can be uncompetitive)
- **Market-based:** Research local competitor pricing, price accordingly → Recommended

**Example:**
- Singapore: Premium pricing (high purchasing power)
- Indonesia: Budget pricing (price-sensitive)
- Same product, different positioning

#### 2. Cultural Localization
- **Product images:** Show models from local ethnicity
- **Holidays:** Chinese New Year (SG/MY), Hari Raya (SG/MY/ID), Songkran (TH)
- **Colors:** Red = luck (Chinese), avoid certain colors (e.g., white = mourning in some cultures)
- **Copy:** Translate, don't just transliterate. Hire native speakers.

#### 3. Mobile-First Design
**APAC is mobile-dominant:**
- 80%+ of traffic from mobile
- Design for small screens first
- Fast load times critical (3G networks common)
- One-click payment (e.g., GrabPay, PayNow)

#### 4. Customer Support
**Localized support channels:**
- **Singapore:** Email, WhatsApp
- **Malaysia:** WhatsApp, Facebook Messenger
- **Indonesia:** WhatsApp (dominant), Instagram DM
- **Thailand:** LINE (dominant)
- **Philippines:** Facebook Messenger

**Language support:**
- Offer support in local language
- Use translation services or local CS team
- Chatbots for common queries

## Cross-Border Logistics

### Fulfillment Options

#### 1. Ship from Singapore (Hub Model)
**Pros:**
- Single inventory location
- Easier to manage
- Lower upfront cost

**Cons:**
- Longer shipping times (5-10 days to Indonesia)
- Higher shipping cost
- Potential customs delays

**Best for:** Early-stage, testing markets

#### 2. Regional Warehouses
**Locations:**
- Singapore (hub)
- Jakarta (Indonesia)
- Bangkok (Thailand)
- Manila (Philippines)

**Pros:**
- Faster delivery (2-3 days)
- Lower shipping cost per unit
- Better customer experience

**Cons:**
- Inventory fragmentation
- Higher operational complexity

**Best for:** Established markets, high-volume

#### 3. Third-Party Logistics (3PL)
**Regional 3PLs:**
- **aCommerce:** SEA-focused, warehousing + fulfillment
- **Ninja Van:** Logistics + fulfillment
- **J&T Express:** Logistics network across SEA

**Setup:**
- Integrate 3PL with Shopify (via app or API)
- Send inventory to 3PL warehouse
- Orders auto-forward to 3PL for fulfillment

### Duties & Taxes (Customs)

**De minimis thresholds** (duty-free import limits):

| Country | Threshold | Notes |
|---------|-----------|-------|
| Singapore | S$400 | Below = duty-free |
| Malaysia | RM 500 | Below = duty-free |
| Indonesia | $75 USD | Very low, almost all shipments taxed |
| Thailand | ฿1,500 | Below = duty-free |
| Philippines | ₱10,000 | Below = duty-free |
| Vietnam | ~$50 USD | Varies by product |

**Recommendation:**
- **Low-value items (<$50):** Ship from Singapore (likely duty-free)
- **High-value items (>$100):** Use Markets Pro or local fulfillment (avoid surprise fees)

## Measuring Multi-Market Success

### Key Metrics

**Per-market metrics:**
- **Conversion rate:** Are local payment methods working?
- **Average order value (AOV):** Is pricing optimized?
- **Cart abandonment:** Shipping cost too high?
- **Return rate:** Sizing/fit issues due to lack of localization?

**Overall metrics:**
- **International revenue %:** Growing or stagnant?
- **Customer acquisition cost (CAC) per market:** Which markets are efficient?
- **Lifetime value (LTV) per market:** Which markets have loyal customers?

### A/B Testing Ideas
- **Currency display:** Show USD vs local currency (which converts better?)
- **Shipping options:** Free shipping vs fast paid shipping
- **Payment methods:** Order of payment methods at checkout
- **Language:** Auto-detect vs manual switcher

## Common Pitfalls

### 1. Auto-Currency Conversion Issues
**Problem:** Exchange rate fluctuations eat margin.
**Solution:** Set manual prices per market, review quarterly.

### 2. Shipping Cost Shock
**Problem:** Customers add to cart, see $50 shipping, abandon.
**Solution:** Show estimated shipping on product page, offer free shipping thresholds.

### 3. Duties Surprise
**Problem:** Customer receives package, asked to pay $30 duty, refuses.
**Solution:** Use DDP (Markets Pro) or clearly communicate "duties may apply" at checkout.

### 4. Language Mix-Up
**Problem:** Auto-translation produces nonsensical product descriptions.
**Solution:** Hire native translators, not just Google Translate.

### 5. Payment Method Missing
**Problem:** 70% of Indonesians want to use GoPay, but it's not available.
**Solution:** Research dominant payment methods per market before launch.

## Resources

- [Shopify Markets Guide](https://help.shopify.com/en/manual/markets)
- [Markets Pro Overview](https://www.shopify.com/plus/solutions/international/markets-pro)
- [Multi-Currency Setup](https://help.shopify.com/en/manual/payments/shopify-payments/multi-currency)
- [International Domains](https://help.shopify.com/en/manual/domains/managing-domains/international-domains)
- [Tax & Duty Calculator](https://www.shopify.com/tools/duty-and-import-tax-calculator)

---

**Next:** [Flow Automation →](07-flow-automation.md)
