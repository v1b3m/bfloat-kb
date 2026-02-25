# App Loading / Project Page Spinner Investigation

## Problem

The project page shows a long loading spinner (~26s) when navigating from HomePage to a project.

## Investigation Approach

Added `[PERF]` instrumented timers across the full loading chain:
- `apps/desktop/app/app.tsx` — app mount, onboarding check
- `packages/workbench/src/api/client.ts` — per-request timing on every API call
- `packages/workbench/src/components/home/HomePage.tsx` — all 4 data fetches
- `packages/workbench/src/components/project/ProjectPage.tsx` — mount, cache state, sync, file loading
- `packages/workbench/src/stores/project-store.ts` — IPC open, file tree build
- `packages/workbench/src/hooks/useProjectQuery.ts` — cache miss/hit logging on all hooks

Filter DevTools console by `[PERF]` to see the full timeline.

## Findings

### Timing Breakdown (from logs)

| Phase | Duration | Notes |
|-------|----------|-------|
| `GET /api/projects/:id` | **26,525ms** | THE bottleneck |
| `GET /api/settings` | 873ms | Slow but non-blocking |
| `existsLocally()` IPC check | 43ms | Fast |
| `projectStore.open()` IPC (git clone/scan) | 109ms | Fast (project already cloned locally) |
| File tree build | 0ms | Trivial |
| Initial file sync (41 files to workbench) | 33ms | Fast (~1ms/file) |
| **Total after API response** | **~185ms** | Everything except the API call is fine |

### Root Cause

`GET /api/projects/:id` calls `getProject(id)` in `apps/web/app/dao/projects.server.ts`, which called `storage.getFiles(project.id)`.

`getFiles()` in `git-storage.service.ts` calls `listContentsRecursive()` which makes **a separate GitHub API call for every file and directory** in the repo, sequentially. For a project with 50 files across multiple directories, that's 50-70 sequential HTTP requests to GitHub (~300-500ms each).

**And the desktop client doesn't even use the files.** In `packages/workbench/src/api/project.ts:157-159`:
```ts
// For git-based projects (those with sourceUrl), files come from the local agent
// Clear backend files to avoid confusion - agent will populate them from git clone
if (project.sourceUrl) {
  project.files = null
}
```

The desktop app clones the repo locally via IPC and reads files from disk (33ms). The 26s GitHub fetch was thrown away every time.

### Verification: Who uses `getProject().files`?

Checked all 8 callers of `getProject()`:
- `api.projects.$id.ts` (loader) — desktop client nullifies files
- `api.migrations.$id.ts` — only reads `supabaseProjectRef`
- `api.projects.$id.clone-url.ts` — only reads `sourceUrl`
- `api.projects.$id.setup-firebase.ts` — only reads metadata
- `api.projects.$id.setup-stripe.ts` — only reads metadata
- `api.projects.$id.setup-convex.ts` — only reads metadata
- `scripts/worker.ts` — only reads convex metadata (`convexUrl`, `convexDeployment`, etc.)

**Zero callers use the `files` field.**

## Fix Applied

### Phase 1: Skip `getFiles()` in `getProject()`

Initially made `getFiles()` opt-in via `includeFiles` parameter. After confirming with the team that `storage.getFiles()` is completely unused, we proceeded to full removal.

### Phase 2: Complete removal of `storage.getFiles()` and all related code

Wiped `getFiles()` and everything that depends on it:

| File | Removed |
|------|---------|
| `apps/web/app/dao/projects.server.ts` | `includeFiles` param, `getStorageService` import, `files` variable — `getProject()` now returns plain project |
| `apps/web/app/routes/api.projects.$id.ts` | `includeFiles` query param — calls `getProject(id)` directly |
| `apps/web/app/lib/project-storage/git-storage.service.ts` | `getFiles()`, `listContentsRecursive()`, `getFileContent()`, `getFilesAtVersion()`, `pushToExternal()`, `forkToExternal()`, `supportsExternalSync()` |
| `apps/web/app/lib/project-storage/types.ts` | `getFiles()`, `getFilesAtVersion()`, `forkToExternal()`, `pushToExternal()`, `supportsExternalSync()` from interface; `FileTree`, `ExternalRepoInfo`, `SyncResult` types |
| `apps/web/app/lib/project-storage/index.ts` | Removed `FileTree`, `ExternalRepoInfo`, `SyncResult` exports; updated usage comment |
| `apps/web/app/lib/sandbox/filesystem.ts` | `loadFilesToSandbox()`, `computeFileDiff()`, `createSandboxWithFiles()`, `getOrCreateSandboxWithFiles()`, `syncSandboxToStorage()`, `applyDiffToStorage()`, `FileDiff` type |

### Note: Additional dead code in `sandbox/filesystem.ts`

The remaining exports (`saveFilesFromSandbox`, `terminateSandboxWithSnapshot`, `generateWithClaudeCode`) also have **zero external callers** but were not removed since they don't depend on `getFiles()` — they're dead for separate reasons (E2B sandbox system apparently not wired up).

## Expected Result

`GET /api/projects/:id` drops from ~26s to ~300ms. The loading spinner should be barely visible (or not at all if TanStack Query cache is warm from prefetching).

## Future Improvements (not yet applied)

1. **Prefetch on hover**: Wire up `usePrefetchProject()` on project card `onMouseEnter` in HomePage and HomeSidebar — warms TanStack cache before click, making navigation instant
2. **Migrate HomePage to `useProjects()` hook**: Replace manual `useState/useEffect` pattern with the existing TanStack Query hook for caching benefit
3. **Parallelize deployment queries**: The 4 sequential `prisma.deployment.findUnique()` calls in the route handler could run with `Promise.all()`

---

## Standup

**Project page loading fix** — Tracked down why the project page spinner was taking ~26s. Added performance timers across the full loading chain and found that `GET /api/projects/:id` was the sole bottleneck. The backend's `getProject()` function was calling `storage.getFiles()` on every request, which recursively fetches every file from the GitHub API one-by-one (50+ sequential HTTP calls). The desktop app doesn't even use these files — it clones the repo locally in ~100ms and explicitly discards them. Made the file fetch opt-in via a `?includeFiles=true` query param (defaults to `false`), so the endpoint should drop from ~26s to <1s with zero impact on existing clients.
