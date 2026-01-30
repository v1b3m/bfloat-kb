## Progress

### Current Feature: iOS App Store Deployment

We are building an automated "Deploy to iOS App Store" feature that streamlines publishing Expo apps to TestFlight/App Store.

---

## Architecture Overview

### Key Files

| File | Purpose |
|------|---------|
| `app/components/deploy/DeployiOSSection.tsx` | Main iOS deploy button component in the deploy modal |
| `app/components/deploy/IOSSetupWizard.tsx` | First-time setup wizard (EAS init, Apple credentials, etc.) |
| `app/components/deploy/IOSDeployModals.tsx` | Renders modals at top level to avoid unmounting issues |
| `app/components/deploy/EmbeddedTerminal.tsx` | Terminal component for interactive credential setup |
| `app/components/deploy/DeployProgressModal.tsx` | Progress UI for automated deployments |
| `app/utils/background-deploy.ts` | Runs iOS deployment in hidden terminal with progress callbacks |
| `app/utils/eas-output-parser.ts` | Parses EAS CLI output to extract progress for UI |
| `app/utils/ios-credentials.ts` | Checks iOS credential status (Expo token, EAS project, certs) |

### Deployment Flow

```
User clicks "Publish to iOS App Store"
           │
           ▼
   Check credential status
           │
           ├─── First build? ───► IOSSetupWizard
           │                            │
           │                      Steps: 1) Expo account
           │                             2) EAS init
           │                             3) Apple credentials (2FA)
           │                             4) Build & Submit to TestFlight
           │                            │
           │                            ▼
           │                     Setup complete
           │
           ├─── Has ascAppId? ───► Background deployment with progress UI
           │
           └─── No ascAppId? ───► Interactive terminal (user selects App Store app)
```

### Two-Phase Approach

1. **First-time setup** (`IOSSetupWizard`):
   - Guides user through Expo connection, EAS init, Apple credentials
   - Uses interactive terminal for Apple 2FA authentication
   - Final step runs `npx -y testflight` to build and submit

2. **Subsequent deploys** (`runBackgroundDeployment`):
   - Fully automated with progress UI
   - Runs in hidden terminal, parses output for progress updates
   - Shows steps: Prepare → Upload → Build → Submit → Complete

---

## Current Bug: EAS Queue/Upload Hanging

### Issue Description (from 013.md)

**Run #0: Build Queue Wait**
```
Waiting for build to complete. You can press Ctrl+C to exit.
  Build queued...

Start builds sooner in the priority queue.
Sign up for a paid plan at https://expo.dev/accounts/ben_afloat/settings/billing

Waiting in Free tier queue
|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■ | starting in a.
```

The build gets stuck waiting in EAS's free tier queue, which can take a long time.

**Run #1: Upload Stuck**
```
Compressing project files and uploading to EAS Build. Learn more: https://expo.fyi/eas-build-archive
⠦ Uploading to EAS Build (31.2 KB / 31.2 KB)
```

The upload appears to be complete (31.2 KB / 31.2 KB) but the process hangs indefinitely.

### Root Cause Analysis

1. **Queue waiting**: EAS free tier has a queue. The process waits for a slot to become available. This is expected behavior but can take minutes or even hours.

2. **Upload hanging**: The upload shows 100% complete but the process doesn't proceed. This could be:
   - A network issue where the final ACK isn't received
   - EAS server-side processing delay
   - A bug in the EAS CLI spinner not detecting completion

### Current Timeout Handling

In `background-deploy.ts`:
```typescript
const maxChecks = 600 // 10 minutes with 1s interval

// Timeout handling:
if (checkCount >= maxChecks) {
  // If we have a build URL, consider it a partial success
  if (buildUrl) {
    resolve({ success: true, buildUrl })
  } else {
    resolve({ success: false, error: 'Deployment timed out' })
  }
}
```

Issues with current approach:
1. 10 minutes is too short for free tier queue wait
2. No user feedback during queue wait
3. No way to distinguish "stuck" from "waiting in queue"

---

## Status

**Current state**: We have a working iOS deployment flow but it can hang indefinitely when:
1. Waiting in EAS free tier queue
2. Upload appears stuck at 100%

**Next step**: Implement better handling for these long-wait scenarios.

---

## Plan to Resolve the Bug

### Problem Summary

The iOS deployment can hang in two scenarios:
1. **EAS Queue Wait** - Free tier builds wait in queue (minutes to hours)
2. **Upload Stuck** - Upload appears complete but process hangs

### Proposed Solutions

#### Option A: Detect Queue State and Show Appropriate UI (Recommended)

Add patterns to detect queue state and surface this to the user:

1. **Add queue detection patterns** to `eas-output-parser.ts`:
   ```typescript
   // Add a new step: 'queued'
   { pattern: /Waiting in Free tier queue/i, step: 'queued', percent: 22, message: 'Waiting in build queue...' },
   { pattern: /Build queued/i, step: 'queued', percent: 20, message: 'Build queued...' },
   ```

2. **Update progress UI** to show queue state:
   - Show "Waiting in queue" with estimated wait time (if available)
   - Add a "Continue in background" button that closes the modal but keeps build running
   - Show a notification when build completes

3. **Increase timeout** for queued builds:
   - Change from 10 minutes to 60 minutes for queued builds
   - Reset timeout when progress is detected

4. **Handle upload stuck**:
   - Detect "Uploading to EAS Build" with upload complete (X KB / X KB)
   - If no progress after 2 minutes in this state, show warning with option to:
     - Cancel and retry
     - Continue waiting

#### Option B: Use `--no-wait` Flag

Run the build without waiting for completion:
```bash
npx -y eas-cli build --platform ios --profile production --no-wait
```

Pros:
- Never hangs, returns immediately with build URL
- User can check build status on Expo dashboard

Cons:
- Requires separate submission step after build completes
- Less integrated UX

#### Option C: Poll EAS API for Build Status

Instead of parsing terminal output, poll the EAS API directly:

1. Start build with `--no-wait` to get build ID
2. Poll `https://api.expo.dev/v2/builds/{buildId}` for status
3. When build completes, run `eas submit --latest`

Pros:
- More reliable than output parsing
- Accurate progress/status

Cons:
- Requires EAS API token
- More complex implementation

### Recommended Implementation (Option A)

**Phase 1: Better Queue Handling**

1. Add `'queued'` step to `DeployStep` type in `eas-output-parser.ts`
2. Add queue detection patterns
3. Update progress UI to show queue state with:
   - "Your build is waiting in the EAS queue"
   - "Free tier builds may take longer during peak hours"
   - Link to Expo billing for paid plans
4. Add "Minimize" button that keeps build running but closes modal
5. Store active deployment in persistent state so it survives page refresh

**Phase 2: Upload Stuck Detection**

1. Track time spent in each state
2. If upload shows 100% complete for > 2 minutes:
   - Show warning: "Upload appears stuck"
   - Offer retry or continue waiting options
3. If no progress for > 5 minutes in any state:
   - Show warning with cancel/retry options

**Phase 3: Background Completion Notification**

1. When modal is minimized, show a small status indicator in the UI
2. When build completes, show a toast notification
3. Store build result so user can see it when they return

### Files to Modify

| File | Changes |
|------|---------|
| `app/utils/eas-output-parser.ts` | Add 'queued' step, queue detection patterns |
| `app/components/deploy/DeployProgressModal.tsx` | Update UI for queue state, add minimize button |
| `app/utils/background-deploy.ts` | Improve timeout logic, track state duration |
| `app/stores/deploy.ts` | Add persistent deployment state |

### Implementation Priority

1. **High**: Add queue detection and improve UI messaging
2. **Medium**: Add minimize/background functionality
3. **Low**: Add persistent state and notifications

---

## Implemented: EAS Account Switching

### Feature Overview

Users can now switch between personal and organization Expo accounts for builds. This allows users with paid organization accounts to use priority builds instead of waiting in the free tier queue.

### New Files

| File | Purpose |
|------|---------|
| `app/utils/eas-accounts.ts` | Utility to fetch user's accounts/organizations from Expo API |
| `app/components/deploy/EasAccountSelector.tsx` | Dropdown component for selecting build account |

### Modified Files

| File | Changes |
|------|---------|
| `app/stores/deploy.ts` | Added `easAccounts`, `selectedEasAccount`, `isLoadingAccounts` atoms and methods |
| `app/components/deploy/DeployiOSSection.tsx` | Added account selector UI and free tier warning |
| `app/components/deploy/IOSDeployModals.tsx` | Sets `expo.owner` in app.json before build based on selected account |

### How It Works

1. **Account Fetching**: When Expo is connected, `fetchEasAccounts()` fetches user's personal and organization accounts from the Expo API
2. **Account Display**: `EasAccountSelector` shows a dropdown with all available accounts, including subscription tier badges (Free/Pro/Enterprise)
3. **Priority Indicator**: Accounts with paid plans show a lightning bolt icon indicating priority builds
4. **Free Tier Warning**: If user has a paid account but is using free tier, a warning is shown suggesting they switch
5. **Owner Setting**: Before each build, `setProjectOwner()` updates `expo.owner` in app.json to match the selected account
6. **Persistence**: Selected account is saved to localStorage and restored on next session

### API Integration

```typescript
// Fetch accounts from Expo API
const result = await fetchEasAccounts(accessToken)
// Returns: { success: true, accounts: EasAccount[], currentAccount: string }

// Update project owner
await setProjectOwner(projectPath, 'organization-name')
// Updates app.json: { expo: { owner: 'organization-name' } }
```

### UI Layout

```
┌─────────────────────────────────────────────────┐
│  iOS Deployment                                 │
├─────────────────────────────────────────────────┤
│  [Smartphone Icon] iOS          [Publish Button]│
│                                                 │
│  Build Account                                  │
│  ┌─────────────────────────────────────────┐   │
│  │ 👤 ben_afloat [Free]               ▼    │   │
│  └─────────────────────────────────────────┘   │
│     └─ 🏢 bfloat-team [Pro] ⚡                 │
│                                                 │
│  ⚠️ Free tier builds may wait in queue.        │
│     Switch to a paid account for priority.     │
└─────────────────────────────────────────────────┘
```


