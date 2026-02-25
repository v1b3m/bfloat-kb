# Stripe MCP Integration for Bfloat IDE

## Overview

This document describes the implementation of custom Stripe MCP (Model Context Protocol) tools that enable Claude Code to manage Stripe resources (products, prices) on behalf of users who connected their Stripe account via Stripe Connect OAuth. The goal is to allow users to create and manage Stripe products directly through the AI agent without ever visiting the Stripe Dashboard.

## Architecture

```
bfloat-ide (Electron App / Claude Code)
       │
       ├─ useLocalAgent hook passes auth token to session
       │
       ▼
AI Agent Handler (Main Process)
       │
       ├─ Fetches user's Stripe OAuth token from app-engineer API
       │
       ▼
SDK MCP Server (in-process)
       │
       ├─ tool: create_stripe_product
       ├─ tool: create_stripe_price
       ├─ tool: list_stripe_products
       └─ tool: list_stripe_prices
       │
       ▼ (uses fetched OAuth token)
    Stripe API
       │
       ▼
    User's Connected Stripe Account
```

## Implementation Approach

We use the Claude Agent SDK's `createSdkMcpServer` and `tool` helpers to create an in-process SDK MCP server. This approach:
- Runs in the same process as the agent (no separate server needed)
- Tools are automatically available to Claude when the session starts
- User's OAuth token is securely passed and never persisted

## Tools Implemented

| Tool | Description | Parameters |
|------|-------------|------------|
| `create_stripe_product` | Create a product in user's Stripe account | `name`, `description?`, `metadata?` |
| `create_stripe_price` | Create a price for a product | `productId`, `unitAmount`, `currency`, `recurring?`, `metadata?` |
| `list_stripe_products` | List products in the account | `limit?`, `active?` |
| `list_stripe_prices` | List prices, optionally filtered by product | `productId?`, `limit?`, `active?` |

## Files Modified/Created

### app-engineer (Backend)

| File | Status | Purpose |
|------|--------|---------|
| `app/routes/api.stripe.token.ts` | **NEW** | API endpoint to retrieve user's Stripe OAuth credentials |

### bfloat-ide (Electron App)

| File | Status | Purpose |
|------|--------|---------|
| `lib/mcp/stripe-mcp-server.ts` | **NEW** | SDK MCP server with Stripe tools |
| `lib/mcp/index.ts` | **NEW** | Exports MCP server factory |
| `lib/conveyor/handlers/stripe-handler.ts` | **NEW** | Main process handler to fetch Stripe token from backend |
| `lib/conveyor/api/stripe-api.ts` | **NEW** | Renderer-side API client for Stripe |
| `lib/conveyor/schemas/stripe-schema.ts` | **NEW** | Zod schemas for Stripe IPC |
| `lib/agents/types.ts` | **MODIFIED** | Added `McpServerConfig` interface and `mcpServers`/`authToken` to session options |
| `lib/agents/providers/claude-provider.ts` | **MODIFIED** | Added MCP server passthrough to SDK options |
| `lib/conveyor/handlers/ai-agent-handler.ts` | **MODIFIED** | Auto-attaches Stripe MCP server when session created with auth token |
| `app/hooks/useLocalAgent.ts` | **MODIFIED** | Passes auth token when creating sessions |
| `lib/conveyor/schemas/ai-agent-schema.ts` | **MODIFIED** | Added `authToken` to session options schema |
| `lib/conveyor/api/index.ts` | **MODIFIED** | Added StripeApi export |
| `lib/conveyor/schemas/index.ts` | **MODIFIED** | Added stripeApiSchema |
| `lib/main/app.ts` | **MODIFIED** | Registered Stripe handlers |
| `package.json` | **MODIFIED** | Added `stripe` dependency |
| `resources/skills/claude/skills/add-stripe/SKILL.md` | **MODIFIED** | Updated with MCP tools documentation |
| `lib/conveyor/handlers/terminal-handler.ts` | **MODIFIED** | Prevent IDE secrets from leaking into user terminals |

## Key Code Patterns

### Creating the MCP Server

```typescript
// lib/mcp/stripe-mcp-server.ts
import { createSdkMcpServer, tool } from '@anthropic-ai/claude-agent-sdk'
import { z } from 'zod/v4'
import Stripe from 'stripe'

export function createStripeMcpServer(credentials: StripeCredentials) {
  const stripe = new Stripe(credentials.accessToken, {
    apiVersion: '2026-01-28.clover',
  })

  return createSdkMcpServer({
    name: 'stripe',
    version: '1.0.0',
    tools: [
      tool(
        'create_stripe_product',
        "Create a product in the user's connected Stripe account",
        {
          name: z.string().describe('Product name'),
          description: z.string().optional(),
        },
        async (args) => {
          const product = await stripe.products.create({
            name: args.name,
            description: args.description,
          })
          return {
            content: [{ type: 'text', text: JSON.stringify({ success: true, product }) }],
          }
        }
      ),
      // ... more tools
    ],
  })
}
```

### Attaching MCP Server to Sessions

```typescript
// lib/conveyor/handlers/ai-agent-handler.ts
if (options.authToken) {
  const stripeResult = await fetchStripeToken(options.authToken)
  if (stripeResult.success && stripeResult.accessToken) {
    const stripeMcpServer = createStripeMcpServer({
      accessToken: stripeResult.accessToken,
      accountId: stripeResult.accountId,
      publishableKey: stripeResult.publishableKey,
    })
    sessionOptions.mcpServers = { stripe: stripeMcpServer }
  }
}
```

### Passing Auth Token from React Hook

```typescript
// app/hooks/useLocalAgent.ts
const authToken = authStore.token.get()
if (authToken) {
  sessionOptions.authToken = authToken
}
```

## Progress

### Completed

- [x] Created API endpoint in app-engineer to retrieve Stripe OAuth credentials
- [x] Implemented SDK MCP server with 4 Stripe tools
- [x] Created IPC handlers for Stripe token fetching
- [x] Modified agent types to support MCP servers
- [x] Modified Claude provider to pass MCP servers to SDK
- [x] Auto-attach Stripe MCP server in ai-agent-handler when auth token present
- [x] Pass auth token from useLocalAgent hook
- [x] Added comprehensive logging throughout the flow
- [x] Updated add-stripe skill documentation

### Pending Verification

- [ ] End-to-end test: User asks Claude to create a product
- [ ] Verify tools appear in Claude's available tools list
- [ ] Verify products/prices are created in user's Stripe account
- [ ] Test error handling (expired token, API errors)

## Challenges Encountered

### 1. Stripe API Version Type Error

**Problem:** TypeScript complained about Stripe API version:
```
Type '"2025-01-27.acacia"' is not assignable to type '"2026-01-28.clover"'
```

**Solution:** Updated to the latest API version supported by the installed Stripe SDK:
```typescript
const stripe = new Stripe(credentials.accessToken, {
  apiVersion: '2026-01-28.clover',
})
```

### 2. pnpm Install Postinstall Failure

**Problem:** Running `pnpm install` after adding stripe dependency caused electron-builder install-app-deps to fail.

**Solution:** Manually added `"stripe": "^20.3.0"` to package.json dependencies. The stripe package was installed in the lockfile despite the postinstall failure.

### 3. MCP Tools Not Being Attached to Sessions

**Problem:** Initial testing showed Claude trying to run shell commands (`mcp`, `claude mcp list`) instead of using MCP tools, confirming the tools weren't being attached to sessions.

**Root Cause:** The MCP server was being created but not passed through to the Claude Agent SDK properly.

**Solution:**
1. Modified `ai-agent-handler.ts` to automatically create and attach the Stripe MCP server when a session is created with an auth token
2. Modified `claude-provider.ts` to properly pass `mcpServers` to the SDK options
3. Added `authToken` to session options schema and types

### 4. Tracing the Integration Flow

**Problem:** Difficult to debug where in the flow the MCP server attachment was failing.

**Solution:** Added comprehensive logging with `[Stripe MCP]`, `[Stripe Handler]`, `[AI Agent Handler]`, and `[Claude Provider]` prefixes throughout:
- Token fetching from backend
- MCP server creation
- Session creation with MCP servers
- Tool calls

## Logging Points

The following log prefixes help trace the flow:

1. `[useLocalAgent]` - Auth token availability
2. `[Stripe Handler]` - Token fetching from backend
3. `[AI Agent Handler]` - Session creation, Stripe MCP attachment
4. `[Claude Provider]` - MCP servers passed to SDK
5. `[Stripe MCP]` - Server creation and tool calls

Expected log flow when working:
```
[useLocalAgent] Auth token available, will check for MCP integrations
[Stripe Handler] FETCHING STRIPE TOKEN
[Stripe Handler] STRIPE TOKEN RETRIEVED SUCCESSFULLY
[AI Agent Handler] Checking for Stripe integration...
[AI Agent Handler] Stripe connected, creating MCP server for account: acct_xxx
[Stripe MCP] CREATING STRIPE MCP SERVER
[Stripe MCP] STRIPE MCP SERVER CREATED SUCCESSFULLY
[AI Agent Handler] Stripe MCP server attached to session
[Claude Provider] MCP Servers: stripe
```

## Security Considerations

1. **Token Retrieval** - Only authenticated users can fetch their own Stripe token via the backend API
2. **Token Scope** - Backend verifies `read_write` scope before returning credentials
3. **In-Process Execution** - Token never leaves the agent process
4. **No Persistence** - Token is fetched per-session, not persisted in bfloat-ide
5. **Auth Token Redaction** - Logs redact auth token values
6. **Terminal Isolation** - Terminal sessions only receive essential system environment variables (PATH, HOME, etc.), preventing IDE-specific secrets like `STRIPE_SECRET_KEY` from leaking into user project terminals

## Next Steps

1. Complete end-to-end testing with a real Stripe-connected user
2. Verify Claude can successfully call the MCP tools
3. Remove debug logging once implementation is verified
4. Consider adding more Stripe tools (customers, subscriptions, etc.)
5. Add user-facing error messages when Stripe is not connected
