# Data on disk

| data | store |
| --- | --- |
| content a person edits (stories, pages, settings) | YAML / Markdown files, one per item |
| append-only records, logs | JSONL |
| analytics, querying lots of rows | DuckDB |

- **Files are the database** for content apps: readable, diffable, no migrations.
  `bookmaker/docs/adr/0002-files-are-the-database.md`.
- All reads and writes go through one `store.py`. Writes are atomic: write a temp
  file next to the target, then `Path.replace()`. `bookmaker/bookmaker/store.py`.
- The data directory is overridable by an env var, so tests and worktrees can point
  elsewhere.
- DuckDB for anything you'd query (`introspect`, `trains-de`). No pandas; polars if
  you need frames in Python.
