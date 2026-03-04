**Ref:** chore/day-4
**Depends:** 2026-03-02_011_web-navigation-problem

### Context

In IDE preview for a generated Next.js app, the **first** navigation to `/pricing` sometimes bounces back to `/` (home). Subsequent navigation attempts often work.

Observed logs (representative):

```
✓ Compiled /pricing in 248ms (583 modules)
⚠ Fast Refresh had to perform a full reload.
GET /pricing 200
GET / 200
GET /pricing 200
✓ Compiled /api/plans
```

This suggests a first-load race between route initialization and preview host synchronization during Fast Refresh/full reload.

Likely technical contributors (from current code paths):
1. Preview proxy injected script normalizes route from `?target=` and immediately calls `history.replaceState(...)` and emits `bfloat-preview-route`.
2. Parent preview host (`Preview.tsx`) consumes route postMessage and also updates `urlInput` based on iframe location and/or current preview URL.
3. First compile/full reload can reorder these events, briefly resolving to root before route settles.
4. Existing preview URL synchronization in `Preview.tsx` (`props.previewUrl` effect) may overwrite in-iframe route state on initial transitions.

Goal: make first navigation deterministic so first click to `/pricing` stays on `/pricing` in IDE preview.

### Acceptance Criteria

- [x] First navigation from `/` to `/pricing` in IDE preview does not bounce back to `/`.
- [x] Behavior is stable across initial compile and Fast Refresh full reload events.
- [x] No regression in route handling for other Next.js pages.
- [x] No regression in existing preview proxy protections (localhost target restrictions).

### Constraints

- Keep scope to preview routing/synchronization logic (`preview-proxy` + preview host handling).
- Do not change app business logic/pages for this fix.
- Preserve current error-capture behavior (`bfloat-preview-error` postMessage).

### Resources

- [[2026-03-02_011_web-navigation-problem]]
- Preview proxy route logic: `/Users/v1b3m/Dev/bfloat/bfloat-ide/packages/sidecar/src/routes/preview-proxy.ts`
- Preview host logic: `/Users/v1b3m/Dev/bfloat/bfloat-ide/app/components/preview/Preview.tsx`
- Repro app: `/Users/v1b3m/.bfloat-ide/projects/proj_1772464675512_ggzqn7tpc`

## Handoff
- **What was done:** Fixed preview-host URL synchronization so in-iframe route changes update both URL display and the canonical `currentUrl`, and `previewUrl` prop updates now preserve the current path when host/port is unchanged. This prevents first-route remount/full-reload from reverting iframe `src` back to root.
- **Commit:** e5bb60e
- **Files touched:** `app/components/preview/Preview.tsx`
- **Decisions made:** Kept scope in preview host synchronization only; did not modify app pages or preview-proxy security logic.
- **Known limitations:** Route stability was validated via code-path analysis plus existing proxy tests/lint; no automated UI integration test currently covers the first-load Fast Refresh bounce scenario.
- **How to verify:** 1) Open IDE preview for a Next.js app at `/`. 2) Click navigation to `/pricing` on first attempt. 3) Trigger an initial compile/full reload (or wait for Fast Refresh full reload). 4) Confirm route remains `/pricing` and does not bounce back to `/`.
