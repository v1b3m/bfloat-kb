Another black screen on exiting the project page:

**Console logs:**

```
[Log] [Chat] pendingPrompt effect fired – {hasPendingPrompt: false, isStreaming: false, hasProjectPath: true, …} (Chat.tsx, line 1027)
{hasPendingPrompt: false, isStreaming: false, hasProjectPath: true, pendingPrompt: undefined}Object
[Log] [Chat] Not sending pending prompt, conditions: – {hasPrompt: false, isStreaming: false, hasProjectPath: true} (Chat.tsx, line 1054)
[Log] [useLocalAgent] Unmounting - detaching from session: – null (useLocalAgent.ts, line 212)
[Log] [Workbench] Running cleanup after 175982ms (Workbench.tsx, line 640)
[Log] [Terminal UI] Killing terminal: terminal-1 (Terminal.tsx, line 193)
[Log] [Terminal UI] Disposing xterm UI for: terminal-1 (Terminal.tsx, line 140)
[Log] [ProjectPage] ===== UNMOUNTING (real) ===== (ProjectPage.tsx, line 122)
[Log] [WorkbenchStore] Skipping commit — no git repository (workbench.ts, line 325)
[Log] [ProjectPage] Closing project on unmount (ProjectPage.tsx, line 124)
[Log] [ProjectStore] Closing project: proj_1772094249569_rerq00na2 (project-store.ts, line 222)
[Log] [ProjectPage] Killing all terminals and agent sessions (ProjectPage.tsx, line 127)
[Log] [ProjectPage] Resetting workbench state on unmount (ProjectPage.tsx, line 142)
[Log] [WorkbenchStore] ===== reset COMPLETE ===== (workbench.ts, line 494)
[Log] [ProjectPage] ===== END UNMOUNT ===== (ProjectPage.tsx, line 144)
```

