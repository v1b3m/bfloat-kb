**Ref:** chore/day-3
**Depends:** none

### Context

After adding expanded images, I just realized the screenshot button next to the mobile preview has been taking wrong screenshots all along for mobile. It takes them in a viewport way larger than mobile (looks like a desktop preview) which leads to incorrect analysis by the agent.

I believe the mcp tool too uses this same buggy code.
#### Sample Mobile screenshot

![[image.png]]

### Acceptance Criteria

- [ ] Mobile screenshots are taken in a mobile viewport
- [ ] Web screenshots are taken in a web viewport

### Constraints

- None

### Resources

- [[2026-03-01_007_expanded-images]]
- 


## Handoff
- **What was done:** Fixed screenshot viewport selection so mobile captures use mobile emulation/dimensions and web captures remain desktop-like. Extended screenshot request options (`width`, `height`, `mobile`, `deviceScaleFactor`) through bridge/route/service. Updated Preview screenshot button to pass mobile frame dimensions for mobile apps. Updated screenshot MCP tool to infer app type from runtime metadata and apply mobile defaults for mobile projects.
- **Commit:** `2b86f398e3902bc5aee2dbbf4d177a60057b6cb7`
- **Files touched:** `app/components/preview/Preview.tsx`, `packages/desktop/src/conveyor-bridge.ts`, `packages/sidecar/src/routes/screenshot.ts`, `packages/sidecar/src/services/screenshot.ts`, `packages/sidecar/src/services/screenshot-mcp.ts`, `_SCRATCH.md`
- **Decisions made:** Kept behavior backward-compatible for existing callers that omit viewport options; only mobile paths opt into mobile emulation. MCP uses runtime `appType` to select defaults.
- **Known limitations:** MCP app-type inference depends on recent runtime metadata updates; if metadata is stale/missing it falls back to web defaults.
- **How to verify:**
  1. In a mobile project preview, click "Screenshot to chat" and confirm attached image matches phone-sized viewport.
  2. In a web project preview, click "Screenshot to chat" and confirm image is desktop viewport.
  3. Trigger MCP `capture_preview_screenshot` in a mobile project and verify output is mobile viewport.
  4. Ran: `npx eslint app/components/preview/Preview.tsx packages/desktop/src/conveyor-bridge.ts packages/sidecar/src/routes/screenshot.ts packages/sidecar/src/services/screenshot.ts packages/sidecar/src/services/screenshot-mcp.ts` (warnings only, no errors).
