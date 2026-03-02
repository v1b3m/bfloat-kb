
**Ref:** chore/day-3
**Depends:** None

### Context

When I click the connect button in the "Connect integration modal" in the ide, I see a second "Cancel" button overlapped with the initial Cancel button

I don't have a screenshot as this happens so fast.

### Acceptance Criteria
- [x] I don't see a second Cancel button on confirming secret addition

### Constraints
- None

### Resourceså
- None

## Progress
- [x] Inspect integration credentials modal and parent settings form state flow
- [x] Fix modal actions to prevent implicit parent form submit from dialog buttons
- [x] Validate lint status for edited files

## Decisions
- Set explicit `type="button"` on integration modal and secret modal action buttons because both dialogs render inside a parent `<form>`, and default button type (`submit`) can trigger unintended form-submit side effects.
- Kept scope limited to modal action types; no unrelated refactor.

## Outcome
- Resolved duplicate `Cancel` flash by preventing implicit submit behavior from dialog buttons in the connect integration flow.
- Commit: `bd31046`
