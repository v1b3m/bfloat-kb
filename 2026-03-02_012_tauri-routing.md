# 2026-03-02_012_tauri-routing

## Why Tauri "intercepts" routes in IDE preview

### Short answer
In IDE preview, the app is intentionally loaded through the sidecar proxy (`/preview-proxy`) instead of directly from `http://localhost:PORT`.

That means browser-relative requests (including `fetch('/api/...')`) resolve against the **sidecar origin** first, not your app origin.

So this is not random interception. It is a consequence of the preview architecture.

---

## Architecture

### Direct browser (normal behavior)

```text
Browser tab (http://localhost:9000)
  ├─ GET /pricing      -> Next.js dev server
  ├─ GET /_next/*      -> Next.js dev server
  └─ GET /api/plans    -> Next.js dev server
```

Everything stays on one origin (`localhost:9000`), so `/api/plans` naturally goes to the app.

### Tauri IDE preview (proxied behavior)

```text
Tauri iframe (sidecar origin, e.g. http://127.0.0.1:61926)
  └─ GET /preview-proxy?target=http://localhost:9000/pricing
       -> sidecar forwards to Next.js
       -> sidecar injects preview script (route/error telemetry)

Then inside iframe page:
  - href="/pricing" and fetch('/api/plans') are origin-relative
  - origin is sidecar, so requests go to sidecar first
```

So `/api/plans` initially hits sidecar `/api/*`, not Next.js `/api/*`.

---

## Why `/api/plans` failed in IDE preview

Before fix, sidecar middleware order was effectively:

```text
1) /preview-proxy route
2) auth middleware on /api/*
3) sidecar api routes (/api/secrets, /api/agent, ...)
4) fallback proxy for unmatched paths
```

When iframe app called `fetch('/api/plans')`:

```text
Request -> sidecar /api/plans
  -> matched /api/* auth middleware first
  -> 401/blocked
  -> never reached fallback proxy to Next.js
```

Result in UI: pricing page showed fallback state ("Stripe Setup Required") even though direct browser `/api/plans` worked.

---

## What was changed

### 1) Preserve route/path behavior in preview proxy
- Root proxy requests now preserve `target` pathname/query.
- Prevents path drift/mismatch issues.

### 2) Route sync from iframe to host UI
- Injected script emits route changes (`bfloat-preview-route`).
- Tauri preview URL display tracks in-app navigation.

### 3) App `/api/*` forwarding before sidecar auth (when preview active)
- Unknown app API paths (e.g. `/api/plans`) are proxied upstream first.
- Sidecar-owned APIs remain protected (`/api/secrets`, `/api/agent`, etc.).

### 4) Hardening
- Centralized preview target validation (localhost + protocol).
- Same validation applied to preview WebSocket path.
- Preview message handling now validates source iframe + origin.

---

## Current routing model (after fix)

```text
Iframe request arrives at sidecar
  ├─ /preview-proxy*              -> preview proxy route handler
  ├─ /api/* (sidecar-owned)       -> auth middleware + sidecar routes
  ├─ /api/* (non-sidecar path)
  │    └─ if preview target active -> forward to app upstream
  └─ everything else              -> fallback proxy to active preview target
```

This keeps sidecar APIs secure while allowing proxied app APIs to behave like they do in normal browser development.

---

## Practical takeaway

In Tauri preview, `fetch('/api/...')` does **not** mean "go directly to Next.js" unless sidecar forwards it.

It means:
1. go to sidecar origin first,
2. then sidecar decides whether this is:
- a sidecar API,
- an app API to proxy,
- or an invalid route.

That decision point is why middleware order and route ownership are critical.
