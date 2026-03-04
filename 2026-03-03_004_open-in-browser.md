**Ref:** develop
**Depends:** none

### Context

The open in browser button on the web preview is not working, clicking it does nothing.
### Acceptance Criteria

- [x] Clicking "Open in Browser" opens the page in external browser

### Constraints

- None

### Resources

- None

## Handoff
- **What was done:** Fixed the web preview "Open in browser" action to use the native bridge (`conveyor.window.webOpenUrl`) with fallback to `window.open`, so clicking the button opens the external browser reliably in Tauri/Electron contexts.
- **Files touched:** `app/components/preview/Preview.tsx`
- **Decisions made:** Matched the existing external-link behavior used elsewhere in the app and in preview postMessage handling to avoid runtime-specific no-op behavior from plain `window.open`.
- **Known limitations:** Could not run lint/typecheck in this worktree because project dependencies are not installed (`eslint` binary unavailable).
- **How to verify:** Start app preview, click **Open in browser** in the web preview toolbar, and confirm the current preview URL opens in the system browser.
