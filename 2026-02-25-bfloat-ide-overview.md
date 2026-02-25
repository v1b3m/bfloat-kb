# Bfloat IDE Overview

This repo is a Tauri desktop app that ships the Bfloat IDE experience with a local-first architecture.

## Purpose
- Desktop IDE runtime using Tauri instead of Electron (lighter app shell).
- AI-assisted app building workflow (project creation, coding, preview, terminal, deployment tooling).
- Local-first data storage for projects/sessions/settings.

## High-Level Architecture
- UI app: `app/` (React + Zustand stores + feature pages)
- Desktop renderer + bridge: `packages/desktop/src/`
- Native shell: `packages/desktop/src-tauri/` (Rust/Tauri)
- Sidecar API server: `packages/sidecar/src/` (Bun + Hono)
- Shared runtime libraries: `lib/`

## Runtime Flow (Startup)
1. Tauri app boots and starts sidecar process (`src-tauri`).
2. Sidecar health is verified.
3. Renderer waits for `await_initialization` before mounting full app.
4. Renderer initializes Sidecar API client and compatibility bridge (`window.conveyor` shim).

See:
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-25-bfloat-ide-integrations-auth]]
- [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]]

## Product Routes
- `/` Home
- `/settings` Settings (including connected accounts)
- `/projects/:id` Project workspace

## Local Persistence
- Projects + sessions: `~/.bfloat-ide/projects.json`
- Settings/integration flags: `~/.bfloat-ide/config/settings.json`

## Read This First (For Agents)
1. Read this note.
2. Read [[2026-02-25-bfloat-ide-runtime-architecture]] for process boundaries and startup ownership.
3. Read [[2026-02-25-bfloat-ide-integrations-auth]] for provider auth behavior.
4. Read [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]] before making auth/integration changes.

## Historical Context
- Existing note: [[2026-02-21-bfloat-ide]]