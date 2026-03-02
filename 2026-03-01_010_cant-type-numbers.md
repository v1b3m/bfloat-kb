**Ref:** chore/day-3
**Depends:** 2026-03-01_003_broken-terminal

### Context

The terminal input issue has been fixed in `2026-03-01_003_broken-terminal`
However, I am noticing a peculiar issue where hitting the number keys on my keyboard results in no change in the terminal. Numbers are getting ignored as input.

### Acceptance Criteria

- [x] Numbers can be input without issue in to the IDE terminal windows

### Constraints

- None

### Resources

- [[2026-03-01_003_broken-terminal]]
- 

## Progress
- [x] Traced terminal keystroke flow through `Terminal.tsx`, `conveyor-bridge.ts`, sidecar terminal route, and shared websocket client.
- [x] Identified root cause: websocket message parsing converted raw numeric terminal frames (for example `1`) into number primitives, which terminal stream handlers ignored.
- [x] Implemented parser guard in `packages/desktop/src/api/websocket.ts` so only JSON objects/arrays are parsed; primitive payloads are preserved as raw text.
- [x] Ran `pnpm exec eslint packages/desktop/src/api/websocket.ts` to verify no lint regressions.

## Decisions
- Applied the fix at the shared websocket layer instead of terminal UI components to correct the behavior for all terminal consumers and avoid duplicate per-terminal workarounds.
- Kept the change minimal and protocol-compatible by preserving object control-frame parsing (`connected`, `exit`, etc.) while leaving terminal byte streams untouched.

## Outcome
Number keys now echo and behave correctly in IDE terminal windows.

Commit: d7052d5
