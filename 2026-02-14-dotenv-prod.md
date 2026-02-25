1. These are the variables used to build the prod images
2. We might need to improve the way we load these inside the actions build step
3. Adding tests, up in line

```
# Backend URL for API calls
# For local development, use localhost. For production, use [https://bfloat.ai](https://bfloat.ai)
BFLOAT_BACKEND_URL=[https://bfloat.dev](https://bfloat.dev)

# Claude Agent SDK - Route through backend proxy (API key injected server-side)
# For local development, use localhost. For production, use [https://bfloat.ai](https://bfloat.ai)
ANTHROPIC_BASE_URL=[https://bfloat.dev/api/claude-proxy](https://bfloat.dev/api/claude-proxy)
ANTHROPIC_API_KEY=placeholder

NODE_ENV=production

# Renderer (Vite) - must be VITE_ prefixed to be available in the renderer process
VITE_BFLOAT_BACKEND_URL=[https://bfloat.dev](https://bfloat.dev)

# LaunchDarkly Feature Flags
VITE_LD_CLIENT_SIDE_ID=6977612260ab8609eed2df50

# PostHog Analytics
VITE_POSTHOG_KEY=phc_2eGSmu0yP5HhgzeF0ZRPhZDQ8BidmIesg55hK65MBRf
POSTHOG_API_KEY=phc_2eGSmu0yP5HhgzeF0ZRPhZDQ8BidmIesg55hK65MBRf

# Sentry Error Tracking
VITE_SENTRY_DSN=[https://43ca3e399b382eeb48610c4c1b847465@o4508803582328833.ingest.us.sentry.io/4510882893922304](https://43ca3e399b382eeb48610c4c1b847465@o4508803582328833.ingest.us.sentry.io/4510882893922304)
SENTRY_DSN=[https://43ca3e399b382eeb48610c4c1b847465@o4508803582328833.ingest.us.sentry.io/4510882893922304](https://43ca3e399b382eeb48610c4c1b847465@o4508803582328833.ingest.us.sentry.io/4510882893922304)
```


