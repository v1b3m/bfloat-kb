# IDE Application MCP Screenshots

## Description
Screenshots were broken in the IDE after migrating to the sidecar architecture. The old Electron in-process path was near-instant, but the sidecar implementation spawned a fresh Chrome process per capture (~2-3s). Needed to both fix screenshot functionality and optimize performance.

## Progress
- [x] Port screenshot MCP tool from Electron main process to sidecar
- [x] Add screenshot route and MCP service in sidecar
- [x] Update conveyor-bridge and Preview.tsx to use sidecar API
- [x] Replace spawn-per-request with persistent headless Chrome via raw CDP
- [x] Attempted puppeteer-core — failed due to BiDi layer incompatibility with Bun --compile
- [x] Implemented raw CDP over WebSocket instead (zero dependencies)
- [x] Add graceful shutdown to prevent orphaned Chrome processes
- [x] Verify binary size unchanged (54MB)

## Decisions
- **Raw CDP over puppeteer-core**: puppeteer-core BiDi transport (BidiMapper.EventEmitter) crashes when bundled with bun build --compile. Setting protocol: cdp does not help since the BiDi code is evaluated at module level. Raw CDP is only ~100 lines and has zero dependencies.
- **Persistent browser**: Chrome is launched once on first screenshot request and reused. If it crashes, the next request re-launches automatically.
- **Random debug port**: Uses 9222 + random to avoid port collisions with user Chrome instances.

## Outcome
Screenshots work via sidecar with ~200-500ms capture time (after initial ~1-2s cold start). No new dependencies added.
Commit: ff929d0