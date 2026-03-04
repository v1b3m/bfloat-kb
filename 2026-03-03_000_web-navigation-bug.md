**Ref:** develop
**Depends:** none
### Context

The issue in [[2026-03-02_014_first-pricing-navigation-bounces-home]] persists.

**Logs:**
```
(Use `node --trace-warnings ...` to show where the warning was created)
 ✓ Compiled / in 2.1s (586 modules)
 GET / 200 in 2356ms
 ✓ Compiled in 75ms (295 modules)
 GET / 200 in 27ms
 ✓ Compiled /pricing in 145ms (583 modules)
 ⚠ Fast Refresh had to perform a full reload. Read more: https://nextjs.org/docs/messages/fast-refresh-reload
 GET /pricing 200 in 234ms
 GET / 200 in 30ms
```

You can view from the logs, the page moves from `/pricing` to `/` 

**Previous attempt patch**

```diff
commit ac456336749ef64d8d89912ffb379404a5bc73a8
Author: v1b3m <vibenjamin6@gmail.com>
Date:   Mon Mar 2 22:28:22 2026 +0300

    Fix preview route sync to prevent first pricing bounce

diff --git a/app/components/preview/Preview.tsx b/app/components/preview/Preview.tsx
index b7a227b..0919730 100644
--- a/app/components/preview/Preview.tsx
+++ b/app/components/preview/Preview.tsx
@@ -100,6 +100,15 @@ function isSameOriginWithHost(url?: string): boolean {
   }
 }
 
+function normalizePath(path: string): string {
+  return path.startsWith('/') ? path : `/${path}`
+}
+
+function buildRouteUrl(baseUrl: string, nextPath: string): string {
+  const base = new URL(baseUrl, window.location.href)
+  return `${base.origin}${normalizePath(nextPath)}`
+}
+
 export function Preview(props: PreviewProps) {
   const webviewRef = useRef<WebviewElement | null>(null)
   const webIframeRef = useRef<HTMLIFrameElement | null>(null) // For Tauri web preview (replaces webview)
@@ -127,16 +136,34 @@ export function Preview(props: PreviewProps) {
   // Only depends on props.previewUrl to avoid missing updates
   useEffect(() => {
     if (props.previewUrl) {
-      // Only update if the base URL actually changed (different port/host)
-      if (!currentUrl || props.previewUrl !== currentUrl) {
-        console.log('[Preview] Setting preview URL:', props.previewUrl)
-        setCurrentUrl(props.previewUrl)
-        setUrlInput(props.previewUrl)
+      let nextUrl = props.previewUrl
+      let shouldUpdate = false
+      try {
+        const incoming = new URL(props.previewUrl)
+        const existing = currentUrl ? new URL(currentUrl) : null
+        const hasOriginChanged = !existing || existing.origin !== incoming.origin
+
+        if (hasOriginChanged) {
+          shouldUpdate = true
+        } else if (!existing) {
+          shouldUpdate = true
+        } else {
+          nextUrl = `${incoming.origin}${existing.pathname}${existing.search}${existing.hash}`
+          shouldUpdate = nextUrl !== currentUrl
+        }
+      } catch {
+        shouldUpdate = !currentUrl || props.previewUrl !== currentUrl
+      }
+
+      if (shouldUpdate) {
+        console.log('[Preview] Setting preview URL:', nextUrl)
+        setCurrentUrl(nextUrl)
+        setUrlInput(nextUrl)
 
         // Register the preview URL with the sidecar for screenshot capture
         const cwd = workbenchStore.projectPath.getState() || ''
         if (cwd) {
-          screenshot.registerPreviewUrl?.(cwd, props.previewUrl)?.catch(() => {
+          screenshot.registerPreviewUrl?.(cwd, nextUrl)?.catch(() => {
             // Non-critical — screenshot registration failure shouldn't block preview
           })
         }
@@ -245,9 +272,9 @@ export function Preview(props: PreviewProps) {
         if (!nextPath) return
 
         try {
-          const base = new URL(currentUrl || props.previewUrl || window.location.href)
-          const normalizedPath = nextPath.startsWith('/') ? nextPath : `/${nextPath}`
-          setUrlInput(`${base.origin}${normalizedPath}`)
+          const nextUrl = buildRouteUrl(currentUrl || props.previewUrl || window.location.href, nextPath)
+          setCurrentUrl(nextUrl)
+          setUrlInput(nextUrl)
         } catch {
           // Ignore malformed route payloads
         }
@@ -294,8 +321,9 @@ export function Preview(props: PreviewProps) {
 
       if (!iframePath) return
 
-      const base = new URL(currentUrl || props.previewUrl || window.location.href)
-      setUrlInput(`${base.origin}${iframePath.startsWith('/') ? iframePath : `/${iframePath}`}`)
+      const nextUrl = buildRouteUrl(currentUrl || props.previewUrl || window.location.href, iframePath)
+      setCurrentUrl(nextUrl)
+      setUrlInput(nextUrl)
     } catch {
       // Ignore cross-origin or malformed URL failures
     }

```

### Acceptance Criteria

- [x] Page does not bounce back home on first click

### Constraints

- None

### Resources

- [[2026-03-02_014_first-pricing-navigation-bounces-home]]
- 

## Handoff
- **What was done:** Hardened preview URL synchronization in `Preview.tsx` to avoid first-navigation route loss. The `previewUrl` reconciliation effect now tracks `currentUrl` to prevent stale overwrite races, and Tauri iframe `onLoad` now resolves proxy URLs (`/preview-proxy?...target=...`) back to the real app route before syncing state.
- **Commit:** ad3c0ee
- **Files touched:** `app/components/preview/Preview.tsx`
- **Decisions made:** Kept scope strictly in preview host synchronization; no app-page or business-logic changes.
- **Known limitations:** Could not run local lint/typecheck because dependencies/tool binaries are not installed in this worktree (`pnpm exec eslint`/`pnpm exec tsc` fail with command not found).
- **How to verify:** 1) Start a Next.js project preview at `/`. 2) Click a link to `/pricing` as first navigation. 3) Observe compile/full-reload cycle. 4) Confirm preview remains on `/pricing` instead of bouncing to `/`.
