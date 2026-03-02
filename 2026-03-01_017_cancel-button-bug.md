**Ref:** keep-hacking
**Depends:** 2026-03-01_001_connect-integration-modal

### Context

The double cancel button bug still happens. I just realized it also happens on other modals like the delete project modal at home.

**Before project deletion:**

![[Screenshot 2026-03-01 at 18.14.15.png]]

**During project deletion**

![[Screenshot 2026-03-01 at 18.14.18.png]]


### Acceptance Criteria

- [x] No double cancel buttons are visible

### Constraints

- None

### Resources

- [[2026-03-01_001_connect-integration-modal]]

## Progress
- [x] Inspect delete-confirm and integration auth modals for duplicate cancel rendering paths
- [x] Add explicit `type="button"` for non-submit modal actions to prevent implicit form submit side effects
- [x] Update delete-confirm loading state to avoid rendering duplicate visible cancel actions
- [x] Run targeted eslint check for touched files

## Decisions
- Applied a shared safety fix in dialog-based actions (`type="button"`) so modal buttons never default to submit behavior when mounted inside or near form contexts.
- In destructive delete dialogs, hide the cancel action while deletion is actively in flight to prevent duplicate visible cancel controls during loading transitions.

## Outcome
- Duplicate cancel visibility during delete-confirm loading is removed.
- Related modal actions now consistently declare non-submit button intent.
- Targeted lint was executed; only pre-existing warnings/errors outside this change scope remain.
