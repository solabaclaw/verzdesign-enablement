# B2B on Shopify Plus

## Overview

Shopify Plus includes **native B2B capabilities**, allowing merchants to run wholesale and retail operations from a single store. This eliminates the need for separate B2B platforms or complex workarounds.

**Key benefit:** One inventory, one platform, D2C + B2B combined.

## Core B2B Features

### 1. Company Accounts
B2B customers belong to **companies** with multiple buyers:

- **Company:** The business entity (e.g., "ABC Retail Pte Ltd")
- **Locations:** Multiple shipping/billing addresses per company
- **Buyers:** Individual users within the company (purchasing manager, store owner, etc.)
- **Roles & Permissions:** Control who can purchase, approve orders, manage company

**Example Structure:**
```
ABC Retail Pte Ltd (Company)
├── Main Office (Location 1)
├── Warehouse (Location 2)
└── Buyers
    ├── john@abc.com (Admin - can purchase & manage)
    ├── mary@abc.com (Buyer - can purchase only)
    └── david@abc.com (Approver - can approve orders)
```

### 2. Custom Price Lists (Catalogs)
Create different pricing for different customer segments:

- **Retail price:** $100
- **VIP B2B:** $80 (20% off)
- **Distributor:** $70 (30% off)
- **Regional partner:** $65 (35% off)

**Catalog options:**
- Fixed price per product/variant
- Percentage discount off retail
- Volume-based pricing (10+ units = 15% off)

### 3. Payment Terms
Offer net payment terms instead of immediate payment:

- **Net 15:** Payment due in 15 days
- **Net 30:** Payment due in 30 days
- **Net 60:** Payment due in 60 days
- **Net 90:** Payment due in 90 days

**Benefits:**
- Builds trust with B2B customers
- Industry standard for wholesale
- Shopify tracks outstanding balances

**Requirements:**
- Plus plan required
- Shopify Payments or Manual Payments
- Set credit limits per company

### 4. Minimum Order Requirements
Set minimum order quantities or values:

- Minimum $500 per order
- Minimum 10 units per product
- Minimum 50 total items per order

**Enforcement:** Via Shopify Functions (see [Shopify Functions](03-shopify-functions.md)).

### 5. Self-Service Company Registration
Allow B2B customers to request access:

1. Customer fills out company registration form
2. Merchant reviews & approves in Shopify admin
3. Company admin receives access
4. They invite additional buyers

**Form fields:**
- Company name
- Tax ID / GST registration number
- Business address
- Contact person
- Reason for B2B account

## B2B Catalog Management

### Creating Price Lists

**Admin → Customers → B2B → Catalogs**

1. Create new catalog
2. Name it (e.g., "Wholesale - 30% off")
3. Set pricing:
   - **All products:** 30% off retail
   - **Specific products:** Fixed prices
   - **Variants:** Per-variant pricing
4. Assign to companies

**Volume Pricing Example:**
```
Product: Widget
- 1-9 units: $10 each
- 10-49 units: $9 each
- 50-99 units: $8 each
- 100+ units: $7 each
```

Set via **price rules** in catalog.

### Product Visibility Control
Show/hide products per catalog:

- **Retail catalog:** All products visible
- **Wholesale catalog:** Only bulk SKUs visible
- **Distributor catalog:** Only distributor-eligible products

**Use case:** Hide sample-size products from B2B buyers (they only want full cases).

## B2B Checkout Flow

### How It Works

1. **B2B buyer logs in** → sees company-specific pricing
2. **Adds products to cart** → prices reflect their catalog
3. **Selects shipping address** → chooses from company locations
4. **Payment method:**
   - **Credit card** (immediate payment)
   - **Net terms** (invoice, pay later)
   - **Purchase order** (manual approval)
5. **Places order** → order created with payment terms
6. **Invoice sent** → due date based on net terms
7. **Payment collected** → via Shopify Payments or manual

### Draft Orders for B2B
Merchants can create **draft orders** for B2B customers:

- Custom pricing (override catalog)
- Send invoice via email
- Customer pays via link
- Useful for quotes, custom orders

## Combining D2C + B2B on One Store

### Why Combine?
- **Single inventory:** No syncing between platforms
- **Unified reporting:** See all revenue in one dashboard
- **Simpler management:** One Shopify admin
- **Lower costs:** No separate B2B platform subscription

### Storefront Strategy

**Option 1: Separate Storefronts (Recommended)**
- **D2C:** mainstore.com (public)
- **B2B:** wholesale.mainstore.com (password-protected or gated)
- Implementation: Use Shopify's native B2B storefront

**Option 2: Single Storefront with Gated Access**
- **Public:** All customers see retail prices
- **Logged-in B2B:** See wholesale prices
- Implementation: Customer tags + catalog assignment

**Option 3: Separate Expansion Store**
- **D2C store:** mainstore.com
- **B2B store:** wholesale.mainstore.com (separate store)
- Shares inventory via multi-location setup

### Recommended for Most: Option 1
Separate B2B storefront = cleaner UX, easier management.

## B2B Customer Experience

### Company Admin Dashboard
B2B buyers get a **company portal** in their account:

- View all company buyers
- Manage locations (shipping addresses)
- See outstanding invoices
- Download past orders
- Request credit increase

### Buyer Permissions
Set what each buyer can do:

- **Purchasing Buyer:** Place orders, view company orders
- **Company Admin:** Manage buyers, locations, view invoices
- **Location Buyer:** Only order for specific location

## Payment Terms & Credit Management

### Setting Up Net Terms

1. **Enable B2B:** Shopify admin → Settings → Customer accounts → B2B
2. **Configure payment terms:** Settings → Payments → Manual payments → Enable "Payment terms"
3. **Set default terms:** Net 30 (customizable per company)
4. **Set credit limits:** Per company (e.g., $50,000 credit line)

### Invoice & Payment Flow

1. **Order placed** with net 30 terms
2. **Invoice auto-sent** to company admin
3. **Due date:** 30 days from order date
4. **Reminder emails:** 7 days before, on due date, overdue
5. **Payment collected:**
   - Online: via invoice link (credit card)
   - Offline: bank transfer (marked paid manually)

### Credit Limit Enforcement
- Set max outstanding balance per company
- Orders decline if limit exceeded
- Request credit increase (merchant approves)

## APAC B2B Considerations

### Singapore as B2B Hub
**Why Singapore?**
- Business-friendly regulations
- Regional logistics hub
- Strong IP protection
- Skilled workforce
- GST-registered businesses (9% GST)

**Typical B2B setup:**
- Singapore entity as regional headquarters
- B2B customers across SEA (Malaysia, Indonesia, Thailand, Philippines)
- Multi-currency pricing (SGD, MYR, IDR, THB, PHP)

### Regional B2B Challenges

#### 1. Tax & Compliance
**Singapore GST:**
- **Registered businesses:** GST-exempt for exports
- **Local B2B:** 9% GST charged
- **Solution:** Use tax exemptions in Shopify (tag B2B export orders)

**Other APAC countries:**
- **Indonesia:** VAT 11%
- **Malaysia:** SST 6-10%
- **Thailand:** VAT 7%
- **Philippines:** VAT 12%

**Shopify setup:**
- Configure tax rules per country
- Use B2B price lists to include/exclude tax

#### 2. Payment Terms by Region
**Singapore/Malaysia:** Net 30 common
**Indonesia:** Net 45-60 (longer payment cycles)
**Philippines:** Often cash-on-delivery or prepayment (credit risk)

**Recommendation:** Set payment terms per company, not blanket policy.

#### 3. Local Payment Methods
**B2B-specific payments:**
- **Bank transfer:** Most common in SEA
- **GIRO (Singapore):** Auto-debit from company bank account
- **PayNow Corporate (Singapore):** Instant bank transfer
- **Cheque:** Still used in some markets (manual processing)

**Shopify setup:**
- Enable manual payments (bank transfer)
- Use draft orders for custom payment arrangements

#### 4. Multi-Language B2B
**Product catalogs:**
- English (default)
- Chinese (simplified) for China-based buyers
- Malay for Malaysian distributors
- Thai for Thailand

**Shopify Markets + B2B:**
- Set language per market
- B2B buyers see translated product info
- Pricing in local currency

### APAC B2B Use Cases

#### Use Case 1: Fashion Brand (Singapore → SEA)
**Setup:**
- D2C: Direct to consumers in Singapore
- B2B: Wholesale to boutiques across Malaysia, Indonesia, Thailand
- **Catalogs:**
  - Retail: Full price
  - Wholesale: 40% off, minimum $1,000 order
- **Payment terms:** Net 30 for Singapore, prepayment for Indonesia (credit risk)

#### Use Case 2: Electronics Distributor
**Setup:**
- B2B-only store (no D2C)
- Sell to retailers across APAC
- **Catalogs:**
  - Tier 1 (high-volume): 35% off
  - Tier 2 (medium): 25% off
  - Tier 3 (new partners): 15% off
- **Payment terms:** Net 60 for established partners
- **Volume pricing:** Bulk discounts via Functions

#### Use Case 3: Food & Beverage (F&B) Supplier
**Setup:**
- D2C: Subscription boxes for consumers
- B2B: Supply cafes, restaurants
- **Catalogs:**
  - Cafe partners: 20% off, case quantities only
  - Restaurants: Custom pricing (draft orders)
- **Payment:** Net 15 (fast-moving goods)

## Implementation Checklist

### Phase 1: Setup (Week 1-2)
- [ ] Enable B2B in Shopify admin
- [ ] Configure payment terms (net 30, 60, etc.)
- [ ] Create B2B storefront (separate domain or gated)
- [ ] Set up company registration form

### Phase 2: Catalogs & Pricing (Week 3-4)
- [ ] Define customer segments (wholesale, distributor, VIP)
- [ ] Create price lists per segment
- [ ] Set volume pricing rules
- [ ] Configure product visibility per catalog

### Phase 3: Customer Onboarding (Week 5-6)
- [ ] Import existing B2B customers
- [ ] Create companies with locations
- [ ] Assign buyers with permissions
- [ ] Send onboarding emails

### Phase 4: Testing (Week 7)
- [ ] Test order flow (cart → checkout → invoice)
- [ ] Test payment terms enforcement
- [ ] Test credit limits
- [ ] Test multi-location shipping

### Phase 5: Launch (Week 8)
- [ ] Open B2B storefront to customers
- [ ] Train customer service team
- [ ] Monitor orders & invoices
- [ ] Collect feedback

## Best Practices

### 1. Clear Onboarding
- **Welcome email:** Explain how to use B2B portal
- **Video tutorial:** Show how to place orders, manage company
- **Support contact:** Dedicated B2B support email

### 2. Tiered Pricing Strategy
- **Entry tier:** Low discount, test partners (10-15% off)
- **Growth tier:** Medium discount, regular buyers (20-25% off)
- **Elite tier:** High discount, high-volume (30-40% off)

### 3. Payment Terms Risk Management
- **New customers:** Prepayment or credit card only
- **Established (6+ months):** Net 30
- **High-volume trusted:** Net 60-90
- **Set credit limits:** Start low ($5k), increase based on payment history

### 4. Inventory Allocation
Reserve inventory for B2B vs D2C:
- Use **inventory locations** to separate stock
- Prevent D2C from selling out B2B inventory
- Set **quantity rules** (B2B: case quantities only)

### 5. Customer Segmentation
Tag companies for targeting:
- **Segment by volume:** high-value, medium, low
- **Segment by region:** SG, MY, ID, TH
- **Segment by vertical:** retail, hospitality, corporate
- Use tags for email campaigns, special offers

## Troubleshooting

### Issue: B2B customers see retail prices
**Solution:** Ensure they're logged in and assigned to a company with a catalog.

### Issue: Payment terms not available at checkout
**Solution:** Enable manual payments + payment terms in Settings → Payments.

### Issue: Company buyers can't place orders
**Solution:** Check buyer permissions (must have "Purchasing" role).

### Issue: Credit limit exceeded error
**Solution:** Review outstanding invoices, increase credit limit, or request payment.

## Resources

- [Shopify B2B Guide](https://help.shopify.com/en/manual/b2b)
- [B2B on Shopify (Docs)](https://shopify.dev/docs/apps/b2b)
- [Payment Terms Setup](https://help.shopify.com/en/manual/payments/payment-terms)
- [Company Management](https://help.shopify.com/en/manual/customers/companies)

---

**Next:** [International Commerce →](06-international-commerce.md)
