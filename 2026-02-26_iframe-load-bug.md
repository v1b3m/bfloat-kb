There's this error in the IDE console

```
[Error] Blocked a frame with origin "http://localhost:1420" from accessing a frame with origin "http://localhost:19000". Protocols, domains, and ports must match.
	(anonymous function) (Preview.tsx:268)
	executeDispatch (react-dom_client.js:13622)
	runWithFiberInDEV (react-dom_client.js:999)
	processDispatchQueue (react-dom_client.js:13658)
	(anonymous function) (react-dom_client.js:14071)
	batchedUpdates$1 (react-dom_client.js:2626)
	dispatchEventForPluginEventSystem (react-dom_client.js:13763)
	dispatchEvent (react-dom_client.js:16784)
```

## Investigation (2026-02-26)
- Root cause: web-preview controls in Tauri attempted iframe DOM/history access even when the preview URL was cross-origin (`localhost:1420` host vs `localhost:19000` app).
- Effect: browser logs security errors when handlers run via React event dispatch.
- Fix applied in code:
  - `app/components/preview/Preview.tsx`: added `isSameOriginWithHost()` guard and skip iframe access (`history.back/forward`, `location.reload`, mobile viewport DOM injection) when cross-origin.
  - `app/components/preview/Preview.tsx`: removed invalid `allow-presentation` sandbox token from iframe attributes.
  - `app/components/ai-elements/web-preview.tsx`: removed invalid `allow-presentation` sandbox token.
- Validation: `pnpm eslint app/components/preview/Preview.tsx app/components/ai-elements/web-preview.tsx` completed with 0 errors (existing `no-console` warnings remain in Preview).


## Commit
- 9c5b0ec - Guard cross-origin iframe access and remove invalid sandbox flags from preview iframes.