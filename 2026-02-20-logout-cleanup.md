I still shows claude and codex as connected when I am signing in with a new user.

Checks the logs below to determine if cleanup is being done correctly (it is not)

```
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
Failed to decode downloaded font: <URL>
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
OTS parsing error: invalid sfntVersion: 1008821359
workbench.ts:655 [WorkbenchStore] ===== reset COMPLETE =====
LaunchDarklyProvider.tsx:45 [LaunchDarkly] Init check — clientSideID: 6977612260ab8609eed2df4f user: undefined
app.tsx:58 [OnboardingDebug] Missing onboarding key: user not ready {onboardingChecked: true, isAuthenticated: false, hasUser: false}
app.tsx:93 [OnboardingDebug] Render gate state {isAuthenticated: false, onboardingChecked: true, hasCompletedOnboarding: true, userId: null}
app.tsx:121 [App] Setting up auth listeners, appApi: true
LaunchDarklyProvider.tsx:53 [LaunchDarkly] The waitForInitialization function was called without a timeout specified. In a future version a default timeout will be applied.
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:1080
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:1095
c22.<computed> @ launchdarkly-react-client-sdk.js?v=d377a481:1108
waitForInitialization @ launchdarkly-react-client-sdk.js?v=d377a481:2153
i3 @ launchdarkly-react-client-sdk.js?v=d377a481:2641
o3 @ launchdarkly-react-client-sdk.js?v=d377a481:2678
Promise.then
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:2688
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:2689
Z2 @ launchdarkly-react-client-sdk.js?v=d377a481:2675
(anonymous) @ LaunchDarklyProvider.tsx:53
react_stack_bottom_frame @ react-dom_client.js?v=d377a481:30596
runWithFiberInDEV @ react-dom_client.js?v=d377a481:13026
commitHookEffectListMount @ react-dom_client.js?v=d377a481:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=d377a481:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23095
flushPassiveEffects @ react-dom_client.js?v=d377a481:25179
flushPendingEffects @ react-dom_client.js?v=d377a481:25117
flushSpawnedWork @ react-dom_client.js?v=d377a481:25091
commitRoot @ react-dom_client.js?v=d377a481:24833
commitRootWhenReady @ react-dom_client.js?v=d377a481:24045
performWorkOnRoot @ react-dom_client.js?v=d377a481:23979
performSyncWorkOnRoot @ react-dom_client.js?v=d377a481:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=d377a481:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=d377a481:25466
(anonymous) @ react-dom_client.js?v=d377a481:25560
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] LaunchDarkly client initialized
LaunchDarklyProvider.tsx:65 [LaunchDarkly] Provider created successfully
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6ImFub255bW91cyIsImFub255bW91cyI6dHJ1ZX0
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6ImFub255bW91cyIsImFub255bW91cyI6dHJ1ZX0
VM2162 preload.js:1730 [AppApi] Received auth-token event: bfloat://auth?token=eyJhbGciOiJSUzI1NiIsImtpZCI6In...
VM2162 preload.js:1734 [AppApi] Parsed as URL, token found: true
VM2162 preload.js:1742 [AppApi] User data from URL: present
app.tsx:124 [App] Received auth token, length: 747 userData: true
token-refresh.ts:63 [TokenRefresh] Scheduling refresh in 0 minutes (expires in 4.97455 minutes)
LaunchDarklyProvider.tsx:45 [LaunchDarkly] Init check — clientSideID: 6977612260ab8609eed2df4f user: user_01KHTMVKV1C5AJW3MTBXM6073B
app.tsx:67 [OnboardingDebug] Evaluate onboarding completion {userId: 'user_01KHTMVKV1C5AJW3MTBXM6073B', onboardingKey: 'desktop_onboarding_complete:user_01KHTMVKV1C5AJW3MTBXM6073B', persistedComplete: true, onboardingChecked: true, isAuthenticated: true}
app.tsx:78 [OnboardingDebug] Onboarding marked complete {userId: 'user_01KHTMVKV1C5AJW3MTBXM6073B', onboardingKey: 'desktop_onboarding_complete:user_01KHTMVKV1C5AJW3MTBXM6073B', source: 'storage'}
app.tsx:93 [OnboardingDebug] Render gate state {isAuthenticated: true, onboardingChecked: true, hasCompletedOnboarding: true, userId: 'user_01KHTMVKV1C5AJW3MTBXM6073B'}
app.tsx:121 [App] Setting up auth listeners, appApi: true
LaunchDarklyProvider.tsx:53 [LaunchDarkly] The waitForInitialization function was called without a timeout specified. In a future version a default timeout will be applied.
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:1080
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:1095
c22.<computed> @ launchdarkly-react-client-sdk.js?v=d377a481:1108
waitForInitialization @ launchdarkly-react-client-sdk.js?v=d377a481:2153
i3 @ launchdarkly-react-client-sdk.js?v=d377a481:2641
o3 @ launchdarkly-react-client-sdk.js?v=d377a481:2678
Promise.then
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:2688
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:2689
Z2 @ launchdarkly-react-client-sdk.js?v=d377a481:2675
(anonymous) @ LaunchDarklyProvider.tsx:53
react_stack_bottom_frame @ react-dom_client.js?v=d377a481:30596
runWithFiberInDEV @ react-dom_client.js?v=d377a481:13026
commitHookEffectListMount @ react-dom_client.js?v=d377a481:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=d377a481:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23095
flushPassiveEffects @ react-dom_client.js?v=d377a481:25179
flushPendingEffects @ react-dom_client.js?v=d377a481:25117
flushSpawnedWork @ react-dom_client.js?v=d377a481:25091
commitRoot @ react-dom_client.js?v=d377a481:24833
commitRootWhenReady @ react-dom_client.js?v=d377a481:24045
performWorkOnRoot @ react-dom_client.js?v=d377a481:23979
performSyncWorkOnRoot @ react-dom_client.js?v=d377a481:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=d377a481:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=d377a481:25466
(anonymous) @ react-dom_client.js?v=d377a481:25560
rrweb-plugin-console-record.js:160 [LaunchDarkly] LaunchDarkly client initialized
LaunchDarklyProvider.tsx:65 [LaunchDarkly] Provider created successfully
rrweb-plugin-console-record.js:160 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
rrweb-plugin-console-record.js:160 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6InVzZXJfMDFLSFRNVktWMUM1QUpXM01UQlhNNjA3M0IiLCJlbWFpbCI6InBleG9rNzI1MjdAaWFjaXUuY29tIiwibmFtZSI6IlBleCBPayJ9
rrweb-plugin-console-record.js:160 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
rrweb-plugin-console-record.js:160 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6InVzZXJfMDFLSFRNVktWMUM1QUpXM01UQlhNNjA3M0IiLCJlbWFpbCI6InBleG9rNzI1MjdAaWFjaXUuY29tIiwibmFtZSI6IlBleCBPayJ9
workbench.ts:655 [WorkbenchStore] ===== reset COMPLETE =====
LaunchDarklyProvider.tsx:45 [LaunchDarkly] Init check — clientSideID: 6977612260ab8609eed2df4f user: undefined
app.tsx:58 [OnboardingDebug] Missing onboarding key: user not ready {onboardingChecked: true, isAuthenticated: false, hasUser: false}
app.tsx:93 [OnboardingDebug] Render gate state {isAuthenticated: false, onboardingChecked: true, hasCompletedOnboarding: true, userId: null}
app.tsx:121 [App] Setting up auth listeners, appApi: true
LaunchDarklyProvider.tsx:53 [LaunchDarkly] The waitForInitialization function was called without a timeout specified. In a future version a default timeout will be applied.
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:1080
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:1095
c22.<computed> @ launchdarkly-react-client-sdk.js?v=d377a481:1108
waitForInitialization @ launchdarkly-react-client-sdk.js?v=d377a481:2153
i3 @ launchdarkly-react-client-sdk.js?v=d377a481:2641
o3 @ launchdarkly-react-client-sdk.js?v=d377a481:2678
Promise.then
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:2688
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:2689
Z2 @ launchdarkly-react-client-sdk.js?v=d377a481:2675
(anonymous) @ LaunchDarklyProvider.tsx:53
react_stack_bottom_frame @ react-dom_client.js?v=d377a481:30596
runWithFiberInDEV @ react-dom_client.js?v=d377a481:13026
commitHookEffectListMount @ react-dom_client.js?v=d377a481:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=d377a481:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23095
flushPassiveEffects @ react-dom_client.js?v=d377a481:25179
flushPendingEffects @ react-dom_client.js?v=d377a481:25117
flushSpawnedWork @ react-dom_client.js?v=d377a481:25091
commitRoot @ react-dom_client.js?v=d377a481:24833
commitRootWhenReady @ react-dom_client.js?v=d377a481:24045
performWorkOnRoot @ react-dom_client.js?v=d377a481:23979
performSyncWorkOnRoot @ react-dom_client.js?v=d377a481:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=d377a481:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=d377a481:25466
(anonymous) @ react-dom_client.js?v=d377a481:25560
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] LaunchDarkly client initialized
LaunchDarklyProvider.tsx:65 [LaunchDarkly] Provider created successfully
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6ImFub255bW91cyIsImFub255bW91cyI6dHJ1ZX0
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Closing stream connection
LaunchDarklyProvider.tsx:20 [LaunchDarkly] Flags received: 1 flags
LaunchDarklyProvider.tsx:26 [LaunchDarkly] Client ready
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Opening stream connection to https://clientstream.launchdarkly.com/eval/6977612260ab8609eed2df4f/eyJraW5kIjoidXNlciIsImtleSI6ImFub255bW91cyIsImFub255bW91cyI6dHJ1ZX0
VM2162 preload.js:1730 [AppApi] Received auth-token event: bfloat://auth?token=eyJhbGciOiJSUzI1NiIsImtpZCI6In...
VM2162 preload.js:1734 [AppApi] Parsed as URL, token found: true
VM2162 preload.js:1742 [AppApi] User data from URL: present
app.tsx:124 [App] Received auth token, length: 747 userData: true
token-refresh.ts:63 [TokenRefresh] Scheduling refresh in 0 minutes (expires in 4.754466666666667 minutes)
LaunchDarklyProvider.tsx:45 [LaunchDarkly] Init check — clientSideID: 6977612260ab8609eed2df4f user: user_01KHWFWMX07YP1CECHJ77XX5TP
app.tsx:67 [OnboardingDebug] Evaluate onboarding completion {userId: 'user_01KHWFWMX07YP1CECHJ77XX5TP', onboardingKey: 'desktop_onboarding_complete:user_01KHWFWMX07YP1CECHJ77XX5TP', persistedComplete: false, onboardingChecked: true, isAuthenticated: true}
app.tsx:85 [OnboardingDebug] Onboarding remains incomplete {userId: 'user_01KHWFWMX07YP1CECHJ77XX5TP', onboardingKey: 'desktop_onboarding_complete:user_01KHWFWMX07YP1CECHJ77XX5TP'}
app.tsx:93 [OnboardingDebug] Render gate state {isAuthenticated: true, onboardingChecked: true, hasCompletedOnboarding: true, userId: 'user_01KHWFWMX07YP1CECHJ77XX5TP'}
app.tsx:121 [App] Setting up auth listeners, appApi: true
LaunchDarklyProvider.tsx:53 [LaunchDarkly] The waitForInitialization function was called without a timeout specified. In a future version a default timeout will be applied.
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:1080
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:1095
c22.<computed> @ launchdarkly-react-client-sdk.js?v=d377a481:1108
waitForInitialization @ launchdarkly-react-client-sdk.js?v=d377a481:2153
i3 @ launchdarkly-react-client-sdk.js?v=d377a481:2641
o3 @ launchdarkly-react-client-sdk.js?v=d377a481:2678
Promise.then
s2 @ launchdarkly-react-client-sdk.js?v=d377a481:2688
(anonymous) @ launchdarkly-react-client-sdk.js?v=d377a481:2689
Z2 @ launchdarkly-react-client-sdk.js?v=d377a481:2675
(anonymous) @ LaunchDarklyProvider.tsx:53
react_stack_bottom_frame @ react-dom_client.js?v=d377a481:30596
runWithFiberInDEV @ react-dom_client.js?v=d377a481:13026
commitHookEffectListMount @ react-dom_client.js?v=d377a481:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=d377a481:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=d377a481:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=d377a481:23095
flushPassiveEffects @ react-dom_client.js?v=d377a481:25179
flushPendingEffects @ react-dom_client.js?v=d377a481:25117
flushSpawnedWork @ react-dom_client.js?v=d377a481:25091
commitRoot @ react-dom_client.js?v=d377a481:24833
commitRootWhenReady @ react-dom_client.js?v=d377a481:24045
performWorkOnRoot @ react-dom_client.js?v=d377a481:23979
performSyncWorkOnRoot @ react-dom_client.js?v=d377a481:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=d377a481:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=d377a481:25466
(anonymous) @ react-dom_client.js?v=d377a481:25560
launchdarkly-react-client-sdk.js?v=d377a481:1080 [LaunchDarkly] Closing stream connection
OnboardingWizard.tsx:22 [OnboardingDebug] Wizard mounted
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 0, canProceedFromAIStep: false, hasAnthropic: false, hasOpenAI: false, hasExpo: false}
app.tsx:93 [OnboardingDebug] Render gate state {isAuthenticated: true, onboardingChecked: true, hasCompletedOnboarding: false, userId: 'user_01KHWFWMX07YP1CECHJ77XX5TP'}
OnboardingWizard.tsx:25 [OnboardingDebug] Wizard unmounted
OnboardingWizard.tsx:22 [OnboardingDebug] Wizard mounted
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 0, canProceedFromAIStep: false, hasAnthropic: false, hasOpenAI: false, hasExpo: false}
provider-auth.ts:174 [providerAuthStore] Loaded tokens: {"anthropic":{"type":"oauth","expiresAt":1803092501804,"accountId":"3bf1547e-6f82-481e-9224-751ab2aca947"},"openai":true,"expo":true}
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 0, canProceedFromAIStep: true, hasAnthropic: true, hasOpenAI: true, hasExpo: true}
provider-auth.ts:174 [providerAuthStore] Loaded tokens: {"anthropic":{"type":"oauth","expiresAt":1803092501830,"accountId":"3bf1547e-6f82-481e-9224-751ab2aca947"},"openai":true,"expo":true}
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 0, canProceedFromAIStep: true, hasAnthropic: true, hasOpenAI: true, hasExpo: true}
rrweb-plugin-console-record.js:160 [LaunchDarkly] LaunchDarkly client initialized
OnboardingWizard.tsx:30 [OnboardingDebug] Wizard next {currentStep: 0}
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 1, canProceedFromAIStep: true, hasAnthropic: true, hasOpenAI: true, hasExpo: true}
OnboardingWizard.tsx:30 [OnboardingDebug] Wizard next {currentStep: 1}
OnboardingWizard.tsx:53 [OnboardingDebug] Wizard state {currentStep: 2, canProceedFromAIStep: true, hasAnthropic: true, hasOpenAI: true, hasExpo: true}

```

