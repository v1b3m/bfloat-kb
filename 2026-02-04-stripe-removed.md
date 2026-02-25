# Stripe Payment System Removal

## When It Was Removed

**Date:** January 29, 2026
**Commit:** `d0ba2a047345f50781edafcfa999de10bd56755a`
**Author:** Atem Aguer
**Branch:** `chore/remove-dead-frontend-code` merged into `develop`

### Commit Message

> Remove payments backend logic (Stripe, subscriptions, billing)
>
> Delete payment API routes, Stripe business logic, subscription DAO,
> and webhook handler. Remove Stripe usage reporting from worker and
> Subscription model from Prisma schema. Stripe client config and env
> vars are retained for future use.

---

## Files Deleted (13 files, 1,566 lines removed)

### Stripe Core Logic

| File | Lines | Purpose |
|------|-------|---------|
| `app/lib/stripe/index.ts` | 596 | Main `StripeService` class |
| `app/lib/stripe/webhooks.ts` | 233 | Webhook event handlers |
| `app/lib/stripe/subscription-utils.ts` | 89 | Tier definitions and features |

### API Routes

| File | Lines | Purpose |
|------|-------|---------|
| `app/routes/webhook.ts` | 33 | Stripe webhook endpoint at `/webhook` |
| `app/routes/api.subscription.ts` | 178 | Subscription management API |
| `app/routes/api.subscription-status.ts` | 87 | Get subscription status |
| `app/routes/api.cancel-subscription.ts` | 34 | Cancel subscription endpoint |
| `app/routes/api.plans.ts` | 82 | List available plans |

### Database & Data Access

| File | Lines | Purpose |
|------|-------|---------|
| `app/dao/subscription.server.ts` | 130 | Subscription data access layer |
| `prisma/schema.prisma` (partial) | 21 | Subscription model removed |

### Scripts & Workers

| File | Lines | Purpose |
|------|-------|---------|
| `app/scripts/worker.ts` (partial) | 13 | Stripe usage reporting removed |
| `app/scripts/purge-duplicate-customers.ts` | 64 | Cleanup script |

---

## What the StripeService Did

### 1. Customer Management

```typescript
async createOrRetrieveCustomer(userId: string, email: string): Promise<Stripe.Customer>
```

- Linked Stripe customers to internal user IDs via metadata
- Prevented duplicate customer creation

### 2. Subscription Initialization

```typescript
async initializeUserSubscription(userId: string, email: string, trialDays: number = 3)
```

- Auto-created free tier subscriptions for new users
- Supported trial periods

### 3. Checkout & Billing Portal

```typescript
async createCheckoutSession(userId: string, email: string, priceId: string): Promise<string>
async createPortalSession(customerId: string): Promise<string>
```

- Created Stripe Checkout sessions for upgrades
- Enabled customer self-service via billing portal

### 4. Usage Metering

```typescript
async reportUsage(subscriptionId: string, value: number = 1): Promise<void>
async getSubscriptionUsage(subscriptionId: string): Promise<{ monthlyMessagesCount, dailyMessagesCount }>
```

- Reported message usage to Stripe meters
- Tracked daily and monthly usage against limits

### 5. Subscription Tier Management

```typescript
async changeSubscriptionTier(subscriptionId: string, newPriceId: string): Promise<void>
async cancelSubscription(subscriptionId: string): Promise<void>
async reactivateSubscription(subscriptionId: string): Promise<void>
```

- Handled upgrades/downgrades between tiers (free/pro/super)
- Managed cancellation at period end or immediate

### 6. Webhook to Database Sync (Critical)

```typescript
async handleSubscriptionChange(event: Stripe.Event): Promise<void>
```

This was the critical function that synced Stripe events to the Prisma database:

```typescript
await prisma.subscription.upsert({
  where: { userId },
  create: { ... },
  update: {
    status: subscription.status,
    tier: tier,
    priceId: priceId,
    cancelAtPeriodEnd: subscription.cancel_at_period_end,
    // etc.
  },
});
```

---

## Subscription Tiers (Removed)

```typescript
enum SubscriptionTier {
  free = "free",
  pro = "pro",
  super = "super"
}

const DEFAULT_TIER_FEATURES = {
  free: {
    monthlyMessageLimit: 1000,
    dailyMessageLimit: 10,
    model: "claude-3-5-sonnet",
    maxProjects: 3,
    supportLevel: "community",
  },
  pro: {
    monthlyMessageLimit: 10000,
    dailyMessageLimit: 100,
    model: "claude-3-5-sonnet",
    maxProjects: 10,
    supportLevel: "email",
  },
  super: {
    monthlyMessageLimit: -1, // unlimited
    dailyMessageLimit: -1,
    model: "claude-3-5-sonnet",
    maxProjects: -1,
    supportLevel: "priority",
  },
};
```

---

## Prisma Subscription Model (Removed)

```prisma
model Subscription {
  id                    String    @id @default(cuid())
  userId                String    @unique
  stripeCustomerId      String
  stripeSubscriptionId  String
  priceId               String
  tier                  String    @default("free")
  status                String
  currentPeriodEnd      DateTime?
  cancelAtPeriodEnd     Boolean   @default(false)
  canceledAt            DateTime?
  createdAt             DateTime  @default(now())
  updatedAt             DateTime  @updatedAt

  user                  User      @relation(fields: [userId], references: [id])
}
```

---

## What Was Retained

- `app/lib/stripe/stripe.server.ts` - Basic Stripe client initialization
- `STRIPE_SECRET_KEY` environment variable
- `STRIPE_WEBHOOK_SECRET` environment variable
- `STRIPE_METER_ID` environment variable
- Stripe Connect OAuth files (separate from subscriptions)

---

## Webhook Events That Were Handled

| Event | Action |
|-------|--------|
| `checkout.session.completed` | Log new subscription from checkout |
| `customer.subscription.created` | Create/update DB subscription record |
| `customer.subscription.updated` | Sync status, tier, cancellation state |
| `customer.subscription.deleted` | Downgrade user to free tier |
| `invoice.payment_succeeded` | Log successful payment |
| `invoice.payment_failed` | Log failed payment |
| `customer.subscription.trial_will_end` | Log trial ending |
| `payment_method.attached` | Set as default payment method |

---

## To Restore Full System

If restoring the payment system, these components need to be brought back:

1. **Prisma model** - Add Subscription model back to schema
2. **StripeService class** - Full `app/lib/stripe/index.ts`
3. **Subscription utils** - Tier definitions
4. **API routes** - Subscription management endpoints
5. **DAO layer** - Database access functions
6. **Webhook handler** - With `handleSubscriptionChange()` database sync
7. **Worker integration** - Usage reporting

---

## Partial Restoration (February 4, 2026)

The webhook endpoint was partially restored but **without database sync**:

- `app/routes/webhook.ts` - Route at `/webhook`
- `app/lib/stripe/webhooks.ts` - Event handlers (logging only)

Changes made from original:
- Uses direct `stripe` import instead of `stripeService`
- `stripeService.getSubscription()` replaced with `stripe.subscriptions.retrieve()`
- `stripeService.handleSubscriptionChange()` **removed** (no DB sync)

The webhook now receives and logs Stripe events but does not persist any changes to the database.
