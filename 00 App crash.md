When I try to run the app, it crashes with the following errors

### Backend (`/Users/v1b3m/Dev/bfloat/bfloat-ide`)

```
[2026-01-22 21:22:12.260 +0300] ERROR: Git API request failed
    host: "MacBook-Pro.local"
    status: 404
    url: "http://localhost:3001/api/v1/repos/bfloat-projects/808268c4-b060-4daf-bc48-ccb5882f4cf5/contents?ref=main"
    error: "{\"errors\":null,\"message\":\"The target couldn't be found.\",\"url\":\"http://localhost:3001/api/swagger\"}\n"
[2026-01-22 21:22:12.785 +0300] ERROR: Error fetching project:
    host: "MacBook-Pro.local"
    err: {
      "type": "TypeError",
      "message": "Cannot read properties of undefined (reading 'findFirst')",
      "stack":
          TypeError: Cannot read properties of undefined (reading 'findFirst')
              at Module.getLatestAgentSession (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/app/dao/projects.server.ts:172:67)
              at loader (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/app/routes/api.projects.$id.ts:331:60)
              at processTicksAndRejections (node:internal/process/task_queues:105:5)
              at callRouteHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:508:16)
              at commonRoute.loader (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:658:19)
              at file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4762:19
              at callLoaderOrAction (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4814:16)
              at async Promise.all (index 0)
              at defaultDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4439:17)
              at callDataStrategyImpl (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4708:17)
    }
```

### IDE dev terminal (`/Users/v1b3m/Dev/bfloat/bfloat-ide`)

```
[Claude Provider] Auth check result: true
[4582:0122/212204.260117:ERROR:CONSOLE:1] "Request Autofill.enable failed. {"code":-32601,"message":"'Autofill.enable' wasn't found"}", source: devtools://devtools/bundled/core/protocol_client/protocol_client.js (1)
[4582:0122/212204.260163:ERROR:CONSOLE:1] "Request Autofill.setAddresses failed. {"code":-32601,"message":"'Autofill.setAddresses' wasn't found"}", source: devtools://devtools/bundled/core/protocol_client/protocol_client.js (1)
IPC Error in user:get-account-settings: ZodError: [
  {
    "expected": "object",
    "code": "invalid_type",
    "path": [],
    "message": "Invalid input: expected object, received null"
  }
]
    at validateReturn (/Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:875:37)
    at /Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:882:14
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)
    at async Session.<anonymous> (node:electron/js2c/browser_init:2:107253)
Error occurred in handler for 'user:get-account-settings': ZodError: [
  {
    "expected": "object",
    "code": "invalid_type",
    "path": [],
    "message": "Invalid input: expected object, received null"
  }
]
    at validateReturn (/Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:875:37)
    at /Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:882:14
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)
    at async Session.<anonymous> (node:electron/js2c/browser_init:2:107253)
```

### ide browser console

```
client:733 [vite] connecting...
client:827 [vite] connected.
react-dom_client.js?v=f327559a:32132 Download the React DevTools for a better development experience: https://react.dev/link/react-devtools
app.tsx:51 [App] Setting up auth listeners, appApi: true userApiInstance: true
app.tsx:51 [App] Setting up auth listeners, appApi: true userApiInstance: true
VM273 preload.js:34 [AppApi] Received auth-token event: eyJhbGciOiJSUzI1NiIsImNhdCI6ImNsX0I3ZDRQRDIyMkFBQS...
VM273 preload.js:44 [AppApi] URL parsing failed, checking if JWT
VM273 preload.js:46 [AppApi] Detected JWT token format
app.tsx:54 [App] Received auth token, length: 698
token-refresh.ts:58 [TokenRefresh] Scheduling refresh in 10074.981783333333 minutes (expires in 10079.981783333333 minutes)
VM273 preload.js:34 [AppApi] Received auth-token event: eyJhbGciOiJSUzI1NiIsImNhdCI6ImNsX0I3ZDRQRDIyMkFBQS...
VM273 preload.js:44 [AppApi] URL parsing failed, checking if JWT
VM273 preload.js:46 [AppApi] Detected JWT token format
app.tsx:54 [App] Received auth token, length: 698
VM273 preload.js:34 [AppApi] Received auth-token event: eyJhbGciOiJSUzI1NiIsImNhdCI6ImNsX0I3ZDRQRDIyMkFBQS...
VM273 preload.js:44 [AppApi] URL parsing failed, checking if JWT
VM273 preload.js:46 [AppApi] Detected JWT token format
app.tsx:54 [App] Received auth token, length: 698
VM273 preload.js:34 [AppApi] Received auth-token event: eyJhbGciOiJSUzI1NiIsImNhdCI6ImNsX0I3ZDRQRDIyMkFBQS...
VM273 preload.js:44 [AppApi] URL parsing failed, checking if JWT
VM273 preload.js:46 [AppApi] Detected JWT token format
app.tsx:54 [App] Received auth token, length: 698
token-refresh.ts:58 [TokenRefresh] Scheduling refresh in 10074.990866666667 minutes (expires in 10079.990866666667 minutes)
token-refresh.ts:35 [TokenRefresh] Already scheduled for this token, skipping
token-refresh.ts:35 [TokenRefresh] Already scheduled for this token, skipping
react-dom_client.js?v=f327559a:301 [Violation] 'message' handler took 328ms
project-store.ts:151 [ProjectStore] IPC listener initialized
ProjectPage.tsx:139 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:145 [ProjectPage] Resetting workbench state on unmount
workbench.ts:400 [WorkbenchStore] Resetting all state
ProjectPage.tsx:63 [ProjectPage] Failed to fetch account settings: Error: Error invoking remote method 'user:get-account-settings': [
  {
    "expected": "object",
    "code": "invalid_type",
    "path": [],
    "message": "Invalid input: expected object, received null"
  }
]
(anonymous) @ ProjectPage.tsx:63
Promise.catch
(anonymous) @ ProjectPage.tsx:62
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30596
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
commitHookEffectListMount @ react-dom_client.js?v=f327559a:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=f327559a:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23095
flushPassiveEffects @ react-dom_client.js?v=f327559a:25179
(anonymous) @ react-dom_client.js?v=f327559a:24805
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321
<ProjectPageContent>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=f327559a:269
ProjectPage @ ProjectPage.tsx:335
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30538
renderWithHooksAgain @ react-dom_client.js?v=f327559a:17758
renderWithHooks @ react-dom_client.js?v=f327559a:17694
updateFunctionComponent @ react-dom_client.js?v=f327559a:19504
beginWork @ react-dom_client.js?v=f327559a:20554
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
performUnitOfWork @ react-dom_client.js?v=f327559a:24590
workLoopConcurrentByScheduler @ react-dom_client.js?v=f327559a:24586
renderRootConcurrent @ react-dom_client.js?v=f327559a:24568
performWorkOnRoot @ react-dom_client.js?v=f327559a:23795
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=f327559a:25534
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321
client.ts:85  GET http://localhost:3000/api/projects/808268c4-b060-4daf-bc48-ccb5882f4cf5 500 (Internal Server Error)
apiClient @ client.ts:85
get @ project.ts:66
(anonymous) @ ProjectPage.tsx:76
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30596
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
commitHookEffectListMount @ react-dom_client.js?v=f327559a:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=f327559a:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23095
flushPassiveEffects @ react-dom_client.js?v=f327559a:25179
(anonymous) @ react-dom_client.js?v=f327559a:24805
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321
<ProjectPageContent>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=f327559a:269
ProjectPage @ ProjectPage.tsx:335
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30538
renderWithHooksAgain @ react-dom_client.js?v=f327559a:17758
renderWithHooks @ react-dom_client.js?v=f327559a:17694
updateFunctionComponent @ react-dom_client.js?v=f327559a:19504
beginWork @ react-dom_client.js?v=f327559a:20554
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
performUnitOfWork @ react-dom_client.js?v=f327559a:24590
workLoopConcurrentByScheduler @ react-dom_client.js?v=f327559a:24586
renderRootConcurrent @ react-dom_client.js?v=f327559a:24568
performWorkOnRoot @ react-dom_client.js?v=f327559a:23795
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=f327559a:25534
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321
ProjectPage.tsx:87 [ProjectPage] Failed to fetch project: ApiError: Internal Server Error
    at apiClient (client.ts:101:13)
    at async Object.get (project.ts:66:22)
(anonymous) @ ProjectPage.tsx:87
Promise.catch
(anonymous) @ ProjectPage.tsx:86
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30596
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
commitHookEffectListMount @ react-dom_client.js?v=f327559a:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=f327559a:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=f327559a:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=f327559a:23095
flushPassiveEffects @ react-dom_client.js?v=f327559a:25179
(anonymous) @ react-dom_client.js?v=f327559a:24805
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321
<ProjectPageContent>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=f327559a:269
ProjectPage @ ProjectPage.tsx:335
react_stack_bottom_frame @ react-dom_client.js?v=f327559a:30538
renderWithHooksAgain @ react-dom_client.js?v=f327559a:17758
renderWithHooks @ react-dom_client.js?v=f327559a:17694
updateFunctionComponent @ react-dom_client.js?v=f327559a:19504
beginWork @ react-dom_client.js?v=f327559a:20554
runWithFiberInDEV @ react-dom_client.js?v=f327559a:13026
performUnitOfWork @ react-dom_client.js?v=f327559a:24590
workLoopConcurrentByScheduler @ react-dom_client.js?v=f327559a:24586
renderRootConcurrent @ react-dom_client.js?v=f327559a:24568
performWorkOnRoot @ react-dom_client.js?v=f327559a:23795
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=f327559a:25534
performWorkUntilDeadline @ react-dom_client.js?v=f327559a:321

```

### IDE Web UI

```
<h2>Error</h2><p>Internal Server Error</p><button class="inline-flex items-center justify-center gap-2 font-medium transition-all duration-150 ease-out focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-offset-background disabled:opacity-50 disabled:pointer-events-none active:scale-[0.98] bg-[hsl(var(--card))] text-[hsl(var(--foreground))] border border-[hsl(var(--border))] hover:bg-[hsl(var(--muted))] rounded-lg h-10 py-2 px-4 text-sm "><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-arrow-left" aria-hidden="true"><path d="m12 19-7-7 7-7"></path><path d="M19 12H5"></path></svg>Back to Home</button>
```

