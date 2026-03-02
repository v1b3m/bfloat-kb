# Codex Provider Not Registered in Sidecar

## Description
Starting a session with `codex` shows the toast `Not found`

## Root Cause
The sidecar's `agent-session.ts` only registered `ClaudeProvider`. When the UI sent `POST /api/agent/sessions` with `provider: "codex"`, the route handler returned a 404: `Provider 'codex' is not registered.`

The `CodexAgentProvider` existed in `lib/agents/providers/codex-provider.ts` (desktop/Electron interface) but was never ported to the sidecar's `AgentProvider` interface.

## Fix
1. Added `@openai/codex-sdk` as a dependency to the sidecar package
2. Created `packages/sidecar/src/services/codex-provider.ts` — implements the sidecar's `AgentProvider` interface using the Codex SDK's Thread API
3. Imported and registered `CodexProvider` in `agent-session.ts` alongside `ClaudeProvider`

## Files Changed
- `packages/sidecar/package.json` — added `@openai/codex-sdk` dependency
- `packages/sidecar/src/services/codex-provider.ts` — new file, sidecar-compatible Codex provider
- `packages/sidecar/src/services/agent-session.ts` — imported and registered `CodexProvider`

## Outcome
Build succeeds, all tests pass. Codex provider is now registered and sessions can be created.