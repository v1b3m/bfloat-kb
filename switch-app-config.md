We keep running into issues like these

```
Linking local project to EAS project a543389a-9795-49c2-81ab-5bfb7db47b77
Cannot automatically write to dynamic config at: app.config.js
Error: build command failed.
```

I think we should change the template to away from `app.config.js` to something compatible with the deploy command

---

## Solution Implemented

Convert templates from dynamic `app.config.js` to static `app.json` which EAS CLI can write to directly.

## Changes Made

### Backend (`bfloat-app-engineer`)
- Converted `templates/expo-default/app.config.js` → `app.json`
- Converted `templates/expo-with-convex/app.config.js` → `app.json`
- Removed the `.js` export format, now uses plain JSON

### IDE (`bfloat-ide`)
- Removed workaround code that temporarily renamed `app.config.js` during deployment
- Simplified deployment commands (no more `mv app.config.js app.config.js.bak`)

### Gitea Templates
- Pushed updated templates to Gitea via `setup-templates.ts`
- Uses `--force` to overwrite existing template repos

## What You Need to Do

### 1. Pull Latest Backend Code
```bash
cd /Users/v1b3m/Dev/bfloat/bfloat-app-engineer
git pull origin main  # or your branch
```

### 2. Update Your Local Gitea Templates
Run the setup script to push updated templates to your local Gitea:
```bash
cd /Users/v1b3m/Dev/bfloat/bfloat-app-engineer
GIT_BASE_URL=http://localhost:3001 GIT_ACCESS_TOKEN=<your-token> npx tsx app/scripts/setup-templates.ts
```

Or use your .env file:
```bash
source .env && npx tsx app/scripts/setup-templates.ts
```

### 3. Pull Latest IDE Code
```bash
cd /Users/v1b3m/Dev/bfloat/bfloat-ide
git pull origin chore/claude-deployments  # or main
```

## Notes
- **New projects** will use `app.json` and work seamlessly with EAS
- **Existing projects** with `app.config.js` are still handled by `prepareForDeployment()` which creates an `app.json` alongside it

---

## PR Title
```
Fix: Replace app.config.js with static app.json for EAS CLI compatibility
```
