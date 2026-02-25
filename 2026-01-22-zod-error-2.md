# Bug: IPC Handler Zod Error - Null Return Value

**Date:** 2026-01-22
**Status:** Fixed

## Symptoms

```
IPC Error in user:get-account-settings: ZodError: [
  {
    "expected": "object",
    "code": "invalid_type",
    "path": [],
    "message": "Invalid input: expected object, received null"
  }
]
```

App crashes when trying to fetch account settings for a user who doesn't have any settings configured yet.

## Root Cause

In `lib/conveyor/schemas/user-schema.ts`, the `accountSettingsSchema` was defined as a strict object:

```typescript
const accountSettingsSchema = z.object({
  id: z.string(),
  userId: z.string(),
  theme: z.string(),
  // ...
})
```

When the backend `/api/settings` endpoint returns `null` (no settings exist for the user), the Zod validation fails because the schema expects a non-null object.

## Fix

Made the schema nullable:

```typescript
const accountSettingsSchema = z.object({
  id: z.string(),
  userId: z.string(),
  theme: z.string(),
  // ...
}).nullable()  // Allow null when user has no settings yet
```

**File:** `lib/conveyor/schemas/user-schema.ts`

## Related Backend Issue

During debugging, a separate backend error was discovered:
```
Cannot read properties of undefined (reading 'findFirst')
at Module.getLatestAgentSession (projects.server.ts:172)
```

This was caused by the `AgentSession` Prisma model existing in `schema.prisma` but the Prisma client not being regenerated. Fixed by running:
```bash
npx prisma generate
npx prisma db push
```

## Prevention

When defining Zod schemas for IPC handlers that fetch from backend APIs:
- Consider whether the API can return `null`
- Use `.nullable()` or `.optional()` as appropriate
- Ensure frontend code handles `null` values gracefully
