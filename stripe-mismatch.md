# Stripe API Key Mismatch Between MCP Tools and App Environment

## Problem

When using the Stripe MCP tools to create products and prices, they are created in a different Stripe account than the one configured in the app's `STRIPE_SECRET_KEY` environment variable.

### Symptoms

1. Products and prices are successfully created via MCP tools (`mcp__stripe__create_stripe_product`, `mcp__stripe__create_stripe_price`)
2. MCP tools can list the created products and prices successfully
3. When the app tries to use these price IDs in checkout, it fails with:
   ```
   No such price: 'price_xxxxx'
   ```

### Root Cause

The Stripe MCP tools use their own API credentials (likely configured through the MCP server), while the Next.js app uses `STRIPE_SECRET_KEY` from the environment. These are connected to **different Stripe accounts** (or different modes - test vs live).

## Solution

Create products and prices using the app's own Stripe API key instead of the MCP tools. This ensures the price IDs exist in the same Stripe account the app is connected to.

### Implementation

1. **Create a setup API route** (`/api/setup-stripe/route.ts`) that:
   - Uses the app's `stripe` instance (which uses `STRIPE_SECRET_KEY`)
   - Creates products if they don't exist
   - Creates prices for those products
   - Returns the created price IDs

2. **Update the pricing page** to:
   - Fetch available prices from the app's Stripe account on load
   - Show an "Initialize Stripe Products" button if no products exist
   - Dynamically use the price IDs from the app's Stripe account

### Code Example

```typescript
// app/api/setup-stripe/route.ts
import { NextResponse } from "next/server";
import { stripe } from "@/lib/stripe";

export async function POST() {
  // Create products using the app's stripe instance
  const product = await stripe.products.create({
    name: "Timer Pro",
    description: "Premium features",
  });

  const price = await stripe.prices.create({
    product: product.id,
    unit_amount: 499,
    currency: "usd",
  });

  return NextResponse.json({ priceId: price.id });
}
```

## Key Takeaway

**Always create Stripe resources using the same API key that will be used to access them.** MCP tools may use different credentials than your app's environment variables.

## Alternative Solutions

1. **Sync API keys**: Ensure the MCP Stripe tool uses the same API key as the app (may not be possible depending on MCP configuration)

2. **Use Stripe Dashboard**: Manually create products/prices in the Stripe Dashboard connected to your app's account, then hardcode those IDs

3. **Environment-aware setup**: Create a one-time setup script that runs with the app's credentials to initialize Stripe resources
