# Fix: Session recovery when agent process dies mid-stream

## Description
When an agent session dies mid-stream (API stream interrupted, CLI process crash), the sidecar's in-memory session is cleaned up but the frontend still holds a stale `sessionIdRef`. The next user message hits the dead session ID → gets a 404 → surfaces as a raw "Not Found" toast. The user has no way to recover without refreshing.

Also ported poisoned-history detection from the workbench so corrupted screenshot data in conversation history is caught and recovered from automatically.

## Progress
- [x] Add `isSessionLostError()` helper in `useLocalAgent.ts`
- [x] Add session-lost detection + auto-retry in `sendPrompt()`
- [x] Add poisoned-history detection in `onMessage` callback (Chat.tsx)
- [x] Add poisoned-history detection in `onError` callback (Chat.tsx)
- [x] Verify `agent-session.ts` error paths (already correct — sessions marked errored, not deleted)
- [x] Type-check passes

## Decisions
- **No changes to `agent-session.ts`**: After review, the sidecar backend already handles error paths correctly — `runStream()` catch block marks sessions as `status: 'error'` and broadcasts error + stream_end frames. Sessions stay in the `sessions` Map (only `closeSession()` deletes them). The `bufferFrame()` helper also updates background session status.
- **Single retry**: Session recovery only retries once. If the second attempt also fails, it surfaces as a regular error. This avoids infinite retry loops.
- **String pattern matching**: `isSessionLostError()` checks for "not found" + "session" in the error string, matching the sidecar's `Session '<id>' not found.` format.

## Outcome
Commit: 68d901f — `fix(agent): recover session when sidecar process dies mid-stream`

Files changed:
- `app/hooks/useLocalAgent.ts` — `isSessionLostError()` helper + auto-retry in `sendPrompt()`
- `app/components/chat/Chat.tsx` — poisoned-history detection in `onError` and `onMessage`
- `packages/sidecar/src/services/agent-session.ts` — verified correct (no changes needed)