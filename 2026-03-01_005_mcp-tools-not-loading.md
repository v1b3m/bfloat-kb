**Ref:** chore/day-3
**Depends:** none
### Context
I keep running into situations where certain mcp tools are not available. For example, for the project below which had a valid revenuecat test key, the revenuecat mcp tools were never loaded.

![[Pasted image 20260301072746.png]]

You can see from the above screenshot that the rc key is available in `.env.local`

### Acceptance Criteria
- [ ] MCP tools are correctly injected into the agent session

### Constraints
- None
- 

### Resources
- Chat excerpt: `/Users/v1b3m/Dev/bfloat/bfloat-ide/pg/chat.logs`
- Project path: `/Users/v1b3m/.bfloat-ide/projects/proj_1772254509221_k1242rvgt`
- 

## Handoff
- **What was done:** Added sidecar session-time MCP auto-configuration so RevenueCat MCP is injected when a RevenueCat key is present in project/session env (reads `.env.local` fallback `.env`, merges with per-session env overrides).
- **Commit:** a80a57b
- **Files touched:** `packages/sidecar/src/services/agent-session.ts`
- **Decisions made:** Implemented server-side in sidecar `createSession()` so all session callers benefit; explicit caller-provided `mcpServers` override auto-generated entries.
- **Known limitations:** Only RevenueCat auto-injection is implemented in this task.
- **How to verify:** In a project with `EXPO_PUBLIC_REVENUECAT_API_KEY` or `REVENUECAT_API_KEY` in `.env.local`, start a new agent session and confirm RevenueCat MCP tools appear and can be called.
- **Validation run:** `bun test` in `packages/sidecar` was attempted; one suite failed in this environment due missing module `@openai/codex-sdk` (pre-existing test runtime issue).