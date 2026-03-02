# Fix global toast bottom clipping

## Description
Follow-up task to fix clipping for the global toast message (e.g. "Add CONVEX_URL ...") where the bottom of the notification gets cut off below the app frame.

Parent task: [[2026-02-25-fix-chat-integration-notification-clipping]]

## Progress
- [x] Locate global toaster configuration and compare with workbench notification pattern
- [x] Increase toaster bottom inset to keep notifications within safe visual bounds
- [x] Verify updated `app/app.tsx` passes targeted lint

## Decisions
- Kept `react-hot-toast` and applied a placement fix at the global `Toaster` config level.
- Added explicit `containerStyle.bottom` using safe-area aware offset and increased fixed inset.
- Added small `gutter` for consistent toast stacking spacing.

## Outcome
Implemented safe-area-aware bottom/right insets for the global toaster so integration guidance toasts no longer render partially outside the app frame.
Commit: pending

### Update (implemented)

## Progress
- [x] Locate global toaster configuration and compare with workbench notification pattern
- [x] Increase toaster bottom inset to keep notifications within safe visual bounds
- [x] Portal global toaster to `document.body` to avoid clipping by app/window content containers
- [x] Verify updated `app/app.tsx` passes targeted lint

## Decisions
- Adopted workbench-style robustness by portaling toast rendering to `document.body` via `createPortal`.
- Kept existing toast API usage, visuals, and durations unchanged.
- Retained bottom/right container insets for stable placement after portaling.

## Outcome
Global integration toast is now rendered through a body portal and should no longer be clipped by project/app container boundaries.
Commit: pending

### Follow-up update (persistent guidance UX)

## Progress
- [x] Replace transient integration connect guidance toast with persistent inline setup card in chat
- [x] Keep navigation behavior to Settings tab on Connect
- [x] Prevent duplicate consecutive setup cards when Connect is clicked repeatedly

## Decisions
- For chat integration Connect action, removed time-limited guidance toast and rely on persistent assistant setup cards (`*-setup-prompt` parts) so instructions remain visible in chat history.
- Added duplicate suppression for consecutive identical setup prompt cards.

## Outcome
Users now retain actionable integration guidance instead of losing it after toast timeout. Connect still routes to Settings and now also leaves persistent instructions in chat.
Commit: pending

### Follow-up update (connect action visibility)

## Progress
- [x] Added `workbenchStore.pendingIntegrationConnect` signal for integration connect actions
- [x] Wired chat/workbench connect handlers to set pending request and suggested key
- [x] Wired Project Settings to auto-open Secret modal from pending request
- [x] Added `defaultKey` support to `SecretModal` for prefilled add flow

## Decisions
- Preserve Settings navigation on Connect, but make it immediately actionable by auto-opening Development Variables modal.
- If suggested key already exists, open in edit mode for that key; otherwise open add mode prefilled with that key.

## Outcome
Clicking Connect now performs a visible action beyond tab switch: it opens the Development Variables modal with the relevant key prefilled (or existing key selected for edit).
Commit: pending

Commits:
- `db5a731` fix(ui): portal global toaster to avoid clipping
- `8b3b3f2` feat(integrations): secrets-first connect flow and settings handoff

### Follow-up update (Convex post-save listener)

## Progress
- [x] Added reusable Convex key detector (`isConvexSecretKey`) in integrations secrets utility
- [x] Added post-save listener in `ProjectSettings.handleSaveSecret` for Convex URL keys
- [x] Added guard to trigger setup only when Convex URL value is newly added/changed and non-empty

## Decisions
- Trigger Convex bootstrap automatically via `workbenchStore.triggerChatPrompt(...)` after successful secret save + reload.
- Do not trigger on no-op edits (same value) to avoid duplicate setup runs.

## Outcome
Saving a Convex URL now starts the Convex setup workflow in chat automatically, which should cover schema/functions bootstrap initiation.
Commit: pending

### Follow-up update (Convex connected-state sync)

## Progress
- [x] Added `secretsVersion` invalidation store in `workbenchStore`
- [x] Bump invalidation on secret save/delete in Project Settings
- [x] Chat now refetches integration secret presence on secrets invalidation
- [x] Prevented explicit setup commands from being intercepted into connect banners (`/convex-setup`, `/add-stripe`, `/add-revenuecat`)

## Decisions
- Secret changes are propagated via explicit version bump instead of relying on active-tab changes.
- Explicit setup commands always bypass "not connected" intercept gates to prevent self-blocking loops.

## Outcome
After saving Convex URL, chat refreshes integration state and should stop showing Convex as disconnected; the auto-triggered `/convex-setup` command now runs instead of being converted back into a Connect banner.
Commit: pending

Additional commit:
- `88c5a64` fix(integrations): refresh convex state after secret save

### Follow-up update (Convex staged status)

## Progress
- [x] Added Convex integration stage model (`disconnected`, `connected`, `setting_up`, `ready`)
- [x] Added Convex bootstrap artifact detection helper
- [x] Wired chat status to stage model (secret + bootstrap artifacts + setup in-flight)
- [x] Updated Convex banner copy/actions by stage
- [x] Guarded Convex post-save auto-trigger to skip when bootstrap artifacts already exist

## Decisions
- URL presence now means "connected", not fully "ready".
- Ready state requires bootstrap artifacts (`convex/schema.ts`, non-generated Convex function file, `convex/_generated/api.*`).
- While setup is running, banner shows "Setting up Convex..." and disables repeated action.

## Outcome
Convex UX now distinguishes connected vs fully set up and should avoid misleading reconnect prompts once URL is saved.
Commit: pending
