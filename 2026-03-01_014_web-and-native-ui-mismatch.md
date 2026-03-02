**Ref:** chore/day-3
**Depends:** none

### Context

I am noticing mismatches in the rendered web preview and when I run the app in expo go on an actual android phone. I've attached screenshots of both here.

**Web preview**

![[Pasted image 20260301113641.png]]

- Notice how the start button is collapsed into an almost square shape
- Notice the small text size for "05:00"

**Expo Go**

![[Pasted image 20260301113502.png|300]]

- Notice the full width "Start button"

**Questions:**

- Is this expected?

### Acceptance Criteria

- [ ] Closer UI match across the platforms
- [ ] 

### Constraints

- None

### Resources

- None




## Handoff
- **What was done:** Removed aggressive mobile preview root transform-fit logic that was shrinking web-rendered Expo UI (text/buttons), and relaxed iframe overflow behavior for closer parity with Expo Go layout.
- **Commit:** e5deebe
- **Files touched:** app/components/preview/Preview.tsx
- **Decisions made:** Kept only lightweight viewport/box-model guardrails and removed dynamic scale-to-fit height/width rewriting to preserve native sizing semantics.
- **Known limitations:** Some apps with oversized layouts may clip/require scrolling instead of being force-fit into the preview frame.
- **How to verify:** Open a mobile Expo project in Preview, compare timer/button sizing in IDE preview vs Expo Go; confirm the button width/text scale are materially closer than before.