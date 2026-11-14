# Standards

The house defaults for new code. Unless the user or a decision in `docs/adr/` says
otherwise, use them. Read the file for the area you are about to touch:

| file | read before |
| --- | --- |
| [code.md](code.md) | creating modules, layout, types, errors |
| [cli.md](cli.md) | adding or changing a command |
| [web.md](web.md) | adding a web UI or API |
| [llm.md](llm.md) | calling Claude |
| [data.md](data.md) | storing anything on disk |
| [testing.md](testing.md) | writing tests |

Not settled yet, so there is no standard: config loading (`.env`, env vars, settings
classes). Follow what the repo already does.

Examples point at the user's other repos (`~/git/<repo>`, GitHub `mojzis/<repo>`).
