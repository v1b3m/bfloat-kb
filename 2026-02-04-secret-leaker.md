# IDE Environment Variables Leaking to User Projects

## Problem

The bfloat IDE's own environment variables (including sensitive secrets like `STRIPE_SECRET_KEY`) were leaking into user projects, causing user apps to use the IDE's credentials instead of their own.

### Symptoms

1. User configures Stripe keys in their project's `.env` file
2. Project's `.env` has correct key: `sk_test_51Q0rBpFWPtXFqtnw...` (user's account)
3. At runtime, app receives wrong key: `sk_test_51PnEAKFOEd8SI4UI...` (IDE's account)
4. Stripe API calls fail with "No such price" errors because resources exist in different accounts

### Debugging Steps Taken

1. Created `/api/stripe-debug` endpoint to log environment variables at runtime
2. Compared runtime values with `.env` file values
3. Discovered mismatch: runtime `STRIPE_SECRET_KEY` had different prefix than `.env` file
4. Traced the leak to the terminal handler

## Root Cause

**Location**: `/Users/v1b3m/Dev/bfloat/bfloat-ide/lib/conveyor/handlers/terminal-handler.ts`

The terminal handler was spreading the entire `process.env` into child processes:

```typescript
// BEFORE (BROKEN):
const env: Record<string, string> = {
  ...process.env as Record<string, string>,  // <-- Leaked ALL IDE secrets!
  TERM: 'xterm-256color',
  // ...
}
```

### How the leak happened:

1. IDE loads its `.env` via `dotenv.config()` in `lib/main/main.ts`
2. IDE's `.env` contains `STRIPE_SECRET_KEY=sk_test_51PnEAKFOEd8SI4UI...`
3. When terminal/PTY is created, it spreads `process.env` to child processes
4. Child process (Next.js dev server) inherits IDE's env vars
5. IDE's `STRIPE_SECRET_KEY` overrides the project's `.env` value
6. App uses wrong Stripe account

## The Fix

Changed terminal handler to use a **whitelist approach** instead of spreading all env vars:

```typescript
// AFTER (FIXED):
const systemEnv = process.env as Record<string, string>
const env: Record<string, string> = {
  // Essential system paths and settings
  PATH: systemEnv.PATH || '',
  HOME: os.homedir(),
  USER: systemEnv.USER || systemEnv.USERNAME || '',
  SHELL: systemEnv.SHELL || shell,
  LANG: systemEnv.LANG || 'en_US.UTF-8',
  // ... only essential system variables
  // NO spreading of process.env!
}
```

### Variables now whitelisted:

**Essential System:**
- `PATH`, `HOME`, `USER`, `SHELL`, `LANG`, `LC_ALL`, `LC_CTYPE`

**Terminal Configuration:**
- `TERM`, `COLORTERM`, `SHLVL`, `TERM_PROGRAM`, `PS1`

**Windows-specific:**
- `SYSTEMROOT`, `COMSPEC`, `PROGRAMFILES`, `APPDATA`, `LOCALAPPDATA`, `USERPROFILE`, `TEMP`, `TMP`

**macOS-specific:**
- `TMPDIR`, `XPC_FLAGS`, `XPC_SERVICE_NAME`

**Node.js paths:**
- `NVM_DIR`, `NVM_BIN`, `NODE_PATH`

**Non-sensitive preferences:**
- `EDITOR`, `VISUAL`

## Key Takeaways

1. **Never spread `process.env` into child processes** - use explicit whitelist
2. **IDE secrets must be isolated** - they should never reach user project runtimes
3. **Debug by logging actual runtime values** - comparing with config files reveals injection issues
4. **dotenv loads into process.env globally** - any child process inherits these unless explicitly filtered

## Files Modified

- `/Users/v1b3m/Dev/bfloat/bfloat-ide/lib/conveyor/handlers/terminal-handler.ts` - Fixed env var leakage

## Related Issue

See also: `stripe-mismatch.md` - Documents the initial symptom (Stripe MCP vs app key mismatch) before root cause was found
