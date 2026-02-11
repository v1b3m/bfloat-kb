# Monorepo Migration: bfloat-ide + bfloat-app-engineer -> bfloat-workbench

**Date completed:** 2026-02-09
**Repo:** `git@bfloat_github:bfloat-inc/bfloat-workbench.git`
**Local path:** `/Users/v1b3m/Dev/bfloat/bfloat-workbench`

---

## What Was Done

Merged two repos into a single pnpm monorepo with Turborepo orchestration.

| Source | Branch | Commits | Destination |
|--------|--------|---------|-------------|
| `bfloat-ide` | `develop` | ~948 | `apps/workbench/` |
| `bfloat-app-engineer` | `main` | ~1325 | `apps/web/` |

**Full git history preserved** via `git filter-repo --to-subdirectory-filter`. `git log`, `git blame`, and `git bisect` all work correctly scoped to each app's subdirectory.

---

## Monorepo Structure

```
bfloat-workbench/
  package.json          # root workspace config (pnpm@10.28.2, turbo)
  pnpm-workspace.yaml   # packages: ["apps/*"]
  .npmrc                # shamefully-hoist=true, node-linker=hoisted
  turbo.json            # build/dev/lint/format tasks
  tsconfig.base.json    # scaffold only (apps don't extend it yet)
  .gitignore
  pnpm-lock.yaml
  apps/
    workbench/          # @bfloat/workbench (Electron desktop app)
    web/                # @bfloat/web (React Router web app)
```

## Key Scripts

```bash
pnpm dev:workbench    # Run workbench in dev mode
pnpm dev:web          # Run web app in dev mode
pnpm build:workbench  # Build workbench (mac, unsigned)
pnpm build:web        # Build web app
pnpm lint             # Turbo-orchestrated lint across both apps
pnpm format           # Turbo-orchestrated format across both apps
```

---

## Changes Made to Source Code

### `apps/workbench/package.json`
- `name`: `bfloat-ide` -> `@bfloat/workbench`
- `postinstall`: Removed `electron-builder install-app-deps &&` (can't resolve hoisted electron in pnpm workspaces). Now just runs `npx @electron/rebuild`.

### `apps/web/package.json`
- `name`: `bfloat` -> `@bfloat/web`
- `npm run` -> `pnpm run` in scripts: `build:sandbox`, `start`, `preview`, `lint:fix`
- Removed `postinstall: "husky"` and `prepare: "husky"` (root-level concern, deferred)

### Deleted Files
- `apps/workbench/.npmrc` (promoted to root)
- `apps/workbench/package-lock.json`
- `apps/workbench/bun.lockb`
- `apps/workbench/pnpm-lock.yaml`
- `apps/web/package-lock.json`

### NOT Touched (Intentionally)
- `electron-builder.yml` -- relative paths still correct from `apps/workbench/`
- `electron.vite.config.ts` -- `__dirname` resolves correctly
- `tsconfig*.json` in either app -- `baseUrl: "."` still correct
- `vite.config.ts` (web) -- `tsconfigPaths()` resolves correctly
- `server.js` -- relative paths resolve from cwd
- Prettier / ESLint configs -- kept per-app (different rules)
- Prisma config -- resolves from `apps/web/` cwd
- Dockerfiles -- broken, need monorepo context rework (deferred)
- `.github/workflows/` -- CI changes deferred

---

## Known Issues / Gotchas

### 1. electron-builder can't find electron in pnpm hoisted workspaces
`electron-builder install-app-deps` uses its own module resolution that doesn't walk up to the workspace root `node_modules`. The fix was to remove it from postinstall and rely on `@electron/rebuild` instead, which uses standard Node resolution.

If electron-builder builds fail in CI, add `electronVersion: "37.10.3"` (or current) to `electron-builder.yml`.

### 2. Peer dependency warnings (pre-existing)
- `ink` (used by `workos` CLI) in workbench expects React 18 but workbench uses React 19
- `workos` in web expects `@types/react>=19` but web uses React 18 types

These are inherited from the source repos and are harmless warnings.

### 3. `pnpm.onlyBuiltDependencies` warning
The field exists in both `apps/workbench/package.json` and root `package.json`. pnpm only reads it from root in workspaces. The per-app field is harmless but generates a warning. Can be removed from `apps/workbench/package.json` when convenient.

### 4. .env files are not committed
After cloning, copy `.env` files manually:
```bash
cp <source>/.env apps/workbench/.env
cp <source>/.env apps/web/.env
```

### 5. Prisma needs generate before web dev
```bash
pnpm --filter @bfloat/web exec prisma generate
pnpm dev:web
```

---

## Deferred Work (Out of Scope)

- [ ] CI/CD pipeline rework (`.github/workflows/`)
- [ ] Docker/GCP Cloud Build updates (need monorepo context paths)
- [ ] Shared packages (`packages/ui`, `packages/shared`, `packages/platform`)
- [ ] tsconfig inheritance (apps extend `tsconfig.base.json`)
- [ ] Prettier/ESLint unification across apps
- [ ] Dependency version alignment (React 18 vs 19, Tailwind 3 vs 4)
- [ ] Husky/lint-staged setup at root level
- [ ] Remove `pnpm.onlyBuiltDependencies` from `apps/workbench/package.json`
- [ ] Remove stale `electron.vite.config.*.mjs` temp files from `apps/workbench/`

---

## Rollback

Original repos (`bfloat-ide`, `bfloat-app-engineer`) were never modified. If anything breaks:
1. Delete `bfloat-workbench` and re-clone from remote
2. Or re-init and re-run the migration from the originals
