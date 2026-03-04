**Ref:** develop
**Depends:** none

### Context

While on a web app, I took a screenshot using the camera icon of a pricing page. Interesting that the screenshot attached to the chat was for the homepage. Something's broken here.

**Pricing page:**

![[Screenshot 2026-03-02 at 18.59.44.png]]

**Attached screenshto**

![[Screenshot 2026-03-02 at 18.59.44 1.png]]


### Acceptance Criteria

- [x] Screnshots are taken from the rendered page

### Constraints

- None

### Resources

- None

## Handoff
- **What was done:** Fixed web preview navigation state so screenshots use the currently rendered page URL (not the initial preview homepage URL). Also updates registered preview URL on navigation so sidecar capture follows the active page.
- **Commit:** a0dc71b
- **Files touched:** `app/components/preview/Preview.tsx`; Obsidian notes `2026-03-02_010_wrong-screenshots` and `BOARDS/IDE Tasks`.
- **Decisions made:** Kept the fix scoped to Electron webview navigation events (`did-navigate` / `did-navigate-in-page`) where URL truth is available and already drives browser controls.
- **Known limitations:** Could not run lint in this worktree because eslint dependencies are not installed (`pnpm exec eslint ...` fails with `Command "eslint" not found`).
- **How to verify:** 1) Open a web app preview and navigate from home to `/pricing`. 2) Click the camera button in preview. 3) Confirm the attached screenshot shows the pricing page (rendered page), not homepage.
