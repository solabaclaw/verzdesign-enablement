# Checkout Extensibility

## Overview

Checkout Extensibility is Shopify's modern, component-based checkout system that replaces the legacy checkout.liquid template. It provides better performance, security, and customization capabilities while maintaining checkout conversion rates.

**Deadline:** All Plus stores must migrate from checkout.liquid by **August 13, 2025**.

## Why Checkout Extensibility?

### Problems with checkout.liquid
- **Performance:** Custom code slows page load
- **Security:** Direct DOM access creates vulnerabilities
- **Maintenance:** Breaking changes with Shopify updates
- **Mobile:** Harder to optimize for mobile
- **Apps:** Conflicts between multiple apps

### Benefits of Extensibility
- **3x faster** checkout load times
- **Component-based:** Pre-optimized UI components
- **App-friendly:** Apps work together without conflicts
- **Future-proof:** Shopify maintains compatibility
- **Conversion:** Average 15% improvement in mobile conversion
- **Branding:** Full design control via Branding API

## Three Pillars of Checkout Extensibility

### 1. Checkout UI Extensions
**Add custom functionality to checkout**

Build custom app blocks that inject into checkout:
- Custom fields (gift messages, delivery instructions)
- Upsells & cross-sells
- Subscription options
- Trust badges & social proof
- Marketing opt-ins
- Custom validations

**Extension targets:**
- `purchase.checkout.block.render` - Static content
- `purchase.checkout.actions.render-before` - Before payment buttons
- `purchase.checkout.contact.render-after` - After contact info
- `purchase.checkout.delivery-address.render-after` - After address
- `purchase.checkout.shipping-option-list.render-after` - After shipping
- `purchase.checkout.reductions.render-after` - After discounts

### 2. Checkout Branding API
**Design checkout to match brand**

Customize via `checkoutBranding` API or admin UI:
- Colors (primary, backgrounds, accents)
- Typography (fonts, sizes, weights)
- Border radius & styling
- Button styles
- Form inputs
- Spacing & layout

**Example customizations:**
- Match brand color palette
- Use brand fonts (Google Fonts or custom)
- Rounded vs sharp corners
- Button size & prominence

### 3. Payment & Delivery Customization (via Functions)
**Control what options appear**

- **Payment Customization:** Hide, reorder, rename payment methods
- **Delivery Customization:** Dynamic shipping rates, hide methods
- Covered in detail in [Shopify Functions](03-shopify-functions.md)

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
  
  // Show banner for orders over $100
  const totalAmount = parseFloat(cost.totalAmount.amount);
  
  if (totalAmount >= 100) {
    return (
      <Banner title="Free shipping activated!">
        Your order qualifies for free shipping 🎉
      </Banner>
    );
  }
  
  return null;
}
```

### Common Use Cases

#### 1. Gift Message Field
```javascript
import { TextField } from '@shopify/ui-extensions-react/checkout';

function GiftMessage() {
  const [message, setMessage] = useState('');
  
  return (
    <TextField
      label="Gift message (optional)"
      value={message}
      onChange={setMessage}
      multiline={3}
    />
  );
}
```

#### 2. Upsell Product
```javascript
function CheckoutUpsell() {
  const { lines, applyCartLinesChange } = useApi();
  const [loading, setLoading] = useState(false);
  
  const hasUpsellProduct = lines.some(
    line => line.merchandise.id === 'gid://shopify/ProductVariant/123'
  );
  
  if (hasUpsellProduct) return null;
  
  const addUpsell = async () => {
    setLoading(true);
    await applyCartLinesChange({
      type: 'addCartLine',
      merchandiseId: 'gid://shopify/ProductVariant/123',
      quantity: 1,
    });
    setLoading(false);
  };
  
  return (
    <Banner title="Add protection plan?">
      <Button onPress={addUpsell} loading={loading}>
        Add for $9.99
      </Button>
    </Banner>
  );
}
```

#### 3. Delivery Instructions
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
    />
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
          global {
            accent
            brand
            critical
          }
        }
        typography {
          primary {
            base {
              weight
            }
          }
        }
      }
    }
  }
}
```

**Variables:**
```json
{
  "checkoutBrandingInput": {
    "designSystem": {
      "colors": {
        "global": {
          "accent": "#FF6B35",
          "brand": "#004E89"
        }
      },
      "typography": {
        "primary": {
          "customFontGroup": {
            "base": {
              "genericFileId": "gid://shopify/GenericFile/123",
              "weight": 400
            }
          }
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

### Migration Steps

#### Step 1: Enable Checkout Extensibility
- Admin → Settings → Checkout → "Upgrade to Checkout Extensibility"
- Creates new extensible checkout (checkout.liquid still works during transition)

#### Step 2: Rebuild Customizations
- Build UI Extensions for custom fields
- Use Functions for payment/shipping logic
- Apply branding via API or admin
- Add web pixels for tracking

#### Step 3: Test Thoroughly
- Test on staging store first
- Check all edge cases (multiple items, discounts, etc.)
- Mobile & desktop
- Different payment methods
- International orders

#### Step 4: Cut Over
- Publish extensions
- Disable checkout.liquid customizations
- Monitor conversion rates
- Have rollback plan ready

### Common Migration Patterns

| checkout.liquid | Extensibility Solution |
|----------------|------------------------|
| Custom input field | Checkout UI Extension (TextField) |
| Upsell widget | Checkout UI Extension (Product offer) |
| Hide payment method | Payment Customization Function |
| Custom shipping rate | Delivery Customization Function |
| Brand styling | Checkout Branding API |
| Google Analytics | Web Pixels API |
| Address validation | Validation Function |
| Gift wrap checkbox | UI Extension + cart attributes |

## Best Practices for Plus Merchants

### 1. Performance First
- Keep extensions lightweight (<50kb)
- Lazy load non-critical content
- Minimize API calls
- Use static content when possible

### 2. Mobile Optimization
- Test on small screens first
- Use responsive components
- Touch-friendly buttons (min 44x44px)
- Concise copy

### 3. A/B Testing
- Use Shopify Scripts or Functions for logic-based tests
- Test one change at a time
- Monitor: conversion rate, AOV, cart abandonment
- 2-week minimum test duration

### 4. Error Handling
- Validate user input
- Show clear error messages
- Don't block checkout on extension failure
- Log errors for debugging

### 5. Internationalization
- Use `useTranslate()` hook for all text
- Support multiple languages
- Respect currency formatting
- Test in all active markets

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

## Testing & Debugging

### Local Development
```bash
npm run dev
# Opens checkout preview with extension loaded
# Hot reload enabled
```

### Debugging
- Use `console.log()` (appears in browser console)
- Shopify DevTools extension
- Check network tab for API errors
- Review extension logs in Partner Dashboard

### Preview in Different States
- Empty cart vs full cart
- Different shipping addresses (domestic/international)
- Multiple products
- Discount codes applied
- Different customer segments (B2B, VIP, etc.)

## Real-World Examples (APAC Context)

### Singapore GST Display
Show GST breakdown clearly:
```javascript
function GSTBreakdown() {
  const { cost } = useApi();
  const subtotal = parseFloat(cost.subtotalAmount.amount);
  const gst = subtotal * 0.09; // 9% GST
  
  return (
    <View>
      <Text>Subtotal: ${subtotal.toFixed(2)}</Text>
      <Text>GST (9%): ${gst.toFixed(2)}</Text>
    </View>
  );
}
```

### Local Payment Method Badge
Show trust badges for regional payments:
```javascript
function PaymentTrustBadge() {
  return (
    <Banner>
      We accept PayNow, GrabPay, and all major cards 🇸🇬
    </Banner>
  );
}
```

## Resources

- [Checkout Extensibility Guide](https://shopify.dev/docs/apps/checkout)
- [UI Extensions API Reference](https://shopify.dev/docs/api/checkout-ui-extensions)
- [Branding API Docs](https://shopify.dev/docs/api/admin-graphql/latest/mutations/checkoutBrandingUpsert)
- [Migration Guide](https://shopify.dev/docs/apps/checkout/migrate)
- [Component Library](https://shopify.dev/docs/api/checkout-ui-extensions/components)

---

**Next:** [Shopify Functions →](03-shopify-functions.md)
