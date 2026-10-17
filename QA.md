---
title: "QA: python-template"
code: [tests/, pyproject.toml]
tiers:
  - name: fast
    cmd: uv run poe check
    when: every change
    expect: {collected_min: 2, skips: [], warnings_max: 0, seconds: [0.5, 30]}
  - name: full
    cmd: uv run poe check-all
    when: dependency or pyproject.toml changes; before merge
    expect: {collected_min: 2, skips: [], warnings_max: 0, seconds: [0.5, 60]}
---

CI (`.github/workflows/ci.yml`) runs `fast` on every push to main and every PR.

| change | run |
|---|---|
| docs only | nothing |
| `src/`, `tests/` | fast |
| `pyproject.toml`, `uv.lock` | full |

**Prerequisites:** vulns needs `codeload.github.com` (PyPA advisory DB). Blocked → vulns exits 2
and `check` fails; report vulns as not run. Never pass `--no-fail-on-partial` to get green.

**Non-evidence here:** a commit that staged no `.py` file: the hook did nothing.

**Oracles**
| name | kind | check | status |
|---|---|---|---|
| hook blocks | negative control | stage a `.py` returning an undefined name → `uv run madoqua run` exits 1 (an unused import gets autofixed, never blocks) | proposed |
