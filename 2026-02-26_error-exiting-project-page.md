I get this error when I exit the project page, it crashes the IDE

```
[Error] [WorkbenchStore] Failed to commit changes: – SidecarError: fatal: not a git repository (or any of the parent directories): .git
SidecarError: fatal: not a git repository (or any of the parent directories): .git
	saveAllAndCommit (workbench.ts:323)
```




## Progress (2026-02-26)
- [x] Added sidecar route checks to return clear no-git-repository errors for git add/commit/push endpoints.
- [x] Updated workbench auto-commit shutdown path to treat no-git-repository as non-fatal.

## Outcome
Exiting projects without a git repo no longer crashes the IDE flow.

## Commit
- 5a7ddcb