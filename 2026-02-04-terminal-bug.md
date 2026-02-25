# Terminal PTY Spawn Failure Bug

## Issue

Terminal creation failed with `posix_spawnp failed` error:

```
[Terminal] Failed to create PTY: Error: posix_spawnp failed.
    at new UnixTerminal (node_modules/node-pty/lib/unixTerminal.js:92:24)
    at Module.spawn (node_modules/node-pty/lib/index.js:30:12)
```

## Root Cause

The `node-pty` native module was not properly rebuilt for Electron's Node.js ABI (Application Binary Interface). Native modules compiled for standard Node.js are incompatible with Electron's embedded Node.js version.

## Resolution

Rebuild native modules for Electron:

```bash
npx @electron/rebuild
```

This is normally handled by the postinstall script:

```json
"postinstall": "electron-builder install-app-deps && npx @electron/rebuild"
```

## When This Happens

- After pulling new changes
- After switching Node.js versions
- After updating Electron version
- After fresh `npm install` if postinstall fails silently

## Verification

Check that the native module architecture matches your system:

```bash
file node_modules/node-pty/build/Release/pty.node
uname -m
```

Both should show the same architecture (e.g., `arm64` on Apple Silicon).

## Related Fix

Added validation in `terminal-handler.ts` to provide better error messages:
- Validates working directory exists (falls back to home)
- Validates shell binary exists (tries fallback shells)
- Catches spawn errors with detailed logging

## Date Resolved

2025-02-04
