
**Ref:** develop
**Depends:** none

### Context

After adding a publishable key for stripe, I get the error below:

```
Failed to fetch RSC payload for http://127.0.0.1:49851/. Falling back to browser navigation. {}

```

I expect the stripe follow up message to be sent automatically in the chat and the stripe integration hardened/completed.

Check how we handle revenuecat as it works E2E
### Acceptance Criteria

- [ ] Stripe is connected fully
- [ ] 

### Constraints

- None

### Resources

- None
- 

## Handoff
- **What was done:** Added Stripe post-save auto setup dispatch so saving Stripe publishable key now automatically triggers the add-stripe chat prompt, with required-secret waiting and improved chat setup-state handling for pending Stripe prompts.
- **Commit:** 53e3a6f
- **Files touched:** _SCRATCH.md; app/components/project/ProjectSettings.tsx; app/components/chat/Chat.tsx
- **Decisions made:** Reused the existing RevenueCat pending prompt pipeline for Stripe to avoid introducing a new dispatch path and to keep secret-read timing safeguards.
- **Known limitations:** Could not run eslint in this worktree because dependencies are not installed (node_modules missing).
- **How to verify:** 1) In Project Settings, connect Stripe and save publishable key. 2) Confirm chat auto-sends the add-stripe setup prompt. 3) Confirm Stripe setup banner shows setup state while prompt runs.