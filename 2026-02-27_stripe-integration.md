# Stripe Integration

## Description
Implement Stripe integration flow parity with existing integrations (especially RevenueCat/Convex patterns): setup prompt in chat, env-var modal flow via settings, and no runtime OAuth step.

## Progress
- [x] Review current Stripe integration UX and define implementation plan.
- [x] Add Stripe setup-in-progress UI state to chat banners/messages.
- [x] Wire Stripe setup state transitions in chat integration auto-submit path.
- [x] Sync `add-stripe` skill and template files from workbench source of truth.
- [x] Bump skills manifest version for rollout.
- [x] Create initial Stripe implementation commit (`aa463dd`).
- [ ] Validate behavior end-to-end in app.

## Decisions
- Keep runtime behavior token/key-based in IDE, with OAuth handled by deployment/prod flows only.
- Use same setup-in-progress UX pattern as RevenueCat to avoid inconsistent integration behavior in chat.
- Keep task in `In Progress` until explicit completion confirmation.

## Outcome
In progress.

Commits:
- `aa463dd` — initial Stripe integration parity and skill sync.
