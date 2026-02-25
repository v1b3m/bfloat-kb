# Convex Integration - Complete Flow Documentation

## Overview

The Convex integration allows users to connect their Convex account via OAuth, then provision a Convex backend for any project. The flow spans two codebases:

- **Frontend (IDE):** `/Users/v1b3m/Dev/bfloat/bfloat-ide`
- **Backend:** `/Users/v1b3m/Dev/bfloat/bfloat-app-engineer`

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           USER FLOW                                     │
│                                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────────────┐   │
│  │ Settings │───>│  OAuth   │───>│ Convex   │───>│ Callback to IDE  │   │
│  │   Page   │    │  Flow    │    │ Consent  │    │ (deep link)      │   │
│  └──────────┘    └──────────┘    └──────────┘    └──────────────────┘   │
│       │                                                  │              │
│       │          ┌───────────────────────────────────────┘              │
│       ▼          ▼                                                      │
│  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐   │
│  │  Chat: User      │───>│  Backend:        │───>│  IDE stores      │   │
│  │  mentions Convex │    │  setup-convex    │    │  env vars        │   │
│  └──────────────────┘    └──────────────────┘    └──────────────────┘   │
│                                                          │              │
│                                                          ▼              │
│                                                  ┌──────────────────┐   │
│                                                  │  Claude Code     │   │
│                                                  │  /add-convex     │   │
│                                                  │  skill runs      │   │
│                                                  └──────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: OAuth Connection (Settings)

User connects their Convex account in the IDE settings page.

### Flow

```
IDE Settings Page                    Backend                         Convex
     │                                  │                               │
     │  Click "Connect Convex"          │                               │
     │─────────────────────────────────>│                               │
     │  window.open(backendUrl/         │                               │
     │    desktop/convex/connect)       │                               │
     │                                  │                               │
     │                                  │  Generate CSRF state token    │
     │                                  │  Store in AccountSettings     │
     │                                  │                               │
     │                                  │  Redirect to Convex OAuth     │
     │                                  │─────────────────────────────> │
     │                                  │  https://dashboard.convex.dev │
     │                                  │    /oauth/authorize           │
     │                                  │                               │
     │                                  │          User consents        │
     │                                  │<───────────────────────────── │
     │                                  │  Callback with auth code      │
     │                                  │                               │
     │                                  │  Exchange code for token      │
     │                                  │─────────────────────────────> │
     │                                  │  POST api.convex.dev/         │
     │                                  │    oauth/token                │
     │                                  │<───────────────────────────── │
     │                                  │  Returns access_token         │
     │                                  │                               │
     │                                  │  Save token to                │
     │                                  │  AccountSettings              │
     │                                  │                               │
     │  Deep link: bfloat://convex/     │                               │
     │    callback?success=true         │                               │
     │<─────────────────────────────────│                               │
     │                                  │                               │
     │  IPC: convex-callback            │                               │
     │  → setConvexConnected(true)      │                               │
```

### Key Files

| File                                                            | Codebase | Role                                           |
| --------------------------------------------------------------- | -------- | ---------------------------------------------- |
| `app/components/settings/sections/ConnectedAccountsSection.tsx` | IDE      | UI: connect/disconnect buttons, status check   |
| `lib/main/main.ts`                                              | IDE      | Deep link handler (`bfloat://convex/callback`) |
| `app/routes/desktop.convex.connect.tsx`                         | Backend  | Initiates OAuth for desktop app                |
| `app/routes/desktop.convex.callback.tsx`                        | Backend  | Handles OAuth callback, returns deep link      |
| `app/routes/api.oauth.convex.connect.tsx`                       | Backend  | Initiates OAuth for web                        |
| `app/routes/api.oauth.convex.callback.ts`                       | Backend  | Handles OAuth callback for web                 |
| `app/routes/api.oauth.convex.status.ts`                         | Backend  | Returns `{ connected, expired }`               |
| `app/routes/api.oauth.convex.revoke.ts`                         | Backend  | Disconnects Convex                             |
| `app/lib/convex-oauth.server.ts`                                | Backend  | Core OAuth2 logic                              |

### Backend OAuth Config (env vars)

```
CONVEX_OAUTH_CLIENT_ID
CONVEX_OAUTH_CLIENT_SECRET
CONVEX_OAUTH_REDIRECT_URI          (web callback)
CONVEX_OAUTH_DESKTOP_REDIRECT_URI  (desktop callback)
```

### Database Schema (Prisma)

```
AccountSettings {
  convexAccessToken    String?   // OAuth token from Convex
  oauthState           String?   // CSRF protection token
}
```

---

## Phase 2: Project Setup (Provisioning)

After OAuth is connected, user can provision Convex for a specific project.

### Trigger Points

1. **Auto-trigger after OAuth callback** - In `Chat.tsx`, when `onConvexCallback` fires with success, it automatically calls `projectApi.setupConvex(projectId)`
2. **Manual trigger** - User mentions "convex" in chat → `ConvexSetupBanner` appears → user clicks "Set up Convex"
3. **Chat intercept** - When user types anything containing "convex" and project doesn't have it, a setup banner is shown

### Flow

```
Chat.tsx                          Backend API                    Convex API
   │                                  │                              │
   │  projectApi.setupConvex(id)      │                              │
   │─────────────────────────────────>│                              │
   │  POST /api/projects/{id}/        │                              │
   │    setup-convex                  │                              │
   │                                  │                              │
   │                                  │  Get convexAccessToken from  │
   │                                  │  AccountSettings             │
   │                                  │                              │
   │                                  │  getTokenDetails(token)      │
   │                                  │─────────────────────────────>│
   │                                  │  GET v1/token_details        │
   │                                  │<─────────────────────────────│
   │                                  │  Returns { teamId }          │
   │                                  │                              │
   │                                  │  createConvex(projectName)   │
   │                                  │─────────────────────────────>│
   │                                  │  POST v1/teams/{id}/         │
   │                                  │    create_project            │
   │                                  │<─────────────────────────────│
   │                                  │  Returns { deploymentName,   │
   │                                  │    deploymentUrl, projectId }│
   │                                  │                              │
   │                                  │  createDeploymentKey()       │
   │                                  │─────────────────────────────>│
   │                                  │  POST v1/deployments/{name}/ │
   │                                  │    create_deploy_key         │
   │                                  │<─────────────────────────────│
   │                                  │  Returns deploy key          │
   │                                  │                              │
   │                                  │  Update Project in DB        │
   │                                  │                              │
   │  Returns { success,              │                              │
   │    convexDeployment,             │                              │
   │    convexUrl,                    │                              │
   │    convexDeploymentKey }         │                              │
   │<─────────────────────────────────│                              │
```

### Key Files

| File                                          | Codebase | Role                                                                          |
| --------------------------------------------- | -------- | ----------------------------------------------------------------------------- |
| `app/components/chat/Chat.tsx`                | IDE      | Triggers setup, handles response, stores env vars                             |
| `app/components/chat/ConvexSetupBanner.tsx`   | IDE      | UI banner in chat                                                             |
| `app/components/chat/AssistantMessage.tsx`    | IDE      | Renders `convex-setup-prompt` part type                                       |
| `lib/api/project.ts`                          | IDE      | `setupConvex()` API client                                                    |
| `app/routes/api.projects.$id.setup-convex.ts` | Backend  | Provisioning endpoint                                                         |
| `app/utils/convex.ts`                         | Backend  | Convex API helpers (`createConvex`, `createDeploymentKey`, `getTokenDetails`) |

### Database Schema (Prisma)

```
Project {
  convexDeployment     String?   // e.g. "proj-abc-123"
  convexUrl            String?   // e.g. "https://proj-abc-123.convex.cloud"
  convexProjectId      Int?      // Numeric Convex project ID
  convexDeploymentKey  String?   // Admin deploy key
}
```

---

## Phase 3: Environment Variables & Claude Code

After provisioning, the IDE sets env vars and triggers the AI agent.

### Env Var Flow

```
Chat.tsx (after setupConvex returns)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_CONVEX_URL', result.convexUrl)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'CONVEX_DEPLOY_KEY', result.convexDeploymentKey)
   │
   │  workbenchStore.setPendingEnvVars({
   │    EXPO_PUBLIC_CONVEX_URL: ...,
   │    CONVEX_DEPLOY_KEY: ...
   │  })
   │
   ▼
useLocalAgent.ts (when AI agent starts)
   │
   │  const pendingEnvVars = workbenchStore.takePendingEnvVars()
   │  if (pendingEnvVars) sessionOptions.env = pendingEnvVars
   │
   │  // Claude Code / Codex CLI now has these env vars
   │
   ▼
Claude Code runs /add-convex skill
   │
   │  1. npm install convex
   │  2. npx convex dev --once  (uses CONVEX_DEPLOY_KEY env var)
   │  3. Generate convex/_generated/ files
   │  4. Create schema.ts with table definitions
   │  5. Create ConvexProvider wrapper component
   │  6. Update app components with useQuery/useMutation
```

### Key Files

| File                                                   | Codebase | Role                                                                 |
| ------------------------------------------------------ | -------- | -------------------------------------------------------------------- |
| `app/stores/workbench.ts`                              | IDE      | `pendingEnvVars` atom, `setPendingEnvVars()`, `takePendingEnvVars()` |
| `app/hooks/useLocalAgent.ts`                           | IDE      | Picks up pending env vars, passes to CLI session                     |
| `resources/skills/claude/skills/add-convex/SKILL.md`   | IDE      | Claude Code skill definition                                         |
| `resources/skills/claude/skills/add-convex/templates/` | IDE      | Templates for schema, provider, functions                            |

---

## Phase 4: Convex Dashboard (Embedded)

Once a project has Convex, an embedded dashboard is available in the IDE's "Database" tab.

```
Workbench (Database tab)
   │
   ▼
ConvexDashboard.tsx
   │
   │  <iframe src="https://dashboard-embedded.convex.dev/data" />
   │
   │  Listens for: 'dashboard-credentials-request' postMessage
   │  Responds with:
   │    {
   │      type: 'dashboard-credentials',
   │      adminKey: deployKey,
   │      deploymentUrl,
   │      deploymentName
   │    }
```

### Key Files

| File                                         | Codebase | Role                                     |
| -------------------------------------------- | -------- | ---------------------------------------- |
| `app/components/project/ConvexDashboard.tsx` | IDE      | Embedded Convex dashboard iframe         |
| `app/components/project/Workbench.tsx`       | IDE      | Shows database tab when Convex is active |
| `app/components/project/ProjectPage.tsx`     | IDE      | Passes Convex config to ProjectContent   |

---

## Phase 5: Backend Deployment (Worker - Mobile Flow)

For the mobile app flow, the backend has a Bull/Redis worker that deploys Convex schemas server-side.

```
Worker (Bull/Redis)
   │
   │  Job from convexQueue: { deploymentKey, files }
   │
   ▼
deployConvexBackend(files, deploymentKey)
   │
   │  1. Create temp directory
   │  2. Filter files starting with convex/
   │  3. Validate config.ts exists
   │  4. npm install convex
   │  5. Write all convex files to temp dir
   │  6. CONVEX_DEPLOY_KEY=xyz npx convex dev --once
   │  7. Clean up temp dir
```

### Key Files

| File                      | Codebase | Role                              |
| ------------------------- | -------- | --------------------------------- |
| `app/scripts/worker.ts`   | Backend  | `provisionConvex()`, job handlers |
| `app/scripts/convex.ts`   | Backend  | `deployConvexBackend()`           |
| `app/lib/queue.server.ts` | Backend  | `convexQueue` (Bull/Redis queue)  |

---

## Complete End-to-End Summary

```
1. SETTINGS: User clicks "Connect Convex"
   └─> OAuth flow → token saved to AccountSettings

2. CHAT: User mentions "convex" or clicks "Set up Convex"
   └─> POST /api/projects/{id}/setup-convex
       ├─> Convex API: create project
       ├─> Convex API: create deploy key
       └─> Update Project record in DB

3. IDE: Receives provisioning response
   ├─> Stores EXPO_PUBLIC_CONVEX_URL + CONVEX_DEPLOY_KEY as secrets
   ├─> Sets pendingEnvVars in workbenchStore
   └─> Auto-sends prompt to Claude Code

4. CLAUDE CODE: Runs /add-convex skill
   ├─> npm install convex
   ├─> npx convex dev --once
   ├─> Creates schema, provider, functions
   └─> Updates app components

5. DONE: Project has working Convex backend
   └─> Database tab shows embedded Convex dashboard
```

---

## Disconnection

- **Settings UI:** Click "Disconnect" → `POST /api/oauth/convex/revoke` → nullifies `convexAccessToken`
- **IPC path:** `user:disconnect-convex` handler in `lib/conveyor/handlers/user-handler.ts`

---

## Notes

- Convex OAuth tokens do not expire (unlike some other providers)
- The setup-convex endpoint is idempotent - if project already has Convex config, it returns early
- Project names are sanitized: lowercase, alphanumeric + dashes, max 64 chars
- The embedded dashboard communicates credentials via postMessage (no URL params)
