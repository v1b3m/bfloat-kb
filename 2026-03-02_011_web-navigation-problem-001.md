**Ref:** [[2026-03-02_011_web-navigation-problem]]
**Depends:** none

### Context
This follow-up captures the remaining IDE preview issues after the original web navigation fix.

### Issue
In IDE preview (Tauri + sidecar proxy), Next.js pricing flow could still fail even when direct browser access worked:

1. Route display/state could drift during proxied navigation.
2. Proxied app API calls like `/api/plans` could be intercepted by sidecar `/api/*` auth middleware instead of being forwarded to the app.
3. Preview proxy target validation was not consistently applied across HTTP and preview WebSocket paths.
4. Preview message handling accepted route/error messages too broadly (potential cross-frame noise/leak surface).

### What Was Fixed
1. Path-preserving proxy behavior
- Preserved target pathname/query when resolving proxy root requests.
- Added route-change postMessage emission from injected preview script (`bfloat-preview-route`).

2. Tauri preview route sync
- Updated preview UI to consume route-change messages and keep URL display synced without forcing iframe reload loops.

3. App API forwarding before sidecar auth
- Added pre-auth bypass for non-sidecar `/api/*` routes when preview proxy is active, so app APIs (e.g. `/api/plans`) are proxied upstream.
- Kept sidecar-owned APIs (`/api/secrets`, `/api/agent`, etc.) protected.

4. Hardening pass (routing + leaks)
- Centralized preview target parsing/validation (localhost + protocol).
- Reused validation for `/preview-proxy/ws` target handling.
- Added unit tests for target parsing and upstream URL building.
- Tightened preview message handling to require expected iframe source and same-origin events.

### Commits
- `49403b8` — preserve target path and sync tauri preview route
- `eb5eb26` — proxy app api routes before sidecar auth
- `125fc2a` — harden preview routing and target validation

### Verification
- `bun test packages/sidecar/src/routes/preview-proxy.test.ts`
- `pnpm --filter ./packages/sidecar build`
- Manual: open IDE preview, navigate `/` -> `/pricing`, confirm URL sync + plans load in IDE, and confirm sidecar APIs remain auth-protected without credentials.
