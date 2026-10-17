---
title: No CI workflow
code: [.github/]
status: open
---

**What happened:** `.github/` holds only dependabot config. Nothing runs `poe check` on a PR.
**Cost:** dependabot PRs merge unverified unless someone runs the checks locally.
**Idea for a fix:** one workflow running `uv run poe check`.
