# RevenueCat Integration (Out-the-Box)

## What This Integration Covers
- Allows both the Remix web app and the Electron shell to connect a user's RevenueCat workspace using OAuth2 with PKCE.
- Persists account-level tokens inside `AccountSettings` so backend loaders/actions can contact RevenueCat without prompting the user again.
- Wraps the RevenueCat v2 REST API with typed helpers for projects, apps, entitlements, offerings, packages, products, and customers, exposing those helpers via Remix route loaders.

## Configuration & Prerequisites
- Environment: `REVENUECAT_OAUTH_CLIENT_ID`, `REVENUECAT_OAUTH_CLIENT_SECRET`, and `REVENUECAT_OAUTH_REDIRECT_URI` (falls back to `http://localhost:3000/api/oauth/revenuecat/callback`).
- OAuth scopes (joined into a single space-delimited string) live in `app/lib/revenuecat-oauth.server.ts`: every scope is prefixed with `project_configuration:` and grants read/write access to RevenueCat configuration resources.
- Prisma: `AccountSettings` contains nullable columns `revenuecatAccessToken`, `revenuecatRefreshToken`, `revenuecatTokenExpiresAt`, `revenuecatAccountId`, `revenuecatScopes`, and `oauthState` (JSON blob used during PKCE handshakes).

## OAuth / Connection Flow
1. **Entry points**
   - Web: `GET /api/oauth/revenuecat/connect` -> `app/routes/api.oauth.revenuecat.connect.tsx`
   - Desktop: `GET /desktop/revenuecat/connect` -> `app/routes/desktop.revenuecat.connect.tsx` (passes `isDesktop=true`)
   - Both routes are wrapped with `authkitLoader`, so an authenticated WorkOS user is required before redirecting to RevenueCat.
2. **`initiateRevenueCatOAuth`**
   - Generates PKCE verifier/challenge and a random state, stores `{ state, verifier, isDesktop }` as JSON in `AccountSettings.oauthState` via `prisma.accountSettings.upsert`.
   - Builds an authorize URL at `https://api.revenuecat.com/oauth2/authorize` with `client_id`, `redirect_uri`, `scope`, `state`, and PKCE params (`code_challenge`, `code_challenge_method=S256`).
   - Issues a Remix redirect so the browser/Electron opens RevenueCat's consent screen.
3. **Callback handling** (`GET /api/oauth/revenuecat/callback`)
   - Grabs `code`, `state`, and optional `error` params.
   - Uses `validateStateAndGetVerifier` when the WorkOS user session is still present; otherwise falls back to `getVerifierByState`, which scans stored `oauthState` blobs to recover the matching user and PKCE verifier.
   - Determines whether the request originated from the desktop shell (`isDesktop`) so it can respond with either a standard redirect to `/?modal=settings&tab=integrations` or a custom deep link `bfloat://revenuecat/callback?...`.
   - Exchanges the authorization code for tokens via `exchangeCodeForToken` (PKCE public-client flow, no `client_secret`).
   - Persists tokens with `saveOAuthToken`, which writes access token, refresh token (if provided), scope list, expiration timestamp, and optional `revenuecatAccountId`, while clearing `oauthState`.
4. **Token lifecycle**
   - `getValidAccessToken` (used by every API call) loads tokens from `AccountSettings`, proactively refreshes them 5 minutes before expiry using `refreshAccessToken`, and saves rotated refresh tokens back through `saveOAuthToken`.
   - `POST /api/oauth/revenuecat/revoke` calls `revokeOAuthToken`, nulling every RevenueCat-related column on `AccountSettings`.
   - `GET /api/oauth/revenuecat/status` surfaces `connected`, `expired`, `expiresAt`, `scopes`, and `revenuecatAccountId` to the UI.

## RevenueCat API Client (`app/lib/revenuecat-api.server.ts`)
- `revenuecatRequest` composes the base URL (`https://api.revenuecat.com/v2`), injects `Authorization: Bearer <token>`, and centralizes error logging + parsing before returning parsed JSON.
- Typed helper groups:
  - **Projects**: `listProjects`, `getProject`.
  - **Apps**: `listApps`, `getApp`, `createApp`.
  - **Entitlements**: `listEntitlements`, `getEntitlement`, `createEntitlement`, `updateEntitlement`, `deleteEntitlement`.
  - **Offerings**: `listOfferings`, `getOffering`, `createOffering`, `updateOffering`, `deleteOffering`.
  - **Packages**: `listPackages`, `getPackage`, `createPackage`, `updatePackage`, `deletePackage`.
  - **Products**: `listProducts`, `getProduct`, `createProduct`, `deleteProduct`.
  - **Customers**: `getCustomer`, `deleteCustomer`, `getActiveEntitlements`, `hasActiveEntitlement`, `hasActiveSubscription` utilities.
  - **Composite**: `getProjectOverview` fetches project + apps + entitlements + offerings + products in parallel for detailed UI panels.

## Remix Routes That Use the Client
- `GET /api/oauth/revenuecat/projects` -> calls `listProjects` and maps fields to `{ id, name, createdAt }`.
- `GET /api/oauth/revenuecat/projects/:projectId` -> `getProjectOverview`, emits normalized arrays for apps/entitlements/offerings/products.
- `GET /api/oauth/revenuecat/status` -> reads prisma directly (no API call) to show connection status.
- `POST /api/oauth/revenuecat/revoke` -> disconnects the account.
- `GET /api/oauth/revenuecat/connect` & `/desktop/revenuecat/connect` -> start the OAuth handshake.
- `GET /api/oauth/revenuecat/callback` -> completes OAuth, handles deep-linking for desktop.

## Desktop vs Web Nuances
- `oauthState` JSON tracks `isDesktop`. When `true`, the callback redirects to `bfloat://revenuecat/callback?...`; otherwise it returns to the Remix settings modal.
- All other logic (token storage, API usage) is shared; only the final redirect path differs.

## Logging & Error Handling
- `logger` is used throughout OAuth and API helper files, logging success/failure (tokens are marked REDACTED in logs).
- User-facing loaders/actions convert thrown errors into structured Remix `data()` responses with HTTP status codes (401 for unauthenticated, 400 for "not connected", 500 for unexpected failures).

This document should be updated whenever we change scopes, move token storage, or expose new RevenueCat endpoints.
