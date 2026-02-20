# Headless Commerce & Hydrogen

## What is Headless Commerce?

**Headless** means decoupling the frontend (presentation layer) from the backend (commerce engine). In Shopify's context:

- **Backend:** Shopify handles products, cart, checkout, orders, inventory
- **Frontend:** Custom storefront built with any tech stack
- **Communication:** Shopify Storefront API (GraphQL)

### Traditional (Monolithic) vs Headless

| Traditional Shopify | Headless Shopify |
|---------------------|------------------|
| Liquid theme | Custom frontend (React, Next.js, etc.) |
| Shopify checkout | Shopify checkout (still hosted by Shopify) |
| Shopify hosting | Your hosting (Vercel, Netlify, etc.) |
| Theme app extensions | Storefront API + custom UI |
| Easy setup | More complex setup |
| Limited UI flexibility | Full UI control |

## What is Hydrogen?

**Hydrogen** is Shopify's official React-based framework for building custom storefronts, optimized for commerce.

### Key Features
- **Built on Remix:** Modern React meta-framework with SSR, routing, data loading
- **Oxygen hosting:** Shopify's edge hosting platform (Cloudflare Workers)
- **Commerce components:** Pre-built cart, product, collection components
- **Storefront API integration:** GraphQL client built-in
- **Performance optimized:** Sub-second page loads globally

### Hydrogen + Oxygen Stack
- **Hydrogen:** Framework (code you write)
- **Oxygen:** Hosting (where it runs)
- **Storefront API:** Data source (Shopify products, cart, etc.)
- **Checkout:** Still Shopify-hosted (same checkout as Plus stores)

## When to Go Headless

### Good Use Cases
- **Unique brand experience:** Design that can't be achieved with themes
- **Content-rich sites:** Editorial content + commerce (e.g., media company selling merch)
- **Multi-platform:** Same backend for web, app, kiosk, smart TV
- **Existing tech stack:** Already have a React/Next.js site, add commerce
- **Performance critical:** Sub-500ms page loads globally
- **Internationalization:** Complex multi-region logic

### Not Recommended When
- **Standard e-commerce needs:** Themes are faster to launch
- **Limited dev resources:** Themes require less maintenance
- **Tight timeline:** Themes ship faster
- **Budget constraints:** Headless requires more dev time
- **App ecosystem dependency:** Many apps only work with themes

### Headless vs Theme Decision Matrix

| Factor | Theme | Headless |
|--------|-------|----------|
| Time to launch | 4-8 weeks | 12-20 weeks |
| Dev cost | $ | $$$ |
| Maintenance | Low | Medium-High |
| Design flexibility | Medium | High |
| Performance | Good | Excellent |
| App compatibility | High | Low-Medium |

## Hydrogen Framework Basics

### Tech Stack
- **React 18+** (with Server Components)
- **Remix** (routing, data loading, SSR)
- **Vite** (build tool)
- **TypeScript** (recommended)
- **Tailwind CSS** (optional, but common)

### Project Structure

```
my-hydrogen-app/
├── app/
│   ├── routes/
│   │   ├── _index.tsx          # Homepage (/)
│   │   ├── products.$handle.tsx # Product page
│   │   ├── collections.$handle.tsx
│   │   └── cart.tsx
│   ├── components/
│   │   ├── ProductCard.tsx
│   │   ├── Header.tsx
│   │   └── Footer.tsx
│   ├── lib/
│   │   └── shopify.ts          # Storefront API client
│   └── root.tsx                # App shell
├── public/
├── package.json
└── remix.config.js
```

### Getting Started

```bash
# Create new Hydrogen app
npm create @shopify/hydrogen@latest

cd my-hydrogen-app

# Install dependencies
npm install

# Run dev server
npm run dev

# Open http://localhost:3000
```

### Connect to Shopify Store

```typescript
// app/lib/shopify.ts
import {createStorefrontClient} from '@shopify/hydrogen';

const client = createStorefrontClient({
  publicStorefrontToken: 'your-storefront-access-token',
  storeDomain: 'your-store.myshopify.com',
  storefrontApiVersion: '2024-10',
});

export const {getStorefrontApiUrl, getPublicTokenHeaders} = client;
```

### Basic Product Page Example

```typescript
// app/routes/products.$handle.tsx
import {json, type LoaderFunctionArgs} from '@shopify/remix-oxygen';
import {useLoaderData} from '@remix-run/react';
import {getStorefrontApiUrl, getPublicTokenHeaders} from '~/lib/shopify';

export async function loader({params, context}: LoaderFunctionArgs) {
  const {handle} = params;
  
  const response = await fetch(getStorefrontApiUrl(), {
    method: 'POST',
    headers: getPublicTokenHeaders(),
    body: JSON.stringify({
      query: PRODUCT_QUERY,
      variables: {handle},
    }),
  });
  
  const {data} = await response.json();
  
  return json({product: data.product});
}

export default function Product() {
  const {product} = useLoaderData<typeof loader>();
  
  return (
    <div>
      <h1>{product.title}</h1>
      <img src={product.featuredImage.url} alt={product.title} />
      <p>{product.description}</p>
      <p>${product.priceRange.minVariantPrice.amount}</p>
      {/* Add to cart button, variants, etc. */}
    </div>
  );
}

const PRODUCT_QUERY = `
  query Product($handle: String!) {
    product(handle: $handle) {
      id
      title
      description
      featuredImage {
        url
      }
      priceRange {
        minVariantPrice {
          amount
          currencyCode
        }
      }
    }
  }
`;
```

## Storefront API

### What is Storefront API?
GraphQL API for querying products, collections, cart, and managing checkout.

**Key capabilities:**
- Query products, collections, pages
- Create & manage cart
- Initiate checkout
- Customer login/register
- Multi-currency, multi-language

**What it CAN'T do:**
- Manage inventory (use Admin API)
- Fulfill orders (use Admin API)
- Access customer order history (use Customer Account API)

### Common Queries

#### Fetch Product by Handle
```graphql
query ProductByHandle($handle: String!) {
  product(handle: $handle) {
    id
    title
    description
    variants(first: 10) {
      edges {
        node {
          id
          title
          priceV2 {
            amount
            currencyCode
          }
          availableForSale
        }
      }
    }
  }
}
```

#### Fetch Collection with Products
```graphql
query CollectionByHandle($handle: String!) {
  collection(handle: $handle) {
    id
    title
    products(first: 20) {
      edges {
        node {
          id
          title
          handle
          featuredImage {
            url
          }
        }
      }
    }
  }
}
```

#### Create Cart & Add Item
```graphql
mutation CreateCart($lines: [CartLineInput!]!) {
  cartCreate(input: { lines: $lines }) {
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
                priceV2 {
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

## Customer Account API

New API (2024+) for customer-specific data:

- Order history
- Addresses
- Account details
- Returns & refunds

**Why separate?** Privacy & security. Requires customer authentication.

### Authentication Flow
1. Customer logs in via Shopify-hosted login
2. Receives access token
3. Frontend uses token to query Customer Account API

```typescript
// Example: Fetch customer orders
const ORDERS_QUERY = `
  query CustomerOrders {
    customer {
      orders(first: 10) {
        edges {
          node {
            id
            orderNumber
            totalPriceV2 {
              amount
            }
            fulfillmentStatus
          }
        }
      }
    }
  }
`;
```

## Oxygen Hosting

### What is Oxygen?
Shopify's edge hosting platform for Hydrogen storefronts, powered by Cloudflare Workers.

**Benefits:**
- **Global edge network:** Sub-100ms response times worldwide
- **Auto-scaling:** Handles traffic spikes (flash sales)
- **Built-in CDN:** Static assets cached globally
- **Integrated with Shopify CLI:** Deploy with one command
- **Environment variables:** Secure secrets management

### Deployment

```bash
# Deploy to Oxygen
npx shopify hydrogen deploy

# Deploys to:
# https://your-store.oxygen.shopifyapps.com
```

### Custom Domains
- Add custom domain in Shopify admin
- Point DNS to Oxygen
- SSL certificates auto-provisioned

### Environments
- **Production:** Live storefront
- **Preview:** Staging environment
- **Development:** Local dev server

## Hydrogen Components

Hydrogen provides pre-built components for common commerce patterns:

### ProductPrice
```tsx
import {ProductPrice} from '@shopify/hydrogen';

<ProductPrice data={product} variantId={selectedVariant.id} />
```

### Image (Optimized)
```tsx
import {Image} from '@shopify/hydrogen';

<Image
  data={product.featuredImage}
  sizes="(min-width: 1024px) 50vw, 100vw"
/>
// Automatically optimizes, lazy loads, responsive
```

### Money (Formatted Currency)
```tsx
import {Money} from '@shopify/hydrogen';

<Money data={product.priceRange.minVariantPrice} />
// Outputs: $49.99 (respects currency, locale)
```

## Headless Checkout Flow

### Cart → Checkout Handoff
1. Build cart in custom frontend (via Storefront API)
2. Create checkout URL from cart ID
3. Redirect to Shopify-hosted checkout
4. Customer completes purchase on Shopify checkout
5. Redirect back to your storefront (thank you page)

**Key point:** Checkout is still Shopify-hosted (extensible checkout from earlier docs applies).

```typescript
// Create cart and get checkout URL
const cart = await createCart([
  { merchandiseId: 'gid://shopify/ProductVariant/123', quantity: 1 }
]);

// Redirect to Shopify checkout
window.location.href = cart.checkoutUrl;
```

## When to Recommend Headless to Plus Merchants

### Strong Headless Candidates
- **Luxury brands:** Need pixel-perfect design control
- **Content + commerce:** Editorial sites with shopping (e.g., lifestyle magazines)
- **Multi-platform:** POS, kiosk, app, web all sharing Shopify backend
- **Performance-obsessed:** Sub-second loads critical to brand
- **Existing React site:** Adding commerce to existing Next.js site

### Stick with Themes
- **Standard D2C store:** Theme customization is sufficient
- **App-heavy:** Relying on 10+ checkout/theme apps
- **Small dev team:** Can't dedicate resources to custom frontend
- **Frequent changes:** Merchants who update site weekly (themes are easier)

### Hybrid Approach (Theme + Headless)
- **Main site:** Shopify theme (fast to market)
- **Landing pages:** Headless for key campaigns
- **Mobile app:** Headless for app experience
- **Kiosks/POS:** Headless for custom UX

## APAC Considerations for Headless

### Multi-Language (Singapore → SEA)
```typescript
// Hydrogen supports @shopify/hydrogen i18n
import {useLocale} from '@shopify/hydrogen';

const locale = useLocale(); // 'en-SG', 'zh-CN', 'ms-MY'

// Query Storefront API with language
const query = `
  query Products($language: LanguageCode) @inContext(language: $language) {
    products(first: 10) {
      edges {
        node {
          title # Translated title
        }
      }
    }
  }
`;
```

### Multi-Currency
```typescript
// Pass currency to Storefront API
const query = `
  query Products($currency: CurrencyCode) @inContext(country: SG, currency: $currency) {
    products(first: 10) {
      edges {
        node {
          priceRange {
            minVariantPrice {
              amount # In SGD
            }
          }
        }
      }
    }
  }
`;
```

### Performance for Asia
- **Oxygen edge:** Sub-100ms in Singapore, Jakarta, Bangkok
- **Image optimization:** Critical for slow mobile connections
- **Lazy loading:** Load products as user scrolls

## Headless Development Tips

### 1. Use TypeScript
Catch errors early, better IDE support:
```typescript
type Product = {
  id: string;
  title: string;
  priceRange: {
    minVariantPrice: {
      amount: string;
      currencyCode: string;
    };
  };
};
```

### 2. Cache Aggressively
```typescript
// Hydrogen caching
export async function loader({context}: LoaderFunctionArgs) {
  return json(
    {products: await fetchProducts()},
    {
      headers: {
        'Cache-Control': 'public, max-age=300, stale-while-revalidate=86400',
      },
    }
  );
}
```

### 3. Monitor Performance
- Core Web Vitals (LCP, FID, CLS)
- Time to Interactive (TTI)
- API response times
- Use: Lighthouse, WebPageTest, Shopify Analytics

### 4. SEO for Headless
- Server-side rendering (Hydrogen does this by default)
- Meta tags, Open Graph, structured data
- Sitemaps (generate from Shopify products)
- Canonical URLs

## Resources

- [Hydrogen Docs](https://shopify.dev/docs/custom-storefronts/hydrogen)
- [Storefront API Reference](https://shopify.dev/docs/api/storefront)
- [Customer Account API](https://shopify.dev/docs/api/customer)
- [Oxygen Hosting](https://shopify.dev/docs/custom-storefronts/oxygen)
- [Hydrogen Examples](https://github.com/Shopify/hydrogen/tree/main/examples)

---

**Next:** [B2B on Shopify →](05-b2b-on-shopify.md)
