# AI-Powered App Development for Shopify Plus

## Overview

Custom apps extend Shopify Plus beyond what themes and existing apps provide. For Southeast Asian merchants with unique requirements—multi-market operations, regional payment integrations, halal compliance, multi-language storefronts—building AI-first custom apps unlocks competitive advantages that off-the-shelf solutions can't match.

This guide covers building AI-powered custom apps for Shopify Plus merchants across Singapore, Malaysia, Indonesia, Thailand, Vietnam, and the Philippines.

## When to Build a Custom App

### Good Use Cases
- **AI-powered features**: Predictive analytics, smart recommendations, dynamic pricing
- **Unique business logic**: Complex pricing rules, regional compliance workflows
- **System integrations**: Connect Shopify to ERP, WMS, CRM, regional logistics (Ninja Van, J&T, Flash Express)
- **B2B portals**: Custom ordering interfaces for wholesale customers
- **Multi-market tools**: Cross-border pricing, tax, and currency management
- **Checkout extensions**: Custom fields, validations, regional payment methods
- **AI admin tools**: Smart reporting dashboards, demand forecasting

### When NOT to Build
- **Existing app works**: Check Shopify App Store first
- **Simple automation**: Use Shopify Flow or Scripts
- **No maintenance plan**: APIs evolve; apps need ongoing care
- **Short-term needs**: Consider no-code alternatives

## App Architecture Types

### 1. Custom Apps (Primary for Plus Agencies)
- Built for a specific merchant
- Installed via API credentials (no App Store review)
- Full access to Admin API, Storefront API, and AI features

### 2. Public Apps
- Listed on Shopify App Store
- OAuth authentication, Shopify review required
- Good for productized solutions serving multiple merchants

### 3. Embedded Apps
- Render inside Shopify Admin via iframe
- Use App Bridge for native navigation
- Ideal for admin tools, dashboards, AI-powered insights

## Shopify AI APIs & Capabilities

### Shopify Magic (Built-in AI)

Shopify's native AI features that apps can leverage:

| Feature | Capability | SEA Application |
|---------|-----------|-----------------|
| **Product Descriptions** | AI-generated copy | Generate in English, Bahasa, Thai, Vietnamese |
| **Shopify Inbox** | AI chat responses | Auto-respond in customer's language |
| **Sidekick** | Admin AI assistant | Natural language store management |
| **Smart Recommendations** | Product suggestions | Region-aware recommendations |
| **Semantic Search** | Intent-based search | Understand queries across SEA languages |

### AI APIs for Custom Apps

#### 1. Shopify Search & Discovery API
Build AI-powered search experiences:

```graphql
# Semantic search — understands intent, not just keywords
query predictiveSearch($query: String!) {
  predictiveSearch(query: $query) {
    products {
      id
      title
      handle
      variants(first: 1) {
        edges {
          node {
            price { amount currencyCode }
          }
        }
      }
    }
  }
}
```

**SEA Enhancement**: Layer multilingual synonyms so "baju kurung" matches "traditional dress" and "kemeja" matches "shirt."

#### 2. Product Recommendations API
AI-driven product suggestions:

```graphql
query productRecommendations($productId: ID!) {
  productRecommendations(productId: $productId) {
    id
    title
    priceRange {
      minVariantPrice { amount currencyCode }
    }
  }
}
```

**SEA Enhancement**: Weight recommendations by market—show heat-appropriate products for tropical climates, adjust for regional preferences (halal-certified in MY/ID).

#### 3. Customer Segmentation via Audiences API
Build AI-powered customer segments:

```graphql
mutation segmentCreate($name: String!, $query: String!) {
  segmentCreate(name: $name, query: $query) {
    segment {
      id
      name
      query
    }
  }
}
```

### Integrating External AI Services

For advanced AI capabilities beyond Shopify's built-in features:

#### OpenAI / Claude Integration Pattern

```typescript
// app/services/ai-product-description.ts
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

export async function generateProductDescription(
  product: { title: string; features: string[]; market: string }
) {
  const marketContext = {
    SG: "Singaporean shoppers value quality, efficiency, and premium positioning",
    MY: "Malaysian shoppers are price-conscious, value halal certification, and respond to bilingual content",
    ID: "Indonesian shoppers prioritize value, community reviews, and mobile-first experiences",
    TH: "Thai shoppers value aesthetics, social proof, and LINE integration",
    VN: "Vietnamese shoppers are mobile-first, value-driven, and prefer local payment methods",
    PH: "Filipino shoppers respond to social commerce, influencer recommendations, and installment options",
  };

  const message = await anthropic.messages.create({
    model: "claude-sonnet-4-20250514",
    max_tokens: 500,
    messages: [{
      role: "user",
      content: `Write a compelling product description for: ${product.title}
Features: ${product.features.join(', ')}
Market: ${marketContext[product.market]}
Tone: Professional but warm. Include SEO keywords naturally.`
    }]
  });

  return message.content[0].text;
}
```

#### AI-Powered Chat Support

```typescript
// app/services/ai-customer-support.ts
export async function handleCustomerQuery(
  query: string,
  customerContext: {
    language: string;
    country: string;
    orderHistory: any[];
  }
) {
  const systemPrompt = `You are a customer support agent for a Shopify Plus store 
serving Southeast Asian customers. Respond in ${customerContext.language}.
Customer is from ${customerContext.country}.
Order history: ${JSON.stringify(customerContext.orderHistory.slice(-5))}
Be helpful, culturally aware, and concise.`;

  // Route to AI service for response generation
  // Include product knowledge base, shipping policies, return policies
  // Handle common SEA-specific queries: COD status, halal certification, regional shipping
}
```

## Building AI-First Apps for SEA

### App Idea 1: AI Multi-Market Price Optimizer

**Problem**: Managing prices across 6 SEA markets with different currencies, purchasing power, and competitive landscapes.

**Solution**: AI app that analyzes competitor pricing, exchange rates, and demand signals to suggest optimal pricing per market.

```typescript
// app/routes/app.price-optimizer.tsx
import { Page, Card, DataTable, Banner } from "@shopify/polaris";
import { useLoaderData } from "@remix-run/react";
import { authenticate } from "../shopify.server";

export async function loader({ request }) {
  const { admin } = await authenticate.admin(request);
  
  // Fetch products with multi-market pricing
  const response = await admin.graphql(`
    query {
      products(first: 20) {
        edges {
          node {
            id
            title
            variants(first: 5) {
              edges {
                node {
                  id
                  price
                  contextualPricing(context: { country: SG }) {
                    price { amount currencyCode }
                  }
                }
              }
            }
          }
        }
      }
    }
  `);

  const { data } = await response.json();
  
  // AI pricing analysis
  const pricingSuggestions = await analyzePricingWithAI(data.products.edges);
  
  return { products: data.products.edges, suggestions: pricingSuggestions };
}

async function analyzePricingWithAI(products) {
  // Analyze competitor data, exchange rates, demand signals
  // Return per-market pricing suggestions
  return products.map(p => ({
    productId: p.node.id,
    suggestions: {
      SG: { current: 49.90, suggested: 52.90, reason: "Premium market, competitor priced at SGD 55" },
      MY: { current: 149, suggested: 139, reason: "Price-sensitive market, competitor at MYR 135" },
      ID: { current: 499000, suggested: 479000, reason: "Volume opportunity with lower price point" },
    }
  }));
}
```

### App Idea 2: AI-Powered Halal Compliance Tracker

**Problem**: MY and ID markets require halal certification tracking. Manual process is error-prone.

```typescript
// app/routes/app.halal-tracker.tsx
export async function loader({ request }) {
  const { admin } = await authenticate.admin(request);
  
  const response = await admin.graphql(`
    query {
      products(first: 50, query: "tag:food OR tag:cosmetics OR tag:supplements") {
        edges {
          node {
            id
            title
            tags
            metafields(namespace: "halal", first: 5) {
              edges {
                node {
                  key
                  value
                }
              }
            }
          }
        }
      }
    }
  `);

  const { data } = await response.json();
  
  // AI scans product ingredients/descriptions for halal compliance
  const complianceReport = await aiHalalComplianceCheck(data.products.edges);
  
  return { products: data.products.edges, compliance: complianceReport };
}

async function aiHalalComplianceCheck(products) {
  // AI analyzes product descriptions and ingredient lists
  // Flags products that may contain non-halal ingredients
  // Checks certification expiry dates
  // Returns compliance status and action items
}
```

### App Idea 3: SEA Logistics Intelligence App

**Problem**: Multiple logistics partners across SEA with varying performance.

```typescript
// AI-powered logistics routing
export async function getOptimalCarrier(order: {
  destination: string;
  weight: number;
  value: number;
  urgency: string;
}) {
  const carriers = {
    SG: ["Ninja Van", "Qxpress", "SingPost"],
    MY: ["Ninja Van", "J&T Express", "Pos Malaysia", "DHL eCommerce"],
    ID: ["J&T Express", "SiCepat", "Anteraja", "JNE"],
    TH: ["Flash Express", "Kerry Express", "Thailand Post"],
    VN: ["Giao Hang Nhanh", "Viettel Post", "J&T Express"],
    PH: ["J&T Express", "Ninja Van", "LBC Express"],
  };

  // AI evaluates:
  // - Historical delivery times by carrier and destination
  // - Current carrier capacity/delays
  // - Cost optimization
  // - Customer delivery preference
  // Returns optimal carrier with estimated delivery time and cost
}
```

### App Idea 4: AI-Powered B2B Wholesale Portal

**Problem**: SEA wholesale buyers need personalized catalogs, dynamic pricing, and regional compliance.

```typescript
// AI-personalized B2B catalog
export async function getPersonalizedCatalog(buyer: {
  companyId: string;
  country: string;
  industry: string;
  orderHistory: any[];
}) {
  // AI curates catalog based on:
  // - Buyer's purchase history and patterns
  // - Regional product demand
  // - Seasonal trends (upcoming festivals/sales)
  // - Halal requirements (if MY/ID buyer)
  // - Price tier based on buyer volume

  // Returns prioritized product list:
  // 1. Frequently reordered items
  // 2. "Others in your industry also bought"
  // 3. Seasonal recommendations
  // 4. New arrivals in buyer's categories
}
```

### App Idea 5: AI Customer Insights Dashboard

```typescript
// app/routes/app.insights.tsx
export async function loader({ request }) {
  const { admin } = await authenticate.admin(request);
  
  // Aggregate customer data
  const customers = await fetchCustomerData(admin);
  
  // AI analysis
  const insights = {
    churnRisk: await predictChurnRisk(customers),
    lifetimeValue: await predictLTV(customers),
    segmentation: await autoSegment(customers),
    marketTrends: await analyzeMarketTrends(customers),
    recommendations: await generateActionItems(customers),
  };

  return { insights };
}

// AI generates actionable insights like:
// "23 VIP customers in Singapore haven't ordered in 45+ days — suggest win-back campaign"
// "Indonesian mobile conversion is 40% below desktop — optimize mobile checkout"
// "Thai customers respond 3x better to LINE notifications vs. email"
```

## Development Stack

### Required Tools
- **Node.js 18+** (LTS recommended)
- **Shopify CLI** (`npm install -g @shopify/cli@latest`)
- **Git** for version control
- **VS Code** (recommended editor)

### Tech Stack
| Layer | Technology |
|-------|-----------|
| Framework | Remix (Shopify default) |
| Language | TypeScript |
| UI | Polaris (Shopify design system) |
| API | GraphQL (Admin + Storefront) |
| AI | OpenAI / Anthropic / Custom models |
| Database | SQLite (dev) → PostgreSQL (prod) |
| Auth | App Bridge + Session tokens |

### Creating a New App

```bash
# Initialize
npm init @shopify/app@latest
# Select: Remix template, TypeScript

# Project structure
my-app/
├── app/
│   ├── routes/          # App pages
│   ├── services/        # AI services, business logic
│   ├── components/      # React components
│   └── shopify.server.ts
├── extensions/          # Checkout, theme, function extensions
├── prisma/              # Database schema
├── shopify.app.toml     # App config
└── package.json

# Start dev server
cd my-app && npm run dev
```

## App Extensions

### Checkout UI Extensions

Add custom blocks to Shopify checkout:

```javascript
// extensions/checkout-ui/src/Checkout.jsx
import {
  reactExtension,
  TextField,
  Select,
  useApi,
} from '@shopify/ui-extensions-react/checkout';

export default reactExtension(
  'purchase.checkout.delivery-address.render-after',
  () => <SEADeliveryOptions />
);

function SEADeliveryOptions() {
  const { applyAttributeChange } = useApi();

  return (
    <>
      <TextField
        label="Delivery instructions (e.g., unit number, landmark)"
        onChange={(value) => {
          applyAttributeChange({
            type: 'updateAttribute',
            key: 'delivery_instructions',
            value,
          });
        }}
      />
      <Select
        label="Preferred delivery time"
        options={[
          { value: 'morning', label: '9 AM - 12 PM' },
          { value: 'afternoon', label: '12 PM - 5 PM' },
          { value: 'evening', label: '5 PM - 9 PM' },
        ]}
        onChange={(value) => {
          applyAttributeChange({
            type: 'updateAttribute',
            key: 'delivery_time_preference',
            value,
          });
        }}
      />
    </>
  );
}
```

### Shopify Functions

Serverless logic for checkout customization:

```javascript
// Payment customization — show/hide methods by market
export function run(input) {
  const { cart, paymentMethods } = input;
  const country = cart.buyerIdentity?.countryCode;
  
  const operations = [];

  // Hide COD for Singapore (not common)
  if (country === 'SG') {
    const cod = paymentMethods.find(m => m.name.includes('COD'));
    if (cod) operations.push({ hide: { paymentMethodId: cod.id } });
  }

  // Show GrabPay only in SG/MY/PH/VN/TH/ID
  const grabPayCountries = ['SG', 'MY', 'PH', 'VN', 'TH', 'ID'];
  if (!grabPayCountries.includes(country)) {
    const grabPay = paymentMethods.find(m => m.name.includes('GrabPay'));
    if (grabPay) operations.push({ hide: { paymentMethodId: grabPay.id } });
  }

  return { operations };
}
```

### Theme App Extensions

Inject app blocks into themes without editing code:

```liquid
<!-- extensions/theme-app/blocks/ai-recommendations.liquid -->
{% schema %}
{
  "name": "AI Product Recommendations",
  "target": "section",
  "settings": [
    {
      "type": "select",
      "id": "rec_type",
      "label": "Recommendation Type",
      "options": [
        { "value": "similar", "label": "Similar Products" },
        { "value": "trending", "label": "Trending in Region" },
        { "value": "complementary", "label": "Frequently Bought Together" }
      ],
      "default": "similar"
    }
  ]
}
{% endschema %}

<div class="ai-recommendations" 
     data-product-id="{{ product.id }}" 
     data-rec-type="{{ block.settings.rec_type }}"
     data-market="{{ localization.country.iso_code }}">
  <h3>{{ block.settings.rec_type | capitalize }} Products</h3>
  <div id="ai-rec-container"></div>
</div>

<script>
  fetch(`/apps/ai-recs/{{ product.id }}?type={{ block.settings.rec_type }}&market={{ localization.country.iso_code }}`)
    .then(res => res.json())
    .then(data => { /* render recommendations */ });
</script>
```

## Webhooks & Real-Time AI Processing

### Register Webhooks

```graphql
mutation {
  webhookSubscriptionCreate(
    topic: ORDERS_CREATE
    webhookSubscription: {
      callbackUrl: "https://your-app.com/webhooks/orders-create"
      format: JSON
    }
  ) {
    webhookSubscription { id topic }
  }
}
```

### AI-Enhanced Webhook Handler

```typescript
// app/routes/webhooks.orders-create.tsx
export async function action({ request }) {
  const { topic, shop, payload } = await authenticate.webhook(request);
  
  const order = payload;
  
  // AI fraud scoring
  const fraudScore = await aiFraudCheck({
    orderValue: order.total_price,
    paymentMethod: order.payment_gateway_names,
    shippingCountry: order.shipping_address?.country_code,
    customerOrderCount: order.customer?.orders_count,
    ipGeolocation: order.browser_ip,
  });
  
  if (fraudScore > 0.8) {
    // Auto-hold order and alert team
    await tagOrder(order.id, "fraud-review");
    await notifySlack(`🚨 High fraud risk order ${order.name} (score: ${fraudScore})`);
  }
  
  // AI demand signal tracking
  await trackDemandSignal({
    product_ids: order.line_items.map(li => li.product_id),
    market: order.shipping_address?.country_code,
    timestamp: order.created_at,
  });
  
  return new Response("OK", { status: 200 });
}
```

## Deployment

### Hosting Options

| Provider | Best For | SEA Consideration |
|----------|----------|-------------------|
| **Vercel** | Remix apps, serverless | Edge functions in SG region |
| **AWS (ap-southeast-1)** | Full control, Singapore region | Lowest latency for SEA |
| **Google Cloud (asia-southeast1)** | AI/ML workloads | Good AI API integration |
| **Shopify Oxygen** | Hydrogen storefronts | Global CDN |

### Production Checklist

- [ ] Environment variables configured
- [ ] Database migrated to PostgreSQL
- [ ] HTTPS enabled
- [ ] Error tracking (Sentry)
- [ ] API rate limit handling (8 req/sec for Plus)
- [ ] Webhook HMAC validation
- [ ] AI API keys secured
- [ ] Hosted in SEA region (ap-southeast-1) for latency
- [ ] Multi-language support tested
- [ ] Regional payment methods tested

### Deploy to Vercel (Singapore Region)

```bash
npm install -g vercel
vercel --prod

# Set region in vercel.json
{
  "regions": ["sin1"]
}
```

## SEA-Specific App Ideas Summary

| App | Market Need | AI Component |
|-----|------------|-------------|
| **Multi-Market Price Optimizer** | Price across 6 currencies | Competitor analysis, demand prediction |
| **Halal Compliance Tracker** | MY/ID regulatory requirement | Ingredient scanning, certification tracking |
| **SEA Logistics Router** | Multiple carriers per market | Delivery time prediction, cost optimization |
| **B2B Wholesale Portal** | Regional wholesale operations | Personalized catalogs, dynamic pricing |
| **AI Customer Insights** | Cross-market analytics | Churn prediction, LTV forecasting |
| **Multi-Language Content Generator** | 6+ languages across SEA | AI translation + cultural adaptation |
| **Regional Payment Optimizer** | GrabPay, GoPay, MoMo, GCash | Conversion optimization by payment method |
| **Festival Campaign Manager** | CNY, Hari Raya, Songkran, etc. | Demand forecasting, auto-campaign creation |

## Best Practices

### 1. API Rate Limits
- Plus stores: **8 requests/sec** (Admin API)
- Use **bulk operations** for large datasets
- Cache responses aggressively
- Queue background jobs for AI processing

### 2. AI-Specific Best Practices
- **Cache AI responses** — don't regenerate identical recommendations
- **Fallback gracefully** — if AI service is down, show default content
- **Human oversight** — flag AI decisions above value thresholds for review
- **A/B test** — compare AI-driven vs. manual approaches with real metrics
- **Data privacy** — comply with PDPA (Singapore), PDPA (Thailand), and regional data laws

### 3. Security
- Validate HMAC for all webhooks
- Environment variables for all secrets
- Sanitize user input (prevent XSS)
- CSRF protection enabled
- Follow OWASP guidelines

### 4. Performance
- Lazy load large datasets
- Paginate API results (250 max per query)
- Host in SEA region for low latency
- Optimize for mobile (70%+ of SEA traffic is mobile)

### 5. Testing

```typescript
// Unit test AI pricing logic
describe('AI Price Optimizer', () => {
  it('suggests lower price for price-sensitive markets', () => {
    const suggestion = optimizePrice({
      basePrice: 50,
      market: 'ID',
      competitorPrice: 45,
    });
    expect(suggestion.price).toBeLessThan(50);
  });

  it('maintains premium pricing for SG market', () => {
    const suggestion = optimizePrice({
      basePrice: 50,
      market: 'SG',
      competitorPrice: 55,
    });
    expect(suggestion.price).toBeGreaterThanOrEqual(50);
  });
});
```

## Resources

- [Shopify App Development Docs](https://shopify.dev/docs/apps)
- [Admin API Reference](https://shopify.dev/docs/api/admin-graphql)
- [Storefront API Reference](https://shopify.dev/docs/api/storefront)
- [Polaris Design System](https://polaris.shopify.com/)
- [Shopify Functions](https://shopify.dev/docs/api/functions)
- [Shopify AI Features](https://www.shopify.com/magic)
- [App Examples (GitHub)](https://github.com/Shopify/shopify-app-examples)

---

**Key Takeaway for SEA Merchants**: AI-powered custom apps are the competitive edge for Shopify Plus stores in Southeast Asia. The region's diversity—6 major markets, multiple languages, varied payment preferences, and unique compliance requirements—makes AI not just beneficial but essential for scaling efficiently. Start with one high-impact app (pricing or logistics), measure ROI, then expand.

**Next:** [Enablement Plan →](09-enablement-plan.md)
