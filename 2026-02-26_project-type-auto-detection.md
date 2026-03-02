We should never get into a situation like the one below:

```
Create a timer

---

I'll help you create a timer! To build exactly what you need, I'd like to clarify a few details:

1. Platform: What type of timer would you like?
    
    - Web app (React, HTML/CSS/JS)
    - Mobile app (Expo/React Native)
    - Desktop app
    - Simple command-line timer
2. Timer type: What functionality do you need?
    
    - Countdown timer (set a specific time and count down)
    - Stopwatch (count up from zero)
    - Pomodoro timer (work/break intervals)
    - Multiple timers
3. Features: Any specific requirements?
    
    - Sound notifications when timer ends
    - Visual alerts or animations
    - Ability to pause/resume
    - Save preset times
    - Background operation

Let me know your preferences and I'll create a timer app tailored to your needs!
```

The agent should be able to explore and understand what the current repo contains (expo / web). This is handled gracefully in the `bfloat-workbench`, check their and borrow what needs to be borrowed.

---



## Progress
- [x] Traced app type usage flow (`ProjectPage` -> `ProjectContent`/`Workbench`) and confirmed launch detection was not persisted to project metadata.
- [x] Added project type auto-detection during initial file sync using existing `detectAppTypeFromPackageJson`.
- [x] Persisted detected app type to local project metadata (`localProjectsStore.update`) and updated in-memory project state for immediate UI consistency.
- [x] Verified no new `ProjectPage` TypeScript errors introduced by this change.

## Decisions
- Reused existing launch detector instead of adding a new detection system to keep behavior consistent and avoid duplication.
- Performed persistence in `ProjectPage` initial sync because that is where a complete file map already exists and metadata can be updated once per project load.
- Limited auto-normalization output to `web`/`mobile` to match current feature gating and app-type context model.

- [x] Verified and ported startup gating from `bfloat-workbench`: only treat project path as usable when it matches the active project id path segment.
- [x] Updated auto-start flow to require aligned project path + ready sync state before initial agent prompt.
- [x] Updated chat/session read/submit/pending-prompt flows to use `usableProjectPath` guard, preventing agent start against stale paths.

## Decisions
- Adopted the `usableProjectPath` pattern from workbench (path normalization + project-id match) instead of relying on raw `projectPath` truthiness.
- Kept “quick scan” behavior via existing new-session system prompt; fixed sequencing so scan+first prompt can only run after workspace path alignment and readiness.

- [x] Implemented hard scaffold guard in sidecar streaming path: blocks `create-*app` commands when workspace profile indicates an existing app repo.
- [x] Added workspace profiling utility (`workspace-profile`) with framework/app-marker detection and scaffold block decision helper.
- [x] Updated new-session system prompt to require quick workspace check and forbid re-scaffolding in non-empty app workspaces.
- [x] Added tests: workspace profile classification + scaffold command block integration in agent session stream.

## Decisions
- Enforced guard at runtime (not prompt-only) so model behavior drift cannot bypass policy.
- Scoped blocking to shell-like tool calls (`Bash`/`Shell`/`Command*`) to avoid false positives on non-shell tools.
- Kept scaffold allowed only for effectively empty workspaces with no app markers.


## Outcome (2026-02-26)
Project startup now waits for aligned project path state, chat/session operations use guarded paths, and app type is auto-detected/persisted during initial sync.

Commits: 5fb8080, f667cab