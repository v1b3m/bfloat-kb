# Dynamic Ports

## Description
Automate Expo dev-server port fallback so users never need to manually answer terminal prompts when `19000` is occupied. The app preview should continue loading on whatever port Expo selects.

## Progress
- [x] Traced current port allocation path in `Workbench.tsx` (`findAvailablePort` + `buildFullCommand`)
- [x] Identified failure mode: Expo can still prompt interactively for fallback port due runtime race/late conflict
- [x] Added non-interactive auto-accept for Expo fallback prompt (`Use port XXXX instead?`)
- [x] Synced fallback port into `actualPortRef` so preview URL detection stays aligned
- [x] Added parsing for additional Expo confirmation output (`using port XXXX`) to keep runtime port state accurate
- [x] Added per-run reset guard to avoid duplicate repeated auto-input on same suggested port
- [x] Ran focused diff + static verification of updated code

## Decisions
- Keep existing dynamic pre-allocation behavior (`findAvailablePort`) and add a runtime safety net instead of replacing it.
- Auto-accept only Expo's explicit fallback-port prompt pattern; avoid broad auto-input heuristics.
- Track last accepted suggested port to prevent repeated `y` writes for the same prompt output.

## Outcome
Implementation completed in `app/components/workbench/Workbench.tsx`.

Commit: ff01257