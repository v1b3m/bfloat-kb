# Convex Skill Parity: IDE vs Workbench

## Description
Compare Convex skill implementation between bfloat-ide and bfloat-workbench and port missing pieces that affect successful setup in IDE-generated apps.

## Progress
- [x] Audit file-level parity for `resources/skills/convex`
- [ ] Port missing framework-specific Convex auth templates
- [ ] Align Convex auth skill instructions with workbench where relevant
- [ ] Re-run parity diff and document intentional differences

## Decisions
- Scope is skill parity for generated app setup (`resources/skills/convex`), not full runtime app feature development.
- Workbench Convex skill is baseline unless a part is clearly workbench-only.

## Have vs Missing (Initial Audit)
- Have: `setup` skill templates and structure aligned.
- Missing: `auth/templates/convex/auth.expo.ts`.
- Missing: `auth/templates/convex/auth.next.ts`.
- Drift: `auth/templates/lib/auth-client-expo.ts` imports `expoClient` from `@better-auth/expo` instead of `@better-auth/expo/client`.
- Drift: `auth/SKILL.md` lacks several workbench fixes (framework-specific auth template selection, Expo dependency list update, cross-domain pairing guidance, icon mapping safety rule).

## Outcome
Pending.
## Progress Update
- [x] Ported missing `auth/templates/convex/auth.expo.ts`.
- [x] Ported missing `auth/templates/convex/auth.next.ts`.
- [x] Aligned `auth/templates/lib/auth-client-expo.ts` to import `expoClient` from `@better-auth/expo/client`.
- [x] Synced `auth/SKILL.md` with workbench guidance (framework-specific auth templates, Expo deps include `expo-network`, cross-domain pairing notes, icon mapping safety rule).
- [x] Re-ran parity diff: `resources/skills/convex` is now fully aligned with workbench.

## Decisions Update
- No workbench-only Convex skill parts were identified in this directory; full parity is appropriate.

## Outcome (Current)
Convex skill files under `resources/skills/convex` are now at file-level parity with workbench, removing known setup drift likely causing IDE setup failures.


## Outcome (Final)
Completed Convex skill parity alignment with workbench (setup/auth guidance + missing auth templates + expo client import fix).
Commit: 67da89e
