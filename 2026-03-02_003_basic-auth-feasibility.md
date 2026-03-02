**Ref:** main
**Depends:** none

### Context

1. Does Basic Auth in bfloat-workbench (`/Users/v1b3m/Dev/bfloat/bfloat-workbench`) require any user intervention. Or having a working convex integration is enough?

2. If the convex integration is enough, how can we port it for this codebase that does not use OAuth? Are the provided user variables good enough?

3. Update the task [[2026-03-02_003_basic-auth-implementation]] with enough context for an agent to start working on basic auth after gathering all the details.

### Acceptance Criteria

- [x] A report on BasicAuth feasiblity

### Constraints

- None

### Resources

- None
---

## BasicAuth report

### Short answer

1. In **bfloat-workbench**, a connected Convex account is effectively the prerequisite. The setup endpoint is OAuth-token gated (`convexAccessToken`) and returns `Connect Convex first` if absent (`apps/web/app/routes/api.projects.$id.setup-convex.ts:20-28`).
2. In **bfloat-ide**, there is no Convex OAuth flow wired in Connected Accounts (Convex is hardcoded disconnected: `app/components/settings/sections/ConnectedAccountsSection.tsx:316-324`). The non-OAuth path is secret-driven.
3. For this codebase, provided Convex vars are good enough to start Basic Auth setup **if** both are present:
   - app-specific URL key (`NEXT_PUBLIC_CONVEX_URL` for web or `EXPO_PUBLIC_CONVEX_URL` for mobile)
   - `CONVEX_DEPLOY_KEY`

### Evidence and implications

- Workbench setup provisions Convex deployment + deploy key + dev env vars, and pre-provisions `BETTER_AUTH_SECRET` + `SITE_URL` on the deployment (`apps/web/app/routes/api.projects.$id.setup-convex.ts:81-90`).
- IDE credentials UX already requires URL + deploy key for Convex (`app/lib/integrations/credentials.ts:47-67`) and computes readiness from those two values (`app/lib/integrations/convex.ts:40-53`).
- IDE project settings already trigger `/convex-setup` chat flow once URL + deploy key are both present (`app/components/project/ProjectSettings.tsx:186-203`, `267-281`).
- IDE Convex auth skill explicitly requires existing URL + `CONVEX_DEPLOY_KEY` before auth setup and then verifies `BETTER_AUTH_SECRET` + `SITE_URL` via `npx convex env list` (`resources/skills/convex/auth/SKILL.md:84-108`).

### Feasibility conclusion

Basic Auth (Better Auth on Convex) is feasible in bfloat-ide **without OAuth** by using secrets-based Convex credentials as the prerequisite. Porting should treat OAuth-dependent setup from workbench as replaced by explicit secrets in IDE. The remaining gap is implementation wiring (provider/routes/screens/templates), not credentials model.

### Porting recommendations

- Keep non-OAuth prerequisite contract: URL + `CONVEX_DEPLOY_KEY` required.
- Derive deployment name from URL where needed (already available in `deriveConvexDeploymentName` in `app/lib/integrations/convex.ts:67-74`).
- Before running Convex auth commands, validate deployment env vars:
  - `BETTER_AUTH_SECRET`
  - `SITE_URL` (use `http://localhost:3000` web, `http://localhost:8081` Expo)
- Reuse `resources/skills/convex/auth/templates` as source of truth for generated auth files.

### Follow-up

Updated [[2026-03-02_003_basic-auth-implementation]] with implementation-ready context and acceptance criteria.
## Handoff
- **What was done:** Analyzed bfloat-workbench OAuth-coupled Convex setup versus bfloat-ide secret-driven flow, produced feasibility conclusions, and updated the follow-up implementation task with implementation-ready scope and acceptance criteria.
- **Commit:** 1b6496e
- **Files touched:** reports/2026-03-02_003_basic-auth-feasibility-report.md; Obsidian notes 2026-03-02_003_basic-auth-feasibility, 2026-03-02_003_basic-auth-implementation, and BOARDS/IDE Tasks.
- **Decisions made:** Treat Basic Auth as Convex Better Auth; keep non-OAuth secret prerequisites (Convex URL + CONVEX_DEPLOY_KEY) as the canonical path for bfloat-ide.
- **Known limitations:** No runtime execution of Convex commands npx convex env list and npx convex dev --once was performed in this feasibility task.
- **How to verify:** Read the updated notes and report; confirm board card is under Review; confirm commit 1b6496e contains the in-repo report.