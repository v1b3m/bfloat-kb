 Syncing Claude Code Skills to Codex

## Overview

Claude Code and Codex use different paths for skills. To share Obsidian skills between them, copy the `SKILL.md` files from the Claude Code plugin cache to the Codex skills directory.

## Paths

| Agent | Skills path |
|-------|-------------|
| Claude Code | `~/.claude/plugins/cache/obsidian-skills/obsidian/1.0.0/skills/` |
| Codex | `~/.agents/skills/` |

## Skills to sync

- `defuddle` — clean markdown extraction from URLs
- `obsidian-markdown` — Obsidian Flavored Markdown syntax
- `obsidian-bases` — `.base` file format (views, filters, formulas)
- `obsidian-cli` — Obsidian CLI command reference
- `json-canvas` — `.canvas` file format (nodes, edges, groups)

## Sync command

```bash
SRC="$HOME/.claude/plugins/cache/obsidian-skills/obsidian/1.0.0/skills"
DEST="$HOME/.agents/skills"
for skill in defuddle obsidian-markdown obsidian-bases obsidian-cli json-canvas; do
  mkdir -p "$DEST/$skill"
  cp "$SRC/$skill/SKILL.md" "$DEST/$skill/SKILL.md"
done
```

## Notes

- Each skill folder contains a single `SKILL.md` with frontmatter (`name` + `description`) — this format is compatible with both Claude Code and Codex.
- Codex auto-detects skills in `~/.agents/skills/` on startup. Restart Codex if newly copied skills don't appear.
- If the Claude Code plugin updates (new version path), re-run the sync command.