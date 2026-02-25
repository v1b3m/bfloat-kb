The desktop app gets stuck in a forever loading state every now and then. It shows:

1. Loading... in the place where the app name should be
2. Preparing workspace right above the chat input
3. Chat input is disabled

Console logs: `/Users/v1b3m/Dev/bfloat/bfloat-workbench/bfloat-kb/pg/console.txt`

Electron backend logs:

```
[SessionReader] Claude parsing complete: {
  lineCount: 171,
  rawMessagesCount: 100,
  mergedMessageCount: 16,
  userMessages: 8,
  assistantMessages: 8,
  textBlocks: 30,
  toolBlocks: 62,
  sessionId: '31f2f02a-5714-492b-968d-408e43f6f31d',
  cwd: '/Users/v1b3m/.bfloat/projects/1c5daddf-8d6c-4112-9152-8110f1ca677f'
}
[sessionToMessages] Converted messages: {
  total: 16,
  userMessages: 8,
  assistantMessages: 8,
  messagesWithTextBlocks: 8
}
[sessionToMessages] First text message: {
  id: '67bf17bb-0ba8-4d32-9646-c68a042fd097',
  contentLength: 1363,
  blocksCount: 19,
  firstBlockType: 'text',
  firstBlockHasContent: true
}
[AI Agent Handler] Messages before IPC return: { totalMessages: 16, assistantMessages: 8, messagesWithTextBlocks: 8 }
[AI Agent Handler] First text message details: {
  id: '67bf17bb-0ba8-4d32-9646-c68a042fd097',
  contentLength: 1363,
  blocksCount: 19,
  firstBlockType: 'text',
  textBlockContent: '\n\nLet me start by exploring the project to understand its structure....'
}
[Claude Provider] Checking authentication at /Users/v1b3m/.claude.json and /Users/v1b3m/.claude/.credentials.json
[Claude Provider] Auth check result: true
[Claude Provider] Found Claude Code binary at: /Users/v1b3m/.local/bin/claude
[Bfloat Provider] Auth check: CLI installed = true
[ProjectService] Committed and pushed: Auto-save before close
[Terminal] Creating PTY with shell: /bin/zsh, cwd: /Users/v1b3m, bundled: false
[Terminal] Spawning shell with args: -l
[Terminal] Terminal process created with pid: 63060
[ProjectService] Nothing to commit
[ProjectService] Closing project: 1c5daddf-8d6c-4112-9152-8110f1ca677f
[Terminal] Found available port: 9000 (searched from 9000)
[Terminal] Found available port: 9000 (searched from 9000)
```

