**Ref:** chore/day-4
**Depends:** 2026-03-02_011_web-navigation-problem

### Context

Stripe checkout session creation now succeeds in IDE preview and browser (`POST /api/checkout` returns `200`), but post-checkout success redirect uses `http://localhost:3000/success?...` when no explicit success URL is provided.

This is caused by fallback URL construction in the generated app checkout API route:
- `success_url = successUrl || ${NEXT_PUBLIC_SITE_URL || 'http://localhost:3000'}/success?...`

In dev/generated projects, this fallback can point to the wrong host/port and break expected return flow.

Task goal: make success/cancel redirect URL resolution robust in local/dev and explicit in client flow, so checkout always returns to the correct app origin.

### Acceptance Criteria

- [ ] Checkout flow uses correct `success_url` and `cancel_url` for the current running app origin in dev.
- [ ] No fallback to incorrect `localhost:3000` when app runs on another port (e.g. `9000`).
- [ ] Browser flow and IDE preview flow both complete checkout and land on intended return route.
- [ ] Behavior is deterministic when `NEXT_PUBLIC_SITE_URL` is unset.

### Constraints

- Do not regress Stripe session creation behavior or payment mode configuration.
- Keep fix scoped to redirect URL resolution and handoff logic.
- Preserve compatibility with explicit environment configuration (`NEXT_PUBLIC_SITE_URL`) when provided.

### Resources

- [[2026-03-02_011_web-navigation-problem]]
- Generated app path: `/Users/v1b3m/.bfloat-ide/projects/proj_1772464675512_ggzqn7tpc`
- Checkout route: `/Users/v1b3m/.bfloat-ide/projects/proj_1772464675512_ggzqn7tpc/app/api/checkout/route.ts`
- Pricing page client trigger: `/Users/v1b3m/.bfloat-ide/projects/proj_1772464675512_ggzqn7tpc/app/pricing/page.tsx`
