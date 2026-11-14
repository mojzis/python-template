# Web

**Stack:** FastAPI + htmx + Jinja2, plain CSS. No frontend framework, no build step.
Reference apps: `cecil`, `bookmaker`.

## App

- `web.py` is a router: routes are a few lines each and call functions from domain
  modules. Split into `APIRouter` modules per area once it grows.
- Bind `127.0.0.1`. No auth until a decision says otherwise. `docs_url=None`,
  `redoc_url=None` unless the API is the product.
- Launch from the CLI: a `serve` command that calls `uvicorn.run(..., reload=...)`.

## Port: pick a free one, don't hard-code

Several apps and worktrees run at once, so a fixed port collides. With no `--port`,
ask the OS for a free port and print the URL as the first line:

```python
def free_port(host: str = "127.0.0.1") -> int:
    with socket.socket() as s:
        s.bind((host, 0))
        return s.getsockname()[1]
```

`--port N` still pins it when needed. Examples: `libris/poe_tasks.toml` (`serve`),
`introspect/src/introspect/cli.py` (`_find_available_port`, probes upward from a
preferred port).

## Templates and htmx

- `templates/base.html` plus pages; fragments are `_name.html`.
- Every fragment has its own GET URL you can curl. Don't branch on `HX-Request`
  to decide between page and fragment.
- htmx is vendored in `static/`, no CDN. Same for any other JS library.
- Extra JS: small vanilla files only. CSS: one hand-written `static/style.css`.
- Server-driven navigation via response headers (`HX-Redirect`, `HX-Trigger`).
- Show error responses instead of swallowing them: htmx `responseHandling` meta
  in `base.html` (`happytube/.../templates/base.html`).

## Slow work

- Run it in a `ThreadPoolExecutor`; the request returns at once with a job fragment.
- The fragment polls with `hx-trigger="every 2s"` only while the job runs, and stops
  by rendering without the trigger when done. SSE only when polling isn't enough;
  then events carry names, not HTML (`bookmaker/docs/adr/0006-...`).

## Checking it

UI changes get a real-browser look before they are called done (Chrome tools or
Playwright), as a QA step, not in the test suite.
