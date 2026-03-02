---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: Desktop Bridge

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 11
- Source root(s): packages/desktop/src

## Inventory

### `packages/desktop/src/api/agent.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `AgentApi`
- Direct dependencies (internal): `packages/desktop/src/api/client.ts`, `packages/desktop/src/api/websocket.ts`
- Used by: 1 file(s) — `packages/desktop/src/api/index.ts`

### `packages/desktop/src/api/client.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `SidecarError`, `HttpClient`
- Used by: 4 file(s) — `packages/desktop/src/api/agent.ts`, `packages/desktop/src/api/filesystem.ts`, `packages/desktop/src/api/index.ts`, `packages/desktop/src/api/terminal.ts`

### `packages/desktop/src/api/filesystem.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `FilesystemApi`
- Direct dependencies (internal): `packages/desktop/src/api/client.ts`
- Used by: 1 file(s) — `packages/desktop/src/api/index.ts`

### `packages/desktop/src/api/index.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `initialiseSidecarApi`, `getSidecarApiSync`, `SidecarApi`, ` HttpClient `, ` SidecarWebSocket `, ` TerminalApi `, ` FilesystemApi `, ` AgentApi `
- Direct dependencies (internal): `packages/desktop/src/api/client.ts`, `packages/desktop/src/api/terminal.ts`, `packages/desktop/src/api/filesystem.ts`, `packages/desktop/src/api/agent.ts`
- Used by: 2 file(s) — `packages/desktop/src/conveyor-bridge.ts`, `packages/desktop/src/entry.tsx`

### `packages/desktop/src/api/terminal.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `TerminalApi`
- Direct dependencies (internal): `packages/desktop/src/api/client.ts`, `packages/desktop/src/api/websocket.ts`
- Used by: 1 file(s) — `packages/desktop/src/api/index.ts`

### `packages/desktop/src/api/websocket.ts`
- Responsibility: Desktop renderer API client/binding for sidecar communication.
- Exports: `SidecarWebSocket`
- Used by: 2 file(s) — `packages/desktop/src/api/agent.ts`, `packages/desktop/src/api/terminal.ts`

### `packages/desktop/src/bindings.ts`
- Responsibility: Desktop renderer boot/bridge/platform module.
- Exports: `commands`
- Direct dependencies (external): `@tauri-apps/api/event`, `@tauri-apps/api/webviewWindow`
- Used by: 0 file(s)

### `packages/desktop/src/conveyor-bridge.ts`
- Responsibility: Desktop renderer boot/bridge/platform module.
- Exports: `getPreviewProxyUrl`, `getPreviewProxyWsUrl`, `initConveyorBridge`, `windowBridge`, `terminalBridge`, `filesystemBridge`, `aiAgentBridge`, `projectSyncBridge`
- Direct dependencies (internal): `packages/desktop/src/api/index.ts`, `packages/desktop/src/entry.tsx`
- Direct dependencies (external): `@tauri-apps/api/window`, `@tauri-apps/api/webviewWindow`, `@tauri-apps/plugin-opener`, `@tauri-apps/plugin-dialog`
- Used by: 1 file(s) — `packages/desktop/src/entry.tsx`

### `packages/desktop/src/entry.tsx`
- Responsibility: Desktop renderer boot/bridge/platform module.
- Exports: `getInitialDeepLink`, `addDeepLinkListener`
- Direct dependencies (internal): `packages/desktop/src/platform.ts`, `packages/desktop/src/api/index.ts`, `packages/desktop/src/conveyor-bridge.ts`, `app/stores/deploy.ts`, `app/components/window/index.ts`
- Direct dependencies (external): `react`, `react-dom/client`, `react-router-dom`, `@tauri-apps/api/core`
- Used by: 1 file(s) — `packages/desktop/src/conveyor-bridge.ts`

### `packages/desktop/src/platform.ts`
- Responsibility: Desktop renderer boot/bridge/platform module.
- Exports: `getPlatformSync`, `platform`
- Used by: 1 file(s) — `packages/desktop/src/entry.tsx`

### `packages/desktop/src/styles.css`
- Responsibility: Desktop renderer boot/bridge/platform module.
- Used by: 0 file(s)

## Related
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-27-bfloat-ide-components-sidecar]]