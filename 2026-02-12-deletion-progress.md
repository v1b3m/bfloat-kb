# Project Deletion Side Effects — Progress Report

When a project is deleted, external resources created for that project should be cleaned up. This document tracks what's implemented and what's missing.

## Implemented

### Convex — Done
- **Utility**: `apps/web/app/utils/convex.ts` → `deleteConvexProject()`
- **API**: `POST https://api.convex.dev/v1/projects/{projectId}/delete`
- **Trigger**: Only if `project.convexProjectId` is set
- **Error handling**: 404 handled gracefully (already deleted)
- **Auth**: `accountSettings.convexAccessToken`

Tested ✅
### Firebase/GCP — Done
- **Utility**: `apps/web/app/utils/firebase.ts` → `deleteGcpProject()`
- **API**: `DELETE https://cloudresourcemanager.googleapis.com/v1/projects/{projectId}` (schedules 30-day deletion)
- **Trigger**: Only if `project.firebaseProjectId` is set
- **Error handling**: 404 and 403 handled gracefully
- **Auth**: Google OAuth token via `getGoogleAccessToken(userId)`

### Supabase — Done
- **Utility**: `apps/web/app/lib/supabase-api.server.ts` → `deleteSupabaseProject()`
- **API**: `DELETE https://api.supabase.com/v1/projects/{projectRef}`
- **Trigger**: Only if `project.supabaseProjectRef` is set
- **Error handling**: 404 and 403 silently handled
- **Auth**: `getValidAccessToken(userId)` from Supabase OAuth

### GitHub — Done
- Deletes the GitHub repository via `gitService.deleteRepo(id)`
- Logs warning if repo doesn't exist, doesn't block deletion

### Vercel — Done
- Deletes Vercel project via `deleteVercelProject(deployment.vercelProjectId)`
- Only if `deployment.vercelProjectId` is set

### Northflank — Done
- Deletes Northflank service via `deleteNorthflankService(deployment.northflankServiceId)`
- Only if `deployment.northflankServiceId` is set

---

## Not Implemented

### RevenueCat — Missing
- **Model**: One RevenueCat account per user, but **per-project apps** are created within it
- **Project field**: `Project.revenuecatProjectId` (unique per project)
- **What to delete**: The RevenueCat app/project associated with this workbench project
- **Auth**: Uses `accountSettings.revenuecatAccessToken` (tokens expire and auto-refresh via `revenuecatRefreshToken`)
- **Action needed**: Create `deleteRevenueCatProject()` utility, add to deletion handler
- **Priority**: High — per-project resource that should be cleaned up

### Stripe — Skipped (by design)
- **Model**: One Stripe account shared across all user's projects (Stripe Connect)
- **Project field**: `Project.stripeConnectedAccountId` (same value across projects)
- **What exists**: The connected account is the user's own Stripe account — should NOT be deleted
- **Products/prices**: Not currently tracked per-project. If the AI creates Stripe products during development, they live in the user's Stripe account with no project-level tracking
- **Action needed**: None for now. The `stripeConnectedAccountId` reference on the Project row cascades with DB deletion. If we later track per-project Stripe resources (products, prices), we'd need cleanup logic
- **Priority**: Low — no per-project resources to clean up currently

---

## Architecture Notes

### Deletion pattern
All cleanup runs as best-effort in parallel after the DB delete:

```
1. Fetch related data (deployments, settings) BEFORE deletion
2. Delete from database (cascades)
3. Best-effort cleanup of external resources via Promise.allSettled
4. Return success regardless of cleanup failures
```

### Key files
| Resource | Creation Route | Deletion Utility |
|----------|---------------|-----------------|
| Convex | `api.projects.$id.setup-convex.ts` | `apps/web/app/utils/convex.ts` |
| Firebase | `api.projects.$id.setup-firebase.ts` | `apps/web/app/utils/firebase.ts` |
| Supabase | `api.projects.$id.setup-supabase.ts` | `apps/web/app/lib/supabase-api.server.ts` |
| Stripe | `api.projects.$id.setup-stripe.ts` | — (not needed) |
| RevenueCat | `api.oauth.revenuecat.connect.tsx` | — (TODO) |

### Deletion handler
`apps/web/app/routes/api.projects.$id.ts` (DELETE handler, lines ~167-303)

### DB schema fields tracking external resources
```
Project.supabaseProjectRef      → deletion implemented
Project.convexProjectId         → deletion implemented
Project.firebaseProjectId       → deletion implemented
Project.stripeConnectedAccountId → no deletion needed (shared account)
Project.revenuecatProjectId     → deletion TODO
```
