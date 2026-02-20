# AI-Enhanced Shopify Flow Automation for Southeast Asia

## Overview

Shopify Flow is a visual workflow automation tool included with Shopify Plus. With AI integration, Flow evolves from simple "if-this-then-that" automation into an intelligent decision engine—predicting customer behavior, detecting fraud, optimizing inventory, and adapting to the unique complexities of Southeast Asian commerce.

This guide covers both foundational Flow concepts and advanced AI-enhanced capabilities tailored for SEA merchants across Singapore, Malaysia, Indonesia, Thailand, Vietnam, and the Philippines.

## Flow Fundamentals

### Three Building Blocks

#### 1. Trigger — What starts the workflow?
- Order created / paid / fulfilled
- Customer created / updated
- Product created / inventory changed
- Cart created / refund created

#### 2. Condition — Should the action run?
- Order total > SGD 500
- Customer tagged "VIP"
- Shipping country = "MY"
- Inventory level < 10

#### 3. Action — What should happen?
- Tag customer / order / product
- Send email or Slack notification
- Hide / unpublish product
- Create discount code
- Call external API (webhook)

### Simple Example

**Tag high-value orders:**
```
Trigger: Order created
Condition: Order total ≥ SGD 500
Action: Tag order "high-value"
```

## AI-Enhanced Flow Capabilities

### Smart Triggers with AI Context

Traditional triggers are event-based. AI adds predictive intelligence:

- **Customer Behavior Prediction**: Trigger workflows based on predicted churn risk or purchase likelihood
- **Sentiment Analysis**: React to customer feedback sentiment in real-time
- **Anomaly Detection**: Automatically flag unusual order patterns or inventory movements
- **Seasonal Intelligence**: AI recognizes regional patterns (Hari Raya, Chinese New Year, Songkran, 9.9/11.11 sales)

### AI Conditions in Workflows

```
IF customer_lifetime_value_prediction > SGD 5,000
AND churn_risk = "high"
THEN assign to premium support + offer loyalty incentive
```

### Dynamic Customer Segmentation

AI Flow can automatically tag customers based on:

**Purchase Behavior:**
```
IF AI detects "bulk buyer" pattern
AND average order value > SGD 1,500
THEN tag as "B2B_Potential" + assign account manager
```

**Engagement Signals:**
```
IF customer visits 5+ times in 7 days
AND no purchase
AND AI predicts "high purchase intent"
THEN tag "Hot_Lead" + trigger personalized email with incentive
```

**Regional Preferences:**
```
IF customer language preference = Bahasa
AND location = Jakarta
THEN tag "ID_Bahasa" + enable regional marketing flows
```

### AI Lifecycle Stage Automation

AI automatically moves customers through stages:

1. **New Customer** → Welcome series (localized by market)
2. **Engaged Buyer** (AI detects repeat pattern) → Loyalty program invite
3. **At-Risk** (AI predicts churn) → Win-back campaign
4. **VIP** (AI identifies high LTV) → Premium tier access

## Predictive Inventory Management

### AI-Driven Stock Forecasting

**Flow Example: Mega Sale Preparation (9.9, 11.11, 12.12)**

```
TRIGGER: 30 days before 11.11 sale
CONDITION: AI predicts demand spike > 200% for product category
ACTION:
  - Auto-create purchase order for predicted quantity
  - Alert warehouse team across SG/MY/ID locations
  - Adjust safety stock levels
  - Enable pre-order if supplier lead time > demand window
```

**SEA-Specific Considerations:**
- Regional festival variations (Hari Raya across MY/SG/ID, Songkran in TH, Tet in VN)
- Monsoon/rainy season demand shifts
- Double-digit sale events (9.9, 10.10, 11.11, 12.12) unique to SEA
- Cross-border demand patterns (SG shoppers buying MY products)

### Smart Low-Stock Alerts

Instead of simple threshold alerts, AI predicts:
- Rate of sale trajectory
- Likelihood of stockout before next restock
- Optimal reorder timing based on supplier lead times (China, local)
- Alternative SKU recommendations for substitution

## Fraud Detection & Prevention with AI

### Real-Time Fraud Scoring

**High-Risk Order Flow:**

```
TRIGGER: Order placed
CONDITION: AI fraud score > 85
AND payment method = COD (common in ID/PH/VN)
AND shipping address = high-risk area
ACTION:
  - Hold order for manual review
  - Request customer verification via SMS/WhatsApp
  - Flag for COD to prepaid conversion attempt
  - Alert fraud team if value > SGD 1,000
```

### COD-Specific Fraud Prevention (Indonesia/Philippines/Vietnam)

Cash on Delivery remains common in parts of SEA. AI detects:
- Multiple COD orders from similar addresses with different names
- Pattern of COD order cancellations / returns
- Suspicious phone number patterns
- Address verification mismatches

**Flow Actions:**
```
CONDITION: AI fraud score < 40 (low risk)
ACTION: Auto-confirm + send WhatsApp confirmation

CONDITION: AI fraud score 40-70 (medium risk)
ACTION: Send OTP verification + limit to 1 active COD order

CONDITION: AI fraud score > 70 (high risk)
ACTION: Hold for manual review + suggest prepaid with discount
```

**Post-Delivery:**
- Successful delivery → Tag "COD_Reliable", increase limit
- Return to Origin → Tag "COD_Risk", restrict future COD
- 3 successful deliveries → Auto-offer loyalty points + suggest e-wallet

### Return Fraud Detection

```
TRIGGER: Return request initiated
CONDITION: AI detects "serial returner" pattern
AND return reason = "defective" but QC found no defect in past returns
ACTION:
  - Escalate to senior CS team
  - Flag account for return abuse review
  - Require photo/video evidence before approval
```

## Dynamic Pricing Suggestions

### AI-Powered Price Optimization

**Mega Sale Dynamic Pricing (9.9 / 11.11 / 12.12):**

```
TRIGGER: Sale window starts
CONDITION: AI predicts high demand for category
AND competitor pricing analysis shows opportunity
ACTION:
  - Suggest price adjustment within margin guardrails
  - Create urgency messaging ("Limited stock at this price")
  - Adjust post-sale price to restore margin
```

**Clearance & Markdown Automation:**

```
TRIGGER: Product age > 90 days
CONDITION: AI predicts low sell-through probability
AND inventory > 50 units
ACTION:
  - Auto-apply 15% markdown
  - Tag for "Clearance" collection
  - If no sales in 14 days → increase to 30%
  - Alert buying team for future planning
```

### Geographic Pricing Intelligence

AI can suggest market-specific pricing:
- **Singapore**: Premium pricing acceptable, high purchasing power
- **Malaysia**: Mid-range, price-competitive market
- **Indonesia/Philippines/Vietnam**: Price-sensitive, bundles and discounts drive conversion
- **Thailand**: Mix of premium (Bangkok) and value-conscious (regional)

## SEA-Specific Workflow Templates

### Template 1: Multi-Market Order Routing

**Route orders to correct regional warehouse:**

```
Trigger: Order created
├─ Condition: Shipping country = SG
│  └─ Action: Tag "Warehouse-SG"
├─ Condition: Shipping country = MY
│  └─ Action: Tag "Warehouse-MY"
├─ Condition: Shipping country = ID
│  └─ Action: Tag "Warehouse-ID"
├─ Condition: Shipping country = TH
│  └─ Action: Tag "Warehouse-TH"
└─ Condition: Shipping country = VN
   └─ Action: Tag "Warehouse-VN"
```

### Template 2: Halal Product Compliance (Malaysia/Indonesia)

```
Trigger: Order created
└─ For each line item:
   └─ Condition: Product NOT tagged "halal-certified"
      └─ Action: Tag order "non-halal-review"
         └─ Action: Alert compliance team
```

### Template 3: GST/Tax Tagging (Singapore)

```
Trigger: Order created
├─ Condition: Shipping country = SG
│  └─ Action: Tag "GST-applicable"
└─ Condition: Shipping country ≠ SG
   └─ Action: Tag "export-zero-rated"
```

### Template 4: Festive Season Automation

**Chinese New Year (SG/MY), Hari Raya (MY/SG/ID), Songkran (TH):**

```
30 Days Before Festival:
1. AI forecasts demand by SKU and market
2. Auto-notify suppliers of predicted orders
3. Alert logistics partners (Ninja Van, J&T, Flash Express) of volume spike
4. Auto-create festive collection, enable gift options
5. Predict CS volume, suggest staffing increases

During Festival:
1. Real-time inventory monitoring with auto-backorder
2. Dynamic pricing based on real-time demand
3. Express shipping with AI-predicted delivery dates
4. Gift wrapping / personalized messaging

Post-Festival:
1. AI predicts return volume, prep reverse logistics
2. Identify slow movers, create markdown schedule
3. Tag first-time festive buyers for nurture campaigns
```

### Template 5: Mega Sale Management (9.9, 11.11, 12.12)

**Pre-Sale (24h before):**
```
- AI predicts traffic volume
- Auto-scale infrastructure alerts
- Enable queue management if needed
- Pre-allocate inventory (prevent overselling)
- Activate fraud monitoring (flash sales attract fraud)
```

**During Sale:**
```
- Real-time inventory sync across channels
- Auto-pause product if stock critical
- Fraud score monitoring (block suspicious bulk orders)
- Dynamic cart abandonment recovery via WhatsApp
```

**Post-Sale:**
```
- Auto-prioritize fulfillment by proximity
- Tag customers by behavior for retargeting
- Generate performance report: conversion, AOV, top products
```

### Template 6: Multi-Language Customer Communication

```
TRIGGER: New customer account created
CONDITION: Browser language detected
ACTION:
  ├─ English (SG/MY/PH) → English templates
  ├─ Bahasa (MY/ID) → Bahasa templates
  ├─ Thai (TH) → Thai templates
  ├─ Vietnamese (VN) → Vietnamese templates
  └─ Chinese (SG/MY) → Chinese templates

Ongoing:
- Order confirmations in preferred language
- Shipping updates via SMS/WhatsApp in local language
- Post-purchase follow-up culturally adapted
- Review requests in customer's language
```

### Template 7: COD to E-Wallet Conversion (ID/PH/VN)

```
TRIGGER: Order placed with COD
ACTION:
  - Offer incentive to switch: "Pay now via GoPay/GCash/MoMo → get 5% off"
  - After successful delivery: suggest e-wallet for next order
  - After 3 successful COD deliveries: auto-offer loyalty bonus for digital payment
```

## Built-in Template Library

Shopify provides **100+ pre-built templates**. Key categories:

### Customer Loyalty & Segmentation
- VIP tagging (lifetime spend thresholds)
- Repeat customer rewards
- Win-back campaigns (90-day inactivity)

### Inventory Management
- Low stock alerts with restock notifications
- Auto-unpublish / republish based on inventory
- Slow-moving inventory flagging

### Order Processing
- International order routing by region
- B2B order priority processing
- Same-day fulfillment tagging (orders before 2 PM)

### Marketing & Engagement
- Welcome series for new customers
- Post-purchase review requests
- Birthday / anniversary discounts

## Advanced Flow Patterns

### Multi-Condition Logic

```
Trigger: Order created
├─ Condition: Total > SGD 300 → Tag "Priority"
├─ Condition: Customer tagged "VIP" → Tag "Priority"
└─ Condition: Product type = "Electronics" → Tag "Priority"
```

### Sequential Actions

```
Trigger: Inventory = 0
├─ Action 1: Unpublish product
├─ Action 2: Email inventory manager
└─ Action 3: Webhook to restock system
```

### Delayed Actions

```
Trigger: Order fulfilled
└─ Wait 7 days
   └─ Send review request email
```

### Looping Over Line Items

```
Trigger: Order created
└─ For each line item:
   └─ Tag order with {lineItem.productType}
```

## Third-Party App Integrations

Flow connects with **200+ apps**:

| App | Use Case |
|-----|----------|
| **Klaviyo** | Tag VIP → Add to Klaviyo segment → Targeted campaigns |
| **Slack** | High-value order → Alert #sales channel |
| **Gorgias** | Urgent order → Create priority support ticket |
| **Ninja Van / J&T** | Tag by region → Route to correct logistics partner |
| **GrabPay / PayNow** | Payment method tracking → Segment by payment preference |

### Custom Webhooks

For systems without Flow connectors:

```
Trigger: Order created
Action: Send HTTP POST
URL: https://your-system.com/api/orders
Headers: Authorization: Bearer {api-key}
Body: {
  "order_id": "{{order.id}}",
  "customer_email": "{{order.customer.email}}",
  "total": "{{order.totalPrice}}",
  "country": "{{order.shippingAddress.country}}"
}
```

## Best Practices

### Start Simple, Scale Smart
1. **Phase 1**: Basic automation (order tagging, notifications)
2. **Phase 2**: Add AI conditions (fraud scores, prediction thresholds)
3. **Phase 3**: Complex multi-step workflows with AI decision trees
4. **Phase 4**: Custom AI models via API integrations

### Testing & Validation
- Use **test mode** before enabling
- A/B test AI flows vs. traditional flows
- Review workflow logs weekly
- Audit AI predictions vs. actual outcomes monthly

### Common Pitfalls to Avoid

❌ **Over-automation**: Keep human review for high-value decisions
❌ **Ignoring regional diversity**: SG ≠ ID ≠ VN — segment by market
❌ **Static thresholds**: Update AI thresholds for sale seasons vs. normal
❌ **Language assumptions**: Never assume English; check language preference
❌ **One-size-fits-all**: Bangkok premium shoppers ≠ regional Thai buyers

### Avoid Infinite Loops

```
❌ Bad: Trigger on "Order tagged" → Action: Tag order (same tag)
✅ Fix: Use different tag names or add "not already tagged" condition
```

## Limitations

| Limit | Value |
|-------|-------|
| Max workflows per store | 200 |
| Max actions per workflow | 100 |
| Max delay | 30 days |
| HTTP requests per day | 1,000 |
| Trigger latency | 1-2 minutes (not instant) |

**What Flow CAN'T do:**
- Modify checkout behavior (use Functions instead)
- Real-time cart changes during browsing
- Complex calculations (use external API)
- Fetch from external databases (webhooks only)

## ROI Metrics for AI Flow

| Metric | Before AI Flow | After AI Flow | Target |
|--------|---------------|---------------|--------|
| Fraud order rate (COD markets) | 8-12% | <3% | 60-75% reduction |
| Stockout incidents during mega sales | 15-20/event | <5/event | 75% reduction |
| Customer response time | 4-6 hours | <1 hour | 80% improvement |
| Manual tagging effort (hours/week) | 10-15h | <2h | 85% reduction |
| Return fraud identification | 30-40% | >80% | 2x improvement |

## Real-World Use Case: Electronics Retailer (Singapore)

**Scenario:** Singapore-based electronics retailer selling across SEA.

**Workflows implemented:**

1. **VIP Customer Rewards** — Lifetime spend ≥ SGD 2,000 → Tag VIP + 15% discount + notify account manager
2. **Low Stock Alert** — Inventory <20 (SG warehouse) for Laptops/Phones → Email procurement + Slack #inventory
3. **High-Value Fraud Check** — Order > SGD 3,000 + fraud score > 0.5 → Hold + alert + pause fulfillment
4. **Export Tax Tagging** — Non-SG shipping → Tag "export" for customs documentation
5. **Post-Purchase Review** — 10 days after fulfillment → Klaviyo review request email

**Results:**
- Saved 15 hours/week on manual tagging
- Reduced stockouts by 40%
- Zero chargebacks in 6 months

## Resources

- [Shopify Flow Guide](https://help.shopify.com/en/manual/shopify-flow)
- [Flow Templates Library](https://help.shopify.com/en/manual/shopify-flow/templates)
- [Flow Connectors (Apps)](https://shopify.dev/docs/apps/flow/connectors)
- [Flow Actions Reference](https://help.shopify.com/en/manual/shopify-flow/actions)

---

**Key Takeaway for SEA Merchants**: AI-enhanced Flow is particularly powerful for managing Southeast Asia's complexity—diverse languages, payment preferences (COD vs. e-wallets vs. cards), regional festivals, multi-market operations, and halal compliance. Automate intelligently, but maintain human oversight for cultural nuance.

**Next:** [App Development →](08-app-development.md)
