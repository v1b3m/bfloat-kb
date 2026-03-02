Generated apps are still not respecting the bounds of the viewport. You can see from this screenshot that the app is pushing outside the viewport bounds. We need to resolve this.


![[Pasted image 20260227213848.png]]

`MINUTES` and `SECONDS` are both cut off




## Progress
- [x] Inspected mobile preview rendering path and existing viewport guards
- [x] Added mobile iframe runtime scale-to-fit guard with resize/mutation reflow handling
- [x] Tightened mobile generation prompt guidance for safe-area and small-screen fit
- [x] Ran desktop build validation (`pnpm --filter @bfloat/desktop build`)
- [ ] Manual visual verification against the timer repro in preview

## Decisions
- Enforced bounds in mobile preview runtime (iframe guard) in addition to prompt guidance, since prompt-only fixes are non-deterministic
- Scoped runtime guard to mobile preview only to avoid changing web preview behavior
- Chose scale-to-fit for overflow cases instead of scroll/clip to keep full UI visible in-frame

## Outcome
Implemented in working tree:
- `app/components/preview/Preview.tsx`
- `lib/launch/system-prompt.ts`

Build check passed for desktop package.
Commit: pending

Commit: 55afbf6