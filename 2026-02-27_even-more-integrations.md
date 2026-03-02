# Even More Integrations

## Description
Implement RevenueCat integration parity with the Convex token/secrets-first flow:
- Use env-var credential modal (no OAuth)
- Auto-trigger RevenueCat setup guidance after credentials are saved
- Inject RevenueCat key into the next agent session env when available
- Fix compile-breaking MCP import gaps for Stripe/RevenueCat token handlers

## Progress
- [x] Added RevenueCat post-save auto-trigger in Project Settings single-key and multi-key credential save paths.
- [x] Added RevenueCat env-var injection (`EXPO_PUBLIC_REVENUECAT_API_KEY`) into pending agent session env before `/add-revenuecat` setup prompt.
- [x] Added compile-safe fallback token handlers for both Stripe and RevenueCat at `lib/conveyor/handlers/stripe-handler.ts` and `lib/conveyor/handlers/revenuecat-handler.ts`.
- [ ] Validate end-to-end in UI runtime manually.

## Decisions
- Kept MCP token resolution intentionally non-functional by default (graceful `success: false` placeholder) to avoid introducing new backend contracts in this pass.
- Applied the same fallback-handler pattern to both Stripe and RevenueCat so we do not leave equivalent import breakage in one integration.

## Outcome
In progress.