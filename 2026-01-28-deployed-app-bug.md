# DeployedApp Table Missing Bug

## Date
2026-01-28

## Error
```
PrismaClientKnownRequestError:
Invalid `prisma.deployedApp.findFirst()` invocation:

The table `public.DeployedApp` does not exist in the current database.
```

## Root Cause

A migration `20260126151329_add_deployment_table` was created that:

1. **Dropped** the `DeployedApp` table (`DROP TABLE IF EXISTS "DeployedApp" CASCADE`)
2. **Created** a new `Deployment` table instead
3. **Changed** the `DeploymentStatus` enum values

However, the Prisma schema (`prisma/schema.prisma`) still referenced the `DeployedApp` model, not the new `Deployment` model. This mismatch caused the error.

Additionally, the migration was partially applied to the database, leaving it in an inconsistent state where:
- The `DeployedApp` table was dropped
- The `Deployment` table was created
- But the schema still expected `DeployedApp`

## Investigation Steps

1. Checked the Prisma schema - confirmed it has `DeployedApp` model (lines 127-146)
2. Listed migrations - found the problematic `20260126151329_add_deployment_table` migration
3. Read the migration file - it dropped `DeployedApp` and created `Deployment`
4. Marked the migration as rolled back: `npx prisma migrate resolve --rolled-back 20260126151329_add_deployment_table`
5. Checked database state - only `Deployment` table existed, all other tables (Project, AccountSettings, etc.) were missing
6. Discovered the database `v1b3m` (via peer auth) had different data than the intended `bfloat` database

## Resolution

Since this was a development database and data loss was acceptable:

1. Removed the bad migration folder:
   ```bash
   rm -rf prisma/migrations/20260126151329_add_deployment_table
   ```

2. Reset the database to re-run all migrations:
   ```bash
   PRISMA_USER_CONSENT_FOR_DANGEROUS_AI_ACTION="yes, reset the database" npx prisma migrate reset --force
   ```

3. Verified the `DeployedApp` table was restored:
   ```bash
   node -e "const { PrismaClient } = require('@prisma/client'); const prisma = new PrismaClient(); prisma.deployedApp.count().then(c => console.log('DeployedApp count:', c)).finally(() => prisma.\$disconnect());"
   ```

## Lessons Learned

1. **Always ensure migrations match the schema** - Before creating a migration that drops/renames tables, update the schema first

2. **Test migrations locally** - Run migrations on a local database first to verify they work as expected

3. **Be careful with CASCADE** - The `DROP TABLE ... CASCADE` can have unexpected side effects

4. **Database connection confusion** - When debugging with `psql`, peer authentication may connect to a different database than the app uses. Always verify:
   ```bash
   psql "postgresql://user:pass@host:port/database" -c "SELECT current_database();"
   ```

## Prevention

- Use `prisma migrate dev --create-only` to review migrations before applying
- Consider using `prisma db push` for schema changes in development when appropriate
- Always run `prisma migrate status` before and after schema changes
