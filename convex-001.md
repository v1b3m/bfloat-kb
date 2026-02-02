
It says `EXPO_PUBLIC_CONVEX_URL` has been set, however, I do not see it in the secrets UI in the project settings.


```
<div class="space-y-4 whitespace-normal *:first:mt-0 *:last:mb-0"><p>Convex is connected and ready to use. Your <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">EXPO_PUBLIC_CONVEX_URL</code> has been configured automatically.</p><p>Here's what was set up:</p><ul class="list-inside list-disc whitespace-normal [li_&amp;]:pl-6" data-streamdown="unordered-list">
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">convex/schema.ts</code></span> — Defines a <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">todos</code> table with <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">text</code>, <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">completed</code>, and <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">createdAt</code> fields</li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">convex/todos.ts</code></span> — Query (<code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">list</code>) and mutations (<code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">add</code>, <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">toggle</code>, <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">remove</code>) for CRUD operations</li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">providers/ConvexClientProvider.tsx</code></span> — Wraps the app with the Convex React client</li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">app/_layout.tsx</code></span> — Updated to wrap the entire app in <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">ConvexClientProvider</code></li>
<li class="py-1 [&amp;>p]:inline" data-streamdown="list-item"><span class="font-semibold" data-streamdown="strong"><code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">app/(tabs)/index.tsx</code></span> — Updated to use <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">useQuery</code> and <code class="rounded bg-muted px-1.5 py-0.5 font-mono text-sm" data-streamdown="inline-code">useMutation</code> hooks instead of local state</li>
</ul><p>Your todos are now persisted in Convex's real-time database — they'll sync across devices and survive app restarts._</p></div>
```

The app is also failing with 

```
Unable to resolve module 'convex/react' from providers/ConvexClientProvider.tsx. The module could not be found in the project or node_modules.
```