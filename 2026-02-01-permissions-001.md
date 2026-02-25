The skill can add convex without issue, it can run commands. However, when run with the main agent, it even fails to trigger the skill, it appears, it has no permission to do anything other than read and edit files. Look at the chat below, when run directly via `/add-convex`, claude code is then able to add convex which it wasn't able to do when prompted to use a skill within the body of a message

Check the chat session at /Users/v1b3m/Dev/bfloat/bfloat-ide/bfloat-kb/pg/raw.html

I've gathered docs on permissions below:

---

I'll search for documentation on all three topics.Now let me fetch the main documentation pages for complete information:Unfortunately I can't fetch the full pages directly, but I've gathered comprehensive information from my searches. Let me compile the documentation for all three topics:

---

## 1. Claude Code Permissions

**Documentation URL:** https://docs.anthropic.com/en/docs/claude-code/sdk/sdk-permissions

### Permission System Overview

Claude Code uses a layered permission system with **allow/deny/ask rules** evaluated in this order:

1. **Deny rules** - Block regardless of other rules (checked first)
2. **Allow rules** - Permit if matched
3. **Ask rules** - Prompt for approval

### Permission Modes

|Mode|Description|
|---|---|
|`default`|Standard permission behavior - prompts for most operations|
|`acceptEdits`|Auto-approves file operations; other tools still need approval|
|`bypassPermissions`|Bypasses all permission checks (**use with extreme caution**)|
|`plan`|Planning mode - no execution, Claude can only analyze and propose|

### Permission Rule Syntax

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git commit *)",
      "Read",
      "Edit"
    ],
    "deny": [
      "Bash(git push *)"
    ]
  }
}
```

- Use `Tool` or `Tool(specifier)` format
- `Bash(*)` is equivalent to `Bash` (matches all)
- Glob patterns with `*` work at any position
- **Space matters:** `Bash(ls *)` matches `ls -la` but not `lsof`

### Tool-Specific Rules

- **Read** - Deny rules block file/directory reading
- **Edit** - Allow/deny rules control write access
- **WebFetch** - Allow/deny rules control network domains
- **MCP** - Wildcard syntax `mcp__server__*` for all tools from a server
- **Task** - Controls subagent invocation

---

## 2. Skills Permissions

**Documentation URL:** https://docs.anthropic.com/en/docs/claude-code/slash-commands

### Skills Overview

Skills are custom prompts stored as Markdown files with YAML frontmatter in:

- **Project skills:** `.claude/skills/`
- **User skills:** `~/.claude/skills/`

### Skill Permission Configuration

Skills can define their own tool access via the `allowed-tools` frontmatter field:

```yaml
---
name: my-skill
description: Does something useful
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash(npm test *)
---
Your skill instructions here...
```

### Key Permission Fields

|Field|Description|
|---|---|
|`allowed-tools`|Tools the skill can use without per-use approval|
|`disable-model-invocation`|Set `true` to prevent Claude from invoking the skill programmatically|
|`user-invocable`|Controls menu visibility (set `false` to hide from `/` menu)|

### Permission Syntax for Skills

```
Skill(name)        # Exact match
Skill(name *)      # Prefix match with any arguments
```

### Important Notes

- Skills that define allowed-tools grant Claude access to those tools without per-use approval when the skill is active. Your permission settings still govern baseline approval behavior for all other tools.
- Hide individual skills by adding `disable-model-invocation: true` to their frontmatter.
- Skills can run in a forked context using `context: fork` and specify which subagent to use with the `agent` field

---

## 3. Subagent Permissions

**Documentation URLs:**

- https://docs.anthropic.com/en/docs/claude-code/sub-agents
- https://docs.anthropic.com/en/docs/claude-code/sdk/subagents

### Subagent Permission Model

Subagents inherit the permission context from the main conversation but can override the mode.

### Defining Subagent Tools

```yaml
---
name: code-reviewer
description: Expert code reviewer
tools: Read, Grep, Glob, Bash
model: sonnet
permission-mode: default
---
System prompt here...
```

### Tool Access Options

|Configuration|Behavior|
|---|---|
|`tools` omitted|Inherits all tools from parent, including MCP tools|
|`tools: Read, Grep, Glob`|Only these specific tools available|
|Specific MCP tools|Use `mcp__server__toolname` format|

### Permission Mode Inheritance

When using `bypassPermissions`, all subagents inherit this mode and it cannot be overridden. Subagents may have different system prompts and less constrained behavior than your main agent.

### Background Subagents

Before launching, Claude Code prompts for any tool permissions the subagent will need, ensuring it has the necessary approvals upfront. Once running, the subagent inherits these permissions and auto-denies anything not pre-approved.

Key constraints:

- MCP tools are **not available** in background subagents
- If a background subagent fails due to missing permissions, you can resume it in the foreground
- Subagents **cannot spawn their own subagents** (no nesting)

### Programmatic Subagent Definition (SDK)

```typescript
const result = query({
  prompt: "Review my code",
  options: {
    agents: {
      "code-reviewer": {
        description: "Code review specialist",
        prompt: "You are a senior code reviewer...",
        tools: ["Read", "Grep", "Glob"],  // Restricted tools
        model: "sonnet"
      }
    },
    allowedTools: ["Task", "Read"]  // Task required for subagents
  }
});
```

### Using Hooks for Fine-Grained Control

For more dynamic control over tool usage, use PreToolUse hooks to validate operations before they execute. This is useful when you need to allow some operations of a tool while blocking others.

Example for read-only database queries:

```yaml
---
name: db-reader
description: Execute read-only database queries
tools: Bash
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./validate-readonly.sh"
---
```

---

Would you like me to dive deeper into any of these topics?