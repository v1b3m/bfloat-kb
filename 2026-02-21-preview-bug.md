# Workbench Mobile Preview Blank Screen (Iframe Rendered, Content Blank)

## Summary
Mobile preview in Workbench rendered a blank screen even though the dev server started and was reachable at `http://localhost:19000` in a normal browser tab.

The preview container and iframe were present in DOM, but the embedded content was not painting when rendered through the phone frame path.

## Symptoms
- Preview panel showed phone frame with black/blank content.
- `Preview` logs showed `serverStatus: running` and `previewUrl: http://localhost:19000`.
- Expo app loaded correctly in external browser tab.
- QR code and `exp://...` URL were present.

## Key Evidence
- `debug.html` confirms iframe exists:
  - `<iframe id="app-preview" src="http://localhost:19000" ...>`
- `console.logs` confirms boot sequence:
  - `[Workbench] Detected Expo URL: exp://...:19000`
  - `[Workbench] Dev server detected at: http://localhost:19000`
  - `[Preview] Setting preview URL: http://localhost:19000`
  - `Running application "main"` from Expo web bundle
- No frame-policy errors found in logs:
  - no `Refused to display ... in a frame`
  - no `X-Frame-Options`/`frame-ancestors` violations

## Isolation Result
A/B rendering test proved the issue is in the phone frame embedding path:

1. **Plain iframe path** (normal HTML container): renders app correctly.
2. **IPhoneFrame path** (SVG + `foreignObject` + iframe): blank.

Conclusion: app/server are healthy; rendering fails in SVG `foreignObject` embedding path.

## Likely Root Cause
`iframe` inside SVG `foreignObject` is a fragile Chromium/Electron path. This can regress due to compositor/painting changes even when app code is unchanged.

### Why this can happen “suddenly”
- The `foreignObject` pattern has existed since `6808ac6b` (Dec 23, 2025).
- Recent `Preview.tsx` commits mostly added logs and did not fundamentally introduce this path.
- Electron was pinned to a newer build in `4f62efe1` (Feb 12, 2026):
  - `electron: ^37.3.1` -> `electron: 37.10.3`
- This aligns with runtime-level behavior change possibility.

## Not Root Cause
- Expo app runtime startup (it starts).
- Preview URL detection (URL is detected and set).
- Missing iframe in DOM (iframe exists).

## Temporary Workaround
Use plain iframe rendering for mobile preview (bypass `foreignObject`).

## Recommended Permanent Fix
Keep iPhone chrome visuals, but render live app content in an absolutely-positioned HTML layer (with CSS border-radius clipping) rather than SVG `foreignObject`.

## Related Commits
- `6808ac6b` introduce `IPhoneFrame` and `foreignObject` pattern
- `1fc6d415` screenshot URL sync update (non-causal)
- `2acfdb28` debug instrumentation
- `a7b382d2` restore `IPhoneFrame` after temporary plain container
- `4f62efe1` pin Electron to `37.10.3` (runtime-level suspect)

## Validation Checklist
- [ ] Preview shows running app in embedded panel.
- [ ] Same behavior in dev and production desktop build.
- [ ] No reliance on SVG `foreignObject` for live iframe content.
- [ ] Screenshot capture still works with current URL.
