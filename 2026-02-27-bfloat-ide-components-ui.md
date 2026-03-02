
---
type: architecture-map
project: bfloat-ide
created: 2026-02-27
scope: full-system
---

# Bfloat IDE Components: UI

Linked from: [[2026-02-27-bfloat-ide-component-map]]

## Scope
- Total files: 134
- Source root(s): app/components/ai-elements, app/components/chat, app/components/common, app/components/deploy, app/components/editor, app/components/ErrorBoundary.tsx, app/components/home, app/components/integrations, app/components/library, app/components/onboarding, app/components/payments, app/components/preview, app/components/project, app/components/settings, app/components/terminal, app/components/ui, app/components/window, app/components/workbench

## Inventory

### `app/components/ai-elements/attachments.tsx`
- Responsibility: UI component/module for ai-elements feature.
- Exports: `getMediaCategory`, `getAttachmentLabel`, `useAttachmentsContext`, `useAttachmentContext`, `Attachments`, `Attachment`, `AttachmentPreview`, `AttachmentInfo`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `lib/utils.ts`
- Direct dependencies (external): `ai`, `react`
- Used by: 0 file(s)

### `app/components/ai-elements/index.ts`
- Responsibility: UI component/module for ai-elements feature.
- Exports: `
  WebPreview,
  WebPreviewNavigation,
  WebPreviewNavigationButton,
  WebPreviewUrl,
  WebPreviewBody,
  WebPreviewConsole,
  type WebPreviewProps,
  type WebPreviewNavigationProps,
  type WebPreviewNavigationButtonProps,
  type WebPreviewUrlProps,
  type WebPreviewBodyProps,
  type WebPreviewConsoleProps,
  type WebPreviewContextValue,
`
- Used by: 0 file(s)

### `app/components/ai-elements/web-preview.tsx`
- Responsibility: UI component/module for ai-elements feature.
- Exports: `WebPreview`, `WebPreviewNavigation`, `WebPreviewNavigationButton`, `WebPreviewUrl`, `WebPreviewBody`, `WebPreviewConsole`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/input.tsx`, `lib/utils.ts`
- Direct dependencies (external): `lucide-react`, `react`
- Used by: 1 file(s) — `app/components/preview/Preview.tsx`

### `app/components/chat/AskUserQuestion.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `AskUserQuestion`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/AssistantMessage.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `AssistantMessage`
- Direct dependencies (internal): `app/components/ui/shimmer.tsx`, `app/components/chat/Markdown.tsx`, `app/components/chat/ToolAccordion.tsx`, `app/components/chat/AskUserQuestion.tsx`, `app/components/chat/ConvexSetupBanner.tsx`, `app/components/chat/FirebaseSetupBanner.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/Messages.tsx`

### `app/components/chat/Chat.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `Chat`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/workbench.ts`, `app/hooks/useLocalAgent.ts`, `app/hooks/useSessions.ts`, `lib/launch/system-prompt.ts`, `lib/conveyor/schemas/ai-agent-schema.ts`
- Direct dependencies (external): `react`, `ai`, `react-hot-toast`
- Used by: 1 file(s) — `app/components/project/ProjectContent.tsx`

### `app/components/chat/ChatInput.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ChatInput`
- Direct dependencies (external): `react`, `lucide-react`, `framer-motion`, `ai`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/ClaudeAuthBanner.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ClaudeAuthBanner`, `isClaudeAuthError`
- Direct dependencies (external): `lucide-react`
- Used by: 2 file(s) — `app/components/chat/AssistantMessage.tsx`, `app/components/chat/Chat.tsx`

### `app/components/chat/ConvexSetupBanner.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ConvexSetupBanner`
- Direct dependencies (internal): `app/components/ui/icons/convex-logo.tsx`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/ErrorMessage.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ErrorMessage`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/FirebaseSetupBanner.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `FirebaseSetupBanner`
- Direct dependencies (internal): `app/components/ui/icons/firebase-logo.tsx`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/generateSuggestions.ts`
- Responsibility: UI component/module for chat feature.
- Exports: `generateSuggestions`
- Direct dependencies (internal): `app/components/chat/types.ts`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/index.ts`
- Responsibility: UI component/module for chat feature.
- Exports: ` Chat `, ` Messages `, ` ChatInput `, ` TaskStatus `, ` TaskSection, TaskItem, TaskText `, ` ToolPill, ToolPillGroup `, ` AssistantMessage `
- Used by: 1 file(s) — `app/components/home/HomePage.tsx`

### `app/components/chat/Markdown.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `Markdown`
- Direct dependencies (external): `streamdown`, `@streamdown/code`, `react`
- Used by: 2 file(s) — `app/components/chat/AssistantMessage.tsx`, `app/components/chat/UserMessage.tsx`

### `app/components/chat/Messages.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `Messages`
- Direct dependencies (internal): `app/stores/workbench.ts`, `app/components/chat/AssistantMessage.tsx`, `app/components/chat/UserMessage.tsx`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/RevenueCatSe
tupBanner.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `RevenueCatSetupBanner`
- Direct dependencies (internal): `app/components/ui/icons/revenuecat-logo.tsx`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/SessionTabs.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `SessionTabs`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/StripeSetupBanner.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `StripeSetupBanner`
- Direct dependencies (internal): `app/components/ui/icons/stripe-logo.tsx`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/styles.css`
- Responsibility: UI component/module for chat feature.
- Used by: 0 file(s)

### `app/components/chat/SuggestionChips.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `SuggestionChips`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `lib/utils.ts`, `app/components/chat/types.ts`
- Direct dependencies (external): `react`, `framer-motion`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/TaskProgress.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `TaskProgress`
- Direct dependencies (external): `react`, `lucide-react`, `framer-motion`
- Used by: 1 file(s) — `app/components/chat/Chat.tsx`

### `app/components/chat/TaskSection.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `TaskSection`, `TaskSectionList`, `TaskItem`, `TaskText`
- Direct dependencies (internal): `app/components/chat/types.ts`, `app/components/chat/ToolPill.tsx`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 0 file(s)

### `app/components/chat/TaskStatus.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `TaskStatus`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 0 file(s)

### `app/components/chat/ToolAccordion.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ToolAccordion`
- Direct dependencies (internal): `app/components/chat/ToolPill.tsx`, `app/components/chat/types.ts`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/chat/ToolPill.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `ToolPill`, `ToolPillGroup`, `ToolPillCompact`
- Direct dependencies (internal): `app/components/chat/types.ts`
- Direct dependencies (external): `react`, `framer-motion`
- Used by: 2 file(s) — `app/components/chat/TaskSection.tsx`, `app/components/chat/ToolAccordion.tsx`

### `app/components/chat/types.ts`
- Responsibility: UI component/module for chat feature.
- Exports: `generateTaskSummary`, `generateTaskTitle`, `getTaskStatus`, `convertToolPartToAction`, `TOOL_ACTION_INFO`
- Used by: 7 file(s) — `app/components/chat/AssistantMessage.tsx`, `app/components/chat/SuggestionChips.tsx`, `app/components/chat/TaskSection.tsx`, `app/components/chat/ToolAccordion.tsx`, `app/components/chat/ToolPill.tsx`

### `app/components/chat/UserMessage.tsx`
- Responsibility: UI component/module for chat feature.
- Exports: `UserMessage`
- Direct dependencies (internal): `app/components/chat/Markdown.tsx`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/chat/Messages.tsx`

### `app/components/common/FeatureGate.tsx`
- Responsibility: UI component/module for common feature.
- Exports: `FeatureGate`, `WebOnly`, `MobileOnly`
- Direct dependencies (internal): `app/hooks/useFeatureAvailable.ts`, `app/hooks/useAppType.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/project/ProjectSettings.tsx`

### `app/components/deploy/AppleAuthStep.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `AppleAuthStep`
- Direct dependencies (internal): `lib/conveyor/schemas/deploy-schema.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/deploy/IOSSetupWizard.tsx`

### `app/components/deploy/AppleCredentialsForm.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `AppleCredentialsForm`
- Direct dependencies (internal): `app/stores/deploy.ts`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/deploy/DeployAndroidSection.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployAndroidSection`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/deploy/DeployModal.tsx`

### `app/components/deploy/DeployiOSSection.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployiOSSection`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`, `app/stores/workbench.ts`, `app/stores/provider-auth.ts`, `app/components/deploy/EasAccountSelector.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/deploy/DeployModal.tsx`

### `app/components/deploy/DeploymentHistory.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeploymentHistory`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`
- Direct dependencies (external): `lucide-react`
- Used by: 0 file(s)

### `app/components/deploy/DeployModal.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployModal`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`, `app/stores/provider-auth.ts`, `app/stores/workbench.ts`, `app/components/deploy/DeployAndroidSection.tsx`, `app/components/deploy/DeployiOSSection.ts
x`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/window/Titlebar.tsx`

### `app/components/deploy/DeployProgressModal.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployProgressModal`
- Direct dependencies (internal): `app/components/ui/dialog.tsx`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/deploy/IOSDeployModals.tsx`

### `app/components/deploy/DeployTerminal.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployTerminal`, `killDeployTerminal`
- Direct dependencies (internal): `app/stores/theme.ts`, `app/components/terminal/terminal-theme.ts`
- Direct dependencies (external): `react`, `@xterm/xterm`, `@xterm/addon-fit`, `@xterm/xterm/css/xterm.css`
- Used by: 0 file(s)

### `app/components/deploy/DeployWebSection.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `DeployWebSection`
- Direct dependencies (internal): `app/stores/workbench.ts`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/deploy/DeployModal.tsx`

### `app/components/deploy/EasAccountSelector.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `EasAccountSelector`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/deploy/DeployiOSSection.tsx`

### `app/components/deploy/EmbeddedTerminal.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `EmbeddedTerminal`, `InteractiveTerminal`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/deploy/index.ts`
- Responsibility: UI component/module for deploy feature.
- Exports: ` DeployModal `, ` DeployWebSection `, ` DeployAndroidSection `, ` DeployiOSSection `, ` DeployTerminal, killDeployTerminal `, ` DeploymentHistory `
- Used by: 0 file(s)

### `app/components/deploy/IOSDeployModals.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `IOSDeployModals`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`, `app/stores/workbench.ts`, `app/stores/provider-auth.ts`, `app/components/deploy/DeployProgressModal.tsx`, `app/components/deploy/IOSSetupWizard.tsx`
- Direct dependencies (external): `react`
- Used by: 0 file(s)

### `app/components/deploy/IOSSetupWizard.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `IOSSetupWizard`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/components/ui/dialog.tsx`, `app/stores/provider-auth.ts`, `app/stores/deploy.ts`, `app/stores/workbench.ts`, `app/components/deploy/LogTerminal.tsx`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/deploy/IOSDeployModals.tsx`

### `app/components/deploy/LogTerminal.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `LogTerminal`
- Direct dependencies (internal): `app/stores/theme.ts`, `app/components/terminal/terminal-theme.ts`
- Direct dependencies (external): `react`, `@xterm/xterm`, `@xterm/addon-fit`, `@xterm/xterm/css/xterm.css`
- Used by: 2 file(s) — `app/components/deploy/IOSSetupWizard.tsx`, `app/components/deploy/TerminalFallbackStep.tsx`

### `app/components/deploy/ProdEnvVarsSection.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `ProdEnvVarsSection`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/deploy/DeployModal.tsx`

### `app/components/deploy/TerminalFallbackStep.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `TerminalFallbackStep`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/components/deploy/LogTerminal.tsx`, `app/stores/deploy.ts`, `lib/conveyor/schemas/deploy-schema.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/deploy/IOSSetupWizard.tsx`

### `app/components/deploy/TwoFactorStep.tsx`
- Responsibility: UI component/module for deploy feature.
- Exports: `TwoFactorStep`
- Direct dependencies (external): `react`
- Used by: 2 file(s) — `app/components/deploy/IOSDeployModals.tsx`, `app/components/deploy/IOSSetupWizard.tsx`

### `app/components/editor/CodeEditor.tsx`
- Responsibility: UI component/module for editor feature.
- Exports: `CodeEditor`
- Direct dependencies (internal): `app/stores/theme.ts`
- Direct dependencies (external): `react`, `@codemirror/state`, `@codemirror/view`, `@codemirror/commands`
- Used by: 1 file(s) — `app/components/editor/EditorPanel.tsx`

### `app/components/editor/EditorPanel.tsx`
- Responsibility: UI component/module for editor feature.
- Exports: `EditorPanel`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/workbench.ts`, `app/components/editor/FileTree.tsx`, `app/components/editor/CodeEditor.tsx`, `app/components/editor/FileIcons.tsx`
- Direct dependencies (external): `react`, `react-resizable-panels`, `lucide-react`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/editor/FileIcons.tsx`
- Responsibility: UI component/module for editor feature.
- Exports: `getFileIcon`, `FileIcon`
- Direct dependencies (external): `lucide-react`
- Used by: 2 file(s) — `app/components/editor/EditorPanel.tsx`, `app/components/editor/FileTree.tsx`

### `app/components/editor/FileTree.tsx`
- Responsibility: UI component/module for editor feature.
- Exports: `FileTree`
- Direct dependencies (internal): `app/components/editor/FileIcons.tsx`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`
- Used by: 1 file(s) — `app/components/editor/EditorPanel.tsx`

### `app/components/editor/index.ts`
- Responsibility: UI component/module for editor feature.
- Exports: ` FileTree `, ` CodeEditor `, ` EditorPanel `, ` FileIcon, getFileIcon `
- Used by: 0 file(s)

### `app/comp
onents/editor/styles.css`
- Responsibility: UI component/module for editor feature.
- Used by: 0 file(s)

### `app/components/ErrorBoundary.tsx`
- Responsibility: UI component/module for ErrorBoundary.tsx feature.
- Exports: `ErrorBoundary`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/badge.tsx`
- Direct dependencies (external): `react`
- Used by: 0 file(s)

### `app/components/home/home-sidebar.css`
- Responsibility: UI component/module for home feature.
- Used by: 0 file(s)

### `app/components/home/HomePage.tsx`
- Responsibility: UI component/module for home feature.
- Exports: `HomePage`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/dialog.tsx`, `app/hooks/useStore.ts`, `app/stores/local-projects.ts`, `lib/conveyor/schemas/ai-agent-schema.ts`, `app/components/chat/index.ts`
- Direct dependencies (external): `react`, `react-router-dom`, `framer-motion`
- Used by: 0 file(s)

### `app/components/home/HomeRightPanel.tsx`
- Responsibility: UI component/module for home feature.
- Exports: `HomeRightPanel`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/home/HomePage.tsx`

### `app/components/home/HomeSidebar.tsx`
- Responsibility: UI component/module for home feature.
- Exports: `HomeSidebar`
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `react`, `react-router-dom`
- Used by: 1 file(s) — `app/components/home/HomePage.tsx`

### `app/components/home/index.ts`
- Responsibility: UI component/module for home feature.
- Exports: ` default as HomePage `
- Used by: 0 file(s)

### `app/components/integrations/AnthropicIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `AnthropicIntegration`
- Direct dependencies (internal): `app/components/ui/icons/claude-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/integrations/AppStoreIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `AppStoreIntegration`
- Direct dependencies (internal): `app/components/ui/icons/app-store-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`, `react-hot-toast`
- Used by: 0 file(s)

### `app/components/integrations/ConvexIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `ConvexIntegration`
- Direct dependencies (internal): `app/components/ui/icons/convex-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/integrations/ExpoIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `ExpoIntegration`
- Direct dependencies (internal): `app/components/ui/icons/expo-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`, `react-hot-toast`
- Used by: 0 file(s)

### `app/components/integrations/FirebaseIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `FirebaseIntegration`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/integrations/IntegrationCard.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `IntegrationCard`
- Direct dependencies (external): `react`, `lucide-react`, `framer-motion`
- Used by: 1 file(s) — `app/components/integrations/IntegrationsGrid.tsx`

### `app/components/integrations/IntegrationsGrid.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `IntegrationsGrid`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/components/integrations/IntegrationCard.tsx`, `app/components/integrations/ProviderAuthModal.tsx`, `app/stores/provider-auth.ts`, `app/components/ui/icons/claude-logo.tsx`, `app/components/ui/icons/openai-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/library/SettingsModal.tsx`

### `app/components/integrations/OpenAIIntegration.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `OpenAIIntegration`
- Direct dependencies (internal): `app/components/ui/icons/openai-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/integrations/ProviderAuthModal.tsx`
- Responsibility: UI component/module for integrations feature.
- Exports: `ProviderAuthModal`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/dialog.tsx`, `app/stores/provider-auth.ts`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 4 file(s) — `app/components/chat/Chat.tsx`, `app/components/integrations/IntegrationsGrid.tsx`, `app/components/onboarding/steps/AIProviderStep.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/library/settings-modal.css`
- Responsibility: UI component/module for library feature.
- Used by: 0 file(s)

### `app/components/library/SettingsModal.tsx`
- Responsibility: UI component/module for library feature.
- Exports: `SettingsModal`
- Direct dependencies (internal): `app/components/integrations/IntegrationsGrid.tsx`
- Direct dependencies (external): `react`, `framer-motion`, `react-hot-toast`
- Used by: 0 file(s)

### `app/components/onboarding/index.ts`
- Responsibility: UI component/module for onboarding feature.
- Exports: ` OnboardingWizard `
- Used by: 0 file(s)

### `app/components/onboarding/OnboardingWizard.tsx`
- Responsibility: UI component/module for onboarding feature.
- Exports: `OnboardingWizard`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/provider-auth.ts`, `app/components/onboarding/steps/WelcomeStep.tsx`, `app/components/onboarding/steps/AIProviderStep.tsx`, `app/components/onboarding/steps/SuccessStep.tsx`
- Direct dependencies (external): `react`, `framer-motion`
- Used by: 0 file
(s)

### `app/components/onboarding/steps/AIProviderStep.tsx`
- Responsibility: UI component/module for onboarding feature.
- Exports: `AIProviderStep`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/provider-auth.ts`, `app/components/ui/icons/claude-logo.tsx`, `app/components/ui/icons/openai-logo.tsx`, `app/components/integrations/ProviderAuthModal.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/onboarding/OnboardingWizard.tsx`

### `app/components/onboarding/steps/ExpoStep.tsx`
- Responsibility: UI component/module for onboarding feature.
- Exports: `ExpoStep`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/provider-auth.ts`, `app/components/ui/icons/expo-logo.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/onboarding/steps/SuccessStep.tsx`
- Responsibility: UI component/module for onboarding feature.
- Exports: `SuccessStep`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/provider-auth.ts`, `app/components/ui/icons/claude-logo.tsx`, `app/components/ui/icons/openai-logo.tsx`, `app/components/ui/icons/expo-logo.tsx`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/onboarding/OnboardingWizard.tsx`

### `app/components/onboarding/steps/WelcomeStep.tsx`
- Responsibility: UI component/module for onboarding feature.
- Exports: `WelcomeStep`
- Direct dependencies (external): `lucide-react`
- Used by: 1 file(s) — `app/components/onboarding/OnboardingWizard.tsx`

### `app/components/payments/PaymentsOverview.tsx`
- Responsibility: UI component/module for payments feature.
- Exports: `PaymentsOverview`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/preview/index.ts`
- Responsibility: UI component/module for preview feature.
- Exports: ` Preview `
- Used by: 0 file(s)

### `app/components/preview/IPhoneFrame.tsx`
- Responsibility: UI component/module for preview feature.
- Exports: `IPhoneFrame`
- Direct dependencies (internal): `lib/utils.ts`, `app/stores/theme.ts`, `app/hooks/useStore.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/preview/Preview.tsx`

### `app/components/preview/Preview.tsx`
- Responsibility: UI component/module for preview feature.
- Exports: `Preview`
- Direct dependencies (internal): `app/hooks/useAppType.ts`, `app/components/preview/IPhoneFrame.tsx`, `app/components/ui/button.tsx`, `app/components/ui/tooltip.tsx`, `app/components/ai-elements/web-preview.tsx`, `app/stores/workbench.ts`
- Direct dependencies (external): `react`, `qrcode.react`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/preview/styles.css`
- Responsibility: UI component/module for preview feature.
- Used by: 0 file(s)

### `app/components/project/ConvexDashboard.tsx`
- Responsibility: UI component/module for project feature.
- Exports: `ConvexDashboard`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/project/index.ts`
- Responsibility: UI component/module for project feature.
- Exports: ` default as ProjectPage `, ` ProjectContent `
- Used by: 0 file(s)

### `app/components/project/ProjectContent.tsx`
- Responsibility: UI component/module for project feature.
- Exports: `ProjectContent`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `lib/conveyor/schemas/ai-agent-schema.ts`, `app/components/chat/Chat.tsx`, `app/components/workbench/Workbench.tsx`, `app/stores/workbench.ts`, `app/stores/deploy.ts`
- Direct dependencies (external): `react`, `react-resizable-panels`, `ai`
- Used by: 1 file(s) — `app/components/project/ProjectPage.tsx`

### `app/components/project/ProjectPage.tsx`
- Responsibility: UI component/module for project feature.
- Exports: `ProjectPage`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/workbench.ts`, `app/stores/project-store.ts`, `app/stores/local-projects.ts`, `lib/conveyor/schemas/ai-agent-schema.ts`, `lib/launch/index.ts`
- Direct dependencies (external): `react`, `react-router-dom`, `lucide-react`, `framer-motion`
- Used by: 0 file(s)

### `app/components/project/ProjectSettings.tsx`
- Responsibility: UI component/module for project feature.
- Exports: `ProjectSettings`
- Direct dependencies (internal): `app/components/common/FeatureGate.tsx`, `app/components/ui/input.tsx`, `app/components/ui/button.tsx`, `app/components/ui/card.tsx`, `app/components/ui/dialog.tsx`, `app/components/ui/image-dropzone.tsx`
- Direct dependencies (external): `react`, `lucide-react`, `react-hot-toast`
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/project/styles.css`
- Responsibility: UI component/module for project feature.
- Used by: 0 file(s)

### `app/components/settings/components.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `SettingsCard`, `SettingsRow`, `SettingsFormGroup`, `SettingsSelect`, `SettingsInput`
- Direct dependencies (internal): `lib/utils.ts`
- Used by: 3 file(s) — `app/components/settings/sections/AppearanceSection.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`, `app/components/settings/sections/PreferencesSection.tsx`

### `app/components/settings/index.ts`
- Responsibility: UI component/module for settings feature.
- Exports: ` SettingsPage `
- Used by: 0 file(s)

### `app/components/settings/sections/AppearanceSection.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `AppearanceSection`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/components/ui/Switch.tsx`, `app/components/settings/components.tsx`, `app/stores/theme.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/settings/SettingsPage.tsx`

### `app/components/settings/sections/Con
nectedAccountsSection.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `ConnectedAccountsSection`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `lib/utils.ts`, `app/components/ui/button.tsx`, `app/components/integrations/ProviderAuthModal.tsx`, `app/stores/provider-auth.ts`, `app/components/settings/components.tsx`
- Direct dependencies (external): `react`, `lucide-react`, `framer-motion`
- Used by: 1 file(s) — `app/components/settings/SettingsPage.tsx`

### `app/components/settings/sections/IntegrationCredentialsModal.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `IntegrationCredentialsModal`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/input.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 0 file(s)

### `app/components/settings/sections/PreferencesSection.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `PreferencesSection`
- Direct dependencies (internal): `app/components/ui/Switch.tsx`, `app/components/settings/components.tsx`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/settings/SettingsPage.tsx`

### `app/components/settings/sections/SecretModal.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `SecretModal`
- Direct dependencies (internal): `app/components/ui/button.tsx`, `app/components/ui/input.tsx`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 1 file(s) — `app/components/project/ProjectSettings.tsx`

### `app/components/settings/settings.css`
- Responsibility: UI component/module for settings feature.
- Used by: 0 file(s)

### `app/components/settings/SettingsPage.tsx`
- Responsibility: UI component/module for settings feature.
- Exports: `SettingsPage`
- Direct dependencies (internal): `lib/utils.ts`, `app/components/settings/sections/PreferencesSection.tsx`, `app/components/settings/sections/AppearanceSection.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`
- Direct dependencies (external): `react`, `react-router-dom`
- Used by: 0 file(s)

### `app/components/terminal/index.ts`
- Responsibility: UI component/module for terminal feature.
- Exports: ` Terminal, killTerminal `
- Used by: 1 file(s) — `app/components/workbench/Workbench.tsx`

### `app/components/terminal/styles.css`
- Responsibility: UI component/module for terminal feature.
- Used by: 0 file(s)

### `app/components/terminal/terminal-theme.ts`
- Responsibility: UI component/module for terminal feature.
- Exports: `getTerminalTheme`, `getDeployTerminalTheme`, `getLogTerminalTheme`, `darkTerminalTheme`, `lightTerminalTheme`, `darkDeployTerminalTheme`, `lightDeployTerminalTheme`, `darkLogTerminalTheme`
- Direct dependencies (external): `@xterm/xterm`
- Used by: 3 file(s) — `app/components/deploy/DeployTerminal.tsx`, `app/components/deploy/LogTerminal.tsx`, `app/components/terminal/Terminal.tsx`

### `app/components/terminal/Terminal.tsx`
- Responsibility: UI component/module for terminal feature.
- Exports: `Terminal`, `killTerminal`
- Direct dependencies (internal): `app/stores/theme.ts`, `app/components/terminal/terminal-theme.ts`
- Direct dependencies (external): `react`, `@xterm/xterm`, `@xterm/addon-fit`, `@xterm/xterm/css/xterm.css`
- Used by: 0 file(s)

### `app/components/ui/badge.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Badge `
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/ErrorBoundary.tsx`

### `app/components/ui/button.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Button `
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `react`
- Used by: 13 file(s) — `app/components/ErrorBoundary.tsx`, `app/components/ai-elements/attachments.tsx`, `app/components/ai-elements/web-preview.tsx`, `app/components/chat/SuggestionChips.tsx`, `app/components/home/HomePage.tsx`

### `app/components/ui/card.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `Card`, `CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, `CardFooter`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/project/ProjectSettings.tsx`

### `app/components/ui/collapsible.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Collapsible, CollapsibleTrigger, CollapsibleContent `
- Direct dependencies (external): `@radix-ui/react-collapsible`
- Used by: 0 file(s)

### `app/components/ui/CommandPalette.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `CommandPalette`
- Direct dependencies (external): `react`, `react-router-dom`, `framer-motion`
- Used by: 0 file(s)

### `app/components/ui/DeploymentNotification.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `DeploymentNotification`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/deploy.ts`
- Direct dependencies (external): `react`, `react-dom`, `framer-motion`, `lucide-react`
- Used by: 0 file(s)

### `app/components/ui/dialog.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `Dialog`, `DialogContent`, `DialogHeader`, `DialogTitle`, `DialogDescription`, `DialogClose`, `DialogFooter`
- Direct dependencies (external): `react`, `lucide-react`
- Used by: 6 file(s) — `app/components/deploy/DeployProgressModal.tsx`, `app/components/deploy/IOSDeployModals.tsx`, `app/components/deploy/IOSSetupWizard.tsx`, `app/components/home/HomePage.tsx`, `app/components/integrations/ProviderAuthModal.tsx`

### `app/components/ui/dropdown-menu.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `
  DropdownMenu,
  DropdownMenuPortal,
  DropdownMenuTrigger,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuLabel,
  DropdownMenuItem,
  DropdownMenuCheckboxItem,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuSeparator,
  Dropdown
MenuShortcut,
  DropdownMenuSub,
  DropdownMenuSubTrigger,
  DropdownMenuSubContent,
`
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `react`, `@radix-ui/react-dropdown-menu`, `lucide-react`
- Used by: 0 file(s)

### `app/components/ui/hover-card.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` HoverCard, HoverCardTrigger, HoverCardContent `
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `react`, `@radix-ui/react-hover-card`
- Used by: 0 file(s)

### `app/components/ui/icons/app-store-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Used by: 1 file(s) — `app/components/integrations/AppStoreIntegration.tsx`

### `app/components/ui/icons/claude-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `ClaudeLogo`
- Used by: 5 file(s) — `app/components/integrations/AnthropicIntegration.tsx`, `app/components/integrations/IntegrationsGrid.tsx`, `app/components/onboarding/steps/AIProviderStep.tsx`, `app/components/onboarding/steps/SuccessStep.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/ui/icons/convex-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `ConvexLogo`
- Used by: 5 file(s) — `app/components/chat/Chat.tsx`, `app/components/chat/ConvexSetupBanner.tsx`, `app/components/integrations/ConvexIntegration.tsx`, `app/components/integrations/IntegrationsGrid.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/ui/icons/expo-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `ExpoLogo`
- Used by: 6 file(s) — `app/components/deploy/DeployModal.tsx`, `app/components/integrations/ExpoIntegration.tsx`, `app/components/integrations/IntegrationsGrid.tsx`, `app/components/onboarding/steps/ExpoStep.tsx`, `app/components/onboarding/steps/SuccessStep.tsx`

### `app/components/ui/icons/firebase-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `FirebaseLogo`
- Used by: 2 file(s) — `app/components/chat/FirebaseSetupBanner.tsx`, `app/components/integrations/IntegrationsGrid.tsx`

### `app/components/ui/icons/openai-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `OpenAILogo`
- Used by: 5 file(s) — `app/components/integrations/IntegrationsGrid.tsx`, `app/components/integrations/OpenAIIntegration.tsx`, `app/components/onboarding/steps/AIProviderStep.tsx`, `app/components/onboarding/steps/SuccessStep.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/ui/icons/revenuecat-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `RevenueCatLogo`
- Direct dependencies (external): `react`
- Used by: 3 file(s) — `app/components/chat/Chat.tsx`, `app/components/chat/RevenueCatSetupBanner.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/ui/icons/stripe-logo.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `StripeLogo`
- Used by: 3 file(s) — `app/components/chat/Chat.tsx`, `app/components/chat/StripeSetupBanner.tsx`, `app/components/settings/sections/ConnectedAccountsSection.tsx`

### `app/components/ui/image-dropzone.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `ImageDropzone`
- Direct dependencies (internal): `app/components/ui/button.tsx`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/project/ProjectSettings.tsx`

### `app/components/ui/input.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Input `, ` Textarea `
- Direct dependencies (external): `react`
- Used by: 4 file(s) — `app/components/ai-elements/web-preview.tsx`, `app/components/project/ProjectSettings.tsx`, `app/components/settings/sections/IntegrationCredentialsModal.tsx`, `app/components/settings/sections/SecretModal.tsx`

### `app/components/ui/shimmer.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: `Shimmer`
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `motion/react`
- Used by: 1 file(s) — `app/components/chat/AssistantMessage.tsx`

### `app/components/ui/switch.css`
- Responsibility: UI component/module for ui feature.
- Used by: 0 file(s)

### `app/components/ui/Switch.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Switch `
- Direct dependencies (external): `react`
- Used by: 3 file(s) — `app/components/project/ProjectSettings.tsx`, `app/components/settings/sections/AppearanceSection.tsx`, `app/components/settings/sections/PreferencesSection.tsx`

### `app/components/ui/tooltip.tsx`
- Responsibility: UI component/module for ui feature.
- Exports: ` Tooltip, TooltipTrigger, TooltipContent, TooltipProvider `
- Direct dependencies (internal): `lib/utils.ts`
- Direct dependencies (external): `react`, `@radix-ui/react-tooltip`
- Used by: 1 file(s) — `app/components/preview/Preview.tsx`

### `app/components/window/index.ts`
- Responsibility: UI component/module for window feature.
- Exports: ` WindowContextProvider, useWindowContext `, ` TitlebarContextProvider, useTitlebarContext `, ` menuItems `
- Used by: 2 file(s) — `app/components/window/TitlebarMenu.tsx`, `packages/desktop/src/entry.tsx`

### `app/components/window/menus.ts`
- Responsibility: UI component/module for window feature.
- Exports: `menuItems`
- Direct dependencies (internal): `app/components/window/TitlebarMenu.tsx`
- Used by: 0 file(s)

### `app/components/window/titlebar.css`
- Responsibility: UI component/module for window feature.
- Used by: 0 file(s)

### `app/components/window/Titlebar.tsx`
- Responsibility: UI component/module for window feature.
- Exports: `Titlebar`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/workbench.ts`, `app/stores/project-store.ts`, `app/components/window/WindowContext.tsx`, `app/components/window/TitlebarContext.
tsx`, `app/components/window/TitlebarMenu.tsx`
- Direct dependencies (external): `react`, `react-router-dom`, `lucide-react`
- Used by: 1 file(s) — `app/components/window/WindowContext.tsx`

### `app/components/window/TitlebarContext.tsx`
- Responsibility: UI component/module for window feature.
- Exports: `TitlebarContextProvider`, `useTitlebarContext`
- Direct dependencies (external): `react`
- Used by: 3 file(s) — `app/components/window/Titlebar.tsx`, `app/components/window/TitlebarMenu.tsx`, `app/components/window/WindowContext.tsx`

### `app/components/window/TitlebarMenu.tsx`
- Responsibility: UI component/module for window feature.
- Exports: ` TitlebarMenu, TitlebarMenuItem, TitlebarMenuPopup, TitlebarMenuPopupItem `
- Direct dependencies (internal): `app/components/window/index.ts`, `app/components/window/TitlebarContext.tsx`
- Direct dependencies (external): `react`
- Used by: 2 file(s) — `app/components/window/Titlebar.tsx`, `app/components/window/menus.ts`

### `app/components/window/user-avatar.css`
- Responsibility: UI component/module for window feature.
- Used by: 0 file(s)

### `app/components/window/UserAvatar.tsx`
- Responsibility: UI component/module for window feature.
- Exports: `UserAvatar`
- Used by: 0 file(s)

### `app/components/window/WindowContext.tsx`
- Responsibility: UI component/module for window feature.
- Exports: `WindowContextProvider`, `useWindowContext`
- Direct dependencies (internal): `app/components/window/Titlebar.tsx`, `app/components/window/TitlebarContext.tsx`, `lib/conveyor/schemas/index.ts`
- Direct dependencies (external): `react`
- Used by: 1 file(s) — `app/components/window/Titlebar.tsx`

### `app/components/workbench/index.ts`
- Responsibility: UI component/module for workbench feature.
- Exports: ` Workbench `
- Used by: 0 file(s)

### `app/components/workbench/styles.css`
- Responsibility: UI component/module for workbench feature.
- Used by: 0 file(s)

### `app/components/workbench/Workbench.tsx`
- Responsibility: UI component/module for workbench feature.
- Exports: `Workbench`
- Direct dependencies (internal): `app/hooks/useStore.ts`, `app/stores/workbench.ts`, `lib/launch/index.ts`, `app/components/editor/EditorPanel.tsx`, `app/components/preview/Preview.tsx`, `app/components/terminal/index.ts`
- Direct dependencies (external): `react`, `framer-motion`, `lucide-react`, `react-hot-toast`
- Used by: 1 file(s) — `app/components/project/ProjectContent.tsx`

## Related
- [[2026-02-25-bfloat-ide-codebase-map]]
- [[2026-02-27-bfloat-ide-components-state]]