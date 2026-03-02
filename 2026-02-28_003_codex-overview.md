### Context
Give me an overview of this codebase

### Acceptance Criteria
- [x] This task is updated to include an overview of this project

### Constraints
- None

### Resources
- None

---

## Codebase Overview

Bfloat IDE is a local-first AI desktop IDE built as a Tauri v2 app with a React/TypeScript UI and a Bun sidecar backend. The desktop shell starts the sidecar, then the renderer talks to it over localhost HTTP/WebSocket APIs.

### High-level architecture

1. `packages/desktop/src-tauri` (Rust/Tauri)
- Native shell, app window lifecycle, deep-link hooks, sidecar process startup.
- Exposes initialization commands used by the renderer.

2. `packages/sidecar` (Bun + Hono)
- Main backend API surface for agent sessions, terminal PTY, filesystem operations, template/project setup, and deploy workflows.
- Uses typed validation via Zod and streams real-time events over WebSockets.

3. `app/` + `packages/desktop/src` (React frontend)
- Core UI: project list, workbench/editor, terminal, AI chat, settings.
- State is primarily managed with Zustand and server interaction via React Query.

### Important directories

- `app/components`: UI modules (chat, editor, preview, deploy, integrations, settings).
- `app/stores`: global app/workbench state stores.
- `lib/agents`: provider abstractions and session orchestration for Claude/Codex/Bfloat.
- `lib/conveyor`: typed schema contracts and compatibility bridge surface.
- `resources/templates`: starter templates for generated projects.
- `resources/skills`: skill markdown injected into agent workflows.

### Runtime flow

1. App boots through Tauri.
2. Tauri starts sidecar and waits for health.
3. Renderer `entry.tsx` waits on `await_initialization`, then initializes sidecar API bindings.
4. React routes load (`/`, `/projects/:id`, `/settings`) and workbench features become available.

### Tooling and stack

- Frontend: React 19, TypeScript 5, Tailwind v4, Radix UI, CodeMirror, XTerm.
- Desktop: Tauri v2 + plugins (store, deep-link, updater, shell, etc.).
- Backend sidecar: Bun, Hono, Zod, PTY integration.
- Workspace/build: pnpm workspace; `pnpm dev` runs the desktop app in development.

### Note on docs drift

`README.md` still mentions Electron in parts, but the active runtime in this worktree is Tauri (`packages/desktop`, `src-tauri`, and `pnpm dev` using `tauri dev`).

## Progress
- [x] Reviewed current repository structure and runtime entry points.
- [x] Added an architecture-focused project overview to this task note.

## Decisions
- Kept this note scoped to architecture and execution flow so it remains a concise onboarding reference.
- Captured the Electron vs Tauri doc mismatch to prevent confusion when new contributors run the project.

## Outcome
Task complete. This note now contains a practical overview of the codebase architecture, major modules, and startup flow.

Commit: N/A (Obsidian task-note documentation update only)