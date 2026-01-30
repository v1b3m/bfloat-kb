# Issue: Deploy Buttons Disabled - projectPath is null

## Summary

The "Publish to iOS App Store" and "Publish to Web" buttons in the Deploy modal are disabled because `workbenchStore.projectPath` is `null`.

## When It Started

After merging the `fix/develop` branch (commit `801fb6b`), though the root cause is likely in the **uncommitted changes** to workbench-related files.

## Symptoms

1. Open a project in the IDE
2. Click the Deploy button in the titlebar
3. The Deploy modal opens
4. EAS account selector dropdown is visible (Expo is connected)
5. But the "Publish to iOS App Store" button is disabled

## Debug Output

Added debug logging to `app/components/deploy/DeployiOSSection.tsx`:

```javascript
console.log('[DeployiOSSection] Button state:', {
  disabled,
  isRunning,
  projectPath,
  isCheckingCredentials,
  isExpoConnected,
  buttonDisabled: disabled || isRunning || !projectPath || isCheckingCredentials
})
```

Output shows:
```json
{
  "disabled": false,
  "isRunning": false,
  "projectPath": null,      // <-- THIS IS THE PROBLEM
  "isCheckingCredentials": false,
  "isExpoConnected": true,
  "buttonDisabled": true
}
```

## Root Cause Analysis

The button's disabled condition in `DeployiOSSection.tsx` (line ~171):

```typescript
disabled={disabled || isRunning || !projectPath || isCheckingCredentials}
```

`projectPath` comes from `workbenchStore.projectPath` (line 28):

```typescript
const projectPath = useStore(workbenchStore.projectPath)
```

### Where projectPath Should Be Set

In `app/components/workbench/Workbench.tsx`, the projectPath is set in two places:

1. **For git-based projects** (line ~574):
   ```typescript
   if (gitProjectPath) {
     workbenchStore.projectPath.set(gitProjectPath)
   }
   ```

2. **For legacy temp directory projects** (line ~602):
   ```typescript
   workbenchStore.projectPath.set(tempPath)
   ```

### Where projectPath Gets Cleared

1. **When switching projects** (line ~473):
   ```typescript
   workbenchStore.projectPath.set(null)
   ```

2. **On workbench unmount** (line ~990):
   ```typescript
   workbenchStore.projectPath.set(null)
   ```

## Potential Causes

1. **Git-based project waiting for sync**: If `isGitBasedProject && !gitProjectPath`, the setup returns early (line 440-443) and never sets projectPath

2. **Setup function not completing**: The async `setupProjectAndRunExpo` function might fail silently

3. **Race condition**: projectPath might be set and then cleared by another effect

4. **Uncommitted changes**: There are significant uncommitted changes to:
   - `app/components/workbench/Workbench.tsx`
   - `app/components/project/ProjectPage.tsx`
   - `app/components/project/ProjectContent.tsx`
   - `app/stores/workbench.ts`

## Files to Investigate

### Primary Files
- `app/components/workbench/Workbench.tsx` - Where projectPath is set
- `app/stores/workbench.ts` - The store definition
- `app/components/project/ProjectPage.tsx` - Where projectPath/gitProjectPath originates

### Secondary Files
- `app/components/project/ProjectContent.tsx` - Passes gitProjectPath to Workbench
- `app/components/deploy/DeployiOSSection.tsx` - Has debug logging
- `app/components/deploy/DeployWebSection.tsx` - Same issue

## Debugging Steps

1. Check browser console for `[Workbench]` logs:
   - `[Workbench] Auto-run useEffect triggered` - Shows initial state
   - `[Workbench] Git-based project, waiting for gitProjectPath...` - Waiting for sync
   - `[Workbench] Using git project path: <path>` - Should set projectPath
   - `[Workbench] Temp directory created: <path>` - Legacy flow

2. Check if terminal/preview is running - if Expo server started, setup completed

3. Look for any errors in console during project load

## Related Commits

- `801fb6b` - Merge PR #27 from fix/develop (when issue was noticed)
- `bcf63cc` - feat: improve iOS deployment with user-controlled prompts
- `4d4b154` - Add EAS account selector for Expo deployments
- `c2908ed` - feat: add automated iOS deployment with setup wizard

## Uncommitted Changes (at time of issue)

Run `git status` to see all uncommitted changes. Key ones:
- `M app/components/workbench/Workbench.tsx`
- `M app/components/project/ProjectPage.tsx`
- `M app/components/project/ProjectContent.tsx`
- `M app/stores/workbench.ts`
- `MM app/components/window/Titlebar.tsx`

## Quick Fix Ideas

1. **Add fallback in DeployiOSSection**: If projectPath is null, try to get it from the current project sync

2. **Check timing**: Ensure projectPath is set before Deploy modal can be opened

3. **Add loading state**: Show "Waiting for project..." instead of disabled button

4. **Debug the flow**: Add console.log in Workbench.tsx useEffect to trace when/if projectPath.set() is called

## Notes

- The EAS account dropdown IS visible, meaning Expo IS connected
- The issue is specifically `projectPath: null`
- The terminal/preview might be running, suggesting setup completed but projectPath was cleared
