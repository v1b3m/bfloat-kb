The app just randomly crashed after I navigated from the preview to the convex dashboard (database tab). Below are the console logs:

```
[Info] Successfully preconnected to https://dashboard-embedded.convex.dev/
[Error] Failed to load resource: the server responded with a status of 404 (Not Found) (check_admin_key, line 0)
[Error] Encountered unexpected state in data page: status: Exhausted, numRowsInTable: 0, numRowsRead: 0, isLoading: false – "warning"
	(anonymous function) (data-37b9022019467a8c.js:1:159027)
	uD (framework-a6339a3b935b453e.js:1:90544)
	oI (framework-a6339a3b935b453e.js:1:119538)
	o_ (framework-a6339a3b935b453e.js:1:102394)
	r4 (framework-a6339a3b935b453e.js:1:51347)
	(anonymous function) (framework-a6339a3b935b453e.js:1:117984)
	oD (framework-a6339a3b935b453e.js:1:117988)
	ow (framework-a6339a3b935b453e.js:1:101397)
	x (framework-a6339a3b935b453e.js:1:136524)
	T (framework-a6339a3b935b453e.js:1:137054)
[Log] [useLocalAgent] Stream message received: – "tool_call" (useLocalAgent.ts, line 130)
[Log] [Chat] Local agent message: – "tool_call" – {id: "toolu_01U7WQ2Q3p4vjon3LLmWSjw4", name: "Read", input: {file_path: "/Users/v1b3m/.bfloat-ide/projects/proj_1772083723097_gfr3bmaj0/convex/_generated/api.js"}, …} (Chat.tsx, line 476)
{id: "toolu_01U7WQ2Q3p4vjon3LLmWSjw4", name: "Read", input: {file_path: "/Users/v1b3m/.bfloat-ide/projects/proj_1772083723097_gfr3bmaj0/convex/_generated/api.js"}, status: "running"}Object
[Log] [Chat] Tool call: – "Read" – {file_path: "/Users/v1b3m/.bfloat-ide/projects/proj_1772083723097_gfr3bmaj0/convex/_generated/api.js"} (Chat.tsx, line 536)
[Log] [useLocalAgent] Stream message received: – "tool_call" (useLocalAgent.ts, line 130)
[Log] [useLocalAgent] Dropping duplicate/old message – {sessionId: "9d9b0df6-a4bd-4b51-802a-0b317071b88b", seq: 115, current: 115} (useLocalAgent.ts, line 54)
[Log] [useLocalAgent] Stream message received: – "tool_call" (useLocalAgent.ts, line 130)
[Log] [useLocalAgent] Dropping duplicate/old message – {sessionId: "9d9b0df6-a4bd-4b51-802a-0b317071b88b", seq: 115, current: 115} (useLocalAgent.ts, line 54)
[Log] [useLocalAgent] Stream message received: – "tool_call" (useLocalAgent.ts, line 130)
[Log] [useLocalAgent] Dropping duplicate/old message – {sessionId: "9d9b0df6-a4bd-4b51-802a-0b317071b88b", seq: 115, current: 115} (useLocalAgent.ts, line 54)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Chat] pendingPrompt effect fired – {hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, …} (Chat.tsx, line 1019)
{hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, pendingPrompt: undefined}Object
[Log] [Chat] Not sending pending prompt, conditions: – {hasPrompt: false, isStreaming: true, hasProjectPath: true} (Chat.tsx, line 1046)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Preview] Current URL: – "http://localhost:19000" – "Status:" – "running" – "AppType:" – "mobile" – "ExpoUrl:" – "exp://192.168.100.175:19000" (Preview.tsx, line 399)
[Log] [Preview] Current URL: – "http://localhost:19000" – "Status:" – "running" – "AppType:" – "mobile" – "ExpoUrl:" – "exp://192.168.100.175:19000" (Preview.tsx, line 399)
[Log] [Chat] pendingPrompt effect fired – {hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, …} (Chat.tsx, line 1019)
{hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, pendingPrompt: undefined}Object
[Log] [Chat] Not sending pending prompt, conditions: – {hasPrompt: false, isStreaming: true, hasProjectPath: true} (Chat.tsx, line 1046)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Chat] Calling useSessions with projectId: – "proj_1772083723097_gfr3bmaj0" (Chat.tsx, line 142)
[Log] [Preview] Current URL: – "http://localhost:19000" – "Status:" – "running" – "AppType:" – "mobile" – "ExpoUrl:" – "exp://192.168.100.175:19000" (Preview.tsx, line 399)
[Log] [Preview] Current URL: – "http://localhost:19000" – "Status:" – "running" – "AppType:" – "mobile" – "ExpoUrl:" – "exp://192.168.100.175:19000" (Preview.tsx, line 399)
[Log] [Chat] pendingPrompt effect fired – {hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, …} (Chat.tsx, line 1019)
{hasPendingPrompt: false, isStreaming: true, hasProjectPath: true, pendingPrompt: undefined}Object
[Log] [Chat] Not sending pending prompt, conditions: – {hasPrompt: false, isStreaming: true, hasProjectPath: true} (Chat.tsx, line 1046)
```




## Progress (2026-02-26)
- [x] Hardened Convex dashboard postMessage handshake in host: strict origin/source checks and explicit target origin.
- [x] Added handshake timeout and iframe load-error handling to prevent indefinite spinner and renderer instability.
- [x] Added inline fallback panel in Database tab context with actions: Retry, Open Settings, Open Convex Dashboard.
- [x] Wired Workbench callbacks for dashboard error/status logging and fallback actions.
- [x] Verified TypeScript compile passes.

## Decisions
- Success criterion is renderer stability (no global crash) rather than eliminating all iframe internal console warnings.
- Dashboard failures should degrade to inline fallback, not tab auto-switching.

## Outcome (Current)
Host-side Convex dashboard integration is now defensive and recoverable. If embedded dashboard handshake/loading fails, the UI remains stable and shows actionable recovery controls instead of crashing the app shell.


## Commit
- 1564a42 — Hardened embedded Convex dashboard host integration (origin/source validation, explicit postMessage target origin, timeout/error fallback UI with Retry/Settings/Open external).
