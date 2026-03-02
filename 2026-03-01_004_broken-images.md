**Ref:** chore/day-3
**Depends:** none

### Context
On reopening any project, the images in the chat are all broken.

See the example below:

![[Screenshot 2026-03-01 at 07.15.45.png]]\
### Acceptance Criteria
- [ ] Images correctly render when we open projects again
- [ ] 

### Constraints
- None

### Resources
- Test project: `/Users/v1b3m/.bfloat-ide/projects/proj_1772254509221_k1242rvgt`

## Handoff
- **What was done:** Fixed chat attachment persistence so image files are saved as binary from base64 payloads (instead of saving data URLs as plain text), and added legacy reload fallback so previously broken persisted images still render when projects are reopened.
- **Commit:** 80efbe0
- **Files touched:** `_SCRATCH.md`, `packages/desktop/src/conveyor-bridge.ts`, `app/components/chat/UserMessage.tsx`
- **Decisions made:** Implemented both forward fix (correct write encoding) and backward compatibility (legacy malformed attachment recovery) to satisfy reopen behavior for both new and existing projects.
- **Known limitations:** Legacy recovery currently targets image data URL payloads only.
- **How to verify:**
  - Send a chat message with image attachment, close/reopen the same project, confirm image renders in the user message.
  - Reopen a project that already had broken image attachments from before this fix and confirm those render.
  - Lint check run: `npx eslint app/components/chat/UserMessage.tsx packages/desktop/src/conveyor-bridge.ts` (warnings only, no errors).
