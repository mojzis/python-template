---
title: check-all exits 0 when a step fails
code: [pyproject.toml]
status: open
---

**What happened:** `check-all` has `ignore_fail = true` and `vulns` ends in `|| true`.
A failing step still exits 0.
**Cost:** a green exit reads as done; findings only show in the output.
**Idea for a fix:** `ignore_fail = "return_non_zero"` (poe runs everything, then fails).
