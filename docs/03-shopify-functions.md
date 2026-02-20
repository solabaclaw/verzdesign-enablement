# Shopify Functions: AI-Driven Commerce Logic for Southeast Asia

## What Are Shopify Functions?

Shopify Functions are **serverless** customization APIs that run on Shopify's infrastructure, allowing you to inject custom business logic into key commerce workflows. Combined with Shopify's AI capabilities — semantic search, recommendation engine, and predictive analytics — Functions become the programmable backbone for intelligent, automated commerce.

### Key Characteristics
- **WebAssembly (Wasm):** Compiled to Wasm for performance & security
- **Sub-50ms execution:** Lightning-fast, no impact on checkout
- **No hosting required:** Runs on Shopify infrastructure
- **Language-agnostic:** Write in Rust, JavaScript, or AssemblyScript
- **Shopify Plus exclusive:** Functions are a Plus-only feature
- **AI-compatible:** Functions can consume AI-generated data (customer segments, predictive scores) via metafields

## Why Shopify Functions?

### Replaces Script Editor
Script Editor (Ruby-based) is being phased out. Functions are the modern replacement:

| Script Editor (Legacy) | Shopify Functions (Modern) |
|------------------------|---------------------------|
| Ruby (Shopify-specific) | Rust, JavaScript (standard) |
| ~200ms execution time | <50ms execution time |
| Limited to discounts, shipping, payments | Growing API surface |
| No local development | CLI-based development |
| Deprecated (2026 sunset) | Future-proof |
| No AI integration | Can leverage AI data via metafields |

### Benefits Over Apps
- **Performance:** Runs on Shopify edge, no external API calls
- **Reliability:** No third-party downtime
- **Cost:** No per-transaction fees
- **Simplicity:** Deploy with CLI, no server management

## AI + Functions: Intelligent Commerce Logic

### How AI Data Feeds Functions

Shopify's AI generates insights that Functions can act on:

```
AI Layer (Shopify Platform)          →    Functions Layer (Your Logic)
─────────────────────────────        →    ──────────────────────────
Customer predictive LTV score        →    Tiered discounts for high-LTV customers
Churn risk prediction                →    Win-back discount at checkout
Product affinity clusters            →    Smart bundling discounts
Seasonal demand forecasting          →    Dynamic shipping rules
Customer segment membership          →    Payment method visibility
```

**Example flow:**
1. Shopify AI scores a customer as "high-LTV, fashion-forward, Singapore-based"
2. This data is stored as customer metafields
3. Your Discount Function reads these metafields
4. Function applies a VIP discount + shows premium payment options

### AI-Powered Smart Discounting

Traditional discounts are static rules. AI-informed Functions create dynamic, personalized pricing:

```javascript
// Smart discount: AI-predicted high-value customers get loyalty pricing
export function run(input) {
  const customer = input.cart.buyerIdentity?.customer;
  const predictedLTV = customer?.metafield?.value; // AI-generated LTV score
  
  if (!predictedLTV || predictedLTV < 500) {
    return { discounts: [] };
  }
  
  // High-LTV customers (predicted SGD 500+ lifetime value)
  const tier = predictedLTV >= 2000 ? 15 : predictedLTV >= 1000 ? 10 : 5;
  
  const targets = input.cart.lines.map(line => ({
    productVariant: { id: line.merchandise.id },
  }));
  
  return {
    discounts: [{
      targets,
      value: { percentage: { value: tier } },
      message: `VIP ${tier}% loyalty discount`,
    }],
  };
}
```

## Function Use Cases

### 1. Discount Functions
Custom discount logic beyond standard discounts:

**Examples:**
- Buy 2, get 3rd at 50% off
- Tiered volume discounts (20+ items = 15% off)
- "Spend SGD 150, get SGD 20 off" with exclusions
- Product bundle discounts
- **AI-informed:** Higher discounts for customers with high churn risk
- **AI-informed:** Personalized discount tiers based on predicted LTV
- Combine multiple discount types (stacking)

### 2. Payment Customization Functions
Control which payment methods appear at checkout:

**SEA Examples:**
- Show PayNow only for Singapore customers
- Show GrabPay only for SG and MY
- Show FPX only for Malaysian customers
- Hide Cash on Delivery for orders >SGD 500
- Show "Buy Now Pay Later" (Atome, Pace) only for orders >SGD 100
- Require bank transfer for B2B customers
- **AI-informed:** Show preferred payment method first based on customer's past behavior

### 3. Delivery Customization Functions
Control shipping options dynamically:

**SEA Examples:**
- Show Ninja Van for SG/MY/ID, J&T for ID/TH/VN
- Free shipping for orders >SGD 80 (SG) / MYR 150 (MY) / IDR 300K (ID)
- Restrict same-day delivery to specific postal codes in Singapore
- Show different carriers based on cart weight
- Hide express shipping for remote areas (Sabah/Sarawak in MY, outer islands in ID)
- **AI-informed:** Suggest delivery speed based on predicted urgency (gift vs personal use)

### 4. Cart & Checkout Validation Functions
Enforce business rules before order completion:

**Examples:**
- Minimum order quantity (B2B: min 10 units per SKU)
- Maximum cart value for COD (fraud prevention — critical in SEA)
- Block checkout if shipping to unsupported regions
- Validate Singapore postal codes (6 digits)
- Validate Malaysian postal codes (5 digits)
- **AI-informed:** Flag high-risk orders based on behavioral patterns

## Development Workflow

### Prerequisites
```bash
# Install Shopify CLI
npm install -g @shopify/cli @shopify/app

# Install Rust (optional, for Rust-based functions)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup target add wasm32-wasi
```

### Create a Function

```bash
# Create new app (if you don't have one)
npm init @shopify/app@latest
cd my-app

# Generate a function
npm run shopify app generate extension
# Select function type:
# - Product discount / Order discount / Shipping discount
# - Payment customization
# - Delivery customization
# - Cart transform / Validation
```

### Function Structure

```
my-app/
├── extensions/
│   └── my-discount-function/
│       ├── src/
│       │   └── run.js (or run.rs for Rust)
│       ├── shopify.extension.toml
│       └── package.json
```

### Example: Volume Discount Function

```javascript
// extensions/volume-discount/src/run.js
export function run(input) {
  const targets = [];
  
  const totalQuantity = input.cart.lines.reduce(
    (sum, line) => sum + line.quantity, 0
  );
  
  if (totalQuantity >= 5) {
    input.cart.lines.forEach(line => {
      targets.push({
        productVariant: { id: line.merchandise.id },
      });
    });
    
    return {
      discounts: [{
        targets,
        value: { percentage: { value: 10 } },
        message: "10% off for 5+ items",
      }],
    };
  }
  
  return { discounts: [] };
}
```

### Example: SEA Payment Routing

```javascript
// Show/hide payment methods based on customer country
export function run(input) {
  const countryCode = input.cart.deliveryGroups[0]?.deliveryAddress?.countryCode;
  const operations = [];
  
  const countryPayments = {
    SG: ['PayNow', 'GrabPay'],
    MY: ['FPX', 'GrabPay', 'Boost'],
    ID: ['DANA', 'OVO', 'GoPay'],
    TH: ['PromptPay', 'TrueMoney'],
    PH: ['GCash', 'Maya'],
    VN: ['MoMo', 'ZaloPay'],
  };
  
  // Hide local payment methods that don't belong to the customer's country
  const localMethods = Object.values(countryPayments).flat();
  const allowedMethods = countryPayments[countryCode] || [];
  
  input.paymentMethods.forEach(method => {
    if (localMethods.includes(method.name) && !allowedMethods.includes(method.name)) {
      operations.push({
        hide: { paymentMethodId: method.id },
      });
    }
  });
  
  return { operations };
}
```

### Example: Singapore Postal Code Validation

```javascript
export function run(input) {
  const errors = [];
  const shippingAddress = input.cart.deliveryGroups[0]?.deliveryAddress;
  
  if (shippingAddress?.countryCode === "SG") {
    const postalCode = shippingAddress.zip || "";
    if (!/^\d{6}$/.test(postalCode)) {
      errors.push({
        localizedMessage: "Please enter a valid 6-digit Singapore postal code",
        target: "$.cart.deliveryGroups[0].deliveryAddress.zip",
      });
    }
  }
  
  if (shippingAddress?.countryCode === "MY") {
    const postalCode = shippingAddress.zip || "";
    if (!/^\d{5}$/.test(postalCode)) {
      errors.push({
        localizedMessage: "Please enter a valid 5-digit Malaysian postal code",
        target: "$.cart.deliveryGroups[0].deliveryAddress.zip",
      });
    }
  }
  
  return { errors };
}
```

### Example: AI-Informed COD Fraud Prevention

```javascript
// Hide COD for high-risk signals (common fraud vector in SEA)
export function run(input) {
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);
  const customer = input.cart.buyerIdentity?.customer;
  const countryCode = input.cart.deliveryGroups[0]?.deliveryAddress?.countryCode;
  
  // AI-generated risk score stored as customer metafield
  const riskScore = parseFloat(customer?.metafield?.value || "0");
  
  const shouldHideCOD = (
    cartTotal > 500 ||           // High-value orders
    riskScore > 0.7 ||           // AI flagged as high-risk
    !customer                     // Guest checkout (no history)
  );
  
  if (shouldHideCOD) {
    const codMethod = input.paymentMethods.find(
      m => m.name === "Cash on Delivery"
    );
    if (codMethod) {
      return {
        operations: [{ hide: { paymentMethodId: codMethod.id } }],
      };
    }
  }
  
  return { operations: [] };
}
```

### Example: GST-Exempt B2B (Singapore)

```javascript
// Remove GST for verified B2B customers
export function run(input) {
  const customer = input.cart.buyerIdentity?.customer;
  const isB2B = customer?.tags?.includes('b2b');
  const countryCode = input.cart.deliveryGroups[0]?.deliveryAddress?.countryCode;
  
  if (isB2B && countryCode === 'SG') {
    const targets = input.cart.lines.map(line => ({
      productVariant: { id: line.merchandise.id },
    }));
    
    return {
      discounts: [{
        targets,
        value: { percentage: { value: 9 } }, // Offset 9% GST
        message: "GST waived (B2B)",
      }],
    };
  }
  
  return { discounts: [] };
}
```

## Shopify's AI APIs & Data for Functions

### Customer Metafields (AI-Generated)

Shopify's AI populates customer data that Functions can consume:

| **Metafield** | **AI Source** | **Function Use** |
|---------------|-------------|-----------------|
| Predicted LTV | Shopify Analytics AI | Tiered discounts, VIP pricing |
| Churn risk score | Customer segmentation AI | Win-back discounts |
| Product affinity | Recommendation engine | Smart bundling |
| Preferred payment | Behavioral analysis | Payment method ordering |
| Customer segment | Predictive segmentation | Conditional logic |

### Leveraging Sidekick for Function Strategy

Before writing Functions, use Sidekick to analyze your data:

```
Merchant: "What payment methods do my Singapore customers prefer?"

Sidekick:
- PayNow: 35% of SG orders
- Credit card: 42% of SG orders  
- GrabPay: 18% of SG orders
- Atome (BNPL): 5% of SG orders
- Insight: Orders >SGD 200 prefer credit card (68%)
- Recommendation: Show credit card first for high-value carts, PayNow first otherwise
```

This analysis directly informs your Payment Customization Function logic.

## Rust vs JavaScript

### JavaScript (Recommended for Most)
**Pros:** Familiar syntax, faster development, easier debugging
**Cons:** Slightly slower, larger bundle size
**When to use:** Most discount, payment, delivery customizations

### Rust (Advanced)
**Pros:** Maximum performance (<10ms), smaller bundle, strong types
**Cons:** Steeper learning curve, longer compile times
**When to use:** Complex logic, ultra-high-traffic stores (1000+ orders/hour)

## Testing & Deployment

### Local Testing
```bash
npm run dev
# CLI provides test queries — simulate different cart states
```

### Deploy
```bash
npm run deploy
# Function is deployed to Shopify → activate in admin
```

### Activate in Admin
1. Settings → Apps and sales channels
2. Find your app
3. Create [Function Type] (e.g., Create Discount)
4. Configure function settings
5. Set priority (if multiple functions)
6. Activate

### Testing Checklist for SEA
- [ ] Empty cart vs full cart
- [ ] Domestic (SG) vs cross-border (MY, ID, TH, PH, VN)
- [ ] Local payment methods per country
- [ ] Multiple currencies (SGD, MYR, IDR, THB, PHP, VND)
- [ ] B2B vs retail customers
- [ ] Discount stacking scenarios
- [ ] COD fraud prevention triggers
- [ ] Postal code validation per country
- [ ] High-traffic simulation (11.11, 12.12 sales)

## Migrating from Script Editor

### Timeline
- **New stores:** Cannot use Script Editor (Functions only)
- **Existing stores:** Script Editor supported until **December 2026**
- **Migration deadline:** Move to Functions by end of 2026

### Migration Strategy

1. **Inventory:** Document all active scripts (line item, shipping, payment)
2. **Map:** Line item → Discount Function, Shipping → Delivery Customization, Payment → Payment Customization
3. **Rewrite:** Convert Ruby → JavaScript/Rust, **add AI enhancements** (predictive LTV tiers, smart payment routing)
4. **Parallel run:** Deploy alongside scripts, validate behavior matches
5. **Cut over:** Activate Function, deactivate Script, monitor

### Example Migration (Enhanced with AI)

**Script Editor (Ruby) — original:**
```ruby
# Hide COD for orders > $500
if cart.total_price > 50000
  payment_gateways.delete_if { |pg| pg.name == "Cash on Delivery" }
end
```

**Shopify Function (JavaScript) — AI-enhanced:**
```javascript
export function run(input) {
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);
  const customer = input.cart.buyerIdentity?.customer;
  const riskScore = parseFloat(customer?.metafield?.value || "0");
  
  // Enhanced: AI risk score + cart value threshold
  if (cartTotal > 500 || riskScore > 0.7) {
    const codMethod = input.paymentMethods.find(
      m => m.name === "Cash on Delivery"
    );
    if (codMethod) {
      return {
        operations: [{ hide: { paymentMethodId: codMethod.id } }],
      };
    }
  }
  
  return { operations: [] };
}
```

**Improvement:** Original only checked cart value. AI-enhanced version also considers customer fraud risk, catching more problematic orders while allowing legitimate high-value COD from trusted customers.

## Performance Optimization

### Best Practices
1. **Keep logic simple:** Fewer loops, less complexity
2. **Use early returns:** Skip unnecessary work
3. **Avoid complex calculations:** Pre-calculate via metafields
4. **Test with large carts:** 100+ line items
5. **Leverage AI metafields:** Let Shopify AI do heavy computation, Functions just read results

### Performance Targets
- **Target:** <20ms execution
- **Warning:** 20-50ms (still acceptable)
- **Critical:** >50ms (needs optimization)

## SEA-Specific Function Patterns

### 1. Mega-Sale Tiered Discounts (11.11 / 12.12)
```javascript
export function run(input) {
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);
  
  let discountPercent = 0;
  let message = "";
  
  if (cartTotal >= 300) {
    discountPercent = 20;
    message = "12.12 Mega Deal: 20% off orders over SGD 300!";
  } else if (cartTotal >= 150) {
    discountPercent = 12;
    message = "12.12 Deal: 12% off orders over SGD 150!";
  } else if (cartTotal >= 50) {
    discountPercent = 5;
    message = "12.12: 5% off orders over SGD 50";
  }
  
  if (discountPercent === 0) return { discounts: [] };
  
  const targets = input.cart.lines.map(line => ({
    productVariant: { id: line.merchandise.id },
  }));
  
  return {
    discounts: [{ targets, value: { percentage: { value: discountPercent } }, message }],
  };
}
```

### 2. Cross-Border Shipping Logic
```javascript
export function run(input) {
  const countryCode = input.cart.deliveryGroups[0]?.deliveryAddress?.countryCode;
  const operations = [];
  
  const domesticCarriers = {
    SG: ['Ninja Van SG', 'Grab Express'],
    MY: ['Ninja Van MY', 'J&T Express MY'],
    ID: ['J&T Express ID', 'SiCepat'],
    TH: ['Flash Express', 'Kerry Express'],
  };
  
  // Hide domestic carriers that don't serve the destination country
  Object.entries(domesticCarriers).forEach(([country, carriers]) => {
    if (country !== countryCode) {
      // Hide these carriers as they're for a different country
      input.deliveryOptions?.forEach(option => {
        if (carriers.includes(option.title)) {
          operations.push({ hide: { deliveryOptionHandle: option.handle } });
        }
      });
    }
  });
  
  return { operations };
}
```

## Limitations & Constraints

### What Functions CAN'T Do
- Make external API calls (no HTTP requests)
- Access databases directly
- Use randomness (must be deterministic)
- Store state between executions
- Run for >5 seconds

### Input Data Limits
- Cart: Max 250 line items
- Customer metafields: Max 10
- Product metafields: Max 10

### Workaround: AI Metafields
Since Functions can't call external APIs, use Shopify Flow + AI to **pre-compute** intelligence into metafields that Functions read at execution time. This is the recommended pattern for AI-informed Functions.

## Troubleshooting

### Common Errors

**"Function exceeded time limit"** — Simplify logic, remove loops, use early returns

**"Invalid target"** — Check product/variant IDs still exist in cart

**"Function not activating"** — Verify enabled in admin, check priority, review conditions

**SEA-specific issues:**
- **Currency precision:** IDR and VND have no decimal places — handle rounding carefully
- **Address formats:** SEA addresses vary widely — validate flexibly
- **Payment method names:** May differ by gateway provider — test with actual payment config

## Resources

- [Functions API Reference](https://shopify.dev/docs/api/functions)
- [Discount Functions Guide](https://shopify.dev/docs/apps/discounts)
- [Payment Customization](https://shopify.dev/docs/apps/payment-customizations)
- [Delivery Customization](https://shopify.dev/docs/apps/delivery-customizations)
- [Functions Examples Repo](https://github.com/Shopify/function-examples)
- [Shopify AI & Magic](https://www.shopify.com/magic)

---

**Next:** [Headless & Hydrogen →](04-headless-hydrogen.md)
