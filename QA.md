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
    expect: {collected_min: 2, skips: [], warnings_max: 0, seconds: [0.5, 60], findings: [vulture greet]}
---

No CI: these local runs are the only gate ([papercut](docs/papercuts/no-ci.md)).

| change | run |
|---|---|
| docs only | nothing |
| `src/`, `tests/` | fast |
| `pyproject.toml`, `uv.lock` | full |

**Expected findings in full:** vulture `unused function 'greet' (60%)`: placeholder API, only tests call it.
Any other finding is a failure.

**Non-evidence here:**
- `check-all` exit code ([papercut](docs/papercuts/check-all-exit-code.md)).
- vulns "No vulnerabilities found!" when the same output says `PARTIAL SCAN` ([papercut](docs/papercuts/vulns-partial-scan.md)).
- A commit that staged no `.py` file: the hook did nothing.

**Oracles**
| name | kind | check | status |
|---|---|---|---|
| hook blocks | negative control | stage a `.py` returning an undefined name → `uv run madoqua run` exits 1 (an unused import gets autofixed, never blocks) | proposed |
