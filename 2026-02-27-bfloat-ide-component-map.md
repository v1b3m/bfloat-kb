---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Component Map

Linked from: [[2026-02-25-bfloat-ide-overview]]

## Summary
- Full-system operational map of runtime components and modules.
- Coverage includes UI, state layer, desktop bridge, sidecar, shared library, and native shell.
- Mapping depth: path, role, direct dependencies, and reverse-usage signal.

## Domain Notes
- [[2026-02-27-bfloat-ide-components-ui]]
- [[2026-02-27-bfloat-ide-components-state]]
- [[2026-02-27-bfloat-ide-components-desktop-bridge]]
- [[2026-02-27-bfloat-ide-components-sidecar]]
- [[2026-02-27-bfloat-ide-components-shared-lib]]
- [[2026-02-27-bfloat-ide-components-native-shell]]

## Visual Graph
- [[2026-02-27-bfloat-ide-component-graph.canvas]]

## Coverage Counts
- UI files: 134
- State files: 16
- Desktop bridge files: 11
- Sidecar files: 25
- Shared lib files: 37
- Native shell files: 7
- Total mapped files: 230

## Critical Paths
### Startup Path
- Native shell startup: `packages/desktop/src-tauri/src/main.rs` + `lib.rs`
- Sidecar readiness: `packages/desktop/src-tauri/src/server.rs`
- Renderer init and bridge install: `packages/desktop/src/entry.tsx` + `conveyor-bridge.ts`

### Project Open/Sync
- Project route entry: `app/components/project/ProjectPage.tsx`
- State orchestration: `app/stores/project-store.ts`, `app/stores/workbench.ts`
- Sidecar route surface: `packages/sidecar/src/routes/project-files.ts`, `project-sync.ts`

### Provider Auth
- UI entry: `app/components/settings/sections/ConnectedAccountsSection.tsx`
- Store entry: `app/stores/provider-auth.ts`
- Sidecar provider routes: `packages/sidecar/src/routes/provider.ts`

### Deployment
- UI flow: `app/components/deploy/*`
- State: `app/stores/deploy.ts`
- Sidecar deploy routes: `packages/sidecar/src/routes/deploy.ts`

## Related
- [[2026-02-25-bfloat-ide-overview]]
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-25-bfloat-ide-integrations-auth]]