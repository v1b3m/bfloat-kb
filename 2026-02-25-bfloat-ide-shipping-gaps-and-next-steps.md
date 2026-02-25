# Bfloat IDE Shipping Gaps & Next Steps

Linked from: [[2026-02-25-bfloat-ide-overview]]

## Target Constraints
1. Ship only the IDE experience from `bfloat-workbench` with project-specific changes.
2. Runtime should be Tauri-based (lighter shell).
3. Integrations must use user-provided tokens only (no OAuth).

## Constraint Alignment (Current)
- Constraint 1 (IDE-focused app): mostly aligned in this repo structure.
- Constraint 2 (Tauri runtime): aligned. Tauri + sidecar architecture is already primary runtime.
- Constraint 3 (token-only integrations): not aligned yet.

## Concrete Gaps vs Token-Only Requirement
- OAuth flows still implemented in sidecar provider routes:
  - `POST /api/provider/connect-openai` (PKCE callback + browser redirect)
  - `POST /api/provider/connect-anthropic` (`claude setup-token` OAuth-style flow)
- UI copy/behavior still assumes browser sign-in in connected accounts modal.
- Deep-link/OAuth success path still part of auth flow handling.

## Recommended Implementation Direction
- Keep token management endpoints as canonical:
  - `load-tokens`, `save-tokens`, `disconnect`, `check-*`
- Remove/deprecate OAuth connect endpoints from shipping path.
- Replace auth modal UX with manual token input UX per provider.
- Ensure provider adapters consume saved tokens/API keys only.
- Update onboarding conditions to match token-input-only flow.

## Suggested Workstream (Phased)
1. Provider API contract cleanup:
   - Mark/remove `connect-openai` and `connect-anthropic` for shipping path.
2. UI/auth flow cleanup:
   - Replace connect modal browser flow with secure token entry forms.
3. Store behavior cleanup:
   - Simplify `provider-auth` state model around explicit user-provided tokens.
4. Onboarding/settings copy update:
   - Remove OAuth language and browser/deeplink assumptions.
5. Verification:
   - Fresh install test confirms setup works without any OAuth/browser callback path.

## Acceptance Criteria For Constraint 3
- No runtime path requires browser OAuth redirect for integrations.
- User can configure integrations solely by entering token/key values.
- Provider auth state and onboarding complete successfully from token-only inputs.

Related:
- [[2026-02-25-bfloat-ide-integrations-auth]]
- [[2026-02-25-bfloat-ide-runtime-architecture]]
- [[2026-02-25-bfloat-ide-codebase-map]]