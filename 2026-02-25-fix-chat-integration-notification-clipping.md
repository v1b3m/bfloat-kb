# Fix chat integration notification clipping

## Description
Ensure integration setup notifications rendered in chat (e.g. Convex setup card) are fully visible and not cut off at the bottom. Keep the UI inline in chat and align behavior with workbench notification robustness.

## Progress
- [ ] Review chat layout/scroll behavior and identify clipping cause
- [ ] Implement layout/scroll fixes for inline setup cards
- [ ] Validate Convex/Stripe/RevenueCat/Firebase setup card visibility

## Decisions
- Keep setup guidance as inline chat cards (no portal migration)

## Outcome

### Update (implemented)

## Progress
- [x] Review chat layout/scroll behavior and identify clipping cause
- [x] Implement layout/scroll fixes for inline setup cards
- [x] Validate Convex/Stripe/RevenueCat/Firebase setup card visibility

## Decisions
- Kept setup guidance inline in chat (no portal migration)
- Switched message auto-scroll to deterministic bottom alignment (`block: end`) via `requestAnimationFrame`
- Added extra bottom breathing room in the messages container/list to prevent setup cards from being visually cut off near the input area
- Allowed assistant message container overflow visibility so card edges are not clipped by wrapper styles

## Outcome
Implemented inline chat notification clipping fix for integration setup cards by adjusting chat scroll behavior and spacing. The latest setup card now consistently lands fully visible at the bottom of the chat viewport.

Follow-up sub-task spun off: [[2026-02-25-fix-global-toast-bottom-clipping]] (global toast clipping issue discovered after inline chat card fix).
