# Expo Onboarding & CLI/Shell Interaction Analysis

## 1. How Expo Onboarding Connects to Expo Account

The **ExpoStep** component (`app/components/onboarding/steps/ExpoStep.tsx`) collects username, password, and optional 2FA code. When submitted, it calls:

```typescript
window.conveyor.provider.connectExpo({ username, password, otp })
```

This triggers an IPC call to the main process where **provider-handler.ts** (`lib/conveyor/handlers/provider-handler.ts`) spawns the CLI command:

```bash
npx expo login -u <username> -p <password> --otp <otp>
```

After login, it reads `~/.expo/state.json` to verify authentication succeeded, then stores the integration metadata in `~/.bfloat/config/settings.json`.

---

## 2. Does the App UI Interact with a Shell/CLI Session?

**Yes**, extensively.

---

## 3. How It Achieves This

The app uses a **three-layer architecture**:

| Layer | Technology | Purpose |
|-------|------------|---------|
| **PTY Backend** | `node-pty` | Spawns actual shell processes with terminal emulation |
| **IPC Bridge** | Electron IPC + Zod schemas | Secure communication between renderer and main process |
| **Terminal UI** | `xterm.js` | Renders terminal output in the browser/React |

### Key Handlers

- **TerminalHandler** (`lib/conveyor/handlers/terminal-handler.ts`) - Creates persistent PTY sessions for interactive terminals
- **ProviderHandler** (`lib/conveyor/handlers/provider-handler.ts`) - Spawns one-off CLI commands (like `expo login`)

### Data Flow

1. UI calls `window.conveyor.terminal.write(id, command)`
2. IPC sends to main process
3. Main process writes to PTY via `ptyProcess.write()`
4. PTY output comes back via `ptyProcess.onData()`
5. Main process broadcasts via `webContents.send('terminal-data', ...)`
6. xterm.js component receives and renders the output

---

## 4. Complete Data Flow Example: Expo Login

```
1. USER ENTERS CREDENTIALS IN ExpoStep UI
   ↓
2. ExpoStep.tsx calls window.conveyor.provider.connectExpo({username, password, otp})
   ↓
3. IPC: Renderer → Main (Electron)
   ↓
4. provider-handler.ts receives 'provider:connect-expo' message
   ↓
5. spawnCliCommand() function:
   - Escapes arguments safely for bash
   - Uses pty.spawn() to launch: npx expo login -u <user> -p <pass> --otp <otp>
   - Sets up PTY with xterm-color emulation
   - Captures output and exit code
   ↓
6. Monitors ~/.expo/state.json for authentication markers
   ↓
7. Saves integration metadata to ~/.bfloat/config/settings.json
   ↓
8. Returns result via IPC to ExpoStep component
   ↓
9. ProviderAuthStore.loadFromStorage() reloads tokens
   ↓
10. UI updates showing "Connected as {username}"
```

---

## 5. Key File Locations

| Component | File Path |
|-----------|-----------|
| Expo Onboarding UI | `/app/components/onboarding/steps/ExpoStep.tsx` |
| CLI Handler (Spawn) | `/lib/conveyor/handlers/provider-handler.ts` |
| Terminal Handler (PTY) | `/lib/conveyor/handlers/terminal-handler.ts` |
| Terminal UI | `/app/components/deploy/DeployTerminal.tsx` |
| IPC Schemas | `/lib/conveyor/schemas/provider-schema.ts` |
| Provider API | `/lib/conveyor/api/provider-api.ts` |
| Auth Store | `/app/stores/provider-auth.ts` |
| IPC Setup | `/lib/main/shared.ts`, `/lib/preload/shared.ts` |

---

## 6. Key Architectural Patterns

| Aspect | Implementation |
|--------|-----------------|
| **PTY Management** | node-pty library with native bindings |
| **Data Forwarding** | PTY output → electron.webContents.send() → React components |
| **User Input** | React component → IPC invoke → PTY write() |
| **Async Handling** | Promise-based with proper cleanup on unmount |
| **Terminal Emulation** | xterm.js for rendering, xterm-256color PTY emulation |
| **Process Tracking** | Map<terminalId, pty.IPty> for lifecycle management |
| **Config Files** | Read from standard CLI config locations (~/.expo, ~/.claude, etc.) |
| **Security** | Bash argument escaping, context isolation, arg validation |
