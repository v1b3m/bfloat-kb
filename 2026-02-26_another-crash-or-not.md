I am randomly seeing a black screen as I work on a project

![[Pasted image 20260226095730.png]]

**Console logs**

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
[Warning] [Layout children]: No route named "index" exists in nested children: – ["modal", "(tabs)"] (2) (entry.bundle, line 53371)
[Warning] props.pointerEvents is deprecated. Use style.pointerEvents (entry.bundle, line 5608)
```

I can access the preview at http://localhost:19000 in an external browser so looks like the "app is still running" despite the black screen?

Also, how does tauri reload the app. In electron, I could just hit `CMD + r`

