---
title: Toolbox and commit hook
code: [pyproject.toml, hooks/pre-commit]
---

**Hook:** `hooks/pre-commit` → `madoqua run` (`core.hooksPath=hooks`, local config: `uv run madoqua install` once per clone).
Acts on staged `.py`/`.pyi` only; a commit without one is a silent no-op. ~0.3 s (`uv run madoqua stats`).

| phase | steps |
|---|---|
| fix (rewrites, re-stages) | `ruff check --fix`, `ruff format` |
| check (blocks) | `ruff check`, `ty check`, `biston scan --focus-args`, `zorilla check`, `gerenuk run` |

Config: `[tool.madoqua]` in `pyproject.toml`.

**Gotchas**
- `ty check` in the hook is staged-only: a file with old diagnostics blocks the first commit touching it.
  `uv run poe typecheck` checks everything.
- Security lint is ruff `S` rules (no bandit). `tests/**` ignores S101/S105/S106.
- gerenuk diffs the **working tree** against `origin/main`: unstaged edits count; any change to
  `pyproject.toml`/`uv.lock` runs the full suite. `--base HEAD` scopes to the commit.
- tyf index goes stale after the hook reformats a file ([papercut](papercuts/tyf-stale-index.md)).

**Manual only:** `uv run pycoati . --format pretty` scores tests for suspicion. Runs the suite 3×,
needs pytest-cov. Never in a hook or CI.
