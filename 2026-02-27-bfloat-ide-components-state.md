---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: State Layer

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 16
- Source root(s): app/contexts/AppTypeContext.tsx, app/hooks/useAppType.ts, app/hooks/useFeatureAvailable.ts, app/hooks/useLocalAgent.ts, app/hooks/useSessions.ts, app/hooks/useStore.ts, app/stores/deploy.ts, app/stores/editor.ts, app/stores/files.ts, app/stores/local-projects.ts, app/stores/onboarding.ts, app/stores/project-store.ts, app/stores/project.ts, app/stores/provider-auth.ts, app/stores/theme.ts, app/stores/workbench.ts

## Inventory

### `app/contexts/AppTypeContext.tsx`
- Responsibility: React context provider/type boundary for app-wide concerns.
- Exports: `AppTypeProvider`, `useAppTypeContext`, ` AppTypeContext `
- Direct dependencies (external): `react`
- Used by: 3 file(s) — `app/components/workbench/Workbench.tsx`, `app/hooks/useAppType.ts`, `app/hooks/useFeatureAvailable.ts`

### `app/hooks/useAppType.ts`
- Responsibility: React hook for shared UI behavior/state access.
- Exports: `useAppType`, `useIsWebApp`, `useIsMobileApp`
- Direct dependencies (internal): `app/contexts/AppTypeContext.tsx`
- Used by: 2 file(s) — `app/components/common/FeatureGate.tsx`, `app/components/preview/Preview.tsx`

### `app/hooks/useFeatureAvailable.ts`
- Responsibility: React hook for shared UI behavior/state access.
- Exports: `useFeatureAvailable`
- Direct dependencies (internal): `app/contexts/AppTypeContext.tsx`
- Used by: 1 file(s) — `app/components/common/FeatureGate.tsx`

### `app/hooks/useLocalAgent.ts`
- Responsibility: React hook for shared UI behavior/state access.
- Exports: `useLocalAgent`
- Direct dependencies (internal): `app/stores/workbench.ts`, `lib/conveyor/schemas/ai-agent-schema.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/hooks/useSessions.ts`
- Responsibility: React hook for shared UI behavior/state access.
- Exports: `useSessions`, `useSaveSession`, `useDeleteSession`, `useUpdateSession`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/hooks/useStore.ts`
- Responsibility: React hook for shared UI behavior/state access.
- Exports: `useStore`
- Direct dependencies (external): `react`, `zustand/vanilla`
- Used by: 24 file(s) — `app/components/chat/Chat.tsx`, `app/components/deploy/DeployModal.tsx`, `app/components/deploy/DeployiOSSection.tsx`, `app/components/deploy/DeploymentHistory.tsx`, `app/components/deploy/EasAccountSelector.tsx`

### `app/stores/deploy.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `deployStore`
- Direct dependencies (internal): `lib/conveyor/schemas/deploy-schema.ts`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 12 file(s) — `app/components/deploy/AppleCredentialsForm.tsx`, `app/components/deploy/DeployModal.tsx`, `app/components/deploy/DeployiOSSection.tsx`, `app/components/deploy/DeploymentHistory.tsx`, `app/components/deploy/EasAccountSelector.tsx`

### `app/stores/editor.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `EditorStore`
- Direct dependencies (internal): `app/stores/files.ts`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 1 file(s) — `app/stores/workbench.ts`

### `app/stores/files.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `filesStore`, `FilesStore`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 2 file(s) — `app/stores/editor.ts`, `app/stores/workbench.ts`

### `app/stores/local-projects.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `localProjectsStore`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 3 file(s) — `app/components/home/HomePage.tsx`, `app/components/project/ProjectPage.tsx`, `app/components/project/ProjectSettings.tsx`

### `app/stores/onboarding.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `isOnboardingComplete`, `onboardingStep`, `hasAnyAIProvider`, `hasExpo`
- Direct dependencies (internal): `app/stores/provider-auth.ts`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 0 file(s)

### `app/stores/project-store.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `projectStore`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 3 file(s) — `app/components/project/ProjectPage.tsx`, `app/components/window/Titlebar.tsx`, `app/stores/workbench.ts`

### `app/stores/project.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `projectStore`, ` projectStore as workbenchStore `
- Direct dependencies (external): `zustand/vanilla`
- Used by: 0 file(s)

### `app/stores/provider-auth.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `providerAuthStore`, `ProviderAuthStore`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 13 file(s) — `app/components/chat/Chat.tsx`, `app/components/deploy/DeployModal.tsx`, `app/components/deploy/DeployiOSSection.tsx`, `app/components/deploy/IOSDeployModals.tsx`, `app/components/deploy/IOSSetupWizard.tsx`

### `app/stores/theme.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `themeStore`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 7 file(s) — `app/components/deploy/DeployTerminal.tsx`, `app/components/deploy/LogTerminal.tsx`, `app/components/editor/CodeEditor.tsx`, `app/components/home/HomePage.tsx`, `app/components/preview/IPhoneFrame.tsx`

### `app/stores/workbench.ts`
- Responsibility: Zustand store/state module for renderer app state.
- Exports: `workbenchStore`, `WorkbenchStore`
- Direct dependencies (internal): `app/stores/files.ts`, `app/stores/editor.ts`, `app/stores/project-store.ts`
- Direct dependencies (external): `zustand/vanilla`
- Used by: 16 file(s) — `app/components/chat/Chat.tsx`, `app/components/chat/Messages.tsx`, `app/components/deploy/DeployModal.tsx`, `app/components/deploy/DeployWebSection.tsx`, `app/components/deploy/DeployiOSSection.tsx`

## Related
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-27-bfloat-ide-components-ui]]