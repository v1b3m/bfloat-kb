**Ref:** chore/day-5-001
**Depends:** none

### Context

We have blocked package installs like `expo-av`, however, I've noticed these cut short the agent stream. The chat just stops streaming altogether, this is aggressive. We need to block these while allowing the agent to continue and perhaps install something else.

### Acceptance Criteria

- [ ] Blocked package installs should not be a death sentence for the agent

### Constraints

- None

### Resources

- None



## Handoff
- **What was done:** Updated deprecated package install blocking to be non-fatal at the session level. The guard now emits a synthetic failed `tool_result` plus a clean `done`/`stream_end` instead of emitting session `error` and killing the turn abruptly.
- **Commit:** `13a3357`
- **Files touched:** `_SCRATCH.md`, `packages/sidecar/src/services/agent-session.ts`, `packages/sidecar/src/services/agent-session.test.ts`
- **Decisions made:** Kept scaffold/dev-server guards unchanged; only changed deprecated package block behavior to preserve install blocking while avoiding fatal session termination.
- **Known limitations:** The blocked command still ends the current turn early; it no longer marks the session as error, so the next user turn can continue normally.
- **How to verify:** Run `bun test packages/sidecar/src/services/agent-session.test.ts` and confirm deprecated install test passes with `completed` status and a failed `tool_result`.
