# Bfloat IDE Codebase Map

Linked from: [[2026-02-25-bfloat-ide-overview]]

## Monorepo Structure
- `app/`
  - Product UI and page-level React components.
  - Key areas: home, project/workbench, settings, onboarding, editor/preview/terminal components.
- `app/stores/`
  - Zustand store layer.
  - Important stores:
    - `local-projects.ts` project/session persistence via sidecar local-project APIs.
    - `project-store.ts` reactive filesystem state backed by project-files APIs.
    - `workbench.ts` UI/workflow state for editor + chat + terminal integration.
    - `provider-auth.ts` provider token/auth state.
- `packages/desktop/src/`
  - Renderer entry and sidecar API client.
  - `api/` typed HTTP + WS clients.
  - `conveyor-bridge.ts` compatibility shim from legacy conveyor API shape.
- `packages/desktop/src-tauri/`
  - Rust native app shell and sidecar process orchestration.
  - Window commands, startup lifecycle, readiness gating.
- `packages/sidecar/src/`
  - Bun/Hono sidecar server.
  - Route modules for terminal, agents, files, project sync, providers, deploy, secrets, templates, local projects.
  - Service modules for agent session lifecycle and streaming.
- `lib/`
  - Shared logic:
    - `lib/agents/` provider adapters (Claude/Codex/Bfloat)
    - `lib/conveyor/schemas/` shared API schema definitions
    - `lib/mcp/` MCP server integration utilities

## Core User Flows (Ownership)
- Create/open project:
  - UI: `app/components/home/HomePage.tsx`, `app/stores/local-projects.ts`
  - Sidecar: `routes/local-projects.ts`, `routes/project-files.ts`
- Workbench coding loop:
  - UI: `app/components/workbench/Workbench.tsx`
  - Stores: `project-store.ts`, `workbench.ts`
  - APIs: terminal/project-files/agent routes through bridge
- Provider setup:
  - UI: settings connected accounts and auth modal
  - Store: `provider-auth.ts`
  - Sidecar: `routes/provider.ts`

## Data Storage Map
- Project index + sessions: `~/.bfloat-ide/projects.json`
- Integration settings flags: `~/.bfloat-ide/config/settings.json`
- Provider-specific credentials currently live in provider-native locations as well (Claude/Codex/Expo files).

## Coupling Notes
- UI still depends on legacy `window.conveyor` shape through bridge.
- Sidecar APIs are the runtime source of truth for filesystem, terminal, and provider state.
- Startup and auth handshake spans Rust + TypeScript codepaths.

Related:
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-25-bfloat-ide-integrations-auth]]
- [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]]