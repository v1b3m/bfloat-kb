**Ref:** chore/day-3
**Depends:** 2026-03-01_004_broken-images

### Context

Now that images are fixed, can we have larger image preview when an image is clicked? This way users don't have to squint to see what the image contains

### Acceptance Criteria

- [ ] Clicking an image opens an expanded view in a modal-like view with a backdrop

### Constraints

- None

### Resources

- [[2026-03-01_004_broken-images]]
- 

## Handoff
- **What was done:** Added click-to-expand behavior for chat image attachments in user messages. Clicking a thumbnail now opens a modal-like expanded preview with a dark backdrop; users can close via backdrop click, close button, or `Escape`.
- **Commit:** `3845bfc`
- **Files touched:** `_SCRATCH.md`, `app/components/chat/UserMessage.tsx`, `app/components/chat/styles.css`
- **Decisions made:** Implemented a lightweight in-component overlay specifically for user-message attachments to keep scope tight and reuse existing image source handling from the previous fix.
- **Known limitations:** Expanded preview is currently implemented for user message attachments only (not assistant-side images).
- **How to verify:**
  - Send a user message with an image attachment.
  - Click the thumbnail and confirm a large modal-like preview opens with backdrop.
  - Confirm close works with backdrop click, close button, and `Escape`.
  - Lint check run: `npx eslint app/components/chat/UserMessage.tsx`.
