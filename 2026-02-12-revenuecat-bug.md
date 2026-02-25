We keep running into this error

```
What does this mean?\n\nStarting project at /Users/v1b3m/.bfloat/projects/387934e0-f899-42a9-bc81-5c9fa9c3595c\nPluginError: Unable to resolve a valid config plugin for react-native-purchases.\n• No "app.plugin.js" file found in react-native-purchases: config plugins are typically exported from an "app.plugin.js" file in the package root.\n• main export of react-native-purchases does not appear to be a config plugin: the following error was thrown when importing /Users/v1b3m/.bfloat/projects/387934e0-f899-42a9-bc81-5c9fa9c3595c/node_modules/react-native-purchases/dist/index.js: Unexpected token 'typeof' \nVerify that react-native-purchases includes a config plugin. If it does not, then remove the entry from plugins in your app config file. Learn more: https://docs.expo.dev/guides/config-plugins/\nSyntaxError: Unexpected token 'typeof'\n    at compileSourceTextModule (node:internal/modules/esm/utils:344:16)\n    at ModuleLoader.importSyncForRequire (node:internal/modules/esm/loader:420:18)\n    at loadESMFromCJS (node:internal/modules/cjs/loader:1565:24)\n    at Module._compile (node:internal/modules/cjs/loader:1716:5)\n    at Object..js (node:internal/modules/cjs/loader:1899:10)\n    at Module.load (node:internal/modules/cjs/loader:1469:32)\n    at Function._load (node:internal/modules/cjs/loader:1286:12)\n    at TracingChannel.traceSync (node:diagnostics_channel:322:14)\n    at wrapModuleLoad (node:internal/modules/cjs/loader:235:24)\n    at Module.require (node:internal/modules/cjs/loader:1491:12)
```

The agent was able to fix it as such

```
The error means react-native-purchases doesn't have a config plugin, so it can't be listed directly in the plugins array of app.json. Let me fix that by removing it:\n\nRead 1 file, Edited 1 file, Ran 1 command\n3\n\nThat's fixed. The react-native-purchases package doesn't ship an Expo config plugin, so it shouldn't be in the plugins array — it only needs to be installed as a dependency (which it already is). The expo-build-properties plugin with the iOS deployment target is still there, which is the important part for ensuring compatibility."
```

Update the skill so this error does not happen again

---

- I keep losing the revenuecat integration in apps
- After leaving the project page, I return and I have to set it up again each time
- 