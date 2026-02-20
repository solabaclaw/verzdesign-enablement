# Shopify Flow Automation

## What is Shopify Flow?

**Shopify Flow** is a visual workflow automation tool included with Shopify Plus. It allows you to automate repetitive tasks based on triggers, conditions, and actions—no code required.

**Think of it as:** "If this happens, then do that."

### Key Benefits
- **Saves time:** Automate manual tasks (tagging, notifications, hiding products)
- **Reduces errors:** Consistent rule execution (no human mistakes)
- **Scales operations:** Handle more orders without more staff
- **Shopify Plus exclusive:** Included free with Plus plan

## Flow Fundamentals

### Three Building Blocks

#### 1. Trigger
**What starts the workflow?**

Common triggers:
- **Order created**
- **Order paid**
- **Order fulfilled**
- **Customer created**
- **Product created**
- **Inventory quantity changed**
- **Cart created**
- **Refund created**

#### 2. Condition
**Should the action run?**

Filter based on criteria:
- Order total > $500
- Customer is tagged "VIP"
- Product type = "Electronics"
- Inventory level < 10
- Shipping country = "Singapore"

#### 3. Action
**What should happen?**

Common actions:
- **Tag** customer/order/product
- **Send email** to staff or customer
- **Send Slack notification**
- **Hide/unpublish product**
- **Add note to order**
- **Create discount code**
- **Call API** (webhooks to external systems)

### Example Flow

**Workflow:** Tag high-value orders
- **Trigger:** Order created
- **Condition:** Order total ≥ $500
- **Action:** Tag order with "high-value"

**Result:** All orders ≥$500 automatically tagged for priority fulfillment.

## Built-in Workflow Templates

Shopify provides **100+ pre-built templates** for common use cases:

### Popular Templates

#### 1. Customer Loyalty & Segmentation
- **VIP tagging:** Tag customers who spend >$1,000
- **Repeat customer reward:** Send 10% discount code after 3rd purchase
- **Win-back:** Email customers who haven't ordered in 90 days
- **High-value customer alert:** Notify sales team when VIP orders

#### 2. Inventory Management
- **Low stock alert:** Email when inventory <10 units
- **Out of stock:** Unpublish product when inventory = 0
- **Restock notification:** Publish product when inventory >0
- **Variant disabling:** Disable specific variants when sold out

#### 3. Fraud Prevention
- **High-risk order:** Tag orders flagged as high fraud risk
- **Cancel suspicious orders:** Auto-cancel if risk score >0.8
- **Notify for manual review:** Slack alert for medium-risk orders
- **Refund monitoring:** Alert on refunds >$500

#### 4. Order Processing
- **International order routing:** Tag international orders for specific warehouse
- **Gift order handling:** Tag orders with gift message
- **B2B order priority:** Tag B2B customer orders for priority processing
- **Same-day fulfillment:** Tag orders placed before 2 PM for same-day ship

#### 5. Marketing & Engagement
- **Welcome series:** Email new customers with onboarding sequence
- **Product review request:** Email 7 days after fulfillment
- **Birthday discount:** Send discount code on customer birthday
- **Abandoned checkout:** Trigger external email sequence

## Building Custom Flows

### Step-by-Step: Tag VIP Customers

**Goal:** Automatically tag customers who have spent ≥$1,000 total.

**Setup:**
1. **Flow → Create workflow**
2. **Trigger:** Customer updated
3. **Condition:** Total spent ≥ 1000
4. **Action:** Add customer tag "VIP"
5. **Save & turn on**

**Visual:**
```
┌─────────────────┐
│ Customer updated│ (Trigger)
└────────┬────────┘
         │
    ┌────▼────┐
    │ Total   │
    │ spent   │ (Condition)
    │ ≥ $1000 │
    └────┬────┘
         │ Yes
    ┌────▼─────────┐
    │ Tag customer │ (Action)
    │ with "VIP"   │
    └──────────────┘
```

### Step-by-Step: Low Inventory Alert

**Goal:** Get Slack notification when any product has <10 units in stock.

**Setup:**
1. **Flow → Create workflow**
2. **Trigger:** Inventory quantity changed
3. **Condition:** Available quantity <10
4. **Action:** Send Slack message to #inventory channel
5. **Message:** "🚨 Low stock alert: {product.title} has {inventory.available} units left"
6. **Save & turn on**

### Step-by-Step: High-Value Order Notification

**Goal:** Email fulfillment team when order >$500 is placed.

**Setup:**
1. **Flow → Create workflow**
2. **Trigger:** Order created
3. **Condition:** Total price >500
4. **Action:** Send internal email
5. **To:** fulfillment@example.com
6. **Subject:** "Priority Order: {order.name}"
7. **Body:** "High-value order placed: ${order.totalPrice}. Expedite fulfillment."
8. **Save & turn on**

## Advanced Flow Patterns

### 1. Multi-Condition Logic

**Use case:** Tag orders as "Priority" if:
- Order total >$300 OR
- Customer is tagged "VIP" OR
- Product type is "Electronics"

**Setup:**
```
Trigger: Order created
├─ Condition: Total price >300
│  └─ Action: Tag "Priority"
├─ Condition: Customer tagged "VIP"
│  └─ Action: Tag "Priority"
└─ Condition: Product type = "Electronics"
   └─ Action: Tag "Priority"
```

### 2. Sequential Actions

**Use case:** When product goes out of stock:
1. Unpublish product
2. Send email to inventory manager
3. Create task in project management tool

**Setup:**
```
Trigger: Inventory quantity changed
└─ Condition: Available = 0
   ├─ Action 1: Unpublish product
   ├─ Action 2: Send email
   └─ Action 3: Send HTTP request (webhook to Asana/Trello)
```

### 3. Delayed Actions

**Use case:** Send review request 7 days after order fulfillment.

**Setup:**
```
Trigger: Order fulfilled
└─ Wait 7 days
   └─ Action: Send email to customer
      Subject: "How was your order?"
```

**Note:** Delays available: hours, days, weeks (max 30 days).

### 4. Looping Over Line Items

**Use case:** Tag order with all product types in the order.

**Setup:**
```
Trigger: Order created
└─ For each line item:
   └─ Action: Tag order with {lineItem.productType}
```

**Example:**
- Order contains: T-shirt (Apparel) + Hat (Accessories)
- Result: Order tagged with "Apparel" and "Accessories"

## Integrating Third-Party Apps

Flow can trigger actions in **200+ apps** via connectors.

### Popular App Integrations

#### Klaviyo (Email Marketing)
- **Trigger:** Customer tagged "VIP"
- **Action:** Add to Klaviyo VIP segment
- **Result:** Klaviyo sends targeted email campaigns

#### Slack
- **Trigger:** High-value order (>$1,000)
- **Action:** Send Slack message to #sales
- **Result:** Sales team gets instant notification

#### Gorgias (Customer Support)
- **Trigger:** Order tagged "urgent"
- **Action:** Create Gorgias ticket with priority "high"
- **Result:** Support team responds faster

#### Recharge (Subscriptions)
- **Trigger:** Subscription canceled
- **Action:** Tag customer "churn risk"
- **Result:** Win-back email sequence triggers

#### ShipStation (Fulfillment)
- **Trigger:** Order tagged "expedite"
- **Action:** Update ShipStation order with "priority" flag
- **Result:** ShipStation ships same-day

### Custom Webhooks (HTTP Requests)

For apps without Flow connectors, use **HTTP request action**.

**Use case:** Send order data to custom CRM.

**Setup:**
1. **Trigger:** Order created
2. **Action:** Send HTTP request
3. **Method:** POST
4. **URL:** `https://your-crm.com/api/orders`
5. **Headers:** `Authorization: Bearer {api-key}`
6. **Body:**
```json
{
  "order_id": "{{order.id}}",
  "customer_email": "{{order.customer.email}}",
  "total": "{{order.totalPrice}}"
}
```

## APAC-Specific Automation Examples

### 1. Route SEA Orders to Regional Warehouse

**Goal:** Tag orders by region for correct warehouse fulfillment.

```
Trigger: Order created
├─ Condition: Shipping country = SG
│  └─ Action: Tag "Warehouse-SG"
├─ Condition: Shipping country = MY
│  └─ Action: Tag "Warehouse-MY"
├─ Condition: Shipping country = ID
│  └─ Action: Tag "Warehouse-ID"
└─ Condition: Shipping country = TH
   └─ Action: Tag "Warehouse-TH"
```

### 2. Alert for COD Orders (Cash on Delivery)

**Goal:** Notify ops team for COD orders (common in Indonesia/Philippines).

```
Trigger: Order created
└─ Condition: Payment method = "Cash on Delivery"
   └─ Action: Send email to ops@example.com
      Subject: "COD Order: {order.name}"
      Body: "Customer: {customer.name}, Total: {order.totalPrice}"
```

### 3. Halal Product Compliance (Malaysia/Indonesia)

**Goal:** Flag orders with non-halal products for separate processing.

```
Trigger: Order created
└─ For each line item:
   └─ Condition: Product NOT tagged "halal"
      └─ Action: Tag order "non-halal"
         └─ Action: Send email to compliance team
```

### 4. Chinese New Year Promotion Automation

**Goal:** Apply 15% discount during CNY period.

```
Trigger: Order created
└─ Condition: Order date between Jan 20 - Feb 5 (CNY period)
   └─ Condition: Customer country = SG OR MY
      └─ Action: Apply discount code "CNY15"
```

### 5. GST Reporting Tag (Singapore)

**Goal:** Tag orders by GST status for accounting.

```
Trigger: Order created
├─ Condition: Shipping country = SG
│  └─ Action: Tag "GST-applicable"
└─ Condition: Shipping country ≠ SG
   └─ Action: Tag "GST-exempt"
```

## Best Practices

### 1. Start Simple
- Begin with 2-3 workflows (e.g., low stock alert, VIP tagging)
- Test thoroughly before enabling
- Add complexity gradually

### 2. Test Before Enabling
- Use **test mode** (preview runs without executing)
- Check workflow log after enabling
- Verify actions executed correctly

### 3. Avoid Loops
**Bad example:**
```
Trigger: Order tagged
Condition: Tag = "high-value"
Action: Tag order "high-value"  ← Creates infinite loop!
```

**Solution:** Use different tag names or add "not already tagged" condition.

### 4. Monitor Performance
- Check **Flow dashboard** weekly
- Review: runs, errors, execution time
- Disable underperforming or broken workflows

### 5. Document Workflows
- Add description to each workflow (what it does, why)
- Use consistent naming: "VIP Tagging - Lifetime Spend"
- Keep inventory of active flows (spreadsheet or doc)

## Limitations

### Rate Limits
- **Max workflows:** 200 per store
- **Max actions per workflow:** 100
- **Delay limit:** 30 days max
- **HTTP requests:** 1000/day per store

### What Flow CAN'T Do
- **Modify checkout:** Can't change checkout behavior (use Functions instead)
- **Real-time cart changes:** Can't update cart while customer shopping
- **Complex math:** Limited calculations (use external API for complex logic)
- **External data:** Can't fetch data from databases (only webhook responses)

### Trigger Delays
- Workflows trigger **within 1-2 minutes** (not instant)
- For critical real-time tasks, consider Functions or webhooks

## Troubleshooting

### Workflow Not Running
**Check:**
- Is workflow enabled? (toggle on/off)
- Does condition match test case? (review logic)
- Are there errors in log? (check run history)

### Wrong Action Executing
**Check:**
- Condition logic (AND vs OR)
- Data fields (correct variable names)
- Test with sample data

### HTTP Request Failing
**Check:**
- Endpoint URL correct
- API key valid
- Request format (JSON, headers)
- Check external API logs

## Real-World Use Case: Electronics Retailer (Singapore)

**Scenario:** Singapore-based electronics retailer selling across SEA.

**Workflows implemented:**

### 1. VIP Customer Rewards
- **Trigger:** Customer lifetime spend reaches $2,000
- **Action 1:** Tag "VIP"
- **Action 2:** Send email with 15% discount code
- **Action 3:** Notify account manager

### 2. Low Stock Alert (Singapore Warehouse)
- **Trigger:** Inventory <20 units (Singapore location)
- **Condition:** Product type = "Laptops" OR "Phones"
- **Action 1:** Email procurement team
- **Action 2:** Slack alert to #inventory

### 3. High-Value Order Fraud Check
- **Trigger:** Order >$3,000
- **Condition:** Fraud risk score >0.5
- **Action 1:** Tag "manual-review"
- **Action 2:** Slack alert to #fraud-team
- **Action 3:** Hold fulfillment (via webhook to WMS)

### 4. International Shipping Tax Tag
- **Trigger:** Order created
- **Condition:** Shipping country ≠ SG
- **Action:** Tag "export" (for customs docs)

### 5. Product Review Request
- **Trigger:** Order fulfilled
- **Action:** Wait 10 days
- **Action:** Send email requesting review (via Klaviyo)

**Results:**
- Saved 15 hours/week on manual tagging
- Reduced stockouts by 40% (faster alerts)
- Improved fraud detection (zero chargebacks in 6 months)

## Workflow Ideas by Department

### Operations
- Auto-tag orders by fulfillment location
- Alert on orders stuck in processing >48 hours
- Notify when international orders need customs docs

### Marketing
- Tag customers by purchase frequency (1x, 2-3x, 4+ buyers)
- Send birthday discount codes
- Alert sales team when VIP places order

### Customer Support
- Auto-tag refund requests >$100 for manager review
- Create support ticket for damaged item reports
- Tag customers who contact support >3 times

### Finance
- Tag B2B orders with payment terms for invoice processing
- Alert on high-value refunds (>$500)
- Monthly report of orders by payment method

### Inventory
- Unpublish out-of-stock products
- Re-publish when restocked
- Alert on slow-moving inventory (no sales in 60 days)

## Resources

- [Shopify Flow Guide](https://help.shopify.com/en/manual/shopify-flow)
- [Flow Templates Library](https://help.shopify.com/en/manual/shopify-flow/templates)
- [Flow Connectors (Apps)](https://shopify.dev/docs/apps/flow/connectors)
- [Flow Actions Reference](https://help.shopify.com/en/manual/shopify-flow/actions)

---

**Next:** [App Development →](08-app-development.md)
