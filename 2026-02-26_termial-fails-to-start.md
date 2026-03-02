The terminal is not starting automatically when opening a project.

## Root Cause
Race condition: the workbench tried to send commands before the PTY was created and the shell was ready. Also, the setup guard checked projectPath state (async) rather than the synchronous setupInitiatedRef, allowing double-triggers.

## Progress
- [x] Defer PTY creation to ResizeObserver (real dimensions required)
- [x] Add waitForShellReady() — resolves on first terminal output
- [x] Fix setup guard to use synchronous setupInitiatedRef
- [x] Verify terminal auto-starts and dev server runs

## Outcome
Terminal reliably auto-starts and runs setup commands on project load.

Commits: 2594186, a7ccd57