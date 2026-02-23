## Console logs

```
client:865 [vite] server connection lost. Polling for restart...
client:935 Refused to create a worker from 'blob:http://localhost:5173/2498e5b9-826c-4c7b-b69b-7e619b0aea84' because it violates the following Content Security Policy directive: "script-src 'self' 'wasm-unsafe-eval' https://us-assets.i.posthog.com". Note that 'worker-src' was not explicitly set, so 'script-src' is used as a fallback.

waitForSuccessfulPing @ client:935
handleMessage @ client:869
await in handleMessage
(anonymous) @ client:458
dequeue @ client:480
(anonymous) @ client:472
enqueue @ client:466
(anonymous) @ client:458
onMessage @ client:305
(anonymous) @ client:426

client:935 Uncaught (in promise) SecurityError: Failed to construct 'SharedWorker': Access to the script at 'blob:http://localhost:5173/2498e5b9-826c-4c7b-b69b-7e619b0aea84' is denied by the document's Content Security Policy.
at waitForSuccessfulPing (client:935:23)
at handleMessage (client:869:12)
waitForSuccessfulPing @ client:935
handleMessage @ client:869
await in handleMessage
(anonymous) @ client:458
dequeue @ client:480
(anonymous) @ client:472
enqueue @ client:466
(anonymous) @ client:458
onMessage @ client:305
(anonymous) @ client:426
```

---



