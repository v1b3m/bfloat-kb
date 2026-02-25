# Merging Bfloat IDE + Backend into a Monorepo

> **Date**: 2026-02-09
> **Status**: Research complete — awaiting decision on approach

---

## Context

We want to merge two separate repositories into a single monorepo:

- **bfloat-ide** — Electron 37 desktop app (React 19, TypeScript 5.9, Electron Vite, pnpm)
- **bfloat-app-engineer** — Full-stack SSR app (React Router 7 + Express, TypeScript 5.7, Vite, npm, Prisma + PostgreSQL + Redis, Docker + GCP Cloud Run)

### IDE Profile (bfloat-ide)
- **Runtime**: Electron 37 (main + renderer processes)
- **Frontend**: React 19, Tailwind 4, Radix UI, CodeMirror, xterm.js
- **Build**: Electron Vite → CommonJS (main) + ESM (renderer)
- **Package manager**: pnpm (with `shamefully-hoist=true`)
- **State**: nanostores, React Query
- **Key integrations**: Claude Agent SDK, Codex SDK, MCP servers, Stripe

### Backend Profile (bfloat-app-engineer)
- **Runtime**: Node.js 20+ (Express + React Router SSR)
- **Frontend**: React 18, Tailwind 3, Radix UI, Monaco Editor
- **Build**: Vite + React Router → Express server
- **Package manager**: npm (migration to pnpm needed)
- **Database**: Prisma 6.3 + PostgreSQL 15 + Redis 7 (BullMQ)
- **Deploy**: Docker multi-stage → GCP Cloud Run (web + worker + migrate services)
- **Key integrations**: Vercel AI SDK, Stripe, Supabase, E2B, WebContainer, Expo EAS
- **Size**: 170+ production dependencies, 8K-line package.json

### Overlap / Shared Surface
Both projects share: React, TypeScript, Tailwind, Radix UI, nanostores, Stripe, LaunchDarkly, Vercel AI SDK, React Query, MCP SDK. This is a strong signal that a `packages/shared` workspace will have real value.

---

## Part 1: Git History Preservation Strategies

How you physically merge the repos determines whether you keep, lose, or rewrite commit history.

### Option A: `git filter-repo` (Recommended)

Rewrite each repo's history so all files appear under a subdirectory, then merge.

```bash
# Clone the backend repo
git clone backend-repo backend-clone
cd backend-clone
git filter-repo --to-subdirectory-filter apps/backend

# In the monorepo (IDE already moved to apps/ide)
git remote add backend ../backend-clone
git fetch backend
git merge backend/main --allow-unrelated-histories
```

| Pros | Cons |
|------|------|
| Fast (20s vs 12min for subtree on 34K commits) | Rewrites commit SHAs |
| Endorsed by official Git docs | Requires `pip install git-filter-repo` |
| Clean, relocatable history | Old links to commits break |

**Verdict**: Best option if you want clean history and don't rely on stable SHAs externally.

### Option B: `git subtree add`

```bash
git remote add backend <backend-repo-url>
git subtree add --prefix=apps/backend backend main
```

| Pros | Cons |
|------|------|
| Built into Git, no extra tools | History interleaving makes `git log` noisy |
| Preserves original SHAs | `git subtree split` is slow on large repos |
| Allows continued syncing from source | Merge commits clutter the log |

**Verdict**: Good if you want zero tooling overhead and may need to sync from the old repo during a transition period.

### Option C: `git merge --allow-unrelated-histories` (manual move)

Move files in the source repo into a subdirectory first, then merge directly.

| Pros | Cons |
|------|------|
| Simplest conceptually | Requires a manual `git mv` commit in the source repo first |
| Preserves SHAs | No automated file relocation |

**Verdict**: Fine for small repos; more manual work than Option A.

### Option D: Copy + fresh start (no history preservation)

Just copy the files. Start history from the merge commit.

| Pros | Cons |
|------|------|
| Simplest | Lose all history |
| Clean slate | `git blame` is useless for old code |

**Verdict**: Only if history genuinely doesn't matter.

---

## Part 2: Monorepo Tool Options

These tools handle dependency management, build orchestration, caching, and task running across packages.

### Option 1: pnpm Workspaces Only (Minimal)

No additional tooling. Just pnpm's built-in workspace support.

**Setup**: One `pnpm-workspace.yaml` file:
```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

| Aspect | Assessment |
|--------|-----------|
| **Complexity** | Very low |
| **Caching** | None (no build caching) |
| **Task orchestration** | Basic (`pnpm --filter <pkg> run build`) |
| **Best for** | 2-3 packages, simple build graphs, teams that don't want extra tooling |
| **Limitation** | No parallel task scheduling, no affected-based testing, no remote caching |

**My take**: This is the foundation layer regardless. The question is whether you need a build orchestrator on top. With just 2-3 apps and a shared package, you might not — but you'll miss caching as the project grows.

### Option 2: pnpm Workspaces + Turborepo (Recommended for your case)

Turborepo adds intelligent caching and parallel task execution on top of pnpm workspaces.

**Setup**: Add `turbo.json` at root:
```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": { "dependsOn": ["^build"], "outputs": ["out/**", "dist/**"] },
    "dev": { "persistent": true, "cache": false },
    "lint": {},
    "typecheck": { "dependsOn": ["^build"] }
  }
}
```

| Aspect | Assessment |
|--------|-----------|
| **Complexity** | Low (single config file) |
| **Caching** | Content-hash local caching; remote caching via Vercel or self-hosted |
| **Task orchestration** | Automatic parallelization based on dependency graph |
| **Build speed** | 40-85% reduction via cache hits |
| **Ecosystem** | JS/TS only |
| **Best for** | Small-to-medium teams, Vercel users, quick adoption |
| **Limitation** | No code generators, no dependency graph visualization, JS/TS only |

**My take**: Best fit for bfloat right now. Minimal config, big payoff. The IDE is already pnpm-based, so adding Turborepo is ~15 minutes of work. You get caching immediately.

### Option 3: pnpm Workspaces + Nx

Nx is a full-featured monorepo platform with plugins, code generators, and rich tooling.

| Aspect | Assessment |
|--------|-----------|
| **Complexity** | Medium (more config, plugin system to learn) |
| **Caching** | Content-hash local caching; Nx Cloud for remote/distributed |
| **Task orchestration** | Dependency-graph-aware parallel execution |
| **Extras** | Code generators, dependency graph visualization, affected-based testing, `nx-electron` plugin |
| **Ecosystem** | Polyglot (JS, Java, Go, Rust, etc.) |
| **Best for** | Larger teams, enterprise, polyglot repos, teams that want conventions |
| **Limitation** | Steeper learning curve, can feel heavy for small projects |

**My take**: More powerful than Turborepo but more to learn. The `nx-electron` plugin is interesting but may not be necessary since you already have a working Electron Vite setup. Choose this if the backend is non-JS or if the team grows significantly.

### Option 4: Lerna v9 (+ Nx under the hood)

Lerna now delegates task running to Nx. Its unique value is npm package versioning/publishing.

| Aspect | Assessment |
|--------|-----------|
| **Complexity** | Low-medium |
| **Unique value** | Coordinated package versioning and publishing (independent or fixed mode) |
| **Best for** | Teams publishing multiple npm packages to a registry |

**My take**: Not relevant unless you're publishing internal packages to npm. The IDE and backend are apps, not libraries.

### Option 5: Moon (moonrepo)

Rust-based task runner with polyglot support and toolchain management.

| Aspect | Assessment |
|--------|-----------|
| **Complexity** | Medium |
| **Unique value** | Manages tool versions (Node, Rust, Go) across machines; polyglot-first |
| **Best for** | Teams with Rust/Go/Ruby + JS codebases |
| **Limitation** | Smaller community, newer, paid cloud features |

**My take**: Interesting for polyglot teams. Overkill if both repos are JS/TS.

---

## Part 3: Target Directory Structure

Regardless of tooling choice, the monorepo should look like this:

```
bfloat/
├── apps/
│   ├── ide/                  # Current bfloat-ide repo (Electron)
│   │   ├── app/              # Renderer (React)
│   │   ├── lib/              # Main process
│   │   ├── web/              # Web version
│   │   ├── resources/
│   │   ├── electron.vite.config.ts
│   │   ├── electron-builder.yml
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── backend/              # Current bfloat-app-engineer repo
│       ├── app/              # React Router SSR app + routes
│       ├── prisma/           # Database schema + migrations
│       ├── templates/        # Project templates (Expo, Next.js)
│       ├── infrastructure/   # E2B templates, sandbox configs
│       ├── mc3po/            # Backend utilities
│       ├── scripts/          # Build + worker scripts
│       ├── server.js         # Express production entry
│       ├── docker-compose.yml
│       ├── Dockerfile
│       ├── package.json
│       └── tsconfig.json
│
├── packages/
│   └── shared/               # Shared types, utils, constants
│       ├── src/
│       ├── package.json
│       └── tsconfig.json
│
├── package.json              # Root (workspace config, shared devDeps)
├── pnpm-workspace.yaml       # Workspace definition
├── turbo.json                # (if using Turborepo)
├── tsconfig.base.json        # Shared TypeScript config
├── .eslintrc.js              # Shared ESLint config
├── .prettierrc               # Shared Prettier config
└── .github/                  # CI/CD workflows
```

### Why `packages/shared/`?

Extracting shared types/utils into a workspace package:
- Eliminates copy-paste between IDE and backend
- Enables type-safe API contracts (shared request/response types)
- Both apps import from `@bfloat/shared` — changes propagate instantly
- TypeScript project references enable incremental compilation

---

## Part 4: Common Pitfalls to Watch For

| Pitfall | Mitigation |
|---------|-----------|
| **Lost Git history** | Use `git filter-repo` or `git subtree`, never copy-paste |
| **Broken CI/CD** | Rewrite pipelines to use `--filter` or `--affected` flags |
| **Phantom dependencies** | pnpm's strict mode prevents this — but currently using `shamefully-hoist=true`, which weakens this guarantee |
| **Inconsistent configs** | Consolidate ESLint/TS/Prettier into root-level shared configs |
| **Stale .gitignore** | Audit both repos' `.gitignore` before merging |
| **Open PRs** | Close/merge all open PRs in both repos before migration |
| **TypeScript pain** | Set up `tsconfig.base.json` with project references from day one |
| **"Big bang" migration** | Migrate one repo first, verify CI/builds, then migrate the second |
| **Electron Vite conflicts** | The IDE's Electron Vite config needs its paths updated to reflect the new location under `apps/ide/` |
| **npm → pnpm migration** | Backend currently uses npm. Must delete `package-lock.json`, run `pnpm import` (converts lockfile), then `pnpm install`. Test thoroughly — hoisting differences can break imports |
| **Docker build context** | Backend Dockerfiles assume repo root = build context. In a monorepo, either update `context: ./apps/backend` in docker-compose or use root context with adjusted `COPY` paths |
| **GCP Cloud Build** | `cloudbuild.yaml` in the backend assumes single-repo structure. Needs rewriting to scope builds to `apps/backend/` changes only |
| **Prisma path resolution** | Prisma expects `prisma/schema.prisma` relative to the package. Verify `prisma generate` and `prisma migrate` still work from `apps/backend/` |
| **React version mismatch** | IDE uses React 19.1, backend uses React 18.2. Both can coexist in separate workspace packages, but `packages/shared` must not export React components unless you align versions |
| **Tailwind version mismatch** | IDE uses Tailwind 4, backend uses Tailwind 3. Same story — keep configs per-app until you choose to align |

---

## Part 5: Decision Matrix

Here's a summary for you to weigh:

### Decision 1: Git History Strategy

| Approach                 | Complexity | History Quality    | Tool Required   | Recommendation                 |
| ------------------------ | ---------- | ------------------ | --------------- | ------------------------------ |
| **A. `git filter-repo`** | Low        | Clean, relocated   | pip install     | **Recommended**                |
| B. `git subtree`         | Low        | Noisy but complete | None (built-in) | Good alternative               |
| C. Manual merge          | Medium     | Manual work        | None            | Fine for small repos           |
| D. Fresh start           | Trivial    | None               | None            | Only if history doesn't matter |

### Decision 2: Monorepo Tooling

| Tool | Complexity | Caching | Polyglot | When to Choose |
|------|-----------|---------|----------|----------------|
| **pnpm only** | Very Low | No | N/A | If you want zero overhead and have <3 packages |
| **pnpm + Turborepo** | Low | Yes (local + remote) | No (JS/TS only) | **Best balance for your case** — fast setup, real caching |
| pnpm + Nx | Medium | Yes (local + Nx Cloud) | Yes | If backend is non-JS, or team grows to 5+ devs |
| Lerna v9 | Low-Med | Yes (via Nx) | No | Only if publishing npm packages |
| Moon | Medium | Yes (paid cloud) | Yes | If heavy polyglot (Rust + JS) |

### Decision 3: Shared Package Strategy

| Approach | Effort | Benefit |
|----------|--------|---------|
| **Create `packages/shared/` from day one** | Medium | Type-safe contracts, no duplication |
| Share nothing initially | Low | Faster migration, extract later when needed |
| Internal npm packages | High | Only if teams are truly independent |

---

## Part 6: Recommended Approach (My Pick)

Both repos are TypeScript/Node — Turborepo is confirmed as viable.

1. **Git strategy**: `git filter-repo` — clean history, fast, well-supported
2. **Tooling**: pnpm workspaces + Turborepo — minimal config, immediate caching benefits
3. **Structure**: `apps/ide` + `apps/backend` + `packages/shared`
4. **Migration order**:
   1. Close/merge all open PRs in both repos
   2. Create the monorepo scaffold (root `package.json`, `pnpm-workspace.yaml`, `turbo.json`, `tsconfig.base.json`)
   3. Migrate IDE first — use `git filter-repo --to-subdirectory-filter apps/ide`, merge into monorepo, verify Electron Vite builds + dev mode + packaging
   4. Migrate backend — use `git filter-repo --to-subdirectory-filter apps/backend`, merge, convert npm → pnpm (`pnpm import`), verify build + Docker + Prisma
   5. Fix Docker build contexts — update `Dockerfile`, `docker-compose.yml`, and `cloudbuild.yaml` for new paths
   6. Hoist shared devDeps to root (ESLint, Prettier, TypeScript)
   7. Extract shared types/utils into `packages/shared/` (start with Stripe types, API contracts, feature flag definitions — the highest-overlap areas)
   8. Update CI/CD pipelines (use Turborepo `--filter` for scoped builds)
   9. Archive old repos (set to read-only, add README pointing to monorepo)

### Version Alignment Plan (not blocking, do later)
- React 18 → 19 in backend (when ready)
- Tailwind 3 → 4 in backend (when ready)
- Align on a single TS version (5.7 vs 5.9)

---

## Next Steps

- [x] Confirm backend tech stack — **TypeScript/Node (React Router + Express + Prisma + PostgreSQL + Redis)**
- [ ] Choose git history strategy (A/B/C/D)
- [ ] Choose monorepo tool (1-5)
- [ ] Choose shared package strategy
- [ ] Execute migration
