---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: Sidecar

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 25
- Source root(s): packages/sidecar/src

## Inventory

### `packages/sidecar/src/middleware/auth.ts`
- Responsibility: Sidecar middleware module.
- Exports: `authMiddleware`
- Direct dependencies (external): `hono`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/agent.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `agentRouter`
- Direct dependencies (external): `hono`, `zod`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/deploy.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `deployRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/filesystem.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `filesystemRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/health.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `healthRouter`
- Direct dependencies (external): `hono`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/local-projects.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `localProjectsRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 3 file(s) — `packages/sidecar/src/routes/project-files.ts`, `packages/sidecar/src/server.ts`, `packages/sidecar/src/services/agent-session.ts`

### `packages/sidecar/src/routes/preview-proxy.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `getActiveProxyTarget`, `previewProxyRouter`
- Direct dependencies (external): `hono`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/project-files.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `projectFilesRouter`
- Direct dependencies (internal): `packages/sidecar/src/routes/template.ts`, `packages/sidecar/src/skills-injector.ts`, `packages/sidecar/src/routes/local-projects.ts`, `packages/sidecar/src/services/agent-instructions.ts`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/project-sync.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `projectSyncRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/provider.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `providerRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/screenshot.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `screenshotRouter`
- Direct dependencies (internal): `packages/sidecar/src/services/screenshot.ts`
- Direct dependencies (external): `hono`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/secrets.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `secretsRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/template.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `templateRouter`
- Direct dependencies (external): `hono`, `zod`, `node:fs/promises`, `node:fs`
- Used by: 2 file(s) — `packages/sidecar/src/routes/project-files.ts`, `packages/sidecar/src/server.ts`

### `packages/sidecar/src/routes/terminal.ts`
- Responsibility: Sidecar HTTP route handler module.
- Exports: `writeToSession`, `resizeSession`, `killSession`, `onTerminalWSOpen`, `onTerminalWSMessage`, `onTerminalWSClose`, `cleanupAllSessions`, `terminalSessions`
- Direct dependencies (external): `hono`, `zod`, `node:os`, `node:fs`
- Used by: 0 file(s)

### `packages/sidecar/src/server.ts`
- Responsibility: Sidecar server/runtime module.
- Direct dependencies (internal): `packages/sidecar/src/middleware/auth.ts`, `packages/sidecar/src/routes/health.ts`, `packages/sidecar/src/routes/agent.ts`, `packages/sidecar/src/routes/filesystem.ts`, `packages/sidecar/src/services/agent-session.ts`, `packages/sidecar/src/routes/project-files.ts`
- Direct dependencies (external): `hono`, `hono/logger`, `hono/cors`
- Used by: 0 file(s)

### `packages/sidecar/src/services/agent-instructions.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `renderAgentsFile`, `renderClaudeFile`
- Direct dependencies (external): `node:fs/promises`, `node:path`
- Used by: 1 file(s) — `packages/sidecar/src/routes/project-files.ts`

### `packages/sidecar/src/services/agent-session.test.ts`
- Responsibility: Sidecar service/business logic module.
- Direct dependencies (external): `bun:test`
- Used by: 0 file(s)

### `packages/sidecar/src/services/agent-session.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `registerProvider`, `getProvider`, `getProviders`, `getBackgroundSessionByProject`, `getBackgroundSessionById`, `getBackgroundSessionByRealId`, `listBackgroundSessions`, `unregisterBackgroundSession`
- Direct dependencies (internal): `packages/sidecar/src/services/screenshot-mcp.ts`, `packages/sidecar/src/services/workspace-profile.ts`, `packages/sidecar/src/routes/local-projects.ts`, `packages/sidecar/src/services/codex-provider.ts`
- Direct dependencies (external): `crypto`, `@anthropic-ai/claude-agent-sdk`, `node:fs`, `node:path`
- Used by: 1 file(s) — `packages/sidecar/src/server.ts`

### `packages/sidecar/src/services/codex-provider.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `CodexProvider`
- Direct dependencies (external): `node:fs`, `node:path`, `node:os`
- Used by: 1 file(s) — `packages/sidecar/src/services/agent-session.ts`

### `packages/sidecar/src/services/screenshot-mcp.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `createScreenshotMcpServer`
- Direct dependencies (internal): `packages/sidecar/src/services/screenshot.ts`
- Direct dependencies (external): `@anthropic-ai/claude-agent-sdk`, `zod`
- Used by: 1 file(s) — `packages/sidecar/src/services/agent-session.ts`

### `packages/sidecar/src/services/screenshot.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `registerPreviewUrl`, `getPreviewUrl`
- Direct dependencies (external): `node:fs`, `node:path`, `node:os`, `node:child_process`
- Used by: 3 file(s) — `packages/sidecar/src/routes/screenshot.ts`, `packages/sidecar/src/server.ts`, `packages/sidecar/src/services/screenshot-mcp.ts`

### `packages/sidecar/src/services/session-reader.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `sessionToMessages`
- Direct dependencies (external): `node:fs`, `node:path`, `node:os`, `node:readline`
- Used by: 0 file(s)

### `packages/sidecar/src/services/workspace-profile.test.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `App`
- Direct dependencies (internal): `packages/sidecar/src/services/workspace-profile.ts`
- Direct dependencies (external): `bun:test`, `node:fs`, `node:os`, `node:path`
- Used by: 0 file(s)

### `packages/sidecar/src/services/workspace-profile.ts`
- Responsibility: Sidecar service/business logic module.
- Exports: `buildWorkspaceProfile`, `shouldBlockScaffoldCommand`
- Direct dependencies (external): `node:fs`, `node:path`
- Used by: 2 file(s) — `packages/sidecar/src/services/agent-session.ts`, `packages/sidecar/src/services/workspace-profile.test.ts`

### `packages/sidecar/src/skills-injector.ts`
- Responsibility: Sidecar server/runtime module.
- Direct dependencies (external): `node:fs/promises`, `node:fs`, `node:path`
- Used by: 1 file(s) — `packages/sidecar/src/routes/project-files.ts`

## Related
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-27-bfloat-ide-components-desktop-bridge]]