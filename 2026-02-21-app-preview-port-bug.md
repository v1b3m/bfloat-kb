# App Preview Port Switch Bug

## Summary
Switching between two mobile projects caused preview inversion:

1. Open App A -> preview shows App A
2. Switch to App B -> preview still shows App A (expected B)
3. Switch back to App A -> preview shows App B (expected A)

The issue looked like stale `http://localhost:19000` routing, but root cause was path/project mismatch during dev server startup.

## Reproduction
- Open project A (mobile/expo)
- Navigate to project B (mobile/expo)
- Navigate back to project A
- Observe preview and direct browser check on `http://localhost:19000`

## Root Cause
In `Workbench` startup flow, `project.id` and `gitProjectPath` could become temporarily out of sync during rapid project switching.

Result:
- Project **B** could launch using project **A** path
- Then project **A** could launch using project **B** path

Since both launches used the same local dev port range (often `19000`), preview appeared swapped.

## Evidence from Logs
During failing runs, logs showed:
- Active `projectId` for one project
- But launch command `cd` path pointing to the other project's directory

Example pattern:
- `projectId: b6e...` with `cd .../0611...`
- Then `projectId: 0611...` with `cd .../b6e...`

## Fix
### 1. Guard startup on project/path match
Added a validation that for git-based projects, startup waits until `gitProjectPath` belongs to the current `project.id`.

Implementation:
- Normalize path separators and trailing slash
- Compare terminal path segment to current `project.id`
- If mismatch: skip launch for that render pass

### 2. Abort launch if mismatch is detected at execution time
Even if control reaches launch logic, an additional guard prevents launching with a mismatched path.

### 3. Reset-order hardening
Project switch reset now executes before mismatch wait checks, so stale preview state is always cleared immediately on project change.

## Files Changed
- `packages/workbench/src/components/workbench/Workbench.tsx`

## Validation
- Re-tested A -> B -> A switching flow
- Preview now follows the active project correctly
- Typecheck passes:
  - `pnpm -C apps/desktop exec tsc -p tsconfig.json --noEmit`

## Commit
- `059c34a6`
- `fix(workbench): keep project-path guard and remove preview diagnostics logs`

## Notes
Temporary deep diagnostics were used during investigation and then removed. Only the guard/hardening fix remains.
