# Firebase Integration - Implementation Plan

## Context

Adding Firebase integration following the same pattern as the existing Convex integration. See `convex-integration.md` for the full Convex flow reference.

## Current Status: ~30% Complete

The Google OAuth layer is fully implemented. Everything after auth (provisioning, chat flow, env var pipeline) is missing.

---

## What's Already Done

- [x] Google OAuth server (`app/lib/google-oauth.server.ts`) — full flow with token refresh
- [x] All 6 OAuth routes (backend: `api.oauth.google.*` + `desktop.google.*`)
- [x] Prisma AccountSettings fields (`firebaseAccessToken`, `firebaseRefreshToken`, `firebaseTokenExpiresAt`)
- [x] Google OAuth env vars in `config.server.ts`
- [x] Settings UI in `ConnectedAccountsSection.tsx` — connect/disconnect buttons
- [x] `google-oauth.ts` utility in IDE — `openGoogleOAuthWindow()`
- [x] Claude Code skill + templates (`resources/skills/claude/skills/add-firebase/`)
- [x] `FirebaseIntegration.tsx` standalone component (unused in main flow)

---

## Implementation Plan

### Phase 1: Backend — Firebase Project Provisioning

#### 1.1 Create `app/utils/firebase.ts` (Backend)
**Status: PENDING**
**Mirrors:** `app/utils/convex.ts`

Firebase Management REST API client functions:
- `createFirebaseProject(projectName, accessToken)` — POST to `https://firebase.googleapis.com/v1beta1/projects` (or use existing GCP project)
- `addFirestoreToProject(projectId, accessToken)` — Enable Firestore via API
- `getWebAppConfig(projectId, accessToken)` — Create web app + get config (apiKey, authDomain, etc.)
- `getTokenDetails(accessToken)` — Validate token, get user info

Key difference from Convex: Firebase project creation is async (returns an Operation that must be polled). May need to use an existing GCP project or poll for completion.

#### 1.2 Create `api.projects.$id.setup-firebase.ts` (Backend)
**Status: PENDING**
**Mirrors:** `api.projects.$id.setup-convex.ts`

Endpoint: `POST /api/projects/{id}/setup-firebase`

Flow:
1. Authenticate user
2. Get `googleAccessToken` from AccountSettings (use `getValidAccessToken()` for refresh)
3. Check if project already has Firebase config (early return if exists)
4. Create Firebase project (or provision under existing GCP project)
5. Enable Firestore
6. Create web app, get config object
7. Update Project record with Firebase config
8. Return config to client

#### 1.3 Prisma Migration — Add Firebase fields to Project model
**Status: PENDING**
**Mirrors:** Convex fields on Project

Add to Project model:
```
firebaseProjectId    String?
firebaseConfig       Json?     // { apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId }
```

#### 1.4 Add `disconnectGoogle` action to `api.settings.tsx` (Backend)
**Status: PENDING**
**Mirrors:** `disconnectConvex` action

Add form action `_action=disconnectGoogle` that nullifies Google/Firebase tokens.

---

### Phase 2: IDE — Deep Link & IPC for Google/Firebase Callback

#### 2.1 Add `bfloat://google/callback` deep link in `main.ts`
**Status: PENDING**
**Mirrors:** `bfloat://convex/callback` handler

Add path handler for `/google/callback` that extracts `success`/`error` params and sends `google-callback` IPC event.

#### 2.2 Add `onGoogleCallback` to `app-api.ts`
**Status: PENDING**
**Mirrors:** `onConvexCallback`

Add IPC listener method for `google-callback` events.

#### 2.3 Add `user:disconnect-google` to user-handler + user-schema
**Status: PENDING**
**Mirrors:** `user:disconnect-convex`

Add IPC handler and schema for Google/Firebase disconnection.

---

### Phase 3: IDE — Chat Integration (Auto-Provisioning Flow)

#### 3.1 Create `FirebaseSetupBanner.tsx`
**Status: PENDING**
**Mirrors:** `ConvexSetupBanner.tsx`

Chat banner component with:
- "Connect Firebase" button (if not connected)
- "Set up Firebase" button (if connected but not provisioned)

#### 3.2 Add `firebase-setup-prompt` handling to `AssistantMessage.tsx`
**Status: PENDING**
**Mirrors:** `convex-setup-prompt` handling

Render `FirebaseSetupBanner` when part type is `firebase-setup-prompt`.

#### 3.3 Add `setupFirebase()` to `lib/api/project.ts`
**Status: PENDING**
**Mirrors:** `setupConvex()`

API client method that POSTs to `/api/projects/{id}/setup-firebase`.

#### 3.4 Add Firebase fields to `app/types/project.ts`
**Status: PENDING**
**Mirrors:** Convex fields

Add `firebaseProjectId`, `firebaseConfig` to Project interface.

#### 3.5 Update `Chat.tsx` — Firebase auto-provisioning + keyword detection
**Status: PENDING**
**Mirrors:** Convex auto-provisioning in Chat.tsx

Add:
- Listen for `onGoogleCallback` IPC → auto-call `setupFirebase()`
- Detect "firebase" keyword in user messages → show `firebase-setup-prompt` banner
- After `setupFirebase()` returns, store env vars via `window.conveyor.secrets.setSecret()`:
  - `EXPO_PUBLIC_FIREBASE_API_KEY`
  - `EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN`
  - `EXPO_PUBLIC_FIREBASE_PROJECT_ID`
  - `EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET`
  - `EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID`
  - `EXPO_PUBLIC_FIREBASE_APP_ID`
- Set `pendingEnvVars` in workbenchStore
- Auto-send prompt to use `/add-firebase` skill

---

### Phase 4: IDE — Project Page & Workbench Integration

#### 4.1 Update `ProjectPage.tsx` — pass Firebase integration status
**Status: PENDING**
**Mirrors:** Convex integration status passing

Check `accountSettings.googleAccessToken` and pass `hasFirebaseIntegration` + Firebase config to `ProjectContent`.

#### 4.2 Update `ProjectContent.tsx` — receive Firebase props
**Status: PENDING**
**Mirrors:** Convex props

Pass Firebase integration status and config to Chat and Workbench.

#### 4.3 Update `Workbench.tsx` — Firebase dashboard/link (optional, MVP: skip)
**Status: PENDING**

Firebase doesn't have an embeddable dashboard. Options:
- **MVP:** Just link to `console.firebase.google.com/project/{id}` from the database tab
- **Later:** Build a simple Firestore data viewer

---

### Phase 5: IDE — user-schema for Google token visibility

#### 5.1 Add `googleAccessToken` to `user-schema.ts`
**Status: PENDING**
**Mirrors:** `convexAccessToken`

So the IDE can check connection status locally.

---

## Implementation Order

```
Phase 1 (Backend provisioning)
  1.3 Prisma migration
  1.1 app/utils/firebase.ts
  1.2 api.projects.$id.setup-firebase.ts
  1.4 disconnectGoogle action

Phase 2 (IDE deep link/IPC)
  2.1 main.ts deep link
  2.2 app-api.ts onGoogleCallback
  2.3 user-handler + user-schema disconnect

Phase 3 (IDE chat flow)
  3.4 Project types
  3.3 setupFirebase() API client
  3.1 FirebaseSetupBanner
  3.2 AssistantMessage firebase-setup-prompt
  3.5 Chat.tsx auto-provisioning

Phase 4 (IDE project/workbench)
  4.1 ProjectPage.tsx
  4.2 ProjectContent.tsx
  4.3 Workbench (optional)

Phase 5 (IDE user schema)
  5.1 user-schema.ts
```

---

## Key Differences from Convex

| Aspect | Convex | Firebase |
|--------|--------|----------|
| Auth | Convex OAuth (tokens don't expire) | Google OAuth (tokens expire, need refresh) |
| Project creation | Sync REST API | Async (Operation polling) or use existing GCP project |
| Credentials | Single deploy key | Config object (6+ fields) |
| Deploy | `npx convex dev --once` | No CLI deploy needed — SDK connects directly |
| Dashboard | Embeddable iframe | No embed — link to Firebase console |
| SDK | `convex` | `firebase` |
| Env vars | 2 (`CONVEX_URL`, `DEPLOY_KEY`) | 6 (`API_KEY`, `AUTH_DOMAIN`, `PROJECT_ID`, etc.) |

## Notes

1. Firebase project creation via Management API can be slow/async — may need polling or a simpler approach
2. Google OAuth tokens expire (unlike Convex) — `getValidAccessToken()` handles refresh automatically
3. No deploy step needed for Firebase — the SDK connects directly using the config object
4. The `/add-firebase` skill already exists and handles `npm install firebase` + code scaffolding
