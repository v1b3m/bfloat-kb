# obs-mcp Tools Reference

Full reference for all tools provided by the [[2026-02-25-obs-mcp]] server. Every tool accepts an optional `vault` parameter to target a specific vault.

---

## Vault

### `obsidian_vaults`
List all available Obsidian vaults.
- `verbose` (boolean) — show paths alongside names

---

## Reading & Navigation

### `obsidian_read`
Read the contents of a note.
- `file` (required) — note name, wikilink style

### `obsidian_files`
List files in vault or folder.
- `folder` — limit to a specific folder
- `format` — `text|json|tsv|csv`

### `obsidian_folders`
List folders in vault.
- `format` — `text|json|tsv|csv`

### `obsidian_recents`
List recently opened notes.
- `limit` — max number of results
- `format` — `text|json|tsv|csv`

### `obsidian_outline`
Show headings outline of a note.
- `file` (required) — note name

### `obsidian_links`
List outgoing links from a note.
- `file` (required) — note name
- `format` — `text|json|tsv|csv`

### `obsidian_backlinks`
List incoming links to a note.
- `file` (required) — note name
- `format` — `text|json|tsv|csv`

### `obsidian_file`
Get file info (path, size, created/modified dates).
- `file` (required) — note name

---

## Search

### `obsidian_search`
Search vault for notes matching a query.
- `query` (required) — search term
- `path` — limit to folder
- `limit` — max results
- `case` (boolean) — case sensitive
- `format` — `text|json|tsv|csv`

### `obsidian_search_context`
Search vault with matching line context.
- Same params as `obsidian_search`

---

## Writing

These tools use direct file I/O (not the CLI) to avoid escaping issues with special characters.

### `obsidian_create`
Create a new note.
- `name` (required) — note name
- `content` — note body
- `folder` — target folder
- `vault` (required) — target vault

### `obsidian_append`
Append content to a note.
- `file` (required) — note name
- `content` (required) — content to append
- `vault` (required) — target vault

### `obsidian_prepend`
Prepend content to a note.
- `file` (required) — note name
- `content` (required) — content to prepend
- `vault` (required) — target vault

### `obsidian_delete`
Delete a note (moves to trash).
- `file` (required) — note name

---

## Daily Notes

### `obsidian_daily_read`
Read today's daily note.

### `obsidian_daily_append`
Append content to today's daily note.
- `content` (required) — content to append

### `obsidian_daily_prepend`
Prepend content to today's daily note.
- `content` (required) — content to prepend

---

## Tasks

### `obsidian_tasks`
List tasks in vault.
- `file` — limit to a specific note
- `filter` — `todo` or `done`
- `format` — `text|json|tsv|csv`

---

## Properties & Tags

### `obsidian_properties`
List all frontmatter properties of a note.
- `file` (required) — note name

### `obsidian_property_set`
Set a frontmatter property on a note.
- `name` (required) — property name
- `value` (required) — property value
- `file` (required) — note name

### `obsidian_tags`
List tags in vault or in a specific note.
- `file` — limit to a specific note
- `counts` (boolean) — show tag counts
- `sort` — `name` or `count`
- `format` — `text|json|tsv|csv`

---

## Kanban

See [[2026-02-25-obs-mcp-kanban]] for the 6 kanban board tools.
