# Bug: Missing createDeployTerminal After Merge

**Date:** 2026-01-22
**Status:** Fixed

## Symptoms

```
ReferenceError: createDeployTerminal is not defined
at Workbench2 (http://localhost:5173/components/workbench/Workbench.tsx:285:19)
```

The IDE crashes immediately after loading due to a missing function.

## Root Cause

When merging `chore/automate-deployments` into `develop`, the `createDeployTerminal` function from `develop`'s `Workbench.tsx` was not included in the merge result.

The function existed at line 147 in `develop`:
```typescript
const createDeployTerminal = useCallback((): string => {
  const deployTabId = 'terminal-deploy'
  setTerminalTabs(prev => {
    const existingTab = prev.find(tab => tab.id === deployTabId)
    if (existingTab) return prev
    return [...prev, { id: deployTabId, name: 'Deploy' }]
  })
  setActiveTerminalId(deployTabId)
  setIsTerminalOpen(true)
  return deployTabId
}, [])
```

But the merge lost this function while keeping the reference to it at line 421:
```typescript
workbenchStore.registerDeployTerminalFunctions({
  createDeployTerminal,  // <-- Reference kept but function definition lost
  openTerminal: () => setIsTerminalOpen(true),
})
```

## Fix

Restored the `createDeployTerminal` function to `app/components/workbench/Workbench.tsx` after the `addTerminalTab` callback (line 155-176).

**File:** `app/components/workbench/Workbench.tsx`

## Prevention

When resolving merge conflicts:
- Verify that all referenced functions are defined
- Check for "uses" of functions that may have been on deleted lines
- Run TypeScript/type check after merge to catch undefined references
