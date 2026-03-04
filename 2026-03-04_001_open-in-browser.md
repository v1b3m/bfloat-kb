---
title: 2026-03-04_001_open-in-browser
ref: chore/day-5-002
depends_on: none
ready: true
tags:
picked_at: 2026-03-04T06:49:04Z
picked_mode: execute
---
### Context

The open in browser button on the web preview is not working, clicking it does nothing.

### Acceptance Criteria
- [ ] Clicking **Open in browser** from web preview opens the active preview URL in the system browser on desktop runtime.
- [ ] Works in both runtime paths:
  - [ ] with bridge available (`conveyor.window.webOpenUrl`)
  - [ ] fallback when bridge unavailable (`window.open`)
- [ ] No regression to existing iframe message path (`bfloat-preview-open-external`).
- [ ] Invalid/empty URL is ignored safely (no crash).
- [ ] Manual verification documented for macOS and Windows behavior.

### Constraints
- Discovery mode only in this task (no production code edits).
- Keep implementation localized to preview external-open behavior; avoid unrelated preview refactors.
- Must preserve current behavior for non-web preview modes.

### Resources
- None

## Discovery (2026-03-04)

### Clarified Context
- The toolbar action in web preview uses `window.open(currentUrl, '_blank')` directly (`app/components/preview/Preview.tsx:437-441`). In desktop runtime, this can no-op depending on WebView popup policy.
- The same component already has a working external-open pattern for iframe messages that uses `conveyor.window.webOpenUrl(...)` with fallback to `window.open(..., 'noopener,noreferrer')` (`app/components/preview/Preview.tsx:291-306`).
- This indicates behavior drift inside one file: message-driven open path is hardened, toolbar-button open path is not.
- Related historical note `[[2026-03-03_004_open-in-browser]]` claims this was fixed, but current branch state still shows unfixed toolbar handler.

### Risks
- Popup-blocking/no-op behavior if relying only on `window.open` in desktop WebView.
- Divergent behavior between toolbar action and iframe message action if separate logic remains.
- Security risk if arbitrary protocols are allowed; URL validation should remain `http/https` for external open.

### Options
1. **Unify on shared helper (Recommended)**
- Extract a small `openExternalUrl(url)` helper in `Preview.tsx`.
- Use bridge first, fallback to `window.open(..., 'noopener,noreferrer')`.
- Reuse helper for both toolbar button and iframe `bfloat-preview-open-external` path.

2. Bridge-only path
- Force `conveyor.window.webOpenUrl` and do nothing without bridge.
- Lower ambiguity, but fails in web/non-bridge contexts.

3. Main-process IPC-only action
- Add dedicated event route for open-browser.
- Most explicit architecture, but larger scope and not needed for this bug.

### Recommendation
- Proceed with **Option 1** to remove logic drift and keep runtime-safe fallback.

### Explicit User Decisions Needed
- [x] Confirm fallback policy: keep `window.open` fallback when bridge fails (recommended: yes).
- [x] Confirm URL policy: allow only `http/https` for toolbar action (recommended: yes).
- [x] Confirm whether to add lightweight telemetry/log on external-open failure (recommended: warn log only).

### Verification Plan (for execution phase)
- Start web preview with valid `currentUrl`.
- Click **Open in browser** and verify system browser opens same URL.
- Simulate bridge unavailable path and verify fallback still opens URL.
- Verify iframe-driven external open still works.
- Run targeted test/lint if available for `Preview.tsx` changes.


## Handoff
- **What was done:** Unified preview external-open behavior behind one helper in  and routed both toolbar **Open in browser** and iframe message  through it. Helper now validates URL ( only), prefers desktop bridge (), and falls back to .
- **Commit:** 
- **Files touched:** 
- **Decisions made:** Kept behavior localized to web preview external-open flow only; preserved existing iframe message path semantics while removing duplicate logic drift.
- **Known limitations:** Could not run lint/typecheck in this worktree because local toolchain binaries are unavailable ().
- **How to verify:**
  - **macOS desktop runtime:** Open a web project preview, click **Open in browser**, and confirm default browser opens active preview URL; repeat with bridge unavailable and confirm fallback open still works.
  - **Windows desktop runtime:** Repeat same test and confirm default browser opens active preview URL in both bridge and fallback paths.
  - **Regression check:** Trigger iframe postMessage  with valid  URL and confirm external open still works; send invalid/empty URL and confirm no crash/no open.

## Handoff (Corrected)
- **What was done:** Unified preview external-open behavior behind one helper in Preview.tsx and routed both toolbar Open in browser and iframe message bfloat-preview-open-external through it. Helper now validates URL (http/https only), prefers desktop bridge (conveyor.window.webOpenUrl), and falls back to window.open(..., noopener,noreferrer).
- **Commit:** 5366b74
- **Files touched:** app/components/preview/Preview.tsx
- **Decisions made:** Kept behavior localized to web preview external-open flow only; preserved existing iframe message path semantics while removing duplicate logic drift.
- **Known limitations:** Could not run lint/typecheck in this worktree because local toolchain binaries are unavailable (eslint command not found).
- **How to verify:**
  - **macOS desktop runtime:** Open a web project preview, click Open in browser, and confirm default browser opens active preview URL; repeat with bridge unavailable and confirm fallback open still works.
  - **Windows desktop runtime:** Repeat same test and confirm default browser opens active preview URL in both bridge and fallback paths.
  - **Regression check:** Trigger iframe postMessage bfloat-preview-open-external with valid https URL and confirm external open still works; send invalid/empty URL and confirm no crash/no open.