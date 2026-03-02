Text in the IDE terminal looks broken — commands are garbled and duplicated because they were sent before the shell prompt was ready.

## Root Cause
PTY sessions were created at default 80x24 dimensions before the container had real layout, and commands were sent after a fixed 500ms delay rather than waiting for the shell prompt.

## Progress
- [x] Defer PTY creation to ResizeObserver callback so container has real dimensions
- [x] Pass initial cols/rows from xterm to sidecar PTY spawn
- [x] Replace fixed 500ms sleep with event-driven waitForShellReady()
- [x] Verify clean terminal output on project load

## Decisions
- PTY creation moved to ResizeObserver rather than requestAnimationFrame — more reliable for split-panel layouts where the container may not have dimensions immediately.

## Outcome
Terminal output is now clean. Shell receives commands only after emitting its first output (prompt), and PTY spawns at the correct dimensions from the start.

Commits: 2594186, a7ccd57