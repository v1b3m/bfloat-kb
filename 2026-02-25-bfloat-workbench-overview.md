# Bfloat Workbench Overview

This repo is a pnpm/turborepo monorepo that ships both the Bfloat desktop workbench (Electron) and the Bfloat web platform (React Router + Express), plus shared UI/runtime packages.

## Purpose
- Desktop workbench for local project editing, AI-agent workflows, terminal access, preview, and deployment tooling.
- Web platform for authentication, project APIs, integrations/OAuth callbacks, updates metadata, and hosted app workflows.
- Shared cross-surface runtime so desktop and web reuse core types, UI primitives, and workbench feature modules.

## High-Level Architecture
- Desktop app shell: `apps/desktop/` (Electron main/preload/renderer)
- Web app + API host: `apps/web/` (React Router v7 + Express server)
- Shared workbench UI/runtime: `packages/workbench/` (`@bfloat/workbench-ui`)
- Desktop/web bridge abstractions: `packages/platform/` (`PlatformProvider`, transport contracts)
- Shared types and UI primitives: `packages/types/`, `packages/ui/`

## Runtime Flow (Desktop Startup)
1. Electron main boots from `apps/desktop/lib/main/main.ts`, loads env, and initializes Sentry/PostHog.
2. Main registers custom protocols (`bfloat://` deep links, `chat://` streaming endpoint) before window creation.
3. Main creates BrowserWindow via `createAppWindow()`, registers IPC/conveyor handlers (filesystem, terminal, AI agent, deploy, updates, imports, secrets, screenshots).
4. Preload exposes `window.conveyor` and `window.electron` to renderer.
5. Renderer mounts React app (`MemoryRouter`) and injects `PlatformProvider` backed by `window.conveyor`.
6. App handles auth token/session IPC, provider onboarding state, and routes into home/settings/project workspace.

## Runtime Flow (Web Startup)
1. `apps/web/server.js` boots Express and initializes Sentry, LaunchDarkly, and PostHog.
2. Server configures compression, CORS, metrics endpoint, static assets/dev middleware.
3. React Router request handler serves UI/API routes from `app/routes/*`.
4. Desktop auth/OAuth handoffs use web routes that redirect back through `bfloat://` deep links.

## Product Routes
Desktop renderer routes:
- `/` Home
- `/settings` Settings
- `/projects/:id` Project workspace
- `/auth` Desktop auth screen (when unauthenticated)

Web routes are file-system routes under `apps/web/app/routes/` and include:
- Marketing/auth pages
- `/api/*` endpoints for projects, integrations, deploy helpers, desktop OAuth callbacks, and update manifests

## Local Persistence
Desktop filesystem state:
- Projects: `~/.bfloat/projects/<projectId>`
- Provider integration settings: `~/.bfloat/config/settings.json`
- Launch config per project: `<project>/.bfloat/launch.json`
- Update state/binaries: Electron `userData` directory (for updater state/tools)

Desktop renderer/browser state:
- Auth/session/provider/theme/onboarding/deploy preferences in `localStorage`

## Read This First (For Agents)
1. Read this note.
2. Read root `README.md` for workspace scripts and environment setup.
3. Read `apps/desktop/lib/main/main.ts` and `apps/desktop/lib/main/app.ts` before touching desktop startup/IPC.
4. Read `apps/web/server.js` and relevant `apps/web/app/routes/*` files before touching auth, integrations, or API behavior.

## Historical Context
- Closely related desktop-only architecture note: [[2026-02-25-bfloat-ide-overview]]
