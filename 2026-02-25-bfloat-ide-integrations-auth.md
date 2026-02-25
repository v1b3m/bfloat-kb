# Bfloat IDE Integrations & Auth (Current State)

Linked from: [[2026-02-25-bfloat-ide-overview]]

## UI + Store Entry Points
- Settings page connected accounts UX:
  - `app/components/settings/sections/ConnectedAccountsSection.tsx`
  - `app/components/integrations/ProviderAuthModal.tsx`
- Provider auth state:
  - `app/stores/provider-auth.ts`

## Sidecar Provider Surface
Implemented in `packages/sidecar/src/routes/provider.ts`.

Important endpoints:
- `GET /api/provider/load-tokens`
- `POST /api/provider/save-tokens`
- `POST /api/provider/disconnect`
- `GET /api/provider/check-auth`
- `GET /api/provider/check-openai-auth`
- `GET /api/provider/check-expo-auth`
- `POST /api/provider/connect-anthropic`
- `POST /api/provider/connect-openai`
- `POST /api/provider/connect-expo`

## Current Auth Behavior by Provider
- Anthropic/Claude:
  - `connect-anthropic` runs `claude setup-token` (browser-based flow handled by CLI).
  - Also reads/writes Claude credential/config files in user home.
- OpenAI/Codex:
  - `connect-openai` starts local PKCE OAuth callback server on localhost and opens browser.
  - Exchanges code for tokens and writes Codex auth file.
- Expo:
  - `connect-expo` uses CLI login with username/password (+optional OTP).

## Token Load/Save Behavior
- Store-side lifecycle uses `provider.loadTokens()` and `provider.saveTokens()` from bridge.
- `providerAuthStore` tracks token presence + auth invalidation flags.
- Onboarding completion currently derives from connected AI provider token state.

## Bridge Layer Involvement
- Frontend provider APIs are routed via `packages/desktop/src/conveyor-bridge.ts` to `/api/provider/*`.
- Some provider auth progress handling uses SSE/event hooks in bridge modal flow.

## Key Observation
Current implementation is not “tokens only / no OAuth” yet.
OAuth-based flows are still active for Claude and OpenAI (and deep-link/browser assumptions remain in auth UX).

Related:
- [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]]
- [[2026-02-25-bfloat-ide-runtime-architecture]]