# Merge Conflict Report: chore/automate-deployments → develop

**Date:** 2026-01-22
**Branch:** chore/automate-deployments (31 commits ahead of develop)

## Summary

Merging `chore/automate-deployments` into `develop` will require resolving conflicts in files that were modified in both branches. The develop branch contains iOS deployment automation features, while the current branch contains cleanup work (dead code removal), chat improvements, and GitHub import features.

---

## Conflicting Files (Modified in Both Branches)

| File | develop Changes | chore/automate-deployments Changes | Resolution Strategy |
|------|----------------|-----------------------------------|---------------------|
| `app/app.tsx` | Entry point changes | Routing/launch changes | Manual merge - preserve both |
| `app/components/project/ProjectPage.tsx` | Added deploy modal integration | Removed `createMinimalExpoTemplate`, added error screen for legacy projects | Manual merge |
| `app/components/workbench/Workbench.tsx` | UI updates | Cleaned up/removed dead code | Manual merge |
| `app/stores/workbench.ts` | Store updates | Store refactoring | Manual merge |
| `lib/conveyor/api/index.ts` | Added deploy/filesystem APIs | Added github-import API | Keep both, merge exports |
| `lib/main/app.ts` | IPC handler registration | IPC handler registration | Merge both registrations |
| `package.json` | Added deploy dependencies | Added agent SDKs, removed unused deps | Merge both dependency sets |
| `package-lock.json` | Dependency lock file | Dependency lock file | Use `npm install` after merge |

---

## Files Added in develop (Not in Current Branch)

### iOS Deployment Components
- `app/components/deploy/AppleAuthStep.tsx`
- `app/components/deploy/DeployModal.tsx`
- `app/components/deploy/DeployProgressModal.tsx`
- `app/components/deploy/DeployWebSection.tsx`
- `app/components/deploy/DeployiOSSection.tsx`
- `app/components/deploy/EasAccountSelector.tsx`
- `app/components/deploy/EmbeddedTerminal.tsx`
- `app/components/deploy/IOSDeployModals.tsx`
- `app/components/deploy/IOSSetupWizard.tsx`
- `app/components/deploy/LogTerminal.tsx`
- `app/components/deploy/TwoFactorStep.tsx`

### iOS Deployment Handlers & Utilities
- `lib/conveyor/handlers/apple-session-manager.ts`
- `lib/conveyor/handlers/deploy-handler.ts`
- `lib/conveyor/handlers/filesystem-handler.ts`
- `lib/conveyor/handlers/prompt-classifier.ts`
- `lib/conveyor/handlers/prompt-humanizer.ts`
- `lib/conveyor/handlers/pty-state-machine.ts`
- `app/utils/background-deploy.ts`
- `app/utils/eas-accounts.ts`
- `app/utils/eas-output-parser.ts`
- `app/utils/ios-credentials.ts`

### Schemas & APIs
- `lib/conveyor/schemas/deploy-schema.ts`
- `lib/conveyor/schemas/filesystem-schema.ts`
- `lib/conveyor/api/deploy-api.ts`
- `lib/conveyor/api/filesystem-api.ts`

### Store
- `app/stores/deploy.ts`

### UI Components
- `app/components/ui/dialog.tsx`

---

## Files Deleted in Current Branch (Dead Code Removal)

### Removed Components
- `app/components/ai-elements/conversation.tsx`
- `app/components/ai-elements/message.tsx`
- `app/components/billing/BillingPlans.tsx`
- `app/components/billing/CreditsBalance.tsx`
- `app/components/billing/UsageStatistics.tsx`
- `app/components/chat/ChatWithUseChat.tsx`
- `app/components/chat/LocalProviderSelector.tsx`
- `app/components/chat/PlannedMessage.tsx`
- `app/components/chat/ProgressAnnotations.tsx`
- `app/components/chat/ProviderSelector.tsx`
- `app/components/chat/SuggestedFollowups.tsx`
- `app/components/landing/LandingPage.tsx`
- `app/components/landing/styles.css`
- `app/components/library/AppLibrary.tsx`
- `app/components/library/DeleteConfirmDialog.tsx`
- `app/components/library/index.ts`
- `app/components/preview/ExpoQRCode.tsx`
- `app/components/settings/AIProviderSettings.tsx`
- `app/components/ui/button-group.tsx`
- `app/components/ui/separator.tsx`
- `app/test/messages-test.tsx`

---

## Files Added in Current Branch (New Features)

### Chat Improvements
- `app/components/chat/ToolAccordion.tsx` - Collapsible tool call display

### GitHub Import
- `app/components/home/GitHubImportDialog.tsx`
- `lib/conveyor/api/github-import-api.ts`
- `lib/conveyor/handlers/github-import-handler.ts`

### Types
- `app/types/launch.ts`

---

## Manual Merge Steps

1. **Create a merge branch**:
   ```bash
   git checkout -b merge-automate-deployments develop
   git merge chore/automate-deployments
   ```

2. **Resolve conflicts in key files**:
   - `app/app.tsx` - merge routing entries
   - `app/components/project/ProjectPage.tsx` - keep deploy modal integration + legacy error screen
   - `lib/conveyor/api/index.ts` - combine API exports
   - `lib/main/app.ts` - merge IPC handlers
   - `package.json` - merge dependencies

3. **Accept all current branch deletions** (dead code removal is intentional)

4. **Test the merge** - verify:
   - iOS deploy modal opens
   - Chat tool accordion works
   - GitHub import works
   - Session loading works

5. **Commit and push**:
   ```bash
   git add .
   git commit -m "Merge chore/automate-deployments into develop"
   git push origin merge-automate-deployments
   ```

---

## Risk Assessment

| Risk | Level | Mitigation |
|------|-------|------------|
| Breaking iOS deploy | Low | Keep all develop branch deploy code |
| Breaking chat improvements | Low | Keep ToolAccordion and session fixes |
| Breaking GitHub import | Low | Keep all import handler code |
| Dead code revival | Medium | Ensure deleted components stay deleted |

---

## Recommended Resolution Order

1. `package.json` - Merge dependencies first (easiest)
2. `lib/conveyor/api/index.ts` - Simple API export merge
3. `lib/main/app.ts` - IPC handler registration
4. `app/components/project/ProjectPage.tsx` - Most complex, requires understanding both features
5. `app/app.tsx` - Entry point routing
6. `app/stores/workbench.ts` - Store state merge
7. `app/components/workbench/Workbench.tsx` - UI component merge

---

## After Merge

Once merged, delete the feature branch:
```bash
git branch -d chore/automate-deployments
git push origin --delete chore/automate-deployments
```

---
