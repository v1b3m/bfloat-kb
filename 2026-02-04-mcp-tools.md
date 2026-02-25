Claude Code is still unable to connect to the mcp tools

```
<div class="assistant-message"><div class="tool-accordion-single"><div class="tool-pill-compact" style="opacity: 1; transform: none;"><span class="tool-pill-compact-icon"><svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-terminal" aria-hidden="true"><path d="M12 19h8"></path><path d="m4 17 6-6-6-6"></path></svg></span><span class="tool-pill-compact-action">Executing command</span><span class="tool-pill-compact-label" title="claude mcp list-tools 2>/dev/null || cat ~/.claude/mcp.json 2>/dev/null || echo &quot;No MCP config found&quot;">claude mcp list-tools 2&gt;/dev/null || cat ~/.claude...</span></div></div><div class="assistant-text"><div class="space-y-4 whitespace-normal *:first:mt-0 *:last:mb-0"><p>I don't have access to MCP tools in this environment. However, I can create the Stripe products and prices using a Node.js script that leverages your already-configured <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">STRIPE_SECRET_KEY</code>:</p></div></div></div>
```

## IDE Node backend logs

```
[Claude Provider] ANTHROPIC_BASE_URL in env: https://api.anthropic.com
[Claude Provider] ANTHROPIC_API_KEY in env: [NOT SET - using OAuth]
[Claude Provider] Spawning Claude Code via @anthropic-ai/claude-agent-sdk...
[Claude Provider] Prompt length: 24 chars
[Claude Provider] Received SDK message: system
[Claude Provider] Init message - tools: 17
[Claude Provider] Init message - model: claude-opus-4-5-20251101
[Claude Provider] Session initialized - Model: claude-opus-4-5-20251101
[Claude Provider] Received SDK message: assistant
[Claude Provider] Assistant message content blocks: 1
[Claude Provider] Block 0 type: tool_use
[Claude Provider] Returning tool_use block: Bash
[Claude Provider] Tool call: Bash
[AI Agent Handler] Sending message: tool_call
[AI Agent Handler] Sending to 1 windows via channel: ai-agent:stream:b6fa907b-1b7b-4384-b4c5-0bf37c34c569:1770192068329
[Claude Provider] Received SDK message: user
[Claude Provider] Received SDK message: assistant
[Claude Provider] Assistant message content blocks: 1
[Claude Provider] Block 0 type: text text length: 189
[Claude Provider] Returning text block (first 200 chars): I don't have access to MCP tools in this environment. However, I can create the Stripe products and prices using a Node.js script that leverages your already-configured `STRIPE_SECRET_KEY`:
[AI Agent Handler] Sending message: text
[AI Agent Handler] Sending to 1 windows via channel: ai-agent:stream:b6fa907b-1b7b-4384-b4c5-0bf37c34c569:1770192068329
```

I don't see any of the logs you just added here.
