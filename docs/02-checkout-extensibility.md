# Checkout Extensibility: AI-Powered Checkout for Southeast Asia

## Overview

Checkout Extensibility is Shopify's modern, component-based checkout system that replaces the legacy checkout.liquid template. Combined with AI-powered personalization, smart recommendations, and regional payment optimization, it delivers the conversion rates SEA merchants need to compete with marketplace giants.

**Deadline:** All Plus stores must migrate from checkout.liquid by **August 13, 2025**.

## Why Checkout Extensibility?

### Problems with checkout.liquid
- **Performance:** Custom code slows page load
- **Security:** Direct DOM access creates vulnerabilities
- **Maintenance:** Breaking changes with Shopify updates
- **Mobile:** Harder to optimize (critical — 70%+ of SEA traffic is mobile)
- **Apps:** Conflicts between multiple apps
- **No AI integration:** Cannot leverage Shopify's recommendation engine

### Benefits of Extensibility
- **3x faster** checkout load times
- **Component-based:** Pre-optimized UI components
- **AI-ready:** Integrate smart recommendations and personalization at checkout
- **App-friendly:** Apps work together without conflicts
- **Future-proof:** Shopify maintains compatibility
- **Conversion:** Average 15% improvement in mobile conversion
- **Branding:** Full design control via Branding API

## Three Pillars of Checkout Extensibility

### 1. Checkout UI Extensions
**Add custom functionality to checkout**

Build custom app blocks that inject into checkout:
- Custom fields (gift messages, delivery instructions)
- **AI-powered upsells & cross-sells** (personalized per customer)
- Subscription options
- Trust badges & social proof
- Marketing opt-ins
- Custom validations

**Extension targets:**
- `purchase.checkout.block.render` — Static content
- `purchase.checkout.actions.render-before` — Before payment buttons
- `purchase.checkout.contact.render-after` — After contact info
- `purchase.checkout.delivery-address.render-after` — After address
- `purchase.checkout.shipping-option-list.render-after` — After shipping
- `purchase.checkout.reductions.render-after` — After discounts

### 2. Checkout Branding API
**Design checkout to match brand**

Customize via `checkoutBranding` API or admin UI:
- Colors (primary, backgrounds, accents)
- Typography (fonts, sizes, weights)
- Border radius & styling
- Button styles
- Form inputs & spacing

### 3. Payment & Delivery Customization (via Functions)
**Control what options appear — powered by AI logic**

- **Payment Customization:** Hide, reorder, rename payment methods based on customer segment, cart value, or geography
- **Delivery Customization:** Dynamic shipping rates, hide methods by region
- Covered in detail in [Shopify Functions](03-shopify-functions.md)

## AI-Powered Checkout Personalization

### Smart Product Recommendations at Checkout

Shopify's AI recommendation engine can power checkout upsells and cross-sells that adapt per customer:

**How it works:**
- AI analyzes cart contents, customer purchase history, and browsing behavior
- Collaborative filtering leverages the entire Shopify network (millions of stores)
- Recommendations are contextually relevant — not random products

**Example: Fashion Brand (Singapore)**
```
Cart: Women's Batik Dress (SGD 89)

AI recommends at checkout:
1. Matching clutch bag (SGD 39) — 62% co-purchase rate
2. Pearl earrings (SGD 29) — "Complete the Look" 
3. Gift wrapping (SGD 5) — high attach rate during festive periods

Result: 28% of customers add at least one item
Average AOV lift: SGD 24 per order
```

**Example: Electronics (Malaysia)**
```
Cart: Wireless earbuds (MYR 299)

AI recommends:
1. Protective case (MYR 49) — 55% co-purchase rate
2. Extended warranty (MYR 39) — 32% attach rate
3. USB-C cable (MYR 19) — complementary accessory

Result: Accessories attach rate increases from 15% to 38%
```

### Personalized Checkout Experience by Segment

AI enables different checkout experiences based on customer data:

| **Customer Segment** | **Checkout Personalization** |
|---------------------|----------------------------|
| First-time visitor | Trust badges, payment security messaging, popular items upsell |
| Returning customer | "Buy again" suggestions, loyalty points display |
| High-LTV customer | Premium upsells, exclusive bundle offers |
| B2B buyer | Payment terms display, bulk discount summary |
| Cart abandoner (returning) | Urgency messaging, limited-time discount |

### AI-Driven Urgency & Social Proof

```javascript
function SmartSocialProof() {
  // AI determines which social proof message converts best
  // for this customer segment and product category
  return (
    <Banner>
      🔥 23 people bought this today in Singapore
    </Banner>
  );
}
```

## Building Checkout UI Extensions

### Tech Stack
- **React** (or vanilla JavaScript)
- **Shopify UI Extensions API**
- **TypeScript** recommended
- Built with Shopify CLI

### Development Workflow

```bash
# Create new checkout extension
npm init @shopify/app@latest
cd my-app
npm run dev

# Generate checkout UI extension
npm run shopify app generate extension
# Select: Checkout UI
```

### Basic Extension Example

```javascript
import {
  Banner,
  useApi,
  useTranslate,
  reactExtension,
} from '@shopify/ui-extensions-react/checkout';

export default reactExtension(
  'purchase.checkout.block.render',
  () => <Extension />
);

function Extension() {
  const { cost } = useApi();
  const translate = useTranslate();
  
  const totalAmount = parseFloat(cost.totalAmount.amount);
  
  if (totalAmount >= 100) {
    return (
      <Banner title="Free shipping activated!">
        Your order qualifies for free shipping across Singapore 🎉
      </Banner>
    );
  }
  
  return null;
}
```

### AI-Powered Upsell Extension

```javascript
import {
  Banner,
  Button,
  Image,
  Text,
  View,
  useApi,
  reactExtension,
} from '@shopify/ui-extensions-react/checkout';

export default reactExtension(
  'purchase.checkout.block.render',
  () => <SmartUpsell />
);

function SmartUpsell() {
  const { lines, applyCartLinesChange, cost } = useApi();
  const [loading, setLoading] = useState(false);

  // Fetch AI-recommended product based on cart contents
  // Uses Shopify's recommendation engine API
  const recommendation = useRecommendation(lines);
  
  if (!recommendation) return null;
  
  const addUpsell = async () => {
    setLoading(true);
    await applyCartLinesChange({
      type: 'addCartLine',
      merchandiseId: recommendation.variantId,
      quantity: 1,
    });
    setLoading(false);
  };
  
  return (
    <Banner title="Frequently bought together">
      <View>
        <Text>{recommendation.title} — {recommendation.price}</Text>
        <Text size="small">{recommendation.reason}</Text>
        <Button onPress={addUpsell} loading={loading}>
          Add to order
        </Button>
      </View>
    </Banner>
  );
}
```

### Common Use Cases for SEA

#### 1. Gift Message (Popular During Festive Seasons)
```javascript
function GiftMessage() {
  const [message, setMessage] = useState('');
  
  return (
    <TextField
      label="Gift message (optional)"
      value={message}
      onChange={setMessage}
      multiline={3}
      placeholder="Happy Chinese New Year! 🧧"
    />
  );
}
```

#### 2. Delivery Instructions (Critical for SEA Addresses)
```javascript
function DeliveryInstructions() {
  const { applyAttributeChange, attributes } = useApi();
  
  const instructions = attributes.find(
    attr => attr.key === 'delivery_instructions'
  )?.value || '';
  
  return (
    <TextField
      label="Delivery instructions"
      value={instructions}
      onChange={(value) => {
        applyAttributeChange({
          type: 'updateAttribute',
          key: 'delivery_instructions',
          value,
        });
      }}
      placeholder="e.g., Leave with lobby security / Condo unit #12-05"
    />
  );
}
```

#### 3. Regional Payment Trust Badges
```javascript
function PaymentTrustBadge() {
  const { deliveryGroups } = useApi();
  const country = deliveryGroups[0]?.deliveryAddress?.countryCode;
  
  const badges = {
    SG: "We accept PayNow, GrabPay & all major cards 🇸🇬",
    MY: "We accept FPX, GrabPay, Boost & all major cards 🇲🇾",
    ID: "We accept DANA, OVO, GoPay & bank transfer 🇮🇩",
    TH: "We accept PromptPay, TrueMoney & all major cards 🇹🇭",
    PH: "We accept GCash, Maya & all major cards 🇵🇭",
  };
  
  const message = badges[country] || "Secure payment with all major methods";
  
  return <Banner>{message}</Banner>;
}
```

#### 4. GST Display (Singapore)
```javascript
function GSTBreakdown() {
  const { cost } = useApi();
  const subtotal = parseFloat(cost.subtotalAmount.amount);
  const gst = subtotal * 0.09; // 9% GST
  
  return (
    <View>
      <Text>Subtotal: S${subtotal.toFixed(2)}</Text>
      <Text>GST (9%): S${gst.toFixed(2)}</Text>
    </View>
  );
}
```

## Checkout Branding Setup

### Via Admin UI
1. Settings → Checkout → Customize
2. Adjust colors, fonts, buttons
3. Preview in real-time
4. Publish changes

### Via Branding API

```graphql
mutation checkoutBrandingUpsert($checkoutBrandingInput: CheckoutBrandingInput!) {
  checkoutBrandingUpsert(checkoutBrandingInput: $checkoutBrandingInput) {
    checkoutBranding {
      designSystem {
        colors {
          global { accent, brand, critical }
        }
        typography {
          primary { base { weight } }
        }
      }
    }
  }
}
```

## Migration from checkout.liquid

### Assessment Phase
1. **Inventory customizations:**
   - Document all checkout.liquid edits
   - List installed checkout apps
   - Track custom scripts & tracking pixels
2. **Map to new approach:**
   - Fields → UI Extensions
   - Custom logic → Shopify Functions
   - Branding → Branding API
   - Scripts → Web Pixels API
   - **New:** Add AI recommendation extensions

### Migration Steps

#### Step 1: Enable Checkout Extensibility
- Admin → Settings → Checkout → "Upgrade to Checkout Extensibility"

#### Step 2: Rebuild Customizations + Add AI
- Build UI Extensions for custom fields
- **Add AI-powered upsell/cross-sell extensions**
- Use Functions for payment/shipping logic
- Apply branding via API or admin
- Add web pixels for tracking

#### Step 3: Test Thoroughly
- Test on staging store first
- Mobile & desktop (prioritize mobile — 70%+ SEA traffic)
- Different payment methods (GrabPay, PayNow, FPX, etc.)
- International orders across SEA markets
- AI recommendation relevance for different cart compositions

#### Step 4: Cut Over
- Publish extensions
- Disable checkout.liquid customizations
- Monitor conversion rates closely
- Have rollback plan ready

### Common Migration Patterns

| checkout.liquid | Extensibility Solution |
|----------------|------------------------|
| Custom input field | Checkout UI Extension (TextField) |
| Upsell widget | **AI-powered** Checkout UI Extension |
| Hide payment method | Payment Customization Function |
| Custom shipping rate | Delivery Customization Function |
| Brand styling | Checkout Branding API |
| Google Analytics | Web Pixels API |
| Address validation | Validation Function |
| Gift wrap checkbox | UI Extension + cart attributes |

## AI Recommendations: Cross-Sell & Upsell Strategy

### Revenue Impact Model for SEA Merchants

| **Tactic** | **Typical AOV Lift** | **Implementation** |
|------------|---------------------|-------------------|
| Product page related items | 5-10% | Native AI recommendations |
| Cart page cross-sell | 10-20% | AI + UI Extension |
| **Checkout AI upsell** | **8-15%** | **Checkout UI Extension** |
| Post-purchase upsell | 5-12% | Flow + thank you page |
| Bundle recommendations | 15-25% | AI bundling logic |
| Email cross-sell | 3-8% | Automated + AI picks |

### SEA-Specific Cross-Sell Patterns

**Fashion & Lifestyle:**
- Batik dress → matching clutch + sandals
- Cheongsam → jade jewelry + silk shawl
- Athleisure set → water bottle + gym bag

**Beauty & Personal Care:**
- Sunscreen (SPF50 for tropical climate) → moisturizer + cleanser
- Hair treatment → conditioner + scalp serum (humidity concerns)

**Electronics:**
- Smartphone → case + screen protector + power bank (essential for SEA commuters)
- Laptop → bag + wireless mouse + USB-C hub

**F&B / DTC:**
- Coffee beans → drip bags + mug + grinder
- Health supplements → shaker + resistance bands

### Festive Season Optimization

During mega-sale and festive periods (CNY, Hari Raya, 11.11, 12.12):
- **Gift bundling:** AI identifies products frequently purchased together as gifts
- **Price-anchored upsells:** "Add gift wrapping for SGD 5" / "Upgrade to premium packaging for SGD 12"
- **Occasion-based recommendations:** Different logic for "buying for self" vs "buying as gift"

## Best Practices for SEA Plus Merchants

### 1. Mobile-First (Non-Negotiable)
- 70%+ of SEA e-commerce traffic is mobile
- Test on small screens first
- Touch-friendly buttons (min 44x44px)
- Concise copy — no walls of text at checkout

### 2. Performance First
- Keep extensions lightweight (<50kb)
- Lazy load non-critical content
- Minimize API calls
- AI recommendations should load asynchronously

### 3. Localization
- Use `useTranslate()` hook for all text
- Support languages per market (EN, ZH, MS, ID, TH, VI, TL)
- Respect currency formatting (SGD, MYR, IDR, THB, PHP, VND)
- Show region-appropriate payment badges

### 4. A/B Testing AI Recommendations
- Test AI upsell placements (before vs after shipping options)
- Test recommendation types (complementary vs similar vs bundle)
- Monitor: conversion rate, AOV, cart abandonment
- 2-week minimum test duration

### 5. Error Handling
- Don't block checkout if AI recommendation fails to load
- Validate user input
- Show clear error messages in the customer's language
- Log errors for debugging

## Checkout Extensibility Limits

### Technical Limits
- **Extension size:** 50kb max (minified)
- **API call rate:** 100 req/min per extension
- **Execution time:** 5 seconds max
- **Extensions per checkout:** 20 max

### What You CAN'T Do
- Access DOM directly
- Use external JavaScript libraries (only Shopify-approved)
- Redirect users away from checkout
- Modify checkout URL structure
- Inject arbitrary HTML/CSS

## Measuring AI Checkout Performance

### Key Metrics

| **Metric** | **Benchmark** | **Target** |
|------------|--------------|-----------|
| AI upsell click-through rate | 5-15% | >10% |
| AI upsell conversion rate | 8-20% | >12% |
| Revenue from AI recommendations | 10-30% of checkout revenue | >15% |
| AOV lift from AI cross-sell | 10-25% | >15% |
| Mobile checkout conversion | 2-4% | >3% |

### Sidekick Analytics Queries
- "What percentage of checkout revenue came from AI recommendations this month?"
- "Compare AOV for customers who accepted upsell vs declined"
- "Show checkout conversion rate by market (SG vs MY vs ID)"

## Resources

- [Checkout Extensibility Guide](https://shopify.dev/docs/apps/checkout)
- [UI Extensions API Reference](https://shopify.dev/docs/api/checkout-ui-extensions)
- [Branding API Docs](https://shopify.dev/docs/api/admin-graphql/latest/mutations/checkoutBrandingUpsert)
- [Product Recommendations API](https://shopify.dev/docs/api/ajax/reference/product-recommendations)
- [Migration Guide](https://shopify.dev/docs/apps/checkout/migrate)
- [Component Library](https://shopify.dev/docs/api/checkout-ui-extensions/components)

---

**Next:** [Shopify Functions →](03-shopify-functions.md)
