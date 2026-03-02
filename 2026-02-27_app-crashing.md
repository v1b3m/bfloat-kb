The dev server can crash as the agent generates code in the chat, the logs below show it:

```
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
ERROR: [Workbench] Dev server error detected
ERROR: [Workbench] Dev server error detected
INFO: [Preview] Current URL:  Status: error AppType: mobile ExpoUrl:
INFO: [Preview] Current URL:  Status: error AppType: mobile ExpoUrl:
INFO: [useLocalAgent] Stream message received: text
INFO: [Chat] Local agent message: text Excellent! Now the dependencies are installed. Let me start the development server:
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [useLocalAgent] Stream message received: tool_call
INFO: [Chat] Local agent message: tool_call {"id":"toolu_01Kem2H9zkL82SeFEaSUpvWG","name":"Bash","input":{"command":"npm start","description":"Start Expo development server","run_in_background":true},"status":"running"}
INFO: [Chat] Tool call: Bash {"command":"npm start","description":"Start Expo development server","run_in_background":true}
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
^[[H^[[FINFO: [useLocalAgent] Stream message received: tool_call
INFO: [Chat] Local agent message: tool_call {"id":"toolu_01Dj4e9BKJKyPzTwKdkHDpf4","name":"TaskOutput","input":{"task_id":"bos53pitd","block":true,"timeout":15000},"status":"running"}
INFO: [Chat] Tool call: TaskOutput {"task_id":"bos53pitd","block":true,"timeout":15000}
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [useLocalAgent] Stream message received: text
INFO: [Chat] Local agent message: text The port is being used by another project. Let me start the server on a different port:
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [useLocalAgent] Stream message received: tool_call
INFO: [Chat] Local agent message: tool_call {"id":"toolu_01Je7VBpuXY1E62Spgk7UUvg","name":"Bash","input":{"command":"npx expo start --port 8097","description":"Start Expo development server on port 8097","run_in_background":true},"status":"running"}
INFO: [Chat] Tool call: Bash {"command":"npx expo start --port 8097","description":"Start Expo development server on port 8097","run_in_background":true}
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [useLocalAgent] Stream message received: tool_call
INFO: [Chat] Local agent message: tool_call {"id":"toolu_01XehbtnZgX4xHsYxrskEzGa","name":"TaskOutput","input":{"task_id":"bsm5uov2i","block":true,"timeout":15000},"status":"running"}
INFO: [Chat] Tool call: TaskOutput {"task_id":"bsm5uov2i","block":true,"timeout":15000}
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] Calling useSessions with projectId: proj_1772213068802_6ggqodxc4
INFO: [Chat] pendingPrompt effect fired {"hasPendingPrompt":false,"isStreaming":true,"hasProjectPath":true}
INFO: [Chat] Not sending pending prompt, conditions: {"hasPrompt":false,"isStreaming":true,"hasProjectPath":true}
```

However, what the agent does is try to start the app on another port. Of course this does not help because then the preview will not work in the workbench.

## What to do?

We have an mcp tool that takes screenshots. How about we create an MCP tool that restarts the app using the same code that is called when we click the restart app button above the preview manually? We expose this to the agent and now it can restart the app. gracefully.




## Progress
- [x] Traced restart flow in  and confirmed it is the preview button path ().
- [x] Added a local MCP server () with  tool in sidecar.
- [x] Registered  MCP server in Claude session setup ().
- [x] Wired tool-call bridge in desktop renderer so  tool invocations trigger  listeners and reuse existing restart UI logic.
- [x] Ran TypeScript check () successfully.

## Decisions
- Reused existing workbench restart callback path instead of introducing a second restart implementation, to keep behavior identical to the preview button.
- Triggered restart from tool-call events in  because that is the integration point already responsible for agent-stream event adaptation in the renderer.
- Added a dedicated MCP server () rather than overloading screenshot tooling to keep responsibilities separated and prompt intent clear.


## Progress (Corrected)
- [x] Traced restart flow in Workbench.tsx and confirmed it is the preview button path (handleRestartServer).
- [x] Added a local MCP server (workbench) with restart_app tool in sidecar.
- [x] Registered workbench MCP server in Claude session setup (agent-session.ts).
- [x] Wired tool-call bridge in desktop renderer so restart_app tool invocations trigger onRestartDevServer listeners and reuse existing restart UI logic.
- [x] Ran TypeScript check (pnpm exec tsc --noEmit) successfully.

## Decisions (Corrected)
- Reused existing workbench restart callback path instead of introducing a second restart implementation, to keep behavior identical to the preview button.
- Triggered restart from tool-call events in conveyor-bridge.ts because that is the integration point already responsible for agent-stream event adaptation in the renderer.
- Added a dedicated MCP server (workbench) rather than overloading screenshot tooling to keep responsibilities separated and prompt intent clear.


## Progress (Status Tool + Guardrails)
- [x] Added sidecar runtime registry for workbench-managed dev-server metadata ().
- [x] Added authenticated sidecar endpoints:  and .
- [x] Added desktop bridge + app API export for workbench runtime updates/queries.
- [x] Wired Workbench heartbeat (5s) to publish server status, URL, port, app type, expo URL, and terminal id.
- [x] Added MCP tool  with health/check metadata.
- [x] Updated  MCP tool to reject restart when server is healthy/starting.
- [x] Added strict shell-command guard in agent stream to block redundant dev-server start commands when managed server is healthy/starting.
- [x] Changed restart trigger bridge to fire only on successful , not on  start.
- [x] Updated system prompt with explicit managed-dev-server policy and status-check requirement.
- [x] Verified TypeScript compile ().

## Decisions (Status Tool + Guardrails)
- Runtime truth source is pushed from Workbench to sidecar (instead of inferred only from terminal logs), then assessed with active checks (port binding + HTTP reachability).
- Guardrails are strict for healthy/starting server states: block duplicate start commands and block restart requests.
- Restart event emission moved to successful tool result to avoid accidental restart side effects when a restart request is denied.
- Added staleness handling ( when runtime state is old) so enforcement does not rely on stale metadata.


## Progress (Corrected - Status Tool + Guardrails)
- [x] Added sidecar runtime registry for workbench-managed dev-server metadata (workbench-runtime.ts).
- [x] Added authenticated sidecar endpoints: POST /api/workbench/runtime and GET /api/workbench/runtime.
- [x] Added desktop bridge and app API export for workbench runtime updates/queries.
- [x] Wired Workbench heartbeat (5s) to publish server status, URL, port, app type, expo URL, and terminal id.
- [x] Added MCP tool get_dev_server_status with health/check metadata.
- [x] Updated restart_app MCP tool to reject restart when server is healthy/starting.
- [x] Added strict shell-command guard in agent stream to block redundant dev-server start commands when managed server is healthy/starting.
- [x] Changed restart trigger bridge to fire only on successful tool_result, not on tool_call start.
- [x] Updated system prompt with explicit managed-dev-server policy and status-check requirement.
- [x] Verified TypeScript compile (pnpm exec tsc --noEmit).

## Decisions (Corrected - Status Tool + Guardrails)
- Runtime truth source is pushed from Workbench to sidecar (instead of inferred only from terminal logs), then assessed with active checks (port binding and HTTP reachability).
- Guardrails are strict for healthy/starting server states: block duplicate start commands and block restart requests.
- Restart event emission moved to successful tool result to avoid accidental restart side effects when a restart request is denied.
- Added staleness handling (unknown when runtime state is old) so enforcement does not rely on stale metadata.