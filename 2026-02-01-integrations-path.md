# The Integrations Journey

How we added third-party integrations (Convex, Firebase, Stripe, Supabase) to bfloat-ide and why it took ~34 hours of iteration across two PRs.

## Timeline

### Phase 1: Subagents (~34 hours ago)

**Starting idea:** Use Claude Agent SDK subagents to handle integrations. Each integration (payments, databases, auth) would get its own subagent with specialized instructions.

**Commit:** `07ded3b feat: Add integration subagents for payments, databases, and auth`

**What happened:** The subagents had no context about the running project environment. When asked to "set up Convex", the agent would try to run `npm install convex` in a loop — literally 60+ times — because the command kept failing (sandboxed environment, no network access from the agent's perspective). Had to manually kill the session every time.

The `integrations-001.md` log captures this: a wall of identical `npm install convex` tool calls, one after another, burning through API credits until the user force-stopped it.

### Phase 2: Making subagents autonomous (~33 hours ago)

**Commits:**
- `4c7bcf2 fix: Clarify that project is already running in integration agents`
- `6961073 fix: Make integration agents fully autonomous`
- `bf9f171 Add default permissions for integration agents`

**What happened:** Tried to fix the loop by giving the agents more context ("the project is already running, don't install packages") and granting default permissions so they wouldn't get stuck on permission prompts. The agents still didn't have the right env vars and would either loop or produce broken code referencing modules that weren't installed.

### Phase 3: Preventing infinite loops (~5 hours ago)

**Commit:** `3371967 fix: Prevent infinite tool call loops in integration agents`

Realized the root cause was deeper than bad prompts. The agent had no way to actually provision a Convex backend — it was just an LLM trying to run shell commands without credentials. No amount of prompt engineering would fix that.

### Phase 4: Architecture pivot — skills, not subagents (~4 hours ago)

**Commit:** `485720c feat: Convert integration subagents to skills`

Key insight: integrations need **platform-level orchestration**, not agent autonomy. Converted subagents to skills (predefined prompts with instructions). The skill tells the agent exactly what files to create and how to wire them up, while the platform handles provisioning (OAuth, API keys, deployment creation) separately.

### Phase 5: Auto-provisioning (~69 min ago)

**Commits:**
- `cfb5f70 fix: Use bypassPermissions for main agent and auto-provision Convex on integration use`
- `0e38823 feat: Automate full Convex flow and wire up disconnect button`

The dropdown flow now works end-to-end:
1. User clicks Convex in integrations menu
2. If not OAuth-connected: opens browser for OAuth
3. After OAuth: backend auto-creates Convex deployment, stores URL + deploy key as secrets
4. Sets env vars as pending in workbench store
5. Auto-submits the `/add-convex` skill prompt to the agent
6. Agent writes the Convex schema, functions, and provider wrapper using the skill instructions

### Phase 6: Visual feedback (~31 min ago)

**Commit:** `6d429bc feat: Add animated gradient border loading indicator during Convex provisioning`

The provisioning API call takes a few seconds and users saw a dead UI. Added an animated gradient border on the chat input during provisioning.

### Phase 7: Freeform prompt interception (now)

**Commit:** `9c3252c feat: Intercept freeform Convex prompts with setup banner`

Final gap: the dropdown flow worked, but typing "Add convex to my app" as a freeform prompt bypassed everything. The agent would run without env vars and fail. Solution: intercept prompts containing "convex" in `handleSubmit`, show a setup banner with a button that feeds back into the existing dropdown flow.

Had one more bug: after provisioning, the auto-submitted prompt also contained "convex" and got intercepted again. Fixed with a local `convexProvisioned` state flag.

## Why it took so long

### 1. Wrong abstraction first

Started with subagents (autonomous AI agents that figure things out). Third-party integrations need **deterministic orchestration** — OAuth flows, API provisioning, secret management. An LLM can't OAuth into Convex for you. The agent's role should be limited to writing code after the platform sets up the environment.

### 2. The `npm install` loop

The most time-consuming single issue. The agent couldn't install packages (sandboxed env or network issues), so it retried `npm install convex` in an infinite loop. Each retry was a full API round-trip. This burned significant credits before we realized the agent should never be responsible for provisioning.

### 3. Two separate state systems

- `integrationStatus.convex` tracks OAuth connection (does the user have a Convex token?)
- `project.convexUrl` tracks project provisioning (does this project have a Convex deployment?)

These are independent. Connected but not provisioned, provisioned but token expired, neither, or both — all valid states. The banner and intercept logic had to account for both, and the freeform prompt fix needed a third piece of state (`convexProvisioned`) because the project object from the DB doesn't update mid-session.

### 4. Multiple entry points

Users can trigger Convex setup through:
- Integration dropdown (connected) -> `handleIntegrationUse`
- Integration dropdown (not connected) -> `handleIntegrationConnect` -> OAuth callback -> auto-provision
- Freeform prompt -> intercepted -> banner -> one of the above
- OAuth callback arriving at any time

Each path needed to converge on the same provisioning + skill flow, and each needed the right state guards to avoid double-provisioning or re-interception.

## Current status

Only **Convex** is working end-to-end. Firebase, Stripe, and Supabase are in the integrations menu but have no provisioning flow behind them yet. The goal was to get one integration fully working first to establish the pattern. Now that Convex is done, the others should be straightforward — each one needs:

1. **OAuth flow** (backend route + callback handler in the IDE)
2. **Provisioning API** (backend creates the resource, returns credentials)
3. **Secret injection** (store credentials via `window.conveyor.secrets`, set pending env vars)
4. **Skill prompt** (predefined instructions for the agent to write integration code)
5. **Freeform prompt interception** (keyword match in `handleSubmit`, show setup banner)
6. **State tracking** (OAuth status, project-level provisioned flag, local session flag)

The Convex implementation is the reference for all of this. The plumbing (integration menu, banner component, intercept logic, OAuth callback pattern) is already generalized — it just needs the integration-specific backends.

## Lessons

- **Don't give LLMs jobs that need deterministic infrastructure.** OAuth, deployment creation, secret injection — these are platform concerns. The agent writes code; the platform sets up the environment.
- **Subagents are the wrong tool for integrations.** Skills (predefined prompts) + platform orchestration is simpler and more reliable.
- **State fragmentation causes cascading bugs.** When "is Convex set up?" lives in three places (OAuth status, project DB, local component state), every new feature needs to check all three.
- **Infinite loops are expensive.** Should have had a tool call retry limit from day one. A single stuck session can burn through a lot of API credits.
- **Get one integration fully working before doing the rest.** Convex took ~34 hours because we were figuring out the pattern. The next ones should be significantly faster since the architecture is proven.

---

## Summary

- Started with subagents — agent tried to `npm install convex` 60+ times in a loop, burned credits, had to force-kill
- Tried fixing with better prompts, default permissions, loop prevention — none worked because the agent can't do OAuth or provision infrastructure
- Pivoted to skills (predefined prompts) + platform-level orchestration (OAuth, deployment creation, secret injection)
- Built full Convex flow: OAuth connect → backend provisions deployment → secrets stored → env vars injected → skill prompt auto-submitted → agent writes code
- Added visual feedback (animated border during provisioning) and freeform prompt interception (banner instead of sending unprepared agent)
- Only Convex is end-to-end right now — Firebase, Stripe, Supabase still need their provisioning backends
- Pattern is established: each new integration needs OAuth flow, provisioning API, secret injection, skill prompt, freeform intercept, and state tracking
- Core lesson: LLMs write code, platforms handle infrastructure — don't mix the two
