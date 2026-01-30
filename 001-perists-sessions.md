# Bug: Chat Session Lost on Project Reload

**Date:** 2026-01-22
**Status:** Fixed

## Symptoms

After exiting a project page and reloading it, all chat messages are lost. The chat appears empty instead of showing the previous conversation.

## Root Cause

In `app/components/chat/Chat.tsx`, the session loading effect had a race condition:

```typescript
// Old code - would run even when projectPath was null
if (!initialSessionId || !aiAgentApi || hasLoadedSession.current) {
  return
}
```

The effect would:
1. Run when `initialSessionId` was available but `projectPath` was still `null`
2. Try to load session with `projectPath = undefined`
3. Set `hasLoadedSession.current = true` (preventing future retries)
4. When `projectPath` later became available, the effect wouldn't re-run

## Fix

Added `projectPath` to the guard condition:

```typescript
// New code - waits for projectPath before loading
if (!initialSessionId || !aiAgentApi || !projectPath || hasLoadedSession.current) {
  return
}
```

**File:** `app/components/chat/Chat.tsx` (line 123)

## Related Issues

The session is stored in Claude's local CLI storage at `~/.claude/projects/{encoded-path}/{session-id}.jsonl`. The `projectPath` is required to find the correct session file. Without it, the session reader can't locate the conversation history.

## Additional Notes

- `latestAgentSession.sessionId` is saved to the backend via `projectApi.saveAgentSession()`
- When the project is reloaded, this session ID is fetched from the backend
- The session reader uses the session ID + project path to find the Claude session file
- Messages are then converted from Claude's JSONL format to the UI's ChatMessage format
