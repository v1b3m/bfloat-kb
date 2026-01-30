# Diff Summary: `develop` vs `local-develop`

This document summarizes the differences between the `develop` and `local-develop` branches.

## Overview

| Files Changed | Insertions | Deletions |
|---------------|------------|-----------|
| 22            | 905        | 3,540     |

The `local-develop` branch represents a **simpler, more streamlined version** that removes several features present in `develop`.

---

## Major Removals in `local-develop`

### 1. HeroPage Component (Removed)
- **File**: `app/components/home/HeroPage.tsx` (456 lines deleted)
- **File**: `app/components/home/index.ts` (removed)
- The landing/hero page with quick actions and recent workspaces is removed
- `AppLibrary` is now the home route (`/`) instead of `/library`

### 2. Command Palette (Removed)
- **File**: `app/components/ui/CommandPalette.tsx` (346 lines deleted)
- Global `Cmd+K` keyboard shortcut removed from `app/app.tsx`
- No command palette functionality in `local-develop`

### 3. Session Persistence/Reader (Removed)
- **File**: `lib/agents/session-reader.ts` (695 lines deleted)
- **File**: `lib/conveyor/api/ai-agent-api.ts` (67 lines removed)
- Local CLI session reading (Claude Code/Codex) is removed
- No `readSession` API endpoint
- No `SessionMessageData` types

### 4. Chat Component Simplifications
- **File**: `app/components/chat/Chat.tsx` (323 lines reduced significantly)
- Removed features:
  - `autoStart` prop for auto-starting AI on new projects
  - `initialSessionId` and `onSessionIdChange` for session persistence
  - Session loading from local CLI storage
  - `TaskProgress` component and todo extraction from messages
  - `AskUserQuestion` handling
  - Provider auth status fetching
- Changed behavior:
  - Messages persisted to backend instead of local CLI storage
  - Simpler message handling

### 5. ChatInput vs PromptInput
- **`develop`**: Uses `ChatInput.tsx` with toggle-style provider selector
- **`local-develop`**: Uses `PromptInput.tsx` (simpler, no provider selector inline)
- Provider selector moved to chat header via `LocalProviderSelector` component

---

## Major Additions in `local-develop`

### 1. CreateAppModal Component
- **File**: `app/components/library/CreateAppModal.tsx` (389 lines added)
- New modal for creating apps (replaces HeroPage functionality)

### 2. Enhanced DeployModal
- **File**: `app/components/deploy/DeployModal.tsx` (183 lines added)
- Requires Expo connection for deployment
- Enhanced deployment flow

---

## Other Notable Changes

### Styling Reductions
- **`app/components/chat/styles.css`**: 904 lines reduced (removed many design system styles)
- **`app/styles/globals.css`**: 480 lines reduced

### Type Simplifications
- **`app/types/project.ts`**: 95 lines reduced
- Simpler message/project types

### Agent Handler Changes
- **`lib/conveyor/handlers/ai-agent-handler.ts`**: 154 lines reduced
- Removed session reading IPC handlers

### Claude Provider
- **`lib/agents/providers/claude-provider.ts`**: 35 lines changed
- Simplified provider implementation

---

## Routing Changes

| Route | `develop` | `local-develop` |
|-------|-----------|-----------------|
| `/` | HeroPage | AppLibrary |
| `/library` | AppLibrary | (redirects to `/`) |
| `/projects/:id` | ProjectPage | ProjectPage |

---

## Summary

`local-develop` is a **stripped-down version** that:
1. Removes the HeroPage landing experience
2. Removes command palette (Cmd+K)
3. Removes local CLI session persistence (Claude Code/Codex session reading)
4. Simplifies the Chat component significantly
5. Adds a CreateAppModal as an alternative to HeroPage
6. Requires Expo connection for deployment

The `develop` branch has more features for a richer user experience, while `local-develop` focuses on a simpler, more direct workflow.
