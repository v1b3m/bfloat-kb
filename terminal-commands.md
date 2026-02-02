We want to limit terminal use as much as possible. For example, this is the message claude code sent over after the `/add-convex` slash command.
```html
<div class="space-y-4 whitespace-normal *:first:mt-0 *:last:mb-0"><p>Convex is set up. Here's what was added:</p><ul class="list-inside list-disc whitespace-normal [li_&amp;]:pl-6" data-streamdown="unordered-list">
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">convex/schema.ts</code></span> — <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">timerSessions</code> table storing <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">durationMs</code> and <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">createdAt</code></li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">convex/timerSessions.ts</code></span> — <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">list</code>, <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">save</code>, and <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">remove</code> functions</li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">providers/ConvexClientProvider.tsx</code></span> — Convex React provider wrapping the app</li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">app/_layout.tsx</code></span> — Updated to wrap with <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">ConvexClientProvider</code></li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">app/(tabs)/index.tsx</code></span> — Timer now has a <span class="font-semibold" data-streamdown="strong">Save</span> button that persists sessions to Convex, and displays saved sessions below (long-press to delete)</li>
</ul><p>To finish setup, you'll need to run <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">npx convex dev</code> in your terminal to link to a Convex project (it requires interactive input). Then go to <span class="font-semibold" data-streamdown="strong">Project Settings &gt; Secrets</span> to add your <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">EXPO_PUBLIC_CONVEX_URL</code> (from the Convex dashboard)._</p></div>
```

We should gather insights on how to limit terminal commands. We support convex integration on the database, how about we ask users to use that instead and we use the connection tokens that are returned after they connect to automatically set everything up

---

Integrations triggered from the ide should redirect back to the IDE and no to the web app as they currently do. I've tested connecting convex and it redirects to the web app after completion.

---

If the user has not connected convex and asks for convex to be added to the project, can we present them with a simple button in the chat prompting that triggers the same flow as when we click connect on the project settings page.

---
