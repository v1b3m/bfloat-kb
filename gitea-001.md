When I create a new project in the IDE, the server throws the error below when it is opened:

```
[2026-01-26 13:38:55.589 +0300] ERROR: Git API request failed
    host: "MacBook-Pro.local"
    status: 404
    url: "http://localhost:3001/api/v1/repos/bfloat-projects/c929b30e-8767-43f0-bda6-11218e65a327/contents?ref=main"
    error: "{\"errors\":null,\"message\":\"The target couldn't be found.\",\"url\":\"http://localhost:3001/api/swagger\"}\n"
[2026-01-26 13:38:55.590 +0300] ERROR: Error fetching project:
    host: "MacBook-Pro.local"
    err: {
      "type": "TypeError",
      "message": "Cannot read properties of undefined (reading 'findUnique')",
      "stack":
          TypeError: Cannot read properties of undefined (reading 'findUnique')
              at loader (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/app/routes/api.projects.$id.ts:255:73)
              at processTicksAndRejections (node:internal/process/task_queues:105:5)
              at callRouteHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:508:16)
              at commonRoute.loader (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:658:19)
              at file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4762:19
              at callLoaderOrAction (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4814:16)
              at async Promise.all (index 0)
              at defaultDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4439:17)
              at callDataStrategyImpl (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4708:17)
              at callDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3748:19)
    }
[2026-01-26 13:39:33.130 +0300] INFO: Icon suggestions from LLM:
    0: "mdi/timer"
    1: "heroicons/clock"
    2: "lucide/clock"
    3: "tabler/timer"
    4: "mdi/clock-outline"
    host: "MacBook-Pro.local"
[2026-01-26 13:39:34.277 +0300] INFO:
    host: "MacBook-Pro.local"
    statusCode: 200
    iconName: "mdi/timer"
    iconUrl: "https://api.iconify.design/mdi/timer.svg?width=512&height=512"
    contentType: "image/svg+xml; charset=utf-8"
[2026-01-26 13:39:34.771 +0300] INFO: Created project from template
    host: "MacBook-Pro.local"
    projectId: "1d8eabcb-7c91-4e37-b565-b8699e3bd234"
    templateId: "expo-default"
    repoName: "1d8eabcb-7c91-4e37-b565-b8699e3bd234"
[2026-01-26 13:39:34.805 +0300] INFO: Git repo created
    host: "MacBook-Pro.local"
    projectId: "1d8eabcb-7c91-4e37-b565-b8699e3bd234"
    sourceUrl: "http://localhost:3001/bfloat-projects/1d8eabcb-7c91-4e37-b565-b8699e3bd234.git"
[2026-01-26 13:39:35.996 +0300] ERROR: Error fetching project:
    host: "MacBook-Pro.local"
    err: {
      "type": "TypeError",
      "message": "Cannot read properties of undefined (reading 'findUnique')",
      "stack":
          TypeError: Cannot read properties of undefined (reading 'findUnique')
              at loader (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/app/routes/api.projects.$id.ts:255:73)
              at processTicksAndRejections (node:internal/process/task_queues:105:5)
              at callRouteHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:508:16)
              at commonRoute.loader (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:658:19)
              at file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4762:19
              at callLoaderOrAction (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4814:16)
              at async Promise.all (index 0)
              at defaultDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4439:17)
              at callDataStrategyImpl (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4708:17)
              at callDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3748:19)
```

The server is at `/Users/v1b3m/Dev/bfloat/bfloat-app-engineer`

---

## Fix

### Root Cause

The error `Cannot read properties of undefined (reading 'findUnique')` occurred because:

1. **Missing Prisma Client Generation**: The `Deployment` model existed in `prisma/schema.prisma` but the generated Prisma client (`node_modules/.prisma/client`) didn't include it. When you add a model to the schema, you must run `prisma generate` to regenerate the client.

2. **Database Out of Sync**: The `Deployment` table didn't actually exist in the PostgreSQL database.

3. **Legacy Schema Conflicts**: The database had an old `DeployedApp` table with enum values (`in_progress`, `aborted`, `successful`) that conflicted with the new `DeploymentStatus` enum (`pending`, `building`, `deploying`, `live`, `failed`).

### Resolution Steps

A migration file has been created at `prisma/migrations/20260126151329_add_deployment_table/migration.sql`. To fix this issue in your environment:

**For a fresh database:**
```bash
cd /path/to/bfloat-app-engineer
npx prisma migrate deploy
npx prisma generate
```

**For an existing database with drift (will delete all data):**
```bash
# Drop and recreate database
psql postgres://postgres@localhost:5432/postgres << 'EOF'
DROP DATABASE IF EXISTS bfloat;
CREATE DATABASE bfloat;
EOF

# Apply all migrations
cd /path/to/bfloat-app-engineer
npx prisma migrate deploy
npx prisma generate
```

**For an existing database that you want to preserve:**
If your database already has the `Deployment` table but the migration history is out of sync, mark the migration as applied:
```bash
npx prisma migrate resolve --applied 20260126151329_add_deployment_table
```

### Preventing This Issue

After modifying `prisma/schema.prisma`, always run:
```bash
npx prisma generate
```

And for schema changes that need migrations:
```bash
npx prisma migrate dev --name descriptive_name
```

### Migration Details

The migration file `20260126151329_add_deployment_table/migration.sql`:

1. Drops the legacy `DeployedApp` table (if exists)
2. Drops old enum types (`DeploymentStatus`, `DeploymentPlatform`)
3. Creates new enums with correct values:
   - `DeploymentPlatform`: `web`, `ios`, `android`
   - `DeploymentStatus`: `pending`, `building`, `deploying`, `live`, `failed`
1. Creates the `Deployment` table with all fields, indexes, and foreign keys

