# RevenueCat

## Description
Align RevenueCat setup behavior in IDE with the working workbench pattern:
- Show "Setting up RevenueCat..." and disable the setup button while setup handoff is in progress.
- Fix RevenueCat skill/template drift causing Expo plugin errors.
- Keep IDE token/API-key-first behavior (no OAuth reintroduction).

## Context
- Desired UX: banner should show setup-in-progress state, similar to Stripe flow expectation.
- Observed runtime error in generated Expo projects:
  - `PluginError: Unable to resolve a valid config plugin for react-native-purchases`
  - Root cause: `react-native-purchases` incorrectly added to `expo.plugins`.

## Progress
- [x] Added RevenueCat setup-in-progress UI state in chat: setup button disables and shows spinner + "Setting up RevenueCat...".
- [x] Threaded `isRevenueCatSettingUp` state from `Chat` through `Messages` + `AssistantMessage` into `RevenueCatSetupBanner`.
- [x] Synced RevenueCat skill Step 3 guidance with workbench baseline to explicitly forbid adding `react-native-purchases` to Expo plugins.
- [x] Synced RevenueCat app.json plugin template with workbench baseline by removing `react-native-purchases` from `expo.plugins`.
- [x] Bumped bundled skills version to `4.1.2` for automatic reinjection into existing projects.
- [ ] Validate end-to-end with a fresh Expo project bootstrap in IDE runtime.

## Decisions
- Task scoped to RevenueCat-focused parity (not full all-skills parity).
- Workbench used as baseline for RevenueCat skill/template content.
- Kept IDE token/API-key-first path; no OAuth behavior added.
- Rolled out via skills version bump because injector compares semantic version, not updatedAt.

## Outcome
In progress.