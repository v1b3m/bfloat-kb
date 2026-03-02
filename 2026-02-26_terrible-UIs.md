The generated applications have vile UIs, I mean these are so bad. We have a UI skill in the workbench (bfloat-workbench). I believe we should trace how it is used and import it into the IDE too.



## Resolution

Resolved in [[2026-02-26-port-frontend-design-skill]] — ported the workbench's comprehensive `frontend-design` skill to the IDE and added system prompt routing so the agent auto-invokes `/frontend-design` for UI work.

Commit: 1fdbc86
