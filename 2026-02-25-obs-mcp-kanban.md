# obs-mcp Kanban Tools

Kanban tools for the [[2026-02-25-obs-mcp]] server. These work with Obsidian Kanban plugin boards — markdown files with `## Column` headings and `- [ ] [[card]]` items.

## How it works

Unlike the CLI wrapper tools, kanban tools parse the board markdown into a structured format, manipulate it, and write the file back directly to disk. The board format is:

```markdown
---
kanban-plugin: board
---

## Column Name

- [ ] [[card-one]]
- [ ] [[card-two]]

## Another Column

- [ ] [[card-three]]

***

## Archive

- [ ] [[old-card]]

%% kanban:settings
{"kanban-plugin":"board"}
%%
```

The `***` separator divides active columns from the Archive.

---

## Tools

### `kanban_list_boards`
List all kanban boards (files in `BOARDS/` folder).
- `vault` — target vault

### `kanban_read`
Read a board and return structured columns with card counts.
- `file` (required) — board path, e.g. `BOARDS/WORKBENCH BOARD`

### `kanban_add_card`
Add a card to a specific column.
- `file` (required) — board path
- `column` (required) — column name (case-insensitive)
- `card` (required) — card text, e.g. `[[my-issue]]`

### `kanban_move_card`
Move a card between columns.
- `file` (required) — board path
- `card` (required) — card text (case-insensitive match)
- `from` (required) — source column
- `to` (required) — destination column

### `kanban_remove_card`
Remove a card from the board entirely.
- `file` (required) — board path
- `card` (required) — card text
- `column` — specific column to search (searches all if omitted)

### `kanban_archive_completed_cards`
Move all cards from the `Complete` column to `Archive`.
- `file` (required) — board path

---

## Examples

These examples use the MCP tool names as they appear in Claude Code (prefixed with `mcp__obsidian__`).

**Read a board:**
```
kanban_read file="BOARDS/WORKBENCH BOARD" vault="bfloat-kb"
```

**Add an issue to the backlog:**
```
kanban_add_card file="BOARDS/WORKBENCH BOARD" column="Backlog" card="[[029-new-issue]]" vault="bfloat-kb"
```

**Move a card to In Progress:**
```
kanban_move_card file="BOARDS/WORKBENCH BOARD" card="[[029-new-issue]]" from="Backlog" to="In Progress" vault="bfloat-kb"
```

**Archive completed work:**
```
kanban_archive_completed_cards file="BOARDS/WORKBENCH BOARD" vault="bfloat-kb"
```

---

## Board locations

Boards live in the `BOARDS/` folder within each vault. Current boards in `bfloat-kb`:
- `BOARDS/WORKBENCH BOARD` — main IDE workbench board
- `BOARDS/APP ENGINEER BOARD` — app engineer board

## Source code

Kanban parsing and serialization: `/Users/v1b3m/Dev/bfloat/obs-mcp/src/kanban.ts`
