# App Development for Shopify Plus

## Overview

Custom apps extend Shopify functionality beyond what themes and existing apps provide. For Plus merchants with unique business requirements, building custom apps allows you to create tailored solutions for inventory management, order processing, B2B workflows, and customer experiences.

This guide covers building custom apps for your Plus merchant clients using modern Shopify development tools.

## When to Build a Custom App

### Good Use Cases
- **Unique business logic:** Complex pricing rules, custom workflows
- **System integrations:** Connect Shopify to ERP, WMS, CRM, POS
- **B2B portals:** Custom ordering interfaces for wholesale customers
- **Admin tools:** Bulk operations, custom reporting dashboards
- **Checkout extensions:** Custom fields, validations, upsells
- **Data migrations:** Import/export custom data formats

### When NOT to Build
- **Existing app works:** Use Shopify App Store first
- **Simple one-off tasks:** Use Shopify Scripts or bulk editor
- **No development resources:** Consider hiring Shopify Expert
- **Short-term needs:** Shopify APIs change; maintenance required

## App Architecture Types

### 1. Public Apps
**Listed on Shopify App Store**

- **Users:** Any Shopify merchant
- **Distribution:** App Store listing
- **Monetization:** Free, one-time, or recurring charge
- **Auth:** OAuth with required scopes
- **Review:** Shopify app review required
- **Examples:** Klaviyo, Yotpo, ReCharge

### 2. Custom Apps
**Built for a specific merchant**

- **Users:** Single Shopify store
- **Distribution:** Installed directly via API credentials
- **Monetization:** Custom pricing/contract
- **Auth:** API access token (admin-created)
- **Review:** No Shopify review needed
- **Examples:** ERP integrations, custom admin tools

### 3. Private Apps (Legacy)
**Deprecated - use Custom Apps instead**

- Being phased out
- Limited to REST Admin API
- No app extensions support

## App Components

### Embedded Apps
**Runs inside Shopify Admin**

- **UI:** React app rendered in iframe within Shopify admin
- **Navigation:** App Bridge for native Shopify navigation
- **Auth:** Session tokens (JWT)
- **Benefits:** Native feel, access to admin context
- **Examples:** Bulk editor, custom dashboard, product importer

### Standalone Apps
**Runs on external domain**

- **UI:** Your own domain/hosting
- **Auth:** OAuth or API tokens
- **Use case:** Complex workflows, external integrations
- **Examples:** Warehouse management system, custom POS

### App Extensions
**Inject into Shopify UI**

#### Types:
- **Checkout UI Extensions:** Custom checkout blocks
- **Theme App Extensions:** Inject into theme without code edits
- **Admin UI Extensions:** Custom cards/actions in admin
- **Post-Purchase Extensions:** Upsells after checkout
- **Function Extensions:** Payment/delivery/discount logic

## Development Stack

### Required Tools
- **Node.js 18+** (LTS recommended)
- **npm or yarn** (package manager)
- **Shopify CLI** (command-line tool)
- **Git** (version control)
- **Code editor:** VS Code recommended

### Tech Stack
- **Framework:** Remix (default for new apps)
- **Language:** JavaScript/TypeScript
- **UI Library:** Polaris (Shopify's design system)
- **GraphQL:** Admin API queries/mutations
- **App Bridge:** Shopify admin integration
- **Session storage:** SQLite (dev), PostgreSQL (production)

### Installation

```bash
# Install Shopify CLI
npm install -g @shopify/cli@latest

# Verify installation
shopify version
```

## Creating Your First App

### Step 1: Initialize App

```bash
# Create new app
npm init @shopify/app@latest

# Prompts:
# - App name: my-custom-app
# - Template: Remix
# - Package manager: npm
# - Language: TypeScript
```

### Step 2: Project Structure

```
my-custom-app/
├── app/                    # Remix app (frontend)
│   ├── routes/            # App pages
│   ├── components/        # React components
│   └── shopify.server.ts  # Shopify config
├── extensions/            # App extensions
│   ├── checkout-ui/       # Checkout extensions
│   └── theme-app/         # Theme extensions
├── prisma/                # Database schema
├── public/                # Static assets
├── shopify.app.toml       # App configuration
└── package.json
```

### Step 3: Start Development Server

```bash
cd my-custom-app
npm run dev

# Opens:
# - Partner Dashboard (select dev store)
# - Local dev server (http://localhost:3000)
# - App installed in dev store
```

### Step 4: Build First Feature

**Example: Product bulk tagger**

```typescript
// app/routes/app.bulk-tag.tsx
import { useState } from "react";
import { Page, Card, TextField, Button } from "@shopify/polaris";
import { useLoaderData, useSubmit } from "@remix-run/react";
import { authenticate } from "../shopify.server";

export async function loader({ request }) {
  const { admin } = await authenticate.admin(request);
  
  // Fetch products
  const response = await admin.graphql(`
    query {
      products(first: 10) {
        edges {
          node {
            id
            title
            tags
          }
        }
      }
    }
  `);
  
  const { data } = await response.json();
  return { products: data.products.edges };
}

export async function action({ request }) {
  const { admin } = await authenticate.admin(request);
  const formData = await request.formData();
  const productId = formData.get("productId");
  const tag = formData.get("tag");
  
  // Add tag to product
  await admin.graphql(`
    mutation productUpdate($input: ProductInput!) {
      productUpdate(input: $input) {
        product {
          id
          tags
        }
      }
    }
  `, {
    variables: {
      input: {
        id: productId,
        tags: tag
      }
    }
  });
  
  return { success: true };
}

export default function BulkTag() {
  const { products } = useLoaderData();
  const submit = useSubmit();
  const [tag, setTag] = useState("");
  
  const handleTag = (productId) => {
    const formData = new FormData();
    formData.append("productId", productId);
    formData.append("tag", tag);
    submit(formData, { method: "post" });
  };
  
  return (
    <Page title="Bulk Tag Products">
      <Card>
        <TextField
          label="Tag to add"
          value={tag}
          onChange={setTag}
          autoComplete="off"
        />
        
        {products.map(({ node: product }) => (
          <div key={product.id}>
            <p>{product.title}</p>
            <Button onClick={() => handleTag(product.id)}>
              Add Tag
            </Button>
          </div>
        ))}
      </Card>
    </Page>
  );
}
```

## Admin API (GraphQL)

### Why GraphQL?
- **Efficient:** Request exactly the data you need
- **Versioned:** Stable, predictable updates
- **Modern:** Replaces REST Admin API (still available)
- **Powerful:** Bulk operations, nested queries

### Authentication

```typescript
// app/shopify.server.ts
import { shopifyApp } from "@shopify/shopify-app-remix/server";

const shopify = shopifyApp({
  apiKey: process.env.SHOPIFY_API_KEY,
  apiSecretKey: process.env.SHOPIFY_API_SECRET,
  scopes: ["read_products", "write_products"],
  appUrl: process.env.SHOPIFY_APP_URL,
});

export default shopify;
export const authenticate = shopify.authenticate;
```

### Common Queries

#### Fetch Products
```graphql
query getProducts($first: Int!) {
  products(first: $first) {
    edges {
      node {
        id
        title
        handle
        variants(first: 10) {
          edges {
            node {
              id
              price
              inventoryQuantity
            }
          }
        }
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

#### Update Product
```graphql
mutation productUpdate($input: ProductInput!) {
  productUpdate(input: $input) {
    product {
      id
      title
      tags
    }
    userErrors {
      field
      message
    }
  }
}
```

#### Bulk Create Variants
```graphql
mutation productVariantsBulkCreate(
  $productId: ID!
  $variants: [ProductVariantsBulkInput!]!
) {
  productVariantsBulkCreate(
    productId: $productId
    variants: $variants
  ) {
    product {
      id
    }
    productVariants {
      id
      price
    }
    userErrors {
      field
      message
    }
  }
}
```

### Pagination (Cursor-Based)

```typescript
async function fetchAllProducts(admin) {
  let hasNextPage = true;
  let cursor = null;
  let allProducts = [];
  
  while (hasNextPage) {
    const response = await admin.graphql(`
      query getProducts($cursor: String) {
        products(first: 250, after: $cursor) {
          edges {
            node { id title }
          }
          pageInfo {
            hasNextPage
            endCursor
          }
        }
      }
    `, { variables: { cursor } });
    
    const { data } = await response.json();
    allProducts.push(...data.products.edges);
    
    hasNextPage = data.products.pageInfo.hasNextPage;
    cursor = data.products.pageInfo.endCursor;
  }
  
  return allProducts;
}
```

## Storefront API

**For customer-facing experiences**

- **Use cases:** Headless storefronts, mobile apps, custom checkout
- **Auth:** Public access token (read-only) or private token
- **Data:** Products, collections, cart, checkout
- **GraphQL only** (no REST)

### Example: Fetch Products for Storefront

```graphql
query getProducts {
  products(first: 20) {
    edges {
      node {
        id
        title
        handle
        priceRange {
          minVariantPrice {
            amount
            currencyCode
          }
        }
        images(first: 1) {
          edges {
            node {
              url
              altText
            }
          }
        }
      }
    }
  }
}
```

### Create Cart (Storefront API)

```graphql
mutation cartCreate($input: CartInput!) {
  cartCreate(input: $input) {
    cart {
      id
      checkoutUrl
      lines(first: 10) {
        edges {
          node {
            id
            quantity
            merchandise {
              ... on ProductVariant {
                id
                title
                price {
                  amount
                }
              }
            }
          }
        }
      }
    }
  }
}
```

## App Extensions

### Checkout UI Extensions

**Add custom blocks to checkout**

```bash
# Generate checkout extension
npm run shopify app generate extension
# Select: Checkout UI

# Creates: extensions/checkout-ui/
```

**Example: Delivery instructions field**

```javascript
// extensions/checkout-ui/src/Checkout.jsx
import {
  reactExtension,
  TextField,
  useApi,
} from '@shopify/ui-extensions-react/checkout';

export default reactExtension(
  'purchase.checkout.delivery-address.render-after',
  () => <DeliveryInstructions />
);

function DeliveryInstructions() {
  const { applyAttributeChange, attributes } = useApi();
  
  const instructions = attributes.find(
    attr => attr.key === 'delivery_instructions'
  )?.value || '';
  
  return (
    <TextField
      label="Delivery instructions (optional)"
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

### Theme App Extensions

**Inject app blocks into themes without code edits**

```bash
# Generate theme extension
npm run shopify app generate extension
# Select: Theme app extension
```

**Example: Product reviews block**

```liquid
<!-- extensions/theme-app/blocks/reviews.liquid -->
{% schema %}
{
  "name": "Product Reviews",
  "target": "section",
  "settings": [
    {
      "type": "checkbox",
      "id": "show_rating",
      "label": "Show star rating",
      "default": true
    }
  ]
}
{% endschema %}

<div class="app-reviews">
  {% if block.settings.show_rating %}
    <div class="star-rating" data-product-id="{{ product.id }}">
      <!-- JavaScript will populate this -->
    </div>
  {% endif %}
  
  <div id="reviews-container"></div>
</div>

<script>
  // Fetch reviews from your app endpoint
  fetch(`/apps/reviews/product/${{{ product.id }}}`)
    .then(res => res.json())
    .then(data => {
      // Render reviews
    });
</script>
```

## Shopify Functions

**Serverless logic for checkout customization**

### Use Cases
- **Discount Functions:** Complex discount logic
- **Payment Customization:** Hide/rename payment methods
- **Delivery Customization:** Dynamic shipping rates
- **Cart Validation:** Enforce business rules

### Example: B2B Payment Restriction

```bash
# Generate function
npm run shopify app generate extension
# Select: Function - Payment customization
```

```javascript
// extensions/payment-customization/src/run.js
export function run(input) {
  const { cart, paymentMethods } = input;
  
  // Hide credit card for B2B customers (must use invoice)
  const isB2BCustomer = cart.buyerIdentity?.customer?.tags?.includes('b2b');
  
  if (!isB2BCustomer) {
    return { operations: [] };
  }
  
  // Hide all payment methods except "Invoice"
  const hideOperations = paymentMethods
    .filter(method => method.name !== "Invoice")
    .map(method => ({
      hide: {
        paymentMethodId: method.id
      }
    }));
  
  return { operations: hideOperations };
}
```

## Polaris Design System

**Shopify's official UI component library**

### Why Use Polaris?
- **Consistent UX:** Matches Shopify admin design
- **Accessibility:** WCAG AA compliant
- **Responsive:** Mobile-friendly out of the box
- **Maintained:** Updated by Shopify

### Common Components

```typescript
import {
  Page,
  Card,
  Button,
  TextField,
  DataTable,
  Banner,
  Modal,
  Loading,
} from '@shopify/polaris';

function ProductList() {
  return (
    <Page
      title="Products"
      primaryAction={{ content: 'Add product', onAction: handleAdd }}
    >
      <Card>
        <DataTable
          columnContentTypes={['text', 'numeric', 'text']}
          headings={['Product', 'Inventory', 'Status']}
          rows={[
            ['T-Shirt', 42, 'Active'],
            ['Hoodie', 8, 'Active'],
          ]}
        />
      </Card>
      
      <Banner status="warning">
        Low inventory detected for 3 products.
      </Banner>
    </Page>
  );
}
```

## Webhooks

**Real-time notifications from Shopify**

### Common Webhooks
- `orders/create` - New order placed
- `orders/updated` - Order status changed
- `products/update` - Product edited
- `inventory_levels/update` - Stock changed
- `customers/create` - New customer registered
- `refunds/create` - Refund issued

### Register Webhook (GraphQL)

```graphql
mutation webhookSubscriptionCreate($topic: WebhookSubscriptionTopic!, $webhookSubscription: WebhookSubscriptionInput!) {
  webhookSubscriptionCreate(
    topic: $topic
    webhookSubscription: $webhookSubscription
  ) {
    webhookSubscription {
      id
      topic
      endpoint {
        __typename
        ... on WebhookHttpEndpoint {
          callbackUrl
        }
      }
    }
  }
}
```

**Variables:**
```json
{
  "topic": "ORDERS_CREATE",
  "webhookSubscription": {
    "callbackUrl": "https://your-app.com/webhooks/orders-create",
    "format": "JSON"
  }
}
```

### Handle Webhook (Remix)

```typescript
// app/routes/webhooks.orders-create.tsx
import { authenticate } from "../shopify.server";

export async function action({ request }) {
  const { topic, shop, session, payload } = await authenticate.webhook(request);
  
  // Verify topic
  if (topic !== "ORDERS_CREATE") {
    return new Response("Invalid topic", { status: 400 });
  }
  
  // Process order
  const order = payload;
  console.log(`New order ${order.id} from ${shop}`);
  
  // Your business logic here
  // e.g., sync to ERP, send to warehouse, etc.
  
  return new Response("OK", { status: 200 });
}
```

## App Proxy

**Serve custom content on merchant's storefront domain**

### Use Case
- **Custom pages:** `/apps/custom-builder`
- **API endpoints:** `/apps/api/products`
- **Liquid access:** Use Shopify Liquid in your app

### Setup

1. **Configure in Partner Dashboard:**
   - App setup → App proxy
   - Subpath: `apps/custom`
   - Proxy URL: `https://your-app.com/proxy`

2. **Handle requests:**

```typescript
// app/routes/proxy.tsx
export async function loader({ request }) {
  const url = new URL(request.url);
  const shop = url.searchParams.get('shop');
  
  // Verify HMAC
  // ... verification logic
  
  // Return Liquid template or JSON
  return new Response(`
    <div class="custom-widget">
      <h2>{{ product.title }}</h2>
      <p>Custom content from your app</p>
    </div>
  `, {
    headers: { 'Content-Type': 'application/liquid' }
  });
}
```

## Deployment

### Production Checklist

- [ ] **Environment variables:** Set in hosting provider
- [ ] **Database:** Migrate to PostgreSQL (from SQLite)
- [ ] **Session storage:** Use database or Redis
- [ ] **HTTPS:** Required for OAuth
- [ ] **Domain:** Custom domain or hosting URL
- [ ] **Error tracking:** Sentry, Bugsnag
- [ ] **Logging:** CloudWatch, Datadog
- [ ] **Rate limiting:** Prevent API abuse

### Hosting Options

#### Shopify Oxygen (for Hydrogen apps)
- **Best for:** Headless storefronts
- **Features:** Global CDN, auto-scaling
- **Pricing:** Pay-as-you-go

#### Vercel / Netlify
- **Best for:** Remix apps, serverless
- **Features:** Git integration, preview deploys
- **Pricing:** Free tier available

#### AWS / Google Cloud / Azure
- **Best for:** Custom infrastructure
- **Features:** Full control, scalability
- **Pricing:** Variable

#### Heroku / Render
- **Best for:** Simple deployments
- **Features:** Easy setup, managed Postgres
- **Pricing:** Starts free

### Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod

# Set environment variables in Vercel dashboard
```

## APAC-Specific App Ideas

### 1. Multi-Market Price Manager
- Manage prices across SG, MY, ID, TH markets
- Auto-convert based on exchange rates
- Region-specific discounts

### 2. Local Payment Gateway Integrator
- Support PayNow, GrabPay, ShopeePay
- Handle COD orders (Indonesia, Philippines)
- Multi-currency settlement

### 3. SEA Logistics Connector
- Integration with Ninja Van, J&T, Flash Express
- Real-time tracking updates
- Regional warehouse routing

### 4. GST/VAT Compliance Tool (Singapore)
- Auto-tag orders by tax status
- Generate GST reports for IRAS
- Invoice generation with GST breakdown

### 5. Halal Certification Tracker
- Tag products with halal certification
- Display halal badge on storefront
- Separate fulfillment for halal/non-halal

## Testing

### Local Testing
```bash
# Run dev server with test data
npm run dev

# Access app in dev store
# Test with sample products/orders
```

### Unit Tests
```typescript
// app/utils/pricing.test.ts
import { calculateDiscount } from './pricing';

describe('calculateDiscount', () => {
  it('applies 10% discount for VIP customers', () => {
    const result = calculateDiscount(100, 'VIP');
    expect(result).toBe(90);
  });
});
```

### Integration Tests
```typescript
// Test GraphQL queries
import { fetchProducts } from './products.server';

test('fetches products from Shopify', async () => {
  const products = await fetchProducts(mockAdmin);
  expect(products).toHaveLength(10);
});
```

## Best Practices

### 1. API Rate Limits
- **Plus stores:** 8 req/sec (Admin API)
- **Use bulk operations** for large datasets
- **Cache responses** when possible
- **Queue background jobs** for heavy processing

### 2. Error Handling
```typescript
try {
  const response = await admin.graphql(query);
  const { data, errors } = await response.json();
  
  if (errors) {
    console.error('GraphQL errors:', errors);
    // Handle errors
  }
} catch (error) {
  console.error('Network error:', error);
  // Retry logic
}
```

### 3. Security
- **Validate HMAC** for webhooks
- **Use environment variables** for secrets
- **Sanitize user input** (prevent XSS)
- **Implement CSRF protection**
- **Follow OWASP guidelines**

### 4. Performance
- **Lazy load** large datasets
- **Paginate** results (250 max per query)
- **Debounce** user inputs
- **Cache** frequently accessed data
- **Optimize images** in app UI

## Resources

- [Shopify App Development Docs](https://shopify.dev/docs/apps)
- [Admin API Reference](https://shopify.dev/docs/api/admin-graphql)
- [Polaris Design System](https://polaris.shopify.com/)
- [Shopify CLI](https://shopify.dev/docs/apps/tools/cli)
- [App Examples (GitHub)](https://github.com/Shopify/shopify-app-examples)

---

**Next:** [Enablement Plan →](09-enablement-plan.md)
