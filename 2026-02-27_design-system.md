We have instructions in the design skill that tell the app to "preserve the existing design system". Well the agents are failing to understand this fully (with context)

See we intend this to work for already established apps, not newly scafolded apps from any of our templates (expo , next js). Well for these new apps, we want the agents to come up with interesting UIs.

We need a robust way for the agents to differentiate new apps from already established apps.


## Progress
- [x] Added deterministic project-origin marker for template-initialized projects (`.bfloat-ide/project-origin.json`).
- [x] Extended workspace profiling to classify `template-bootstrap` vs imported/unknown workspaces.
- [x] Added runtime agent-session design-mode directive:
  - template-bootstrap -> greenfield design behavior
  - non-template -> adapt/preserve existing design system
- [x] Updated managed AGENTS/CLAUDE instruction block to use project-origin marker for design-mode decisions.
- [x] Added/updated tests for workspace profile classification and scaffold guard regression.

## Decisions
- Persisted origin at project initialization time (template route) instead of inferring solely from workspace files.
- Kept design-mode enforcement server-side in sidecar stream options so behavior is consistent across providers and sessions.
- Treated missing origin marker as established/imported by default (safer for existing products).

- [x] Added fallback starter-template signature detection when origin marker is missing (for already-created template projects).
- [x] Added regression test for marker-missing starter template classification.

## Decisions (Follow-up)
- Marker is preferred source of truth, but starter signature fallback is required for backward compatibility and sidecar-restart gaps.