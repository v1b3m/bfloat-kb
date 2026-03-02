---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: Shared Lib

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 37
- Source root(s): lib/agents/background-registry.ts, lib/agents/index.ts, lib/agents/manager.ts, lib/agents/providers, lib/agents/session-reader.test.ts, lib/agents/session-reader.ts, lib/agents/skills-injector.ts, lib/agents/types.ts, lib/conveyor/conveyor.d.ts, lib/conveyor/handlers, lib/conveyor/schemas, lib/launch/index.ts, lib/launch/system-prompt.ts, lib/mcp/index.ts, lib/mcp/registry.ts, lib/mcp/servers, lib/mcp/types.ts, lib/platform/shell.ts, lib/utils.ts, lib/utils/convex.ts, lib/utils/integration-env-vars.ts

## Inventory

### `lib/agents/background-registry.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `getBackgroundRegistry`
- Direct dependencies (internal): `lib/agents/types.ts`
- Used by: 0 file(s)

### `lib/agents/index.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: ` ClaudeAgentProvider `, ` CodexAgentProvider `, ` getAgentManager, resetAgentManager, DefaultAgentManager `, ` ensureSkillsInjected, needsSkillsInjection, injectSkills `
- Used by: 0 file(s)

### `lib/agents/manager.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `getAgentManager`, `resetAgentManager`, ` DefaultAgentManager `
- Direct dependencies (internal): `lib/agents/providers/claude-provider.ts`, `lib/agents/providers/codex-provider.ts`, `lib/agents/providers/bfloat-provider.ts`
- Used by: 0 file(s)

### `lib/agents/providers/bfloat-provider.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `BfloatAgentProvider`, ` getProviderFromModel, PROVIDER_CONFIG `
- Direct dependencies (internal): `lib/agents/providers/claude-provider.ts`
- Used by: 1 file(s) — `lib/agents/manager.ts`

### `lib/agents/providers/claude-provider.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `findClaudeCodeBinaryPath`, `ClaudeAgentSession`, `ClaudeAgentProvider`
- Direct dependencies (internal): `lib/platform/shell.ts`
- Direct dependencies (external): `@anthropic-ai/claude-agent-sdk`, `path`, `fs`, `os`
- Used by: 2 file(s) — `lib/agents/manager.ts`, `lib/agents/providers/bfloat-provider.ts`

### `lib/agents/providers/codex-provider.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `CodexAgentProvider`
- Direct dependencies (external): `path`, `fs`, `os`
- Used by: 1 file(s) — `lib/agents/manager.ts`

### `lib/agents/session-reader.test.ts`
- Responsibility: Shared agent/provider orchestration module.
- Direct dependencies (internal): `lib/agents/session-reader.ts`
- Direct dependencies (external): `fs`, `path`, `os`
- Used by: 0 file(s)

### `lib/agents/session-reader.ts`
- Responsibility: Shared agent/provider orchestration module.
- Exports: `sessionToMessages`
- Direct dependencies (internal): `app/components/chat/types.ts`
- Direct dependencies (external): `fs`, `path`, `os`, `readline`
- Used by: 1 file(s) — `lib/agents/session-reader.test.ts`

### `lib/agents/skills-injector.ts`
- Responsibility: Shared agent/provider orchestration module.
- Direct dependencies (external): `fs/promises`, `fs`, `path`
- Used by: 0 file(s)

### `lib/agents/types.ts`
- Responsibility: Shared agent/provider orchestration module.
- Used by: 5 file(s) — `lib/agents/background-registry.ts`, `lib/mcp/registry.ts`, `lib/mcp/servers/revenuecat.ts`, `lib/mcp/servers/stripe.ts`, `lib/mcp/types.ts`

### `lib/conveyor/conveyor.d.ts`
- Responsibility: Shared runtime utility module.
- Used by: 0 file(s)

### `lib/conveyor/handlers/revenuecat-handler.ts`
- Responsibility: Shared runtime utility module.
- Used by: 1 file(s) — `lib/mcp/servers/revenuecat.ts`

### `lib/conveyor/handlers/stripe-handler.ts`
- Responsibility: Shared runtime utility module.
- Used by: 1 file(s) — `lib/mcp/servers/stripe.ts`

### `lib/conveyor/schemas/agent-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `agentApiSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/ai-agent-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `providerIdSchema`, `permissionModeSchema`, `agentToolSchema`, `agentDefinitionSchema`, `agentModelSchema`, `sessionOptionsSchema`, `messageTypeSchema`, `toolCallContentSchema`
- Direct dependencies (external): `zod`
- Used by: 6 file(s) — `app/components/chat/Chat.tsx`, `app/components/home/HomePage.tsx`, `app/components/project/ProjectContent.tsx`, `app/components/project/ProjectPage.tsx`, `app/hooks/useLocalAgent.ts`

### `lib/conveyor/schemas/app-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `appIpcSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/deploy-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `SaveASCApiKeyArgsSchema`, `IOSBuildInteractiveArgsSchema`, `Submit2FACodeArgsSchema`, `SubmitTerminalInputArgsSchema`, `AppleSessionInfoSchema`, `PromptTypeSchema`, `HumanizedPromptOptionSchema`, `HumanizedPromptSchema`
- Direct dependencies (external): `zod`
- Used by: 5 file(s) — `app/components/deploy/AppleAuthStep.tsx`, `app/components/deploy/IOSSetupWizard.tsx`, `app/components/deploy/TerminalFallbackStep.tsx`, `app/stores/deploy.ts`, `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/filesystem-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `filesystemIpcSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/index.ts`
- Responsibility: Shared schema contract module.
- Exports: `ipcSchemas`, `validateArgs`, `validateReturn`
- Direct dependencies (internal): `lib/conveyor/schemas/window-schema.ts`, `lib/conveyor/schemas/app-schema.ts`, `lib/conveyor/schemas/terminal-schema.ts`, `lib/conveyor/schemas/filesystem-schema.ts`, `lib/conveyor/schemas/agent-schema.ts`, `lib/conveyor/schemas/provider-schema.ts`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `app/components/window/WindowContext.tsx`

### `lib/conveyor/schemas/project-files-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `projectFilesApiSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/provider-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `ProviderTypeSchema`, `OAuthTokensSchema`, `ProviderAuthStateSchema`, `AuthStatusSchema`, `ConnectResultSchema`, `GitBashSelectionResultSchema`, `ExpoCredentialsSchema`, `ExpoConnectResultSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/screenshot-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `screenshotIpcSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/secrets-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `secretsApiSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/terminal-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `terminalIpcSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/conveyor/schemas/window-schema.ts`
- Responsibility: Shared schema contract module.
- Exports: `windowIpcSchema`
- Direct dependencies (external): `zod`
- Used by: 1 file(s) — `lib/conveyor/schemas/index.ts`

### `lib/launch/index.ts`
- Responsibility: Shared runtime utility module.
- Exports: `detectAppTypeFromPackageJson`, `detectLaunchConfig`, `parseLaunchConfig`, `getLaunchConfig`, `buildFullCommand`, ` getSystemPrompt, PROJECT_EXPLORATION_PROMPT `
- Used by: 2 file(s) — `app/components/project/ProjectPage.tsx`, `app/components/workbench/Workbench.tsx`

### `lib/launch/system-prompt.ts`
- Responsibility: Shared runtime utility module.
- Exports: `getSystemPrompt`, `PROJECT_EXPLORATION_PROMPT`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `lib/mcp/index.ts`
- Responsibility: Shared MCP integration/runtime module.
- Exports: ` getMcpRegistry, McpServerRegistry `
- Used by: 0 file(s)

### `lib/mcp/registry.ts`
- Responsibility: Shared MCP integration/runtime module.
- Exports: `getMcpRegistry`, `McpServerRegistry`
- Direct dependencies (internal): `lib/agents/types.ts`, `lib/mcp/types.ts`, `lib/mcp/servers/terminal.ts`, `lib/mcp/servers/stripe.ts`, `lib/mcp/servers/revenuecat.ts`
- Used by: 0 file(s)

### `lib/mcp/servers/revenuecat.ts`
- Responsibility: Shared MCP integration/runtime module.
- Exports: `revenuecatServer`
- Direct dependencies (internal): `lib/mcp/types.ts`, `lib/agents/types.ts`, `lib/conveyor/handlers/revenuecat-handler.ts`
- Used by: 1 file(s) — `lib/mcp/registry.ts`

### `lib/mcp/servers/stripe.ts`
- Responsibility: Shared MCP integration/runtime module.
- Exports: `stripeServer`
- Direct dependencies (internal): `lib/mcp/types.ts`, `lib/agents/types.ts`, `lib/conveyor/handlers/stripe-handler.ts`
- Used by: 1 file(s) — `lib/mcp/registry.ts`

### `lib/mcp/servers/terminal.ts`
- Responsibility: Shared MCP integration/runtime module.
- Exports: `terminalServer`
- Direct dependencies (internal): `lib/mcp/types.ts`
- Used by: 1 file(s) — `lib/mcp/registry.ts`

### `lib/mcp/types.ts`
- Responsibility: Shared MCP integration/runtime module.
- Direct dependencies (internal): `lib/agents/types.ts`
- Used by: 4 file(s) — `lib/mcp/registry.ts`, `lib/mcp/servers/revenuecat.ts`, `lib/mcp/servers/stripe.ts`, `lib/mcp/servers/terminal.ts`

### `lib/platform/shell.ts`
- Responsibility: Shared runtime utility module.
- Exports: `getShellPaths`, `isBundledShellAvailable`, `getInteractiveShellConfig`, `getScriptShellConfig`, `getGitPath`, `toUnixPath`, `toWindowsPath`, `logShellDiagnostics`
- Direct dependencies (external): `path`, `fs`
- Used by: 1 file(s) — `lib/agents/providers/claude-provider.ts`

### `lib/utils.ts`
- Responsibility: Shared runtime utility module.
- Exports: `cn`
- Direct dependencies (external): `clsx`, `tailwind-merge`
- Used by: 13 file(s) — `app/components/ai-elements/attachments.tsx`, `app/components/ai-elements/web-preview.tsx`, `app/components/chat/SuggestionChips.tsx`, `app/components/home/HomeSidebar.tsx`, `app/components/preview/IPhoneFrame.tsx`

### `lib/utils/convex.ts`
- Responsibility: Shared runtime utility module.
- Exports: `getTokenDetails`, `createConvex`, `createDeploymentKey`
- Used by: 0 file(s)

### `lib/utils/integration-env-vars.ts`
- Responsibility: Shared runtime utility module.
- Direct dependencies (internal): `app/stores/workbench.ts`
- Used by: 0 file(s)

## Related
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-27-bfloat-ide-components-sidecar]]