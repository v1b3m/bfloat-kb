# Mobile-Only Packages

## Description
Prevent generated Expo apps from crashing on web when mobile/device-only packages are used without guards (starting with `expo-haptics`, plus other common risky APIs).

## Progress
- [x] Reviewed existing launch/system prompt guardrails and identified insertion point in `lib/launch/system-prompt.ts`
- [x] Added `MOBILE_ONLY_PACKAGE_SAFETY_PROMPT` with safe usage pattern (`Platform.OS` guard, dynamic import, try/catch, no-op fallback)
- [x] Added curated risky package examples: `expo-haptics`, `expo-notifications`, `expo-sensors`, `expo-camera`, `expo-location`
- [x] Injected the new guardrail into both new-session and resumed-session prompt composition paths
- [x] Ran source/syntax verification for updated prompt file

## Decisions
- Scope is prompt guardrails only (no runtime block/warning in this task).
- Coverage is a curated list of common risky device APIs, expressed as guidance examples rather than hard denylist enforcement.
- Keep feature behavior on web as graceful degradation (no-op fallback) instead of crash/failure.

## Outcome
Implemented in `lib/launch/system-prompt.ts` by adding and composing `MOBILE_ONLY_PACKAGE_SAFETY_PROMPT` across both prompt modes.

Commit: b22cd4e