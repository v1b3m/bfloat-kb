## Unmounting

```
ProjectPage.tsx:265 [ProjectPage] ===== UNMOUNTING =====
workbench.ts:298 [WorkbenchStore] Saving all files before closing project
Workbench.tsx:799 [Workbench] Running cleanup after 211090ms
Terminal.tsx:236 [Terminal UI] Killing terminal: terminal-1
Workbench.tsx:820 [Workbench] Skipping cleanup for git project directory: /Users/v1b3m/.bfloat/projects/39ca3e3f-b2f0-44a2-b761-9953c70b64ea
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
react-dom_client.js?v=8ad32e62:301 [Violation] 'message' handler took 350ms
workbench.ts:308 [WorkbenchStore] Changes committed to git
ProjectPage.tsx:272 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:280 [ProjectPage] Resetting workbench state on unmount
workbench.ts:474 [WorkbenchStore] ===== reset CALLED =====
workbench.ts:476 [WorkbenchStore] Current project ID: 39ca3e3f-b2f0-44a2-b761-9953c70b64ea
workbench.ts:477 [WorkbenchStore] Current files count: 21
workbench.ts:502 [WorkbenchStore] ===== reset COMPLETE =====
ProjectPage.tsx:282 [ProjectPage] ===== END UNMOUNT =====

```

changes are now preserved

---

## Opening second project and unmounting

```
ProjectPage.tsx:265 [ProjectPage] ===== UNMOUNTING =====
workbench.ts:298 [WorkbenchStore] Saving all files before closing project
Workbench.tsx:799 [Workbench] Running cleanup after 139689ms
Terminal.tsx:236 [Terminal UI] Killing terminal: terminal-1
Workbench.tsx:820 [Workbench] Skipping cleanup for git project directory: /Users/v1b3m/.bfloat/projects/43d998af-c86f-4679-a796-15c8cf66e3fe
Terminal.tsx:183 [Terminal UI] Disposing xterm UI for: terminal-1
workbench.ts:308 [WorkbenchStore] Changes committed to git
ProjectPage.tsx:272 [ProjectPage] Closing project on unmount
project-store.ts:269 [ProjectStore] Closing project
ProjectPage.tsx:280 [ProjectPage] Resetting workbench state on unmount
workbench.ts:474 [WorkbenchStore] ===== reset CALLED =====
workbench.ts:476 [WorkbenchStore] Current project ID: 43d998af-c86f-4679-a796-15c8cf66e3fe
workbench.ts:477 [WorkbenchStore] Current files count: 21
workbench.ts:502 [WorkbenchStore] ===== reset COMPLETE =====
ProjectPage.tsx:282 [ProjectPage] ===== END UNMOUNT =====
react-dom_client.js?v=8ad32e62:301 [Violation] 'message' handler took 336ms

```

Reopening its changes shows they are preserved

---

## Reopening initial project

