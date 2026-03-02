it looks like every subsequent message is starting a new session. This is not why we added sessions. We added them in cases where chats error to states that are not recoverable. This way a user can spin up a new session to continue working on the chat.

![[Pasted image 20260226095333.png]]

Check `bfloat-workbench` for how we handle this?

## Questions

1. Are sessions getting lost between messages? Are they getting cleared/wiped? If this is the case this should not happen as context from previous messages is very important for an app like this.
2. 

## Progress
- [x] Identified root cause in sidecar session loop: provider `realSessionId` was captured but not persisted back into `session.options.resumeSessionId` for subsequent turns.
- [x] Implemented core fix in `packages/sidecar/src/services/agent-session.ts` to persist resume ID after init.
- [x] Added UI diagnostic guard in `app/hooks/useLocalAgent.ts` to warn when provider session ID rotates unexpectedly during an active sidecar session.
- [x] Added regression test `packages/sidecar/src/services/agent-session.test.ts` validating second turn reuses first turn provider session ID.
- [x] Added sidecar test script (`packages/sidecar/package.json`: `bun test`) and verified test pass.

## Decisions
- Session continuity is enforced in sidecar core (single source of truth), not through UI-only workarounds.
- UI changes are logging/diagnostic only; no behavior override in chat/session switching flow.
- Regression protection is automated at sidecar service level with a provider stub test.


## Outcome (2026-02-26)
Session continuity now persists provider resume IDs across turns and prevents accidental scaffold attempts in existing app workspaces.

Commit: f667cab