---
title: tyf index stale after the hook reformats a file
code: [pyproject.toml]
status: open
---

**What happened:** the hook's `ruff format` rewrites a file; ty-find keeps the old index.
Tell: `tyf refs <sym>` lists only the definition, or gerenuk reports `selected` with an empty list.
**Cost:** wrong "no references" answers; tests silently not selected.
**Fix now:** `uv run tyf daemon restart`.
