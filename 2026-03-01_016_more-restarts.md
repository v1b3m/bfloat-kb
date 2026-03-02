**Ref:** keep-hacking
**Depends:** 2026-03-01_011_restart-app-mcp

### Context

We need to make app restarts a lot more robust, the first time worked, second failed. The delay does help but we're not yet there.

**Logs**


```
cd "/Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh" && npm install --legacy-peer-deps && PORT=19000 BROWSER=none npx expo start --web --clear --port 19000
➜  ~ cd "/Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh" && npm install --legacy-peer-deps && PORT=19000 BROWSER=none npx expo start --web 
--clear --port 19000
npm warn EBADENGINE Unsupported engine {
npm warn EBADENGINE   package: 'eslint-visitor-keys@5.0.1',
npm warn EBADENGINE   required: { node: '^20.19.0 || ^22.13.0 || >=24' },
npm warn EBADENGINE   current: { node: 'v23.11.1', npm: '10.9.2' }
npm warn EBADENGINE }

up to date, audited 982 packages in 5s

181 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
env: load .env.local .env
env: export EXPO_PUBLIC_REVENUECAT_API_KEY REVENUECAT_API_KEY
Starting project at /Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh
› Port 19000 is running expo-template-default in another window
  /Users/v1b3m/.bfloat-ide/projects/proj_1772219031320_0gwe4murc (pid 80485)
✔ Use port 19001 instead? … yes
React Compiler enabled
Starting Metro Bundler
warning: Bundler cache is empty, rebuilding (this may take a minute)
The following packages should be updated for best compatibility with the installed expo version:
  @react-native-async-storage/async-storage@2.1.2 - expected version: 2.2.0
  expo-audio@55.0.8 - expected version: ~1.1.1
  react-native@0.81.4 - expected version: 0.81.5
Your project may not work correctly until you install the expected versions of the packages.
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
█ ▄▄▄▄▄ █▀▄█▀     █ ▄▄▄▄▄ █
█ █   █ █▄   ▄██▀ █ █   █ █
█ █▄▄▄█ █ ▀█▀█ ▀ ██ █▄▄▄█ █
█▄▄▄▄▄▄▄█ ▀▄█ █ ▀ █▄▄▄▄▄▄▄█
█▄ ▀▄▀█▄▄▀█ ▀▄ ▀██▀  ▄▀▄▄▀█
██ ▀█▀▄▄▀ ▄▄██▀█▀▄▄▀ ▀▀█▄▄█
██ ▀▄ ▀▄▀▄█▀██▄ █▀█ ▄█ ██▀█
█▄▀▄▀▀▀▄███  ▄▀ █ ▄▄ ▀▀██▄█
█▄▄▄▄█▄▄▄ ▄ ▀██▄  ▄▄▄ █ ▄ █
█ ▄▄▄▄▄ █▄▀█▀▄▄▄  █▄█  ▀▄ █
█ █   █ █▀█▀▄█ ▀▀▄ ▄▄ █▀▄██
█ █▄▄▄█ █▀▄▄██ ▄█  █▄  ▄█▄█
█▄▄▄▄▄▄▄█▄▄█▄█▄▄█▄███▄▄█▄▄█

› Metro waiting on exp://192.168.100.175:19001
› Scan the QR code above with Expo Go (Android) or the Camera app (iOS)

› Web is waiting on http://localhost:19001

› Using Expo Go
› Press s │ switch to development build

› Press a │ open Android
› Press i │ open iOS simulator
› Press w │ open web

› Press j │ open debugger
› Press r │ reload app
› Press m │ toggle menu
› shift+m │ more tools
› Press o │ open project code in your editor

› Press ? │ show all commands

Logs for your project will appear below. Press Ctrl+C to exit.
λ Bundled 8462ms node_modules/expo-router/node/render.js (1115 modules)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 9864ms node_modules/expo-router/entry.js (1168 modules)
Web Bundled 4353ms node_modules/expo-router/entry.js (1170 modules)
 LOG  [web] Logs will appear in the browser console
λ Bundled 55ms node_modules/expo-router/node/render.js (1 module)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 465ms node_modules/expo-router/entry.js (1 module)
λ Bundled 20ms node_modules/expo-router/node/render.js (1 module)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 182ms node_modules/expo-router/entry.js (1 module)
› Stopped server
➜  proj_1772354582086_p6j97h7dh cd "/Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh" && npm install --legacy-peer-deps && PORT=19000 BROWSER
=none npx expo start --web --clear --port 19000
npm warn EBADENGINE Unsupported engine {
npm warn EBADENGINE   package: 'eslint-visitor-keys@5.0.1',
npm warn EBADENGINE   required: { node: '^20.19.0 || ^22.13.0 || >=24' },
npm warn EBADENGINE   current: { node: 'v23.11.1', npm: '10.9.2' }
npm warn EBADENGINE }

up to date, audited 982 packages in 3s

181 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
env: load .env.local .env
env: export EXPO_PUBLIC_REVENUECAT_API_KEY REVENUECAT_API_KEY
Starting project at /Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh
› Port 19000 is running expo-template-default in another window
  /Users/v1b3m/.bfloat-ide/projects/proj_1772219031320_0gwe4murc (pid 80485)
✔ Use port 19001 instead? … yes
React Compiler enabled
Starting Metro Bundler
warning: Bundler cache is empty, rebuilding (this may take a minute)
The following packages should be updated for best compatibility with the installed expo version:
  @react-native-async-storage/async-storage@2.1.2 - expected version: 2.2.0
  expo-audio@55.0.8 - expected version: ~1.1.1
  react-native@0.81.4 - expected version: 0.81.5
Your project may not work correctly until you install the expected versions of the packages.
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
█ ▄▄▄▄▄ █▀▄█▀     █ ▄▄▄▄▄ █
█ █   █ █▄   ▄██▀ █ █   █ █
█ █▄▄▄█ █ ▀█▀█ ▀ ██ █▄▄▄█ █
█▄▄▄▄▄▄▄█ ▀▄█ █ ▀ █▄▄▄▄▄▄▄█
█▄ ▀▄▀█▄▄▀█ ▀▄ ▀██▀  ▄▀▄▄▀█
██ ▀█▀▄▄▀ ▄▄██▀█▀▄▄▀ ▀▀█▄▄█
██ ▀▄ ▀▄▀▄█▀██▄ █▀█ ▄█ ██▀█
█▄▀▄▀▀▀▄███  ▄▀ █ ▄▄ ▀▀██▄█
█▄▄▄▄█▄▄▄ ▄ ▀██▄  ▄▄▄ █ ▄ █
█ ▄▄▄▄▄ █▄▀█▀▄▄▄  █▄█  ▀▄ █
█ █   █ █▀█▀▄█ ▀▀▄ ▄▄ █▀▄██
█ █▄▄▄█ █▀▄▄██ ▄█  █▄  ▄█▄█
█▄▄▄▄▄▄▄█▄▄█▄█▄▄█▄███▄▄█▄▄█

› Metro waiting on exp://192.168.100.175:19001
› Scan the QR code above with Expo Go (Android) or the Camera app (iOS)

› Web is waiting on http://localhost:19001

› Using Expo Go
› Press s │ switch to development build

› Press a │ open Android
› Press i │ open iOS simulator
› Press w │ open web

› Press j │ open debugger
› Press r │ reload app
› Press m │ toggle menu
› shift+m │ more tools
› Press o │ open project code in your editor

› Press ? │ show all commands

Logs for your project will appear below. Press Ctrl+C to exit.
λ Bundled 7144ms node_modules/expo-router/node/render.js (1101 modules)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 8414ms node_modules/expo-router/entry.js (1163 modules)
Web Bundled 3066ms node_modules/expo-router/entry.js (1170 modules)
 LOG  [web] Logs will appear in the browser console
λ Bundled 28ms node_modules/expo-router/node/render.js (1 module)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 362ms node_modules/expo-router/entry.js (1 module)
λ Bundled 29ms node_modules/expo-router/node/render.js (1 module)
Web platform detected. Using RevenueCat in Browser Mode.
Web Bundled 346ms node_modules/expo-router/entry.js (1 module)
› Stopped server
➜  proj_1772354582086_p6j97h7dh 
➜  proj_1772354582086_p6j97h7dh cd "/Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh" && npm install --legacy-peer-deps && PORT=19000 BROWSER
=none npx expo start --web --clear --port 19000
cd "/Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh" && npm install --legacy-peer-deps && PORT=19000 BROWSER=none npx expo start --web --clear --port 19000
npm warn EBADENGINE Unsupported engine {
npm warn EBADENGINE   package: 'eslint-visitor-keys@5.0.1',
npm warn EBADENGINE   required: { node: '^20.19.0 || ^22.13.0 || >=24' },
npm warn EBADENGINE   current: { node: 'v23.11.1', npm: '10.9.2' }
npm warn EBADENGINE }

up to date, audited 982 packages in 4s

181 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
env: load .env.local .env
env: export EXPO_PUBLIC_REVENUECAT_API_KEY REVENUECAT_API_KEY
Starting project at /Users/v1b3m/.bfloat-ide/projects/proj_1772354582086_p6j97h7dh
› Port 19000 is running expo-template-default in another window
  /Users/v1b3m/.bfloat-ide/projects/proj_1772219031320_0gwe4murc (pid 80485)
✔ Use port 19001 instead? … no
› Skipping dev server
y                                                                                                                                                          
➜  proj_1772354582086_p6j97h7dh y
zsh: command not found: y
➜  proj_1772354582086_p6j97h7dh 
```

### Acceptance Criteria

- [ ] App restarts work all the time

### Constraints

- None

### Resources

- [[2026-03-01_015_restart-app]]
- 



## Handoff
- **What was done:** Hardened Expo restart prompt handling by resetting port-prompt de-duplication at the start of each Expo startup sequence (`Starting project at ...`). This allows repeated restarts to auto-accept the same fallback port across runs instead of getting stuck on a later restart.
- **Commit:** 344cf26
- **Files touched:** `_SCRATCH.md`; `app/components/workbench/Workbench.tsx`
- **Decisions made:** Kept existing de-duplication (to avoid repeated `y` spam in a single prompt) and only reset state at a clear run boundary.
- **Known limitations:** If Expo changes its startup text and no longer emits `Starting project at`, this reset trigger may miss and require an updated matcher.
- **How to verify:** In a project where Expo asks to switch from port 19000 to 19001, trigger restart repeatedly (including back-to-back). Each run should continue auto-accepting fallback and proceed instead of showing `Skipping dev server`.
