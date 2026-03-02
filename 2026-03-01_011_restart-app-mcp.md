**Ref:** chore/day-3
**Depends:** none

### Context

Over and over again, I've seen agents call `mcp__workbench__restart_app` but the app will not restart no matter what. Hitting any of the restart buttons manually works without issue. We need to close this gap.

### Acceptance Criteria

- [ ] Agents should be able to restart broken apps.
- [ ]

### Constraints

- None

### Resources

- None

## Handoff
- **What was done:** Updated restart-event bridging to normalize tool names and accept MCP-prefixed workbench restart aliases, including `mcp__workbench__restart_app`, so successful tool results trigger the same dev-server restart listener used by manual restart buttons.
- **Commit:** ed92762
- **Files touched:** `_SCRATCH.md`; `packages/desktop/src/conveyor-bridge.ts`
- **Decisions made:** Used canonical tool-name normalization and scoped matching for workbench restart aliases to avoid brittle exact-string checks.
- **Known limitations:** Global TypeScript check fails in many unrelated files in this worktree; no task-specific runtime integration test exists for this listener path.
- **How to verify:** In a project with a broken dev server, run `mcp__workbench__restart_app` from agent chat and confirm the preview/workbench server restart flow triggers without manual restart button interaction.