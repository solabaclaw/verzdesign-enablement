# Shopify Functions

## What Are Shopify Functions?

Shopify Functions are **serverless** customization APIs that run on Shopify's infrastructure, allowing you to inject custom business logic into key commerce workflows without hosting your own servers.

### Key Characteristics
- **WebAssembly (Wasm):** Compiled to Wasm for performance & security
- **Sub-50ms execution:** Lightning-fast, no impact on checkout
- **No hosting required:** Runs on Shopify infrastructure
- **Language-agnostic:** Write in Rust, JavaScript, or AssemblyScript
- **Shopify Plus exclusive:** Functions are a Plus-only feature

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

### Benefits Over Apps
- **Performance:** Runs on Shopify edge, no external API calls
- **Reliability:** No third-party downtime
- **Cost:** No per-transaction fees
- **Simplicity:** Deploy with CLI, no server management

## Function Use Cases

### 1. Discount Functions
Custom discount logic beyond standard discounts:

**Examples:**
- Buy 2, get 3rd at 50% off
- Tiered volume discounts (20+ items = 15% off)
- "Spend $100, get $20 off" with exclusions
- Product bundle discounts
- First-time customer discounts
- Combine multiple discount types

**Key point:** Discount Functions can **stack** with automatic discounts and codes (unlike Script Editor).

### 2. Payment Customization Functions
Control which payment methods appear at checkout:

**Examples:**
- Hide cash on delivery for orders >$500
- Show "Buy Now Pay Later" only for orders >$100
- Require bank transfer for B2B customers
- Show crypto payments for specific products
- Rename payment methods ("Credit Card" → "Pay Securely")

**APAC use case:** Hide international payment methods for domestic orders to reduce choice paralysis.

### 3. Delivery Customization Functions
Control shipping options dynamically:

**Examples:**
- Hide express shipping for PO boxes
- Free shipping for orders >$100 (specific products only)
- Restrict same-day delivery to specific postal codes
- Show different carriers based on cart weight
- Add handling fees for fragile items

**Singapore example:** Show Ninja Van for local, DHL for international.

### 4. Cart & Checkout Validation Functions
Enforce business rules before order completion:

**Examples:**
- Minimum order quantity (B2B: min 10 units per SKU)
- Maximum cart value for specific payment methods
- Require company info for B2B purchases
- Block checkout if mixing retail + wholesale items
- Prevent orders to unsupported regions

## Development Workflow

### Prerequisites
```bash
# Install Shopify CLI
npm install -g @shopify/cli @shopify/app

# Install Rust (for Rust-based functions)
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
# - Product discount
# - Order discount
# - Shipping discount
# - Payment customization
# - Delivery customization
# - Cart transform (beta)
# - Validation (beta)
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

### Example: Volume Discount Function (JavaScript)

```javascript
// extensions/volume-discount/src/run.js

export function run(input) {
  const targets = [];
  
  // Apply 10% discount if cart has 5+ items
  const totalQuantity = input.cart.lines.reduce(
    (sum, line) => sum + line.quantity,
    0
  );
  
  if (totalQuantity >= 5) {
    input.cart.lines.forEach(line => {
      targets.push({
        productVariant: {
          id: line.merchandise.id,
        },
      });
    });
    
    return {
      discounts: [
        {
          targets,
          value: {
            percentage: {
              value: 10,
            },
          },
          message: "10% off for 5+ items",
        },
      ],
    };
  }
  
  return {
    discounts: [],
  };
}
```

### Example: Hide COD for High-Value Orders (Payment Customization)

```javascript
export function run(input) {
  const hidePaymentMethods = [];
  
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);
  
  // Hide Cash on Delivery if order > $500
  if (cartTotal > 500) {
    const codMethod = input.paymentMethods.find(
      method => method.name === "Cash on Delivery"
    );
    
    if (codMethod) {
      hidePaymentMethods.push({
        paymentMethodId: codMethod.id,
        reason: "Not available for orders over $500",
      });
    }
  }
  
  return {
    operations: hidePaymentMethods.map(method => ({
      hide: {
        paymentMethodId: method.paymentMethodId,
      },
    })),
  };
}
```

### Example: Postal Code Validation (Validation Function)

```javascript
export function run(input) {
  const errors = [];
  
  const shippingAddress = input.cart.deliveryGroups[0]?.deliveryAddress;
  
  if (shippingAddress?.countryCode === "SG") {
    const postalCode = shippingAddress.zip || "";
    
    // Singapore postal codes are 6 digits
    if (!/^\d{6}$/.test(postalCode)) {
      errors.push({
        localizedMessage: "Please enter a valid 6-digit Singapore postal code",
        target: "$.cart.deliveryGroups[0].deliveryAddress.zip",
      });
    }
  }
  
  return {
    errors,
  };
}
```

## Rust vs JavaScript

### JavaScript (Recommended for Most)
**Pros:**
- Familiar syntax for web developers
- Faster development
- Easier debugging
- Good performance (<50ms)

**Cons:**
- Slightly slower than Rust
- Larger bundle size

**When to use:** Most discount, payment, delivery customizations.

### Rust (Advanced)
**Pros:**
- Maximum performance (<10ms)
- Smaller bundle size
- Strong type safety

**Cons:**
- Steeper learning curve
- Longer compile times
- Fewer developers know Rust

**When to use:** Complex logic, high-traffic stores (1000+ orders/hour).

## Testing & Deployment

### Local Testing
```bash
npm run dev

# CLI provides test queries
# Simulate different cart states
```

### Deploy
```bash
npm run deploy

# Function is deployed to Shopify
# Activate in Shopify admin
```

### Activate in Admin
1. **Settings → Apps and sales channels**
2. Find your app
3. **Create [Function Type]** (e.g., Create Discount)
4. Configure function settings
5. Set priority (if multiple functions)
6. Activate

### Testing in Production
- Use test mode orders
- Test edge cases:
  - Empty cart
  - Multiple items
  - International addresses
  - Different customer segments (B2B, retail)
- Monitor performance metrics

## Migrating from Script Editor

### Script Editor Sunset
- **New stores:** Cannot use Script Editor (Functions only)
- **Existing stores:** Script Editor supported until **December 2026**
- **Migration deadline:** Move to Functions by end of 2026

### Migration Strategy

#### 1. Inventory Scripts
Document all active scripts:
- Line item scripts (discounts)
- Shipping scripts
- Payment scripts

#### 2. Map to Functions
| Script Type | Function Type |
|-------------|---------------|
| Line item (discount) | Discount Function |
| Shipping | Delivery Customization |
| Payment | Payment Customization |

#### 3. Rewrite Logic
- Convert Ruby → JavaScript or Rust
- Test extensively
- Deploy alongside scripts (parallel run)

#### 4. Cut Over
- Activate Function
- Deactivate Script
- Monitor for issues

### Example Migration

**Script Editor (Ruby):**
```ruby
# Hide COD for orders > $500
if cart.total_price > 50000 # cents
  payment_gateways.delete_if { |pg| pg.name == "Cash on Delivery" }
end
```

**Shopify Function (JavaScript):**
```javascript
export function run(input) {
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);
  
  if (cartTotal > 500) {
    const codMethod = input.paymentMethods.find(
      method => method.name === "Cash on Delivery"
    );
    
    if (codMethod) {
      return {
        operations: [{
          hide: {
            paymentMethodId: codMethod.id,
          },
        }],
      };
    }
  }
  
  return { operations: [] };
}
```

## Performance Optimization

### Best Practices
1. **Keep logic simple:** Fewer loops, less complexity
2. **Minimize calculations:** Pre-calculate when possible
3. **Use early returns:** Skip unnecessary work
4. **Avoid external calls:** No HTTP requests allowed
5. **Test with large carts:** 100+ line items

### Performance Targets
- **Target:** <20ms execution
- **Warning:** 20-50ms (still acceptable)
- **Critical:** >50ms (needs optimization)

### Monitoring
- View function metrics in Partner Dashboard
- Track: execution time, error rate, activation count
- Set up alerts for >50ms average

## APAC-Specific Function Examples

### 1. Local Payment Methods (Singapore)
```javascript
// Show PayNow only for SG customers
export function run(input) {
  const countryCode = input.cart.deliveryGroups[0]?.deliveryAddress?.countryCode;
  
  if (countryCode !== "SG") {
    const paynow = input.paymentMethods.find(m => m.name === "PayNow");
    if (paynow) {
      return {
        operations: [{
          hide: { paymentMethodId: paynow.id },
        }],
      };
    }
  }
  
  return { operations: [] };
}
```

### 2. GST-Exempt Products (B2B)
```javascript
// Remove GST for specific B2B customers
export function run(input) {
  const customer = input.cart.buyerIdentity?.customer;
  const isB2B = customer?.tags?.includes('b2b');
  
  if (isB2B) {
    const targets = input.cart.lines
      .filter(line => line.merchandise.product.tags.includes('gst-exempt'))
      .map(line => ({
        productVariant: { id: line.merchandise.id },
      }));
    
    // Apply 9% discount to offset GST
    return {
      discounts: [{
        targets,
        value: { percentage: { value: 9 } },
        message: "GST waived (B2B)",
      }],
    };
  }
  
  return { discounts: [] };
}
```

## Limitations & Constraints

### What Functions CAN'T Do
- Make external API calls (no HTTP requests)
- Access databases
- Use randomness (must be deterministic)
- Store state between executions
- Run for >5 seconds

### Input Data Limits
- Cart: Max 250 line items
- Customer metafields: Max 10
- Product metafields: Max 10

### Rate Limits
- No per-function rate limit (runs on every checkout)
- But must complete <5 seconds

## Troubleshooting

### Common Errors

**"Function exceeded time limit"**
- Simplify logic
- Remove unnecessary loops
- Use early returns

**"Invalid target"**
- Check product/variant IDs
- Ensure target still exists in cart

**"Function not activating"**
- Verify function is enabled in admin
- Check function priority
- Review activation conditions

## Resources

- [Functions API Reference](https://shopify.dev/docs/api/functions)
- [Discount Functions Guide](https://shopify.dev/docs/apps/discounts)
- [Payment Customization](https://shopify.dev/docs/apps/payment-customizations)
- [Delivery Customization](https://shopify.dev/docs/apps/delivery-customizations)
- [Functions Examples Repo](https://github.com/Shopify/function-examples)

---

**Next:** [Headless & Hydrogen →](04-headless-hydrogen.md)
