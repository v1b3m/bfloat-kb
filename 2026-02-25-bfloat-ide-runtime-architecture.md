# Bfloat IDE Runtime Architecture

Linked from: [[2026-02-25-bfloat-ide-overview]]

## Component Boundaries
- Native shell/runtime lifecycle: `packages/desktop/src-tauri/src/`
- Renderer boot + routing: `packages/desktop/src/entry.tsx`, `app/app.tsx`
- Sidecar HTTP/WS API: `packages/sidecar/src/server.ts`
- Compatibility bridge (legacy API shape): `packages/desktop/src/conveyor-bridge.ts`

## Startup Sequence
1. `main.rs` starts Tauri runtime and calls `bfloat_lib::run()`.
2. `lib.rs` initializes plugins, spawns async initialization, and prepares app/window lifecycle.
3. Sidecar connection logic in `lib.rs`:
   - If custom server URL is set and healthy, connect to it.
   - Otherwise spawn local sidecar.
4. Sidecar spawn path:
   - `src-tauri/src/cli.rs` starts `bfloat-sidecar` with `--hostname`, `--port`, `--password`.
   - `src-tauri/src/server.rs` performs health checks and readiness gating.
5. Renderer (`packages/desktop/src/entry.tsx`) calls `await_initialization` and waits.
6. Renderer initializes `SidecarApi` (`packages/desktop/src/api/index.ts`).
7. Renderer installs `initConveyorBridge()` so existing UI code can keep using `window.conveyor` semantics.

## Sidecar Security + Transport
- Sidecar binds localhost (default 127.0.0.1).
- All `/api/*` routes require auth middleware (`packages/sidecar/src/middleware/auth.ts`).
- Auth model: HTTP Basic (`bfloat:<password>`), with query-password fallback for SSE/WebSocket paths.
- HTTP is consumed via Tauri HTTP plugin (`packages/desktop/src/api/client.ts`).

## Sidecar Route Domains
Mounted in `packages/sidecar/src/server.ts`:
- `/api/terminal`
- `/api/agent`
- `/api/fs`
- `/api/project-files`
- `/api/project-sync`
- `/api/deploy`
- `/api/secrets`
- `/api/provider`
- `/api/local-projects`
- `/api/template`

## Renderer Boot + Route Ownership
- App entry: `packages/desktop/src/entry.tsx`
- Main app routes: `app/app.tsx`
  - Home: `/`
  - Settings: `/settings`
  - Project workspace: `/projects/:id`

## Why This Matters For Changes
- If changing startup/auth handshake: edit `src-tauri` + sidecar auth middleware + desktop API client together.
- If changing API semantics: update sidecar routes and bridge/client mapping together.
- If removing legacy conveyor behavior: update `conveyor-bridge.ts` and all callsites in `app/`.

Related:
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-25-bfloat-ide-integrations-auth]]
- [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]]