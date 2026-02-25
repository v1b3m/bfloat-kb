# Bug: New Expo projects fail to start with BABEL error

## Error

```
ERROR  Error: [BABEL]: Cannot find module 'react-native-worklets/plugin'
Require stack:
- node_modules/react-native-reanimated/plugin/index.js
```

## Root Cause

The `react-native-reanimated` package (used by `expo-router`) requires `react-native-worklets` as a peer dependency. The Gitea template repository (`bfloat-templates/expo-default`) was missing this dependency in its `package.json`.

When projects were forked from this template, `npm install` would install `react-native-reanimated` but skip its peer dependency `react-native-worklets`, causing the Babel plugin to fail.

## Solution

The source template at `bfloat-app-engineer/templates/expo-default/package.json` already included the dependency. The Gitea template was out of sync.

Synced templates to Gitea by running:

```bash
cd bfloat-app-engineer
GIT_BASE_URL="http://localhost:3001" \
GIT_ACCESS_TOKEN="<token>" \
npx tsx app/scripts/setup-templates.ts
```

The `setup-templates.ts` script pushes template files from the source directories to Gitea, ensuring consistency across all environments.

## Dependency Added

```json
"react-native-worklets": "~0.4.0"
```

## Verification

After the fix, new projects forked from the Gitea template will include `react-native-worklets` and start without Babel errors.
