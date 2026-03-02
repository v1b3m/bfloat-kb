I am getting another black screen

**Logs:**

```
[Log] [Preview] Current URL: – "http://localhost:19000" – "Status:" – "running" – "AppType:" – "mobile" – "ExpoUrl:" – "exp://192.168.100.175:19000" (Preview.tsx, line 361)
[Error] Error: Invalid negative value for <circle> attribute r="-20" (localhost, line 814)
[Error] Error: Invalid negative value for <circle> attribute r="-20" (localhost, line 814)
[Log] Running application "main" with appParams: (entry.bundle, line 21721)
 (2)
{rootTag: "#root", hydrate: undefined}
"
Development-level warnings: ON.
Performance optimizations: OFF."
[Log] Web platform detected. Using RevenueCat in Browser Mode. (entry.bundle, line 73911)
[Warning] props.pointerEvents is deprecated. Use style.pointerEvents (entry.bundle, line 5608)
[Warning] [IconSymbol] Missing mapping for "timer.circle.fill", using "help-outline". (entry.bundle, line 48620)
[Warning] [IconSymbol] Missing mapping for "star.fill", using "help-outline". (entry.bundle, line 48620)
[Error] Unhandled Promise Rejection: Error: There is no singleton instance. Make sure you configure Purchases before trying to get the default instance. More info here: https://errors.rev.cat/configuring-sdk
	initPurchases (entry.bundle:75655)
[Warning] Animated: `useNativeDriver` is not supported because the native animated module is missing. Falling back to JS-based animation. To resolve this, add `RCTAnimation` module to this app, or remove `useNativeDriver`. Make sure to run `bundle exec pod install` first. Read more about autolinking: https://github.com/react-native-community/cli/blob/master/docs/autolinking.md (entry.bundle, line 16254)
[Log] [useLocalAgent] Unmounting - detaching from session: – null (useLocalAgent.ts, line 212)
[Log] [Workbench] Running cleanup after 882302ms (Workbench.tsx, line 606)
[Log] [Terminal UI] Killing terminal: terminal-1 (Terminal.tsx, line 201)
[Log] [Terminal UI] Disposing xterm UI for: terminal-1 (Terminal.tsx, line 98)
[Log] [ProjectPage] ===== UNMOUNTING (real) ===== (ProjectPage.tsx, line 122)
```

---

The app keeps crashing (black screen) for no reason, no error logs or whatever, could this be an issue with  opening dev tools. Is it possible to route logs to a file which I can then tail in my terminal?

----

