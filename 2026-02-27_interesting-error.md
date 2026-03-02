When I take a screenshot of the app is shows the running app, I am not sure how it is doing this given the preview is not rendering correctly

**Preview:**

![[Pasted image 20260227185715.png]]

**Console logs:**

```
[Error] Failed to load resource: the server responded with a status of 404 (Not Found) (preview-proxy, line 0)
[Log] Running application "main" with appParams: (entry.bundle, line 21721)
 (2)
{rootTag: "#root", hydrate: undefined}
"
Development-level warnings: ON.
Performance optimizations: OFF."
[Warning] "shadow*" style props are deprecated. Use "boxShadow". (entry.bundle, line 5608)
[Warning] "textShadow*" style props are deprecated. Use "textShadow". (entry.bundle, line 5608)
[Warning] Image: style.resizeMode is deprecated. Please use props.resizeMode. (entry.bundle, line 5608)
[Error] WebSocket connection to 'ws://127.0.0.1:61919/message' failed: There was a bad response from the server.
[Error] WebSocket connection to 'ws://127.0.0.1:61919/hot' failed: There was a bad response from the server.
```

