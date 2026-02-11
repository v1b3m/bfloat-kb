# Shared UI for Electron + Web

> **Date**: 2026-02-09
> **Status**: Research complete — awaiting decision on approach
> **Prerequisite**: Monorepo merge (see `merge-codebases.md`)

---

## Context

After the monorepo merge, we want a shared UI that works for both:
- **bfloat-ide** — Electron 37 desktop app (React 19, Electron Vite)
- **bfloat-app-engineer** — Web app (React Router 7 + Express SSR)

Goals:
- Build once, release to both platforms
- Share UI components where possible
- Keep platform-specific capabilities (terminal, file system, native menus) working on each target

Initial hypothesis: IoC / dependency injection. Research below evaluates this and alternatives.

---

## Current State (What We're Working With)

### The good news: the IDE renderer is already web-ready

The Electron renderer (`app/`) has **zero direct Electron imports**. All platform access goes through `window.conveyor`, which is injected by the preload script. The renderer doesn't know or care that it's running in Electron.

This means: **the React UI layer is already portable**. The abstraction boundary exists — it's `window.conveyor`.

### The `web/` folder is empty

`bfloat-ide/web/` contains only empty `app/` and `public/` directories. This is greenfield.

### Component overlap is real but limited to primitives

| Category | IDE | Backend | Shared |
|----------|-----|---------|--------|
| **UI primitives** (Button, Badge, etc.) | 18 | 38 | 10 overlap |
| **Feature directories** | 22 | 3 | ~0 |
| **Stores** | 7 | 3 | Pattern overlap, not code |
| **Hooks** | 7 | 4 | Pattern overlap, not code |

**Overlapping primitives**: Button, Badge, Card, Dialog, Dropdown Menu, Tooltip, Hover Card, Image Dropzone, Input, Collapsible.

**Divergent**: The IDE has 22 domain-specific directories (workbench, terminal, chat, agents, deploy, etc.) that the web app doesn't have. The web app has landing pages, OAuth flows, and Prisma-backed routes the IDE doesn't.

### Styling mismatch

- IDE: Manual Tailwind variants (no CVA)
- Backend: CVA (class-variance-authority) for component variants
- Both: Tailwind + Radix UI primitives (shadcn pattern)

### The Conveyor IPC system

The IDE's `window.conveyor` exposes 15 API classes (terminal, filesystem, deploy, AI agent, etc.) backed by 22 main-process handlers validated with Zod schemas. This is the surface that needs platform abstraction.

---

## Part 1: How Real Teams Solve This

### VS Code (vscode.dev + Desktop) — The Gold Standard

VS Code runs the **same codebase** on desktop and web using a **service-oriented architecture with interface-based abstraction**:
- Code split into `browser/`, `node/`, and `common/` directories
- Every platform capability (file system, terminal, networking) defined as an **interface**
- Desktop provides Node.js implementations; web provides browser/API-backed implementations
- Environment-aware loading with fallback chains

### Slack — Hybrid Approach

Slack explored four strategies and settled on a hybrid:
- Shared business logic through Redux actions bridged across processes
- Preload script as API gateway between web and native capabilities
- `electron-remote` using ES6 Proxy for async IPC, replacing synchronous `remote` module
- Local snapshot + background updates for performance

### Lyricistant — Clean Open-Source Example

Uses a **Delegate Pattern**: an interface with `send(channel, ...args)` and `on(channel, listener)`. In Electron, delegates to IPC. On web, emulates two processes with direct function calls. Platform-agnostic "Managers" hide all platform differences behind interfaces.

### Discord / Notion

Desktop apps are essentially wrappers around the web client, adding native integrations (file system, menus, offline) on top. This is the simplest approach but limits what the desktop app can do beyond the web.

---

## Part 2: IoC / Dependency Injection — Your Intuition Evaluated

**Your instinct is correct.** DI is the right pattern. The question is *which flavor*.

### Option A: React Context as DI (Recommended)

The most natural React pattern. No extra libraries.

```typescript
// packages/platform/src/types.ts
interface PlatformServices {
  transport: PlatformTransport;    // IPC ↔ HTTP/WS
  fileSystem: FileSystemService;
  terminal: TerminalService;
  notifications: NotificationService;
  fileDialog: FileDialogService;
}

// packages/platform/src/context.ts
const PlatformContext = createContext<PlatformServices | null>(null);

export function usePlatform() {
  const ctx = useContext(PlatformContext);
  if (!ctx) throw new Error('PlatformProvider missing');
  return ctx;
}

// In Electron app entry:
<PlatformContext.Provider value={electronServices}>
  <App />
</PlatformContext.Provider>

// In web app entry:
<PlatformContext.Provider value={webServices}>
  <App />
</PlatformContext.Provider>
```

| Pros                                             | Cons                               |
| ------------------------------------------------ | ---------------------------------- |
| Zero extra dependencies                          | No constructor injection           |
| Native React pattern, familiar to all React devs | Manual wiring of service instances |
| Works with React DevTools                        | Can get unwieldy with 20+ services |
| Easy to test (mock provider)                     | No lifecycle management            |
| Redux, Apollo, React Query all use this pattern  |                                    |
|                                                  |                                    |
|                                                  |                                    |

### Option B: InversifyJS (Full IoC Container)

Used in VS Code's architecture. Decorator-based (`@injectable`, `@inject`).

| Pros | Cons |
|------|------|
| Real IoC with automatic resolution | Requires `reflect-metadata` polyfill |
| Symbol-based type-safe identifiers | Heavier bundle size |
| Singleton/transient/request scoping | Steeper learning curve |
| Battle-tested in VS Code | Over-engineered for most React apps |

### Option C: tsyringe (Microsoft, lighter)

Similar to Inversify but lighter footprint.

| Pros | Cons |
|------|------|
| Simpler API | Still needs decorators + reflect-metadata |
| Good TS integration | Less documentation than Inversify |
| Smaller bundle | |

### Verdict

**Start with React Context (Option A).** Your service surface is bounded (~15 Conveyor API classes). Context handles this cleanly. Move to tsyringe/Inversify only if you hit 20+ services or need automatic resolution chains.

The key insight: you already have the interface boundary (`window.conveyor`). You just need to make it injectable rather than global.

---

## Part 3: The Critical Abstraction — PlatformTransport

This is the single most important interface. It replaces `window.conveyor` with something both platforms can implement.

```typescript
// packages/platform/src/transport.ts
interface PlatformTransport {
  invoke<T>(channel: string, ...args: any[]): Promise<T>;
  send(channel: string, ...args: any[]): void;
  on(channel: string, callback: (...args: any[]) => void): () => void;
}
```

### Electron implementation (wraps existing Conveyor)

```typescript
class ElectronTransport implements PlatformTransport {
  async invoke<T>(channel: string, ...args: any[]): Promise<T> {
    return window.conveyor.invoke(channel, ...args);
  }
  send(channel: string, ...args: any[]) {
    window.conveyor.send(channel, ...args);
  }
  on(channel: string, callback: (...args: any[]) => void) {
    return window.conveyor.on(channel, callback);
  }
}
```

### Web implementation (HTTP + WebSocket)

```typescript
class WebTransport implements PlatformTransport {
  private ws: WebSocket;

  async invoke<T>(channel: string, ...args: any[]): Promise<T> {
    const res = await fetch(`/api/${channel}`, {
      method: 'POST',
      body: JSON.stringify(args),
    });
    return res.json();
  }
  send(channel: string, ...args: any[]) {
    this.ws.send(JSON.stringify({ channel, args }));
  }
  on(channel: string, callback: (...args: any[]) => void) {
    const handler = (event: MessageEvent) => {
      const msg = JSON.parse(event.data);
      if (msg.channel === channel) callback(...msg.args);
    };
    this.ws.addEventListener('message', handler);
    return () => this.ws.removeEventListener('message', handler);
  }
}
```

### What this buys you

Every existing Conveyor API class (`TerminalApi`, `AppApi`, `FilesystemApi`, etc.) currently talks to `window.conveyor` directly. Replace that with `PlatformTransport` via Context, and the entire renderer works on both platforms without code changes.

---

## Part 4: Native Capability Fallbacks

Not everything has a 1:1 web equivalent. Here's how each Electron capability maps:

| Electron Capability | Web Fallback | Parity |
|---------------------|-------------|--------|
| Terminal (`node-pty` via IPC) | WebSocket → backend PTY spawner | Full |
| File System (native `fs`) | HTTP REST API to backend | Full |
| File Watch (`chokidar`) | WebSocket events from backend | Full |
| File Dialog (`dialog.showOpenDialog`) | `<input type="file">` or File System Access API | Good enough |
| Native Notifications | Browser Notification API (needs permission) | Good enough |
| Native Menu | Custom React context menu | Good enough |
| Window Controls | N/A (browser handles this) | N/A |
| Auto-Updates (`electron-updater`) | Service worker + backend versioning | Different |
| Screenshots (native) | `html2canvas` or similar | Degraded |

**Key principle**: You don't need 1:1 parity. Web fallbacks that serve the same purpose are fine.

---

## Part 5: Shared Package Strategy

### Proposed monorepo structure (extends `merge-codebases.md`)

```
bfloat/
├── apps/
│   ├── ide/      // workbench              # Electron app
│   └── backend/  // web              # Web app (bfloat-app-engineer)
├── packages/
│   ├── ui/                     # Shared UI primitives
│   │   ├── src/
│   │   │   ├── button.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── card.tsx
│   │   │   ├── dialog.tsx
│   │   │   └── ...
│   │   └── package.json        # peerDeps: react, react-dom
│   ├── platform/               # Platform abstraction layer
│   │   ├── src/
│   │   │   ├── transport.ts    # PlatformTransport interface
│   │   │   ├── services.ts     # PlatformServices interface
│   │   │   ├── context.tsx     # PlatformProvider + usePlatform()
│   │   │   ├── electron/       # Electron implementations
│   │   │   └── web/            # Web implementations
│   │   └── package.json
│   ├── shared/                 # Shared types, utils, constants
│   │   ├── src/
│   │   │   ├── types/          # API contracts, shared models
│   │   │   └── utils/          # Pure functions
│   │   └── package.json
│   └── tsconfig/               # Shared TS configs
```

### What goes where

| Package            | Contains                                                                     | Consumers                |
| ------------------ | ---------------------------------------------------------------------------- | ------------------------ |
| `@bfloat/ui`       | Button, Badge, Card, Dialog, Dropdown, Tooltip, Input, etc.                  | Both apps                |
| `@bfloat/platform` | `PlatformTransport`, `PlatformServices`, `PlatformProvider`, implementations | Both apps                |
| `@bfloat/shared`   | TypeScript types, Zod schemas, pure utility functions                        | Both apps                |
| `@bfloat/tsconfig` | Shared `tsconfig.base.json`                                                  | Both apps + all packages |

### What stays app-specific

| App | Keeps |
|-----|-------|
| **IDE** | Workbench, Terminal UI, Agent Chat, CodeMirror editor, Electron main process, MCP servers, preload script |
| **Backend** | Landing pages, OAuth routes, Prisma models, Server routes, deployment logic, worker scripts |

### Build approach

**Raw TypeScript, no pre-build.** Shared packages export `.ts` files directly. Each consuming app transpiles through its own Vite pipeline. Turborepo caches the results.

```json
// @bfloat/ui package.json
{
  "name": "@bfloat/ui",
  "main": "./src/index.ts",
  "types": "./src/index.ts",
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0",
    "react-dom": "^18.0.0 || ^19.0.0"
  }
}
```

React as `peerDependency` is critical — duplicate React instances cause hook errors.

---

## Part 6: Build-Time vs Runtime Platform Detection

### Build-time (Vite `define` — for tree-shaking)

```typescript
// electron.vite.config.ts (renderer section)
define: { '__PLATFORM__': JSON.stringify('electron') }

// vite.config.ts (web app)
define: { '__PLATFORM__': JSON.stringify('web') }

// In shared code — dead-code eliminated per target
if (__PLATFORM__ === 'electron') {
  // Stripped from web build
}
```

### Runtime detection (for minor UI variations)

```typescript
// Already works — check for the preload bridge
const isElectron = typeof window !== 'undefined'
  && typeof window.conveyor !== 'undefined';
```

### Vite resolve aliases (for swapping entire modules)

```typescript
// Each app's vite.config.ts
resolve: {
  alias: {
    '@platform-impl': isElectron
      ? '@bfloat/platform/electron'
      : '@bfloat/platform/web'
  }
}
```

**Use build-time for anything that should be tree-shaken. Use runtime for minor UI tweaks. Use aliases for swapping platform implementation modules.**

---

## Part 7: Styling Unification

The two apps have different Tailwind/component patterns:

| Aspect | IDE | Backend | Resolution |
|--------|-----|---------|------------|
| Tailwind version | 4 | 3 | Align to 4 during monorepo merge |
| Variant system | Manual | CVA | Adopt CVA in shared `@bfloat/ui` |
| CSS variables | Minimal | 50+ | Define shared design tokens in `@bfloat/ui` |
| Dark mode | Class-based | Class-based | Already aligned |

**Recommendation**: Standardize on CVA + Tailwind 4 in `@bfloat/ui`. Both apps can import shared components and extend with app-specific styles via Tailwind's `@apply` or additional classes.

---

## Part 8: What Needs to Happen (Backend API for Web)

The 22 Conveyor handlers in the IDE's main process need web equivalents. These become backend API endpoints:

| Handler Group | Electron (main process) | Web Equivalent |
|---------------|------------------------|----------------|
| Terminal | `node-pty` spawn/write/kill | WebSocket + backend PTY |
| File System | `fs.readFile/writeFile` | REST API (`GET/PUT /api/files`) |
| Project Files | `chokidar` watch + fs | REST + WebSocket events |
| Deploy | Electron-builder hooks | REST API (already exists in backend) |
| AI Agent | Claude SDK in main process | WebSocket + backend agent runner |
| App Lifecycle | `BrowserWindow` control | N/A (browser handles this) |
| Window | Minimize/maximize/close | N/A |
| Auth | IPC token exchange | Standard OAuth/cookie flow |

Many of these already exist in `bfloat-app-engineer` (it has its own terminal, file system, deployment, and agent logic). The work is unifying the API contracts, not building from scratch.

---

## Part 9: Decision Matrix

### Decision 1: DI Approach

| Approach | Complexity | Bundle Size | When to Choose |
|----------|-----------|-------------|----------------|
| **React Context** | Low | Zero | **Recommended** — service count is bounded (~15) |
| tsyringe | Medium | Small | If services exceed 20+ with complex resolution |
| InversifyJS | High | Larger | Enterprise scale, VS Code-like architecture |

### Decision 2: Shared UI Scope

| Strategy | Effort | Reuse | Risk |
|----------|--------|-------|------|
| **Primitives only** (`@bfloat/ui`) | Low | ~10 components | **Recommended start** — low risk, immediate value |
| Primitives + hooks + stores | Medium | ~25 modules | Good next step after primitives are stable |
| Full shared app shell | High | Maximum | High risk — the apps are architecturally different |

### Decision 3: Platform Abstraction Depth

| Strategy | Effort | Description |
|----------|--------|-------------|
| **Transport interface only** | Low | Just abstract `invoke/send/on` — keeps existing API classes | **Recommended start** |
| Transport + per-service interfaces | Medium | Define `FileSystemService`, `TerminalService`, etc. |
| Full VS Code-style service architecture | High | Interface-based with resolution, scoping, lifecycle |

### Decision 4: Build/Release Strategy

| Strategy | Complexity | Description |
|----------|-----------|-------------|
| Separate builds, shared source | Low | Each app builds independently via Turborepo |
| **Unified build pipeline** | Medium | **Recommended** — `turbo run build` builds everything, CI deploys each target |
| Single build, dual output | High | One Vite config producing both Electron + web — fragile, not recommended |

---

## Part 10: Recommended Phased Approach

### Phase 1: Extract shared primitives (during monorepo merge)
- Create `packages/ui` with the 10 overlapping components
- Standardize on CVA + Tailwind 4
- Both apps import from `@bfloat/ui`
- No platform abstraction yet — just UI components

### Phase 2: Platform abstraction layer
- Create `packages/platform` with `PlatformTransport` interface
- Implement `ElectronTransport` (wraps `window.conveyor`)
- Implement `WebTransport` (HTTP + WebSocket)
- Create `PlatformProvider` React Context
- Wire up in both app entry points

### Phase 3: Migrate Conveyor APIs to use PlatformTransport
- Existing `TerminalApi`, `FilesystemApi`, etc. now call `usePlatform().transport` instead of `window.conveyor` directly
- Electron behavior unchanged — just indirection
- Web gets the same APIs backed by HTTP/WebSocket

### Phase 4: Build backend API endpoints
- For each Conveyor handler, create a matching REST/WebSocket endpoint in `apps/backend`
- Reuse existing backend logic where it already exists (terminal, files, deploy, agents)
- Unified Zod schemas from `@bfloat/shared` validate both IPC and HTTP

### Phase 5: Unified CI/CD
- Turborepo pipeline: `turbo run build` builds IDE + backend + shared packages
- CI detects which packages changed, only builds/deploys affected targets
- Electron packaging (electron-builder) and web deployment (Docker/GCP) as separate pipeline stages

---

## Summary

Your IoC intuition is right. **React Context-based DI is the correct implementation** for your scale. The architecture is already 90% there — the `window.conveyor` abstraction means the renderer has no idea it's in Electron. You just need to:

1. Make `window.conveyor` injectable (React Context) instead of global
2. Provide an HTTP/WebSocket implementation for web
3. Extract shared UI primitives into a package
4. Let Turborepo orchestrate builds for both targets

The ~60-70% of renderer code (components, stores, hooks, types) can be shared. The remaining 30-40% is platform-specific and should stay in each app.
