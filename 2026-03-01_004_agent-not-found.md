**Ref:** main
**Depends:** none

### Context
![[Pasted image 20260301071951.png]]

Every so often, user messages never make it to the agent as it is unavailable. A "Not Found" toast is also shown in the bottom right.

### Acceptance Criteria
- [ ] Agents are always correctly initialized
- [ ] User messages are correctly sent to the agent session

### Constraints
- None

### Resources
- None


## Handoff
- **What was done:** Added stale-session recovery in the local agent send flow so a session-not-found prompt failure recreates the session and retries once automatically.
- **Commit:** f201fc8
- **Files touched:** app/hooks/useLocalAgent.ts
- **Decisions made:** Implemented recovery in useLocalAgent.sendPrompt() instead of changing sidecar API semantics to keep scope local and low-risk.
- **Known limitations:** Recovery only retries once and only for explicit session-not-found errors.
- **How to verify:** Open a project chat, trigger a prompt with an invalid or stale session context, confirm no Not Found toast appears and the message is sent after automatic session recreation.