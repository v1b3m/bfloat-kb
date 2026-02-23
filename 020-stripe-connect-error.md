# Stripe Connect OAuth: Invalid redirect URI (desktop)

## Date
2026-02-23

## Symptom
Connecting Stripe from the workbench desktop flow failed with:

```json
{
  "error": {
    "message": "Invalid request: Invalid redirect URI: 'https://bfloat.dev/desktop/stripe/callback'. Ensure this uri exactly matches one of the uris specified in your application settings"
  }
}
```

## Observed OAuth request
The failing authorize URL was:

```text
https://connect.stripe.com/oauth/v2/authorize?client_id=ca_SBD9HksSDUNfBW3hMUyzeK4mMt2DLlTR&redirect_uri=https%3A%2F%2Fbfloat.dev%2Fdesktop%2Fstripe%2Fcallback&response_type=code&scope=read_write&state=...&stripe_landing=login&type=test
```

Key details:
- `client_id=ca_SBD9HksSDUNfBW3hMUyzeK4mMt2DLlTR`
- `redirect_uri=https://bfloat.dev/desktop/stripe/callback`
- `type=test`

## Root cause
Stripe OAuth redirect URI configuration existed in the wrong environment entry:
- The test client ID used by the app had only `http://localhost:3000/desktop/stripe/callback` configured.
- The app was requesting `https://bfloat.dev/desktop/stripe/callback`.
- Stripe requires an exact URI match per client/mode, so test OAuth rejected it.

Additional context:
- The app intentionally forces test onboarding by setting `type=test` in OAuth URL construction (`apps/web/app/lib/stripe-connect-oauth.server.ts`).
- Therefore redirect must be configured under **Test OAuth** for the test client ID.

## Resolution
Added the missing URI to Stripe Dashboard under:
- **Connect -> Onboarding options -> OAuth -> Test OAuth**
- Client: `ca_SBD9HksSDUNfBW3hMUyzeK4mMt2DLlTR`
- Added redirect URI: `https://bfloat.dev/desktop/stripe/callback`

Kept localhost redirect as well:
- `http://localhost:3000/desktop/stripe/callback`

## Why this works
After adding the exact URI to the same client ID and mode (`test`), Stripe validation passes and OAuth callback succeeds.

## Code references
- OAuth redirect selection: `apps/web/app/lib/stripe-connect-oauth.server.ts`
- Desktop redirect env var: `STRIPE_CONNECT_DESKTOP_REDIRECT_URI`
- Current env in repo: `prod.env`

## Prevention / checklist
When debugging Stripe OAuth redirect errors:
1. Copy full authorize URL and compare `client_id`, `redirect_uri`, and `type`.
2. Ensure redirect is configured for that exact client ID and mode (test vs live).
3. Ensure URI matches byte-for-byte (scheme/domain/path/trailing slash).
4. Configure both local and hosted callback URIs when both flows are used.
