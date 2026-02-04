# Firebase Integration - Complete Flow Documentation

## Overview

The Firebase integration allows users to connect their Google account via OAuth, then provision a full Firebase backend (GCP project, Firestore, web app) for any project. The flow spans two codebases:

- **Frontend (IDE):** `/Users/v1b3m/Dev/bfloat/bfloat-ide`
- **Backend:** `/Users/v1b3m/Dev/bfloat/bfloat-app-engineer`

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           USER FLOW                                     │
│                                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────────────┐   │
│  │ Settings │───>│  OAuth   │───>│  Google  │───>│ Callback to IDE  │   │
│  │   Page   │    │  Flow    │    │ Consent  │    │ (deep link)      │   │
│  └──────────┘    └──────────┘    └──────────┘    └──────────────────┘   │
│       │                                                  │              │
│       │          ┌───────────────────────────────────────┘              │
│       ▼          ▼                                                      │
│  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐   │
│  │  Chat: User      │───>│  Backend:        │───>│  IDE stores      │   │
│  │  clicks Firebase │    │  setup-firebase  │    │  env vars        │   │
│  └──────────────────┘    └──────────────────┘    └──────────────────┘   │
│                                                          │              │
│                                                          ▼              │
│                                                  ┌──────────────────┐   │
│                                                  │  Claude Code     │   │
│                                                  │  /add-firebase   │   │
│                                                  │  skill runs      │   │
│                                                  └──────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: OAuth Connection (Settings)

User connects their Google account in the IDE settings page. The same Google OAuth token is used for both Firebase and GCP operations.

### Flow

```
IDE Settings Page                    Backend                         Google
     │                                  │                               │
     │  Click "Connect Google"          │                               │
     │─────────────────────────────────>│                               │
     │  window.open(backendUrl/         │                               │
     │    desktop/google/connect)       │                               │
     │                                  │                               │
     │                                  │  Generate CSRF state token    │
     │                                  │  Store in AccountSettings     │
     │                                  │                               │
     │                                  │  Redirect to Google OAuth     │
     │                                  │─────────────────────────────> │
     │                                  │  accounts.google.com/         │
     │                                  │    o/oauth2/v2/auth           │
     │                                  │                               │
     │                                  │  Scopes:                      │
     │                                  │    - firebase (manage)        │
     │                                  │    - cloud-platform (full)    │
     │                                  │    - openid, email, profile   │
     │                                  │                               │
     │                                  │          User consents        │
     │                                  │<───────────────────────────── │
     │                                  │  Callback with auth code      │
     │                                  │                               │
     │                                  │  Exchange code for tokens     │
     │                                  │─────────────────────────────> │
     │                                  │  POST oauth2.googleapis.com/  │
     │                                  │    token                      │
     │                                  │<───────────────────────────── │
     │                                  │  Returns access_token +       │
     │                                  │    refresh_token              │
     │                                  │                               │
     │                                  │  Save tokens to               │
     │                                  │  AccountSettings              │
     │                                  │                               │
     │  Deep link: bfloat://google/     │                               │
     │    callback?success=true         │                               │
     │<─────────────────────────────────│                               │
     │                                  │                               │
     │  IPC: google-callback            │                               │
     │  → setIntegrationStatus(         │                               │
     │      firebase: true)             │                               │
```

### Key Files

| File                                                            | Codebase | Role                                           |
| --------------------------------------------------------------- | -------- | ---------------------------------------------- |
| `app/components/settings/sections/ConnectedAccountsSection.tsx` | IDE      | UI: connect/disconnect buttons, status check   |
| `lib/main/main.ts`                                              | IDE      | Deep link handler (`bfloat://google/callback`) |
| `lib/conveyor/api/app-api.ts`                                   | IDE      | `onGoogleCallback()` listener                  |
| `app/routes/desktop.google.connect.tsx`                         | Backend  | Initiates OAuth for desktop app                |
| `app/routes/desktop.google.callback.tsx`                        | Backend  | Handles OAuth callback, returns deep link      |
| `app/routes/api.oauth.google.connect.tsx`                       | Backend  | Initiates OAuth for web                        |
| `app/routes/api.oauth.google.callback.ts`                       | Backend  | Handles OAuth callback for web                 |
| `app/routes/api.oauth.google.status.ts`                         | Backend  | Returns `{ connected, expired }`               |
| `app/routes/api.oauth.google.revoke.ts`                         | Backend  | Disconnects Google                             |
| `app/lib/google-oauth.server.ts`                                | Backend  | Core OAuth2 logic, token refresh               |

### Backend OAuth Config (env vars)

```
GOOGLE_OAUTH_CLIENT_ID
GOOGLE_OAUTH_CLIENT_SECRET
GOOGLE_OAUTH_DESKTOP_REDIRECT_URI  (desktop callback)
```

### OAuth Scopes

```
https://www.googleapis.com/auth/firebase
https://www.googleapis.com/auth/cloud-platform
openid
email
profile
```

### Database Schema (Prisma)

```
AccountSettings {
  googleAccessToken        String?   // OAuth access token
  googleRefreshToken       String?   // Refresh token (for auto-renewal)
  googleTokenExpiresAt     DateTime? // Token expiry
  firebaseAccessToken      String?   // Firebase-specific token (if separate)
  firebaseRefreshToken     String?   // Firebase-specific refresh token
  firebaseTokenExpiresAt   DateTime? // Firebase token expiry
  oauthState               String?   // CSRF protection token
}
```

### Token Refresh

Unlike Convex (tokens don't expire), Google tokens expire. The backend auto-refreshes via `getValidAccessToken(userId)` in `google-oauth.server.ts`:

1. Checks if `googleAccessToken` exists and is not expired
2. If expired, uses `googleRefreshToken` to get a new access token via `POST oauth2.googleapis.com/token`
3. Saves the new token + expiry to `AccountSettings`
4. If refresh fails, returns `null` → setup returns `{ error: "Connect Google first" }`

---

## Phase 2: Project Setup (Provisioning)

After OAuth is connected, user can provision Firebase for a specific project. This is a 6-step process that creates a full GCP project with Firebase, Firestore, and a web app.

### Trigger Points

1. **Auto-trigger after OAuth callback** - In `Chat.tsx`, when `onGoogleCallback` fires with success, it automatically calls `projectApi.setupFirebase(projectId)`
2. **Manual trigger via integration menu** - User clicks "Use" on Firebase in the integration picker → `handleIntegrationUse('firebase')` calls `projectApi.setupFirebase(projectId)`

### Flow

```
Chat.tsx                          Backend API                    GCP / Firebase APIs
   │                                  │                              │
   │  projectApi.setupFirebase(id)    │                              │
   │─────────────────────────────────>│                              │
   │  POST /api/projects/{id}/        │                              │
   │    setup-firebase                │                              │
   │                                  │                              │
   │                                  │  getValidAccessToken(userId) │
   │                                  │  (auto-refresh if expired)   │
   │                                  │                              │
   │                                  │  STEP 1: Create GCP project  │
   │                                  │─────────────────────────────>│
   │                                  │  POST cloudresourcemanager   │
   │                                  │    .googleapis.com/v1/       │
   │                                  │    projects                  │
   │                                  │  (polls operation until done)│
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 2: Enable APIs         │
   │                                  │─────────────────────────────>│
   │                                  │  POST serviceusage           │
   │                                  │    .googleapis.com/v1/       │
   │                                  │    projects/{id}/services/   │
   │                                  │    {api}:enable              │
   │                                  │  (for each: firebase,        │
   │                                  │   firestore, serviceusage)   │
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 2b: Link billing       │
   │                                  │─────────────────────────────>│
   │                                  │  GET cloudbilling            │
   │                                  │    .googleapis.com/v1/       │
   │                                  │    billingAccounts           │
   │                                  │  PUT cloudbilling            │
   │                                  │    .googleapis.com/v1/       │
   │                                  │    projects/{id}/billingInfo │
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 3: Add Firebase        │
   │                                  │─────────────────────────────>│
   │                                  │  POST firebase.googleapis    │
   │                                  │    .com/v1beta1/projects/    │
   │                                  │    {id}:addFirebase          │
   │                                  │  (polls operation until done)│
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 4: Create Firestore DB │
   │                                  │─────────────────────────────>│
   │                                  │  POST firestore.googleapis   │
   │                                  │    .com/v1/projects/{id}/    │
   │                                  │    databases?databaseId=     │
   │                                  │    (default)                 │
   │                                  │  (retries on 403             │
   │                                  │   SERVICE_DISABLED, 5x 5s)  │
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 5: Create web app      │
   │                                  │─────────────────────────────>│
   │                                  │  POST firebase.googleapis    │
   │                                  │    .com/v1beta1/projects/    │
   │                                  │    {id}/webApps              │
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  STEP 6: Get web app config  │
   │                                  │─────────────────────────────>│
   │                                  │  GET firebase.googleapis     │
   │                                  │    .com/v1beta1/projects/    │
   │                                  │    {id}/webApps/{appId}/     │
   │                                  │    config                    │
   │                                  │<─────────────────────────────│
   │                                  │                              │
   │                                  │  Update Project in DB        │
   │                                  │                              │
   │  Returns { success,              │                              │
   │    firebaseProjectId,            │                              │
   │    firebaseConfig: {             │                              │
   │      apiKey, authDomain,         │                              │
   │      projectId, storageBucket,   │                              │
   │      messagingSenderId, appId    │                              │
   │    }                             │                              │
   │  }                               │                              │
   │<─────────────────────────────────│                              │
```

### GCP Project ID Generation

The GCP project ID is derived from the Bfloat project title:

```
projectName = title.toLowerCase()
  .replace(/[^a-z0-9-]/g, '-')   // only alphanumeric + hyphens
  .replace(/-+/g, '-')            // collapse multiple hyphens
  .replace(/^-|-$/g, '')          // trim hyphens
  .slice(0, 24)

gcpProjectId = `bf-${projectName}-${id.slice(0, 4)}`.slice(0, 30)
// e.g. "bf-my-cool-app-6592"
```

### Key Files

| File                                               | Codebase | Role                                                  |
| -------------------------------------------------- | -------- | ----------------------------------------------------- |
| `app/components/chat/Chat.tsx`                     | IDE      | Triggers setup, handles response, stores env vars     |
| `app/components/chat/FirebaseSetupBanner.tsx`      | IDE      | UI banner in chat                                     |
| `app/components/chat/AssistantMessage.tsx`         | IDE      | Renders `firebase-setup-prompt` part type             |
| `lib/api/project.ts`                               | IDE      | `setupFirebase()` API client                          |
| `app/routes/api.projects.$id.setup-firebase.ts`   | Backend  | Provisioning endpoint (6-step orchestrator)           |
| `app/utils/firebase.ts`                            | Backend  | GCP/Firebase API helpers (all 6 step implementations) |
| `app/lib/google-oauth.server.ts`                   | Backend  | Token management, auto-refresh                        |

### Firebase Utility Functions (`app/utils/firebase.ts`)

| Function                  | API Used                      | Purpose                                        |
| ------------------------- | ----------------------------- | ---------------------------------------------- |
| `createGcpProject()`      | Cloud Resource Manager v1     | Creates a new GCP project, polls until ready   |
| `enableRequiredApis()`    | Service Usage v1              | Enables firebase, firestore, serviceusage APIs |
| `findBillingAccount()`    | Cloud Billing v1              | Lists user's billing accounts, finds open one  |
| `linkBillingAccount()`    | Cloud Billing v1              | Links billing account to GCP project           |
| `createFirebaseProject()` | Firebase Management v1beta1   | Adds Firebase to GCP project, polls operation  |
| `addFirestoreToProject()` | Firestore v1                  | Creates default Firestore database             |
| `createWebApp()`          | Firebase Management v1beta1   | Creates a web app in the Firebase project      |
| `getWebAppConfig()`       | Firebase Management v1beta1   | Retrieves the web app config (API keys, etc.)  |
| `pollOperation()`         | Firebase Management v1beta1   | Generic long-running operation poller           |

### Database Schema (Prisma)

```
Project {
  firebaseProjectId   String?   // GCP project ID, e.g. "bf-my-app-6592"
  firebaseConfig      Json?     // Full Firebase web app config object
}
```

### Error Handling

The backend returns structured errors:

| Condition                    | Status | Response                                              |
| ---------------------------- | ------ | ----------------------------------------------------- |
| Not authenticated            | 401    | `{ success: false, error: "Unauthorized" }`           |
| Google not connected         | 400    | `{ success: false, error: "Connect Google first" }`   |
| Project not found            | 404    | `{ success: false, error: "Project not found" }`      |
| No billing account           | 400    | `{ success: false, error: "...", noBilling: true }`   |
| Any other failure            | 500    | `{ success: false, error: "<error message>" }`        |
| Already configured           | 200    | `{ success: true, firebaseProjectId, firebaseConfig }`|

The frontend (`Chat.tsx`) checks `result.success` before proceeding. On failure, it calls `setError(errorMsg)` to show an error banner and does **not** send the AI skill prompt.

### Retry Logic

**Firestore 403 SERVICE_DISABLED:** After enabling APIs (step 2), GCP needs propagation time before Firestore is usable. `addFirestoreToProject()` retries up to 5 times with 5-second intervals when it receives a 403 with `SERVICE_DISABLED`.

**Firebase 429 Rate Limit:** `createFirebaseProject()` retries up to 5 times with linear backoff (5s, 10s, 15s, 20s, 25s) on 429 responses.

---

## Phase 3: Environment Variables & Claude Code

After provisioning, the IDE sets env vars and triggers the AI agent.

### Env Var Flow

```
Chat.tsx (after setupFirebase returns success)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_API_KEY', config.apiKey)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN', config.authDomain)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_PROJECT_ID', config.projectId)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET', config.storageBucket)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID', config.messagingSenderId)
   │
   │  window.conveyor.secrets.setSecret(projectId,
   │    'EXPO_PUBLIC_FIREBASE_APP_ID', config.appId)
   │
   │  workbenchStore.setPendingEnvVars({
   │    EXPO_PUBLIC_FIREBASE_API_KEY: ...,
   │    EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN: ...,
   │    EXPO_PUBLIC_FIREBASE_PROJECT_ID: ...,
   │    EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET: ...,
   │    EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID: ...,
   │    EXPO_PUBLIC_FIREBASE_APP_ID: ...
   │  })
   │
   ▼
useLocalAgent.ts (when AI agent starts)
   │
   │  const pendingEnvVars = workbenchStore.takePendingEnvVars()
   │  if (pendingEnvVars) sessionOptions.env = pendingEnvVars
   │
   │  // Claude Code now has these env vars
   │
   ▼
Claude Code runs /add-firebase skill
   │
   │  1. npm install firebase
   │  2. Create firebase config file using env vars
   │  3. Initialize Firebase app
   │  4. Set up Firestore client
   │  5. Create FirebaseProvider wrapper component
   │  6. Update app components with Firestore reads/writes
```

### Key Files

| File                                                     | Codebase | Role                                                                 |
| -------------------------------------------------------- | -------- | -------------------------------------------------------------------- |
| `app/stores/workbench.ts`                                | IDE      | `pendingEnvVars` atom, `setPendingEnvVars()`, `takePendingEnvVars()` |
| `app/hooks/useLocalAgent.ts`                             | IDE      | Picks up pending env vars, passes to CLI session                     |
| `resources/skills/claude/skills/add-firebase/SKILL.md`   | IDE      | Claude Code skill definition                                         |

---

## Phase 4: Billing Account Handling

Firestore requires a linked billing account, even on the free Spark plan. The setup flow handles this automatically.

### Flow

```
setup-firebase (Step 2b)
   │
   │  findBillingAccount(accessToken)
   │  GET cloudbilling.googleapis.com/v1/billingAccounts
   │
   ├─ Found open billing account?
   │  │
   │  YES ──> linkBillingAccount(projectId, accountName, token)
   │  │       PUT cloudbilling.googleapis.com/v1/
   │  │         projects/{id}/billingInfo
   │  │       { billingAccountName, billingEnabled: true }
   │  │
   │  NO ───> Return 400:
   │          { success: false, noBilling: true,
   │            error: "No billing account found..." }
   │          │
   │          ▼
   │          Chat.tsx shows error banner with link to
   │          console.cloud.google.com/billing
```

---

## Complete End-to-End Summary

```
1. SETTINGS: User clicks "Connect Google"
   └─> Google OAuth flow → tokens saved to AccountSettings
       (access_token + refresh_token, auto-refreshable)

2. CHAT: User clicks "Use Firebase" or OAuth callback fires
   └─> POST /api/projects/{id}/setup-firebase
       ├─> Step 1: Create GCP project (cloudresourcemanager)
       ├─> Step 2: Enable APIs (firebase, firestore, serviceusage)
       ├─> Step 2b: Find + link billing account (cloudbilling)
       ├─> Step 3: Add Firebase to project (firebase management)
       ├─> Step 4: Create Firestore database (firestore, with retry)
       ├─> Step 5: Create web app (firebase management)
       ├─> Step 6: Get web app config (firebase management)
       └─> Update Project record in DB

3. IDE: Receives provisioning response
   ├─> Stores 6 EXPO_PUBLIC_FIREBASE_* env vars as secrets
   ├─> Sets pendingEnvVars in workbenchStore
   └─> Auto-sends prompt to Claude Code

4. CLAUDE CODE: Runs /add-firebase skill
   ├─> npm install firebase
   ├─> Creates firebase config + initialization
   ├─> Sets up Firestore client
   └─> Updates app components

5. DONE: Project has working Firebase backend
   └─> Firestore database accessible via Firebase SDK
```

---

## Disconnection

- **Settings UI:** Click "Disconnect" → `POST /api/oauth/google/revoke` → nullifies `googleAccessToken`, `googleRefreshToken`, `googleTokenExpiresAt`, `firebaseAccessToken`, `firebaseRefreshToken`, `firebaseTokenExpiresAt`
- **IPC path:** `google-callback` handler in `lib/main/main.ts`

---

## Differences from Convex Integration

| Aspect                   | Convex                        | Firebase                                         |
| ------------------------ | ----------------------------- | ------------------------------------------------ |
| OAuth provider           | Convex                        | Google                                           |
| Token expiry             | Never expires                 | Expires, auto-refreshed via refresh token        |
| Setup steps              | 2 (create project, deploy key)| 7 (GCP project, APIs, billing, Firebase, Firestore, web app, config) |
| Billing requirement      | None                          | Billing account must be linked for Firestore     |
| API propagation delays   | None                          | 403 SERVICE_DISABLED retries needed              |
| Env vars set             | 2 (URL, deploy key)           | 6 (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId) |
| Embedded dashboard       | Yes (iframe)                  | No                                               |
| Backend deploy (worker)  | Yes (convex dev --once)       | No                                               |

---

## Notes

- Google OAuth tokens expire and require refresh — `getValidAccessToken()` handles this transparently
- The setup-firebase endpoint is idempotent — if project already has Firebase config, it returns early with the existing config
- GCP project IDs are prefixed with `bf-` and limited to 30 chars (GCP requirement: 6-30 chars, lowercase, hyphens, must start with letter)
- Firestore is created in `FIRESTORE_NATIVE` mode with `locationId: "nam5"` (US multi-region)
- The billing account lookup takes the first open billing account — most Google accounts have exactly one
- If no billing account exists, the user sees an error with a link to set one up at the Google Cloud Console
