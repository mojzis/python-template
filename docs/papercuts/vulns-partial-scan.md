---
title: vulns reports clean on a partial scan
code: [pyproject.toml]
status: open
---

**What happened:** in the cloud sandbox pysentry gets 403 from GitHub for the PyPA advisory DB,
prints `PARTIAL SCAN`, then `No vulnerabilities found!`.
**Cost:** a scan missing one source passes as clean.
**Idea for a fix:** fail the task when the output contains `PARTIAL SCAN`.
