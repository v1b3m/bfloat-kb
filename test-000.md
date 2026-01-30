## Unmounting current project

```
ProjectPage.tsx:265 [ProjectPage] ===== UNMOUNTING =====
workbench.ts:298 [WorkbenchStore] Saving all files before closing project
ProjectPage.tsx:273 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:279 [ProjectPage] Resetting workbench state on unmount
workbench.ts:474 [WorkbenchStore] ===== reset CALLED =====
workbench.ts:476 [WorkbenchStore] Current project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
workbench.ts:477 [WorkbenchStore] Current files count: 21
workbench.ts:502 [WorkbenchStore] ===== reset COMPLETE =====
ProjectPage.tsx:281 [ProjectPage] ===== END UNMOUNT =====
Workbench.tsx:799 [Workbench] Running cleanup after 136059ms
Terminal.tsx:236 [Terminal UI] Killing terminal: terminal-1
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
workbench.ts:310 [WorkbenchStore] Failed to commit changes: Error: Error invoking remote method 'project:commitAndPush': Error: No project open
saveAllAndCommit @ workbench.ts:310
await in saveAllAndCommit
(anonymous) @ ProjectPage.tsx:268
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30602
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
commitHookEffectListUnmount @ react-dom_client.js?v=8ad32e62:21478
commitHookPassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:21497
commitPassiveUnmountEffectsInsideOfDeletedTree_begin @ react-dom_client.js?v=8ad32e62:23622
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23494
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23527
flushPassiveEffects @ react-dom_client.js?v=8ad32e62:25175
flushPendingEffects @ react-dom_client.js?v=8ad32e62:25117
performSyncWorkOnRoot @ react-dom_client.js?v=8ad32e62:25543
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=8ad32e62:25443
flushSpawnedWork @ react-dom_client.js?v=8ad32e62:25096
commitRoot @ react-dom_client.js?v=8ad32e62:24833
commitRootWhenReady @ react-dom_client.js?v=8ad32e62:24045
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23979
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=8ad32e62:25534
performWorkUntilDeadline @ react-dom_client.js?v=8ad32e62:321
react-dom_client.js?v=8ad32e62:301 [Violation] 'message' handler took 347ms

```

## Reopening current project

Files have been overwritten

```
ProjectPage.tsx:265 [ProjectPage] ===== UNMOUNTING =====
workbench.ts:298 [WorkbenchStore] Saving all files before closing project
ProjectPage.tsx:273 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:279 [ProjectPage] Resetting workbench state on unmount
workbench.ts:474 [WorkbenchStore] ===== reset CALLED =====
workbench.ts:476 [WorkbenchStore] Current project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
workbench.ts:477 [WorkbenchStore] Current files count: 21
workbench.ts:502 [WorkbenchStore] ===== reset COMPLETE =====
ProjectPage.tsx:281 [ProjectPage] ===== END UNMOUNT =====
Workbench.tsx:799 [Workbench] Running cleanup after 136059ms
Terminal.tsx:236 [Terminal UI] Killing terminal: terminal-1
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
workbench.ts:310 [WorkbenchStore] Failed to commit changes: Error: Error invoking remote method 'project:commitAndPush': Error: No project open
saveAllAndCommit @ workbench.ts:310
await in saveAllAndCommit
(anonymous) @ ProjectPage.tsx:268
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30602
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
commitHookEffectListUnmount @ react-dom_client.js?v=8ad32e62:21478
commitHookPassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:21497
commitPassiveUnmountEffectsInsideOfDeletedTree_begin @ react-dom_client.js?v=8ad32e62:23622
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23494
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23518
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23545
recursivelyTraversePassiveUnmountEffects @ react-dom_client.js?v=8ad32e62:23510
commitPassiveUnmountOnFiber @ react-dom_client.js?v=8ad32e62:23527
flushPassiveEffects @ react-dom_client.js?v=8ad32e62:25175
flushPendingEffects @ react-dom_client.js?v=8ad32e62:25117
performSyncWorkOnRoot @ react-dom_client.js?v=8ad32e62:25543
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=8ad32e62:25443
flushSpawnedWork @ react-dom_client.js?v=8ad32e62:25096
commitRoot @ react-dom_client.js?v=8ad32e62:24833
commitRootWhenReady @ react-dom_client.js?v=8ad32e62:24045
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23979
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=8ad32e62:25534
performWorkUntilDeadline @ react-dom_client.js?v=8ad32e62:321
react-dom_client.js?v=8ad32e62:301 [Violation] 'message' handler took 347ms
ProjectPage.tsx:265 [ProjectPage] ===== UNMOUNTING =====
workbench.ts:298 [WorkbenchStore] Saving all files before closing project
ProjectPage.tsx:273 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:279 [ProjectPage] Resetting workbench state on unmount
workbench.ts:474 [WorkbenchStore] ===== reset CALLED =====
workbench.ts:476 [WorkbenchStore] Current project ID: undefined
workbench.ts:477 [WorkbenchStore] Current files count: 0
workbench.ts:502 [WorkbenchStore] ===== reset COMPLETE =====
ProjectPage.tsx:281 [ProjectPage] ===== END UNMOUNT =====
ProjectPage.tsx:201 [ProjectPage] ===== PROJECT LOADED FROM API =====
ProjectPage.tsx:202 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:203 [ProjectPage] Project title: TaskLite
ProjectPage.tsx:204 [ProjectPage] Project sourceUrl: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:205 [ProjectPage] Project gitInfo?.cloneUrl: undefined
ProjectPage.tsx:206 [ProjectPage] Project files keys: NO FILES
ProjectPage.tsx:207 [ProjectPage] Project files count: 0
ProjectPage.tsx:208 [ProjectPage] Latest agent session: {id: 'e8c6a9d4-32c1-4693-b925-36950fd3166b', projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', sessionId: 'a1afdeb0-c411-492d-9433-ac980fd83353', provider: 'claude', name: null, …}
ProjectPage.tsx:209 [ProjectPage] ===== END PROJECT LOADED =====
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: false, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: false, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Chat.tsx:163 [Chat] Loading session from local storage: {sessionId: 'a1afdeb0-c411-492d-9433-ac980fd83353', provider: 'claude', projectPath: null, initialMessagesCount: 0}
Chat.tsx:238 [Chat] Project path received: null
Chat.tsx:239 [Chat] Initial provider: claude -> using: claude
Chat.tsx:240 [Chat] Initial session ID: a1afdeb0-c411-492d-9433-ac980fd83353
Chat.tsx:241 [Chat] Initial messages count: 0
Terminal.tsx:45 [Terminal UI] Initializing terminal: terminal-1
Terminal.tsx:104 [Terminal UI] Setting up data listener for: terminal-1
Terminal.tsx:145 [Terminal UI] Creating PTY for: terminal-1
Workbench.tsx:66 [Workbench] App type from project: mobile → using: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 0, updateInProgress: true, setupInitiated: false, lastProjectId: null, …}
Workbench.tsx:483 [Workbench] Git-based project, waiting for gitProjectPath...
ProjectPage.tsx:247 [ProjectPage] Opening project via projectStore: {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', sourceUrl: 'http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git'}
project-store.ts:233 [ProjectStore] Opening project: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:357 [ProjectPage] ===== METADATA EFFECT RUNNING =====
ProjectPage.tsx:358 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:359 [ProjectPage] Project sourceUrl: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:363 [ProjectPage] isLegacyProject: false
ProjectPage.tsx:368 [ProjectPage] hasFiles: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:369 [ProjectPage] project.files keys: NO FILES
ProjectPage.tsx:370 [ProjectPage] syncStatus: idle
ProjectPage.tsx:375 [ProjectPage] Waiting for sync to load files from git...
ProjectPage.tsx:376 [ProjectPage] ===== END METADATA EFFECT (WAITING) =====
Workbench.tsx:795 [Workbench] Skipping cleanup - component was only mounted for 2ms (StrictMode?)
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
Chat.tsx:238 [Chat] Project path received: null
Chat.tsx:239 [Chat] Initial provider: claude -> using: claude
Chat.tsx:240 [Chat] Initial session ID: a1afdeb0-c411-492d-9433-ac980fd83353
Chat.tsx:241 [Chat] Initial messages count: 0
Terminal.tsx:45 [Terminal UI] Initializing terminal: terminal-1
Terminal.tsx:104 [Terminal UI] Setting up data listener for: terminal-1
Terminal.tsx:170 [Terminal UI] PTY already exists for: terminal-1, reconnecting
Workbench.tsx:66 [Workbench] App type from project: mobile → using: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 0, updateInProgress: true, setupInitiated: false, lastProjectId: null, …}
Workbench.tsx:483 [Workbench] Git-based project, waiting for gitProjectPath...
Workbench.tsx:795 [Workbench] Skipping cleanup - component was only mounted for 3ms (StrictMode?)
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
ProjectPage.tsx:357 [ProjectPage] ===== METADATA EFFECT RUNNING =====
ProjectPage.tsx:358 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:359 [ProjectPage] Project sourceUrl: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:363 [ProjectPage] isLegacyProject: false
ProjectPage.tsx:368 [ProjectPage] hasFiles: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:369 [ProjectPage] project.files keys: NO FILES
ProjectPage.tsx:370 [ProjectPage] syncStatus: opening
ProjectPage.tsx:375 [ProjectPage] Waiting for sync to load files from git...
ProjectPage.tsx:376 [ProjectPage] ===== END METADATA EFFECT (WAITING) =====
Terminal.tsx:100 [Terminal UI] Terminal fitted: 80x24
Terminal.tsx:100 [Terminal UI] Terminal fitted: 80x24
Workbench.tsx:109 [Workbench] Terminal terminal-1 is ready
Chat.tsx:177 [Chat] Session loaded from storage: {messageCount: 4, userMessages: 2, assistantMessages: 2, messagesWithTextBlocks: 2, cwd: '/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e'}
Chat.tsx:189 [Chat] First text message from storage: {id: 'ee2ddd3a-471e-4df0-a057-425ef0b36469', contentLength: 683, blocksCount: 1, firstBlockType: 'text', textBlockContent: "I'd be happy to help you create a simple todo list…wever, I need a bit more information:\n\n1. **Wh..."}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '8981a521-6073-41fa-ac90-0e0d0ba244ef', role: 'user', contentLength: 25, hasBlocks: false, blocksCount: 0, …}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: 'ee2ddd3a-471e-4df0-a057-425ef0b36469', role: 'assistant', contentLength: 683, hasBlocks: true, blocksCount: 1, …}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 683}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '5204920b-cb3e-474d-86e5-09152ea9c4b5', role: 'user', contentLength: 23, hasBlocks: false, blocksCount: 0, …}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '377fd45a-b5b6-470f-b53a-f65345fe7e47', role: 'assistant', contentLength: 1123, hasBlocks: true, blocksCount: 15, …}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 173}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 217}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 731}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 15, textParts: 3}
Chat.tsx:214 [Chat] After conversion: {totalMessages: 4, messagesWithTextParts: 2}
Terminal.tsx:149 [Terminal UI] PTY create result: {success: true}
Workbench.tsx:109 [Workbench] Terminal terminal-1 is ready
Terminal.tsx:157 [Terminal UI] Sending initial resize: 80x24
project-store.ts:256 [ProjectStore] Project ready with 24 files
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Chat.tsx:163 [Chat] Loading session from local storage: {sessionId: 'a1afdeb0-c411-492d-9433-ac980fd83353', provider: 'claude', projectPath: '/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e', initialMessagesCount: 0}
Chat.tsx:238 [Chat] Project path received: /Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e
Chat.tsx:239 [Chat] Initial provider: claude -> using: claude
Chat.tsx:240 [Chat] Initial session ID: a1afdeb0-c411-492d-9433-ac980fd83353
Chat.tsx:241 [Chat] Initial messages count: 0
Terminal.tsx:45 [Terminal UI] Initializing terminal: terminal-1
Terminal.tsx:104 [Terminal UI] Setting up data listener for: terminal-1
Terminal.tsx:170 [Terminal UI] PTY already exists for: terminal-1, reconnecting
Workbench.tsx:66 [Workbench] App type from project: mobile → using: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 0, updateInProgress: true, setupInitiated: false, lastProjectId: null, …}
Workbench.tsx:492 [Workbench] Waiting for files to be loaded from git...
ProjectPage.tsx:293 [ProjectPage] ===== FILE SYNC EFFECT RUNNING =====
ProjectPage.tsx:294 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:295 [ProjectPage] fileTree length: 24
ProjectPage.tsx:296 [ProjectPage] fileTree paths: (24) ['app', 'app/(tabs)', 'components', 'components/navigation', 'constants', 'app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package.json', 'tailwind.config.js', 'tsconfig.json']
ProjectPage.tsx:297 [ProjectPage] lastFileChange: 1769114663035
project-store.ts:287 [ProjectStore] Opening file: app.config.js
ProjectPage.tsx:357 [ProjectPage] ===== METADATA EFFECT RUNNING =====
ProjectPage.tsx:358 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:359 [ProjectPage] Project sourceUrl: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:363 [ProjectPage] isLegacyProject: false
ProjectPage.tsx:368 [ProjectPage] hasFiles: http://localhost:3001/bfloat-projects/d786edd3-fe96-46e4-9fa1-231b7491127e.git
ProjectPage.tsx:369 [ProjectPage] project.files keys: NO FILES
ProjectPage.tsx:370 [ProjectPage] syncStatus: ready
ProjectPage.tsx:395 [ProjectPage] GIT PROJECT - setting metadata only (files from sync)
workbench.ts:98 [WorkbenchStore] ===== setProjectMetadata CALLED =====
workbench.ts:99 [WorkbenchStore] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
workbench.ts:101 [WorkbenchStore] ===== setProjectMetadata COMPLETE =====
ProjectPage.tsx:399 [ProjectPage] Called workbenchStore.setProjectMetadata
ProjectPage.tsx:401 [ProjectPage] ===== END METADATA EFFECT =====
Workbench.tsx:795 [Workbench] Skipping cleanup - component was only mounted for 3ms (StrictMode?)
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
Chat.tsx:238 [Chat] Project path received: /Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e
Chat.tsx:239 [Chat] Initial provider: claude -> using: claude
Chat.tsx:240 [Chat] Initial session ID: a1afdeb0-c411-492d-9433-ac980fd83353
Chat.tsx:241 [Chat] Initial messages count: 0
Terminal.tsx:45 [Terminal UI] Initializing terminal: terminal-1
Terminal.tsx:104 [Terminal UI] Setting up data listener for: terminal-1
Terminal.tsx:170 [Terminal UI] PTY already exists for: terminal-1, reconnecting
Workbench.tsx:66 [Workbench] App type from project: mobile → using: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 0, updateInProgress: true, setupInitiated: false, lastProjectId: null, …}
Workbench.tsx:492 [Workbench] Waiting for files to be loaded from git...
ProjectPage.tsx:255 [ProjectPage] Project opened successfully
Terminal.tsx:100 [Terminal UI] Terminal fitted: 80x24
Workbench.tsx:109 [Workbench] Terminal terminal-1 is ready
Terminal.tsx:100 [Terminal UI] Terminal fitted: 96x1
Workbench.tsx:109 [Workbench] Terminal terminal-1 is ready
Chat.tsx:177 [Chat] Session loaded from storage: {messageCount: 4, userMessages: 2, assistantMessages: 2, messagesWithTextBlocks: 2, cwd: '/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e'}
Chat.tsx:189 [Chat] First text message from storage: {id: 'ee2ddd3a-471e-4df0-a057-425ef0b36469', contentLength: 683, blocksCount: 1, firstBlockType: 'text', textBlockContent: "I'd be happy to help you create a simple todo list…wever, I need a bit more information:\n\n1. **Wh..."}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '8981a521-6073-41fa-ac90-0e0d0ba244ef', role: 'user', contentLength: 25, hasBlocks: false, blocksCount: 0, …}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: 'ee2ddd3a-471e-4df0-a057-425ef0b36469', role: 'assistant', contentLength: 683, hasBlocks: true, blocksCount: 1, …}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 683}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '5204920b-cb3e-474d-86e5-09152ea9c4b5', role: 'user', contentLength: 23, hasBlocks: false, blocksCount: 0, …}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 1, textParts: 1}
Chat.tsx:93 [Chat] convertSessionMessage: {id: '377fd45a-b5b6-470f-b53a-f65345fe7e47', role: 'assistant', contentLength: 1123, hasBlocks: true, blocksCount: 15, …}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 173}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 217}
Chat.tsx:108 [Chat] Adding text part: {contentLength: 731}
Chat.tsx:127 [Chat] convertSessionMessage result: {totalParts: 15, textParts: 3}
Chat.tsx:214 [Chat] After conversion: {totalMessages: 4, messagesWithTextParts: 2}
project-store.ts:287 [ProjectStore] Opening file: App.tsx
project-store.ts:287 [ProjectStore] Opening file: app/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/explore.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/index.tsx
project-store.ts:287 [ProjectStore] Opening file: app/+not-found.tsx
project-store.ts:287 [ProjectStore] Opening file: babel.config.js
project-store.ts:287 [ProjectStore] Opening file: components/navigation/TabBarIcon.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedText.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedView.tsx
project-store.ts:287 [ProjectStore] Opening file: constants/Colors.ts
project-store.ts:287 [ProjectStore] Opening file: eslint.config.js
project-store.ts:287 [ProjectStore] Opening file: global.css
project-store.ts:287 [ProjectStore] Opening file: metro.config.js
project-store.ts:287 [ProjectStore] Opening file: nativewind-env.d.ts
project-store.ts:287 [ProjectStore] Opening file: package.json
project-store.ts:287 [ProjectStore] Opening file: tailwind.config.js
project-store.ts:287 [ProjectStore] Opening file: tsconfig.json
ProjectPage.tsx:336 [ProjectPage] Syncing 19 files to workbench
ProjectPage.tsx:337 [ProjectPage] File keys: (19) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:372 [WorkbenchStore] ===== setFiles CALLED =====
workbench.ts:373 [WorkbenchStore] Files keys: (19) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:374 [WorkbenchStore] Files count: 19
workbench.ts:377 [WorkbenchStore] ===== setFiles COMPLETE =====
ProjectPage.tsx:339 [ProjectPage] Called workbenchStore.setFiles
ProjectPage.tsx:340 [ProjectPage] ===== END FILE SYNC EFFECT =====
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 19, updateInProgress: true, setupInitiated: false, lastProjectId: null, …}
Workbench.tsx:525 [Workbench] Setup check: {setupInitiated: false, currentProjectPath: null, fileCount: 19}
Workbench.tsx:537 [Workbench] Starting setup for project: d786edd3-fe96-46e4-9fa1-231b7491127e with 19 files
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Workbench.tsx:611 [Workbench] Using git project path: /Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e
index.ts:141 [Launch] No .bfloat/launch.json found
index.ts:40 [Launch] Detected Expo/React Native project from package.json
index.ts:69 [Launch] Auto-detected app type: mobile
Workbench.tsx:629 [Workbench] Using launch config: {type: 'mobile', setup: 'npm install --legacy-peer-deps', dev: 'BROWSER=none npx expo start --web --clear'}
Workbench.tsx:545 [Workbench] Waiting for terminal terminal-1 to be ready...
Workbench.tsx:552 [Workbench] Terminal terminal-1 is ready!
Workbench.tsx:556 [Workbench] Waiting for shell to fully initialize...
Workbench.tsx:580 [Workbench] Error finding available port: Error: Error invoking remote method 'terminal-find-port': [
  {
    "code": "too_big",
    "maximum": 1,
    "origin": "array",
    "path": [],
    "message": "Too big: expected array to have <1 items"
  }
]
runDevServer @ Workbench.tsx:580
await in runDevServer
setupProjectAndRunExpo @ Workbench.tsx:632
await in setupProjectAndRunExpo
(anonymous) @ Workbench.tsx:734
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30596
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
commitHookEffectListMount @ react-dom_client.js?v=8ad32e62:21440
commitHookPassiveMountEffects @ react-dom_client.js?v=8ad32e62:21494
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23069
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23062
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23084
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23230
recursivelyTraversePassiveMountEffects @ react-dom_client.js?v=8ad32e62:23039
commitPassiveMountOnFiber @ react-dom_client.js?v=8ad32e62:23095
flushPassiveEffects @ react-dom_client.js?v=8ad32e62:25179
flushPendingEffects @ react-dom_client.js?v=8ad32e62:25117
flushSpawnedWork @ react-dom_client.js?v=8ad32e62:25091
commitRoot @ react-dom_client.js?v=8ad32e62:24833
commitRootWhenReady @ react-dom_client.js?v=8ad32e62:24045
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23979
performSyncWorkOnRoot @ react-dom_client.js?v=8ad32e62:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=8ad32e62:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=8ad32e62:25466
(anonymous) @ react-dom_client.js?v=8ad32e62:25560
<ForwardRef(Workbench2)>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=8ad32e62:269
ProjectContent @ ProjectContent.tsx:140
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30538
renderWithHooksAgain @ react-dom_client.js?v=8ad32e62:17758
renderWithHooks @ react-dom_client.js?v=8ad32e62:17694
updateFunctionComponent @ react-dom_client.js?v=8ad32e62:19504
beginWork @ react-dom_client.js?v=8ad32e62:20554
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
performUnitOfWork @ react-dom_client.js?v=8ad32e62:24590
workLoopSync @ react-dom_client.js?v=8ad32e62:24453
renderRootSync @ react-dom_client.js?v=8ad32e62:24437
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23795
performSyncWorkOnRoot @ react-dom_client.js?v=8ad32e62:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=8ad32e62:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=8ad32e62:25466
(anonymous) @ react-dom_client.js?v=8ad32e62:25560
<ProjectContent>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=8ad32e62:269
ProjectPageContent @ ProjectPage.tsx:499
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30538
renderWithHooksAgain @ react-dom_client.js?v=8ad32e62:17758
renderWithHooks @ react-dom_client.js?v=8ad32e62:17694
updateFunctionComponent @ react-dom_client.js?v=8ad32e62:19504
beginWork @ react-dom_client.js?v=8ad32e62:20554
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
performUnitOfWork @ react-dom_client.js?v=8ad32e62:24590
workLoopSync @ react-dom_client.js?v=8ad32e62:24453
renderRootSync @ react-dom_client.js?v=8ad32e62:24437
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23795
performSyncWorkOnRoot @ react-dom_client.js?v=8ad32e62:25546
flushSyncWorkAcrossRoots_impl @ react-dom_client.js?v=8ad32e62:25443
processRootScheduleInMicrotask @ react-dom_client.js?v=8ad32e62:25466
(anonymous) @ react-dom_client.js?v=8ad32e62:25560
<ProjectPageContent>
exports.jsxDEV @ react_jsx-dev-runtime.js?v=8ad32e62:269
ProjectPage @ ProjectPage.tsx:514
react_stack_bottom_frame @ react-dom_client.js?v=8ad32e62:30538
renderWithHooksAgain @ react-dom_client.js?v=8ad32e62:17758
renderWithHooks @ react-dom_client.js?v=8ad32e62:17694
updateFunctionComponent @ react-dom_client.js?v=8ad32e62:19504
beginWork @ react-dom_client.js?v=8ad32e62:20554
runWithFiberInDEV @ react-dom_client.js?v=8ad32e62:13026
performUnitOfWork @ react-dom_client.js?v=8ad32e62:24590
workLoopConcurrentByScheduler @ react-dom_client.js?v=8ad32e62:24586
renderRootConcurrent @ react-dom_client.js?v=8ad32e62:24568
performWorkOnRoot @ react-dom_client.js?v=8ad32e62:23795
performWorkOnRootViaSchedulerTask @ react-dom_client.js?v=8ad32e62:25534
performWorkUntilDeadline @ react-dom_client.js?v=8ad32e62:321
Workbench.tsx:585 [Workbench] Running mobile command: cd "/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e" && npm install --legacy-peer-deps && PORT=19000 BROWSER=none npx expo start --web --clear --port 19000
Workbench.tsx:594 [Workbench] Command sent successfully
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
project-store.ts:162 [ProjectStore] File change: add package-lock.json
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
ProjectPage.tsx:293 [ProjectPage] ===== FILE SYNC EFFECT RUNNING =====
ProjectPage.tsx:294 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:295 [ProjectPage] fileTree length: 25
ProjectPage.tsx:296 [ProjectPage] fileTree paths: (25) ['app', 'app/(tabs)', 'components', 'components/navigation', 'constants', 'app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
ProjectPage.tsx:297 [ProjectPage] lastFileChange: 1769114757099
project-store.ts:287 [ProjectStore] Opening file: app.config.js
project-store.ts:287 [ProjectStore] Opening file: App.tsx
project-store.ts:287 [ProjectStore] Opening file: app/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/explore.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/index.tsx
project-store.ts:287 [ProjectStore] Opening file: app/+not-found.tsx
project-store.ts:287 [ProjectStore] Opening file: babel.config.js
project-store.ts:287 [ProjectStore] Opening file: components/navigation/TabBarIcon.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedText.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedView.tsx
project-store.ts:287 [ProjectStore] Opening file: constants/Colors.ts
project-store.ts:287 [ProjectStore] Opening file: eslint.config.js
project-store.ts:287 [ProjectStore] Opening file: global.css
project-store.ts:287 [ProjectStore] Opening file: metro.config.js
project-store.ts:287 [ProjectStore] Opening file: nativewind-env.d.ts
project-store.ts:287 [ProjectStore] Opening file: package-lock.json
project-store.ts:287 [ProjectStore] Opening file: package.json
project-store.ts:287 [ProjectStore] Opening file: tailwind.config.js
project-store.ts:287 [ProjectStore] Opening file: tsconfig.json
ProjectPage.tsx:336 [ProjectPage] Syncing 20 files to workbench
ProjectPage.tsx:337 [ProjectPage] File keys: (20) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:372 [WorkbenchStore] ===== setFiles CALLED =====
workbench.ts:373 [WorkbenchStore] Files keys: (20) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:374 [WorkbenchStore] Files count: 20
workbench.ts:377 [WorkbenchStore] ===== setFiles COMPLETE =====
ProjectPage.tsx:339 [ProjectPage] Called workbenchStore.setFiles
ProjectPage.tsx:340 [ProjectPage] ===== END FILE SYNC EFFECT =====
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 20, updateInProgress: true, setupInitiated: true, lastProjectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', …}
Workbench.tsx:525 [Workbench] Setup check: {setupInitiated: true, currentProjectPath: '/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 20}
Workbench.tsx:527 [Workbench] Setup already completed for this project, skipping
project-store.ts:162 [ProjectStore] File change: add expo-env.d.ts
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
ProjectContent.tsx:77 [ProjectContent] Auto-start decision: {initialProvider: undefined, hasProjectPath: true, initialMessagesLength: 0, latestAgentSession: 'a1afdeb0-c411-492d-9433-ac980fd83353', shouldAutoStart: false, …}
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
ProjectPage.tsx:293 [ProjectPage] ===== FILE SYNC EFFECT RUNNING =====
ProjectPage.tsx:294 [ProjectPage] Project ID: d786edd3-fe96-46e4-9fa1-231b7491127e
ProjectPage.tsx:295 [ProjectPage] fileTree length: 26
ProjectPage.tsx:296 [ProjectPage] fileTree paths: (26) ['app', 'app/(tabs)', 'components', 'components/navigation', 'constants', 'app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'expo-env.d.ts', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
ProjectPage.tsx:297 [ProjectPage] lastFileChange: 1769114764514
project-store.ts:287 [ProjectStore] Opening file: app.config.js
project-store.ts:287 [ProjectStore] Opening file: App.tsx
project-store.ts:287 [ProjectStore] Opening file: app/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/_layout.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/explore.tsx
project-store.ts:287 [ProjectStore] Opening file: app/(tabs)/index.tsx
project-store.ts:287 [ProjectStore] Opening file: app/+not-found.tsx
project-store.ts:287 [ProjectStore] Opening file: babel.config.js
project-store.ts:287 [ProjectStore] Opening file: components/navigation/TabBarIcon.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedText.tsx
project-store.ts:287 [ProjectStore] Opening file: components/ThemedView.tsx
project-store.ts:287 [ProjectStore] Opening file: constants/Colors.ts
project-store.ts:287 [ProjectStore] Opening file: eslint.config.js
project-store.ts:287 [ProjectStore] Opening file: expo-env.d.ts
project-store.ts:287 [ProjectStore] Opening file: global.css
project-store.ts:287 [ProjectStore] Opening file: metro.config.js
project-store.ts:287 [ProjectStore] Opening file: nativewind-env.d.ts
project-store.ts:287 [ProjectStore] Opening file: package-lock.json
project-store.ts:287 [ProjectStore] Opening file: package.json
project-store.ts:287 [ProjectStore] Opening file: tailwind.config.js
project-store.ts:287 [ProjectStore] Opening file: tsconfig.json
ProjectPage.tsx:336 [ProjectPage] Syncing 21 files to workbench
ProjectPage.tsx:337 [ProjectPage] File keys: (21) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'expo-env.d.ts', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:372 [WorkbenchStore] ===== setFiles CALLED =====
workbench.ts:373 [WorkbenchStore] Files keys: (21) ['app.config.js', 'App.tsx', 'app/_layout.tsx', 'app/(tabs)/_layout.tsx', 'app/(tabs)/explore.tsx', 'app/(tabs)/index.tsx', 'app/+not-found.tsx', 'babel.config.js', 'components/navigation/TabBarIcon.tsx', 'components/ThemedText.tsx', 'components/ThemedView.tsx', 'constants/Colors.ts', 'eslint.config.js', 'expo-env.d.ts', 'global.css', 'metro.config.js', 'nativewind-env.d.ts', 'package-lock.json', 'package.json', 'tailwind.config.js', 'tsconfig.json']
workbench.ts:374 [WorkbenchStore] Files count: 21
workbench.ts:377 [WorkbenchStore] ===== setFiles COMPLETE =====
ProjectPage.tsx:339 [ProjectPage] Called workbenchStore.setFiles
ProjectPage.tsx:340 [ProjectPage] ===== END FILE SYNC EFFECT =====
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: starting AppType: mobile
Workbench.tsx:462 [Workbench] Auto-run useEffect triggered {projectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 21, updateInProgress: true, setupInitiated: true, lastProjectId: 'd786edd3-fe96-46e4-9fa1-231b7491127e', …}
Workbench.tsx:525 [Workbench] Setup check: {setupInitiated: true, currentProjectPath: '/Users/v1b3m/.bfloat/projects/d786edd3-fe96-46e4-9fa1-231b7491127e', fileCount: 21}
Workbench.tsx:527 [Workbench] Setup already completed for this project, skipping
Workbench.tsx:196 [Workbench] Detected Expo URL: exp://192.168.100.154:19000
Workbench.tsx:231 [Workbench] Dev server detected at: http://localhost:19000
Preview.tsx:211 [Preview] Current URL:  Status: running AppType: mobile
Preview.tsx:211 [Preview] Current URL:  Status: running AppType: mobile
Preview.tsx:71 [Preview] Setting preview URL: http://localhost:19000
Preview.tsx:211 [Preview] Current URL: http://localhost:19000 Status: running AppType: mobile
Preview.tsx:211 [Preview] Current URL: http://localhost:19000 Status: running AppType: mobile
entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40290 Running application "main" with appParams:
 {rootTag: '#root', hydrate: undefined} 
Development-level warnings: ON.
Performance optimizations: OFF.
entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:47060 Uncaught Error: Cannot manually set color scheme, as dark mode is type 'media'. Please use StyleSheet.setFlag('darkMode', 'class')
    at Object.set (entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:47060:15)
    at MutationObserver.observe.attributes (entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:47046:27)
set @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:47060
MutationObserver.observe.attributes @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:47046
childList
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:90933
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:91470
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:166
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:90647
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:159
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
get @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:50681
metroContext @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:50686
loadRoute @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:82099
getDirectoryTree @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:82173
getRoutes @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:81902
getRoutes @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:81771
useStore @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:78731
ContextNavigator @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:85558
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ContextNavigator>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
ExpoRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:85512
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ExpoRoot>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
App @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:50631
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<App>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
ErrorOverlay @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92764
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ErrorOverlay>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
WithDevTools @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:73913
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<withDevTools(ErrorOverlay)>
exports.createElement @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:2002
renderApplication @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40356
run @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40255
runApplication @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40293
registerRootComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:73885
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92696
exports.startTransition @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:2074
renderRootComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92691
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:690
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:166
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:677
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:159
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:93230
Preview.tsx:211 [Preview] Current URL: http://localhost:19000 Status: running AppType: mobile
Preview.tsx:211 [Preview] Current URL: http://localhost:19000 Status: running AppType: mobile
entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:23895 props.pointerEvents is deprecated. Use style.pointerEvents
warnOnce @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:23895
createDOMProps @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:20140
createElement @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:19117
View @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:26209
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateForwardRef @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8452
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9204
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<View>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
Screen @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:68001
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<Screen>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:63513
BottomTabView @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:63457
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<BottomTabView>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
BottomTabNavigator @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:63244
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<BottomTabNavigator>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:61797
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateForwardRef @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8452
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9204
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ForwardRef>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
Object.assign.Screen @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:76934
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<...>
exports.jsxs @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18157
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
TabLayout @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:50728
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<TabLayout(./(tabs)/_layout.tsx)>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
BaseRoute @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:62735
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<Route((tabs))>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
SceneView @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58458
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<SceneView>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
render @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58209
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58243
useDescriptors @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58240
useNavigationBuilder @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:57868
NativeStackNavigator @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:76281
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<NativeStackNavigator>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:61797
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateForwardRef @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8452
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9204
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ForwardRef>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
Object.assign.Screen @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:51197
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<...>
exports.jsxs @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18157
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
RootLayout @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:90656
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<RootLayout(./_layout.tsx)>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
BaseRoute @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:62735
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<Route()>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
SceneView @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58458
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<SceneView>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
render @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58209
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58243
useDescriptors @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:58240
useNavigationBuilder @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:57868
Content @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:85611
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<Content>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
ContextNavigator @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:85584
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ContextNavigator>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
ExpoRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:85512
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ExpoRoot>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
App @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:50631
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<App>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
ErrorOverlay @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92764
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<ErrorOverlay>
exports.jsx @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18153
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:18182
WithDevTools @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:73913
react-stack-bottom-frame @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:16641
renderWithHooks @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:6851
updateFunctionComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:8544
beginWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:9130
runWithFiberInDEV @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:4623
performUnitOfWork @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11203
workLoopConcurrentByScheduler @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11199
renderRootConcurrent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11182
performWorkOnRoot @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:10820
performWorkOnRootViaSchedulerTask @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:11800
performWorkUntilDeadline @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:17448
<withDevTools(ErrorOverlay)>
exports.createElement @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:2002
renderApplication @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40356
run @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40255
runApplication @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:40293
registerRootComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:73885
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92696
exports.startTransition @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:2074
renderRootComponent @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:92691
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:690
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:166
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:677
loadModuleImplementation @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:254
guardedLoadModule @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:159
metroRequire @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:79
(anonymous) @ entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:93230
entry.bundle?platform=web&dev=true&hot=false&lazy=true&transform.engine=hermes&transform.routerRoot=app&unstable_transformProfile=hermes-stable:87414 [Intervention] Slow network is detected. See https://www.chromestatus.com/feature/5636954674692096 for more details. Fallback font will be used while loading: http://localhost:19000/assets/?unstable_path=.%2Fnode_modules%2F%40expo%2Fvector-icons%2Fbuild%2Fvendor%2Freact-native-vector-icons%2FFonts/Ionicons.ttf

```