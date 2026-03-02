# Integrations: Token-Only, No OAuth

## Description
Ensure integrations work using user-provided tokens only, with no OAuth/browser callback paths in the shipping IDE flow.

## Progress
- [ ] Audit current provider/auth flow across UI, bridge, sidecar, and adapters
- [ ] Remove/deprecate OAuth connect paths from shipping runtime
- [ ] Implement and validate token-input-only provider flow
- [ ] Update onboarding/auth copy and state handling
- [ ] Verify end-to-end setup works without OAuth

## Decisions
- Initial direction follows [[2026-02-25-bfloat-ide-shipping-gaps-and-next-steps]] and [[2026-02-25-bfloat-ide-integrations-auth]].

## Outcome
Pending.


## Progress Update (Implementation)
- [x] Audited generated-app integration flows (`firebase`, `convex`, `stripe`, `revenuecat`) in chat/workbench/settings.
- [x] Added shared secret-detection utility at `app/lib/integrations/secrets.ts`.
- [x] Replaced generated-app OAuth connect actions with Settings-tab routing + required-key guidance toasts.
- [x] Added missing chat gating/rendering support for `stripe-setup-prompt` and `revenuecat-setup-prompt`.
- [x] Added `RevenueCatSetupBanner` and wired it through `AssistantMessage`/`Messages`/`Chat`.
- [x] Updated integration menu behavior so RevenueCat uses `Connect` flow when disconnected.
- [x] Updated secret key suggestions to align with actual required env keys for web/mobile.

## Decisions
- Kept Claude/Codex local provider auth flow untouched (explicitly out of scope).
- Set generated-app integration “Connect” to route users to Workbench `settings` tab, not external OAuth URLs.
- Defined generated-app integration readiness from project-level secret presence + existing project metadata flags.

## Outcome (Current)
Implemented the token/secrets-first generated-integration flow in app UI/runtime paths. No active `chat`/`workbench` path now depends on `/desktop/{service}/connect` OAuth redirects for generated integrations.

Validation notes:
- Ran targeted scans to ensure those OAuth connect URLs are removed from active generated-integration paths.
- Full repo TypeScript check still reports many pre-existing unrelated errors; this pass focused on integration-flow changes only.

- Spawned focused sub-task: [[2026-02-26-convex-skill-parity-ide-vs-workbench]] to track Convex setup parity with workbench.

- Convex skill parity pass completed in [[2026-02-26-convex-skill-parity-ide-vs-workbench]]: IDE `resources/skills/convex` now matches workbench.

## Progress Update (2026-02-26 Convex Reliability)
- [x] Tightened Convex configured-state detection to require both URL (`EXPO_PUBLIC_CONVEX_URL`/`NEXT_PUBLIC_CONVEX_URL`) and `CONVEX_DEPLOY_KEY`.
- [x] Updated chat Convex connect/setup gating to block setup until both secrets are present and to inject both secrets into agent session env.
- [x] Updated Settings save flow to trigger Convex setup only when both secrets exist; URL-only saves now prompt for `CONVEX_DEPLOY_KEY`.
- [x] Wired Workbench Database tab to read project secrets (`/api/secrets`) as source of truth for embedded Convex dashboard config.
- [x] Added Database-tab missing-key guidance when URL exists but `CONVEX_DEPLOY_KEY` is absent.
- [x] Updated Convex skill docs to remove incorrect “deploy key pre-configured” assumptions and require fail-fast on `npx convex dev --once` failures.

## Decisions (2026-02-26)
- Convex dashboard and setup readiness should be derived from secrets, not stale project metadata flags.
- We require deploy credentials (`CONVEX_DEPLOY_KEY`) before any setup command, to avoid fake `_generated` artifacts and runtime query mismatches.


## Progress Update (2026-02-26 Multi-field Integration Modal)
- [x] Added dynamic integration credentials schema in  for firebase/convex/stripe/revenuecat (app-type aware keys).
- [x] Added reusable  that renders arbitrary required fields, prefills existing secrets, validates required fields, and supports best-effort multi-key save with per-key failure reporting.
- [x] Updated connect request contract to integration-only ( no longer uses ).
- [x] Rewired Chat and Workbench connect actions to open the new integration modal.
- [x] Updated integration presence detection to use shared required-key logic ().
- [x] Wired Project Settings to save all integration keys, refresh secrets once, bump , and run Convex post-save setup trigger when credentials become complete.


## Progress Update (2026-02-26 Multi-field Integration Modal, corrected)
- Added dynamic integration credentials schema in app/lib/integrations/credentials.ts for firebase/convex/stripe/revenuecat with app-type-aware keys.
- Added reusable IntegrationCredentialsModal that renders arbitrary required fields, prefills existing secrets, validates required fields, and supports best-effort multi-key save with per-key failure reporting.
- Updated connect request contract to integration-only; pendingIntegrationConnect no longer carries suggestedKey.
- Rewired Chat and Workbench connect actions to open the new integration modal.
- Updated integration presence detection to use shared required-key logic via hasRequiredSecrets.
- Wired Project Settings to save all integration keys, refresh secrets once, bump secretsVersion, and run Convex post-save setup trigger when credentials become complete.


## Commits (2026-02-26)
- 345edbb — multi-field integration credentials modal + integration-driven connect flow + shared required-key detection + Convex secrets-first setup gating in IDE runtime.
- 67da89e — Convex skill parity/docs/templates alignment with workbench.
