# Python Template

uv · ruff · ty (beta) · pytest · poethepoet. Toolbox: [aesop](https://mojzis.github.io/aesop/llms.txt).
Run `uv run <tool> guide` before reaching for flags, `guide tune` before changing a threshold.

## Commands

| command | does |
|---|---|
| `uv run poe check` | lint, typecheck, vulns, test-lint, clones, then tests |
| `uv run poe check-all` | + dead-code, unused-deps; exits 0 on failure, read the output |
| `uv run poe fix` | format + autofix lint |
| `uv run poe upgrade` | upgrade every locked dependency, then sync |
| `uv run tyf find\|refs\|list\|calls` | symbol lookup; use instead of grep |
| `uv run gerenuk impacted-tests` | tests the working-tree diff reaches |

Fresh clone: `uv sync && uv run madoqua install`.

Commit hook is madoqua: fixes and re-stages, then blocks on ruff, ty, biston, zorilla,
gerenuk. Staged `.py` only. Never bypass a true finding (`--no-verify`, `MADOQUA_SKIP`).
Details and gotchas: [docs/toolbox.md](docs/toolbox.md).

ty false positive → `# ty: ignore[rule]`, never a blanket suppression.

<!-- project-knowledge rules v1, minimal subset — canonical: https://mojzis.github.io/clauding/rules/v1/ -->
## Project knowledge

**Done needs evidence → [QA.md](QA.md).** Run the tier the change needs, compare against its
`expect`, end with a QA report: what ran, vs `expect`, what wasn't run and why.
Not evidence: reasoning, weakened tests, new skips, "output exists".
Baseline moved on purpose → update `expect` in the same commit.

**Papercuts → `docs/papercuts/slug.md`.** Anything that slowed you down: confusing error,
flaky step, misleading doc. Log it when you hit it; fix inline only if trivial.
Read the open ones touching your area before a task.

**Decisions → `docs/adr/NNNN-slug.md`** when choosing between real alternatives, adding a
dependency, or changing a public interface. Never rewrite an accepted one; supersede it.

**Docs:** dense, no filler, tables over prose. Frontmatter `title`, `code:` (paths the doc
describes), `status` on papercuts/ADRs. Changed something in a doc's `code:` → update the
doc in the same commit.
