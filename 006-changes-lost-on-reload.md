When I reopen a session, the changes that were made to files are all lost. Changes are supposed to be committed to the repo if this is where files are loaded from.

---

I've noticed something peculiar, for the last opened project, changes are preserved, however, this is not done for older projects. They revert back to the original template. This should not happen.

Wait, after I opened an older project, the current project too resets to the default template. So the changes are lost.

Are we writing projects to the same path? Does this reset when project Id changes? This is a peculiar bug? Let's first debug what's going on

---
1. [[test-000]]
2. [[test-001]]
3. [[test-002]]
4. 

---

## PR details

### Title
Fix: Auto-save files when switching projects

### Description
When editing a file in a git-based project and switching to another project without manually saving (Cmd+S), the changes were lost. This fix ensures files are auto-saved and committed to git when switching projects.

### Root Cause
1. **Workbench cleanup deleting git directories** - The `Workbench.tsx` cleanup effect was calling `cleanupTempDir()` for ALL projects, including git-based projects. This deleted the entire git repository directory before saves could complete.

2. **No auto-save on project unmount** - Files were only saved when manually pressing Cmd+S, not when switching projects.

### Changes
1. **`app/stores/workbench.ts`** - Added `saveAllAndCommit()` method
   - Saves all unsaved files to disk via `saveAllFiles()`
   - Commits changes to git via `projectStore.commitAndPush()`
   - Gracefully handles errors (local saves still succeed even if commit fails)

2. **`app/components/project/ProjectPage.tsx`** - Updated cleanup effect
   - Calls `saveAllAndCommit()` before closing
   - Properly chains promises: save → commit → close → reset

3. **`app/components/workbench/Workbench.tsx`** - Fixed cleanup logic
   - Added check to skip `cleanupTempDir()` for git-based projects
   - Git project directories are managed by `projectStore` and should NOT be deleted

### Verification
1. Edit a file in a git-based project
2. Do NOT press Cmd+S
3. Switch to another project
4. Switch back
5. Changes are preserved (saved to disk and committed to git)
