**Ref:** main
**Depends:** none

### Context

Implement Convex Better Auth (email/password) in bfloat-ide using the existing **non-OAuth** Convex credential model.

This task is intentionally not using account-level Convex OAuth. Instead, rely on project secrets and Convex CLI flow.

#### Preconditions already validated

- Convex OAuth is not currently wired in IDE Connected Accounts (`app/components/settings/sections/ConnectedAccountsSection.tsx:316-324`).
- Convex integration in IDE is secrets-driven (`app/lib/integrations/credentials.ts`, `app/lib/integrations/convex.ts`).
- Convex setup trigger already exists once URL + deploy key are present (`app/components/project/ProjectSettings.tsx:186-203`, `267-281`).

#### Required secret contract

- Web: `NEXT_PUBLIC_CONVEX_URL`
- Mobile: `EXPO_PUBLIC_CONVEX_URL`
- Both: `CONVEX_DEPLOY_KEY`

Auth setup must stop early if this contract is not satisfied.

#### Implementation direction

Use the existing Convex auth skill templates under:
- `resources/skills/convex/auth/templates/...`

Primary sequence:
1. Validate required Convex credentials exist.
2. Validate Convex deployment env vars (`BETTER_AUTH_SECRET`, `SITE_URL`) via `npx convex env list`; set if missing.
3. Generate/install Better Auth files from templates (`convex.config.ts`, `convex/auth.config.ts`, `convex/auth.ts`, `convex/http.ts`, plus framework-specific client/provider/pages).
4. Run `npx convex dev --once`.
5. Verify app auth flow (sign-up/sign-in/sign-out + auth-gated content).

### Acceptance Criteria

- [ ] Task enforces non-OAuth prerequisites and fails with clear error when Convex URL/deploy key are missing.
- [ ] Better Auth files are created from canonical templates in `resources/skills/convex/auth/templates`.
- [ ] `BETTER_AUTH_SECRET` and `SITE_URL` are verified on the target Convex deployment before finalizing setup.
- [ ] `npx convex dev --once` succeeds after auth wiring.
- [ ] App has working sign-in + sign-up UX and authenticated/unauthenticated gating.
- [ ] No requirement for user to connect Convex via OAuth for this flow.

### Constraints

- Do not introduce Convex OAuth backend dependencies from bfloat-workbench.
- Keep scope to Convex Better Auth integration only.
- Do not alter unrelated integration flows (Stripe/RevenueCat/Firebase).

### Resources

- Feasibility report: [[2026-03-02_003_basic-auth-feasibility]]
- IDE Convex auth skill: `resources/skills/convex/auth/SKILL.md`
- IDE Convex credential model: `app/lib/integrations/credentials.ts`, `app/lib/integrations/convex.ts`
- Existing setup trigger path: `app/components/project/ProjectSettings.tsx`
- Workbench reference for prior OAuth-coupled setup: `/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/routes/api.projects.$id.setup-convex.ts`

## Handoff

- **What was done:** Implemented non-OAuth Convex auth gating in IDE chat flow and skill instructions. Convex setup now routes to `/convex-auth`, checks required secret contract (`NEXT_PUBLIC_CONVEX_URL` or `EXPO_PUBLIC_CONVEX_URL` + `CONVEX_DEPLOY_KEY`) before triggering auth setup, and returns clear missing-key errors. Updated Convex quick-add secret suggestions to the same contract.
- **Files touched:** `app/components/chat/Chat.tsx`, `app/components/settings/sections/SecretModal.tsx`, `resources/skills/convex/auth/SKILL.md`.
- **Board update:** moved `[[2026-03-02_009_basic-auth-implementation]]` from `In Progress` to `Review`.
- **Known limitations:** Could not verify `npx convex env list`/`npx convex dev --once` end-to-end here because this repo is the IDE codebase, not a generated target app workspace with live Convex credentials.
