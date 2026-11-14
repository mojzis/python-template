# CLI

**Stack:** typer + rich + [typer-agentic](https://github.com/mojzis/typer-agentic).

## Shape

```python
app = typer.Typer(
    no_args_is_help=True,
    add_completion=False,
    context_settings={"help_option_names": ["-h", "--help"]},
)
main = agent_errors(app)  # [project.scripts] points at main, not app
```

- `-h` works everywhere (the `context_settings` above).
- `cli.py` is a router: each command parses options and calls one function from
  another module. See [code.md](code.md).
- Shared options are `Annotated[...]` aliases defined once at module level. Example:
  `agentic-tests/src/agentic_tests/cli.py`.
- **Add a `guide` subcommand.** Run `curl -s https://mojzis.github.io/aesop/guide/index.md`
  (curl, not a fetch tool, which would summarise it) and follow it.

## Output

rich for humans, plain for agents. Rich already drops colour when stdout is not a
TTY, but its tables wrap at 80 columns and draw box characters. In agent mode use
markdown tables and a wide console:

```python
import os
from rich import box
from rich.console import Console
from typer_agentic import DEFAULT_AGENT_ENV_VARS

AGENT = any(os.environ.get(v) for v in DEFAULT_AGENT_ENV_VARS)
console = Console(no_color=AGENT, highlight=not AGENT, width=200 if AGENT else None)
err = Console(stderr=True, no_color=AGENT, highlight=not AGENT)
TABLE_BOX = box.MARKDOWN if AGENT else box.ROUNDED
```

- Results to stdout, diagnostics to stderr.
- Errors: `error: <what>` then `hint: <what to do>` on stderr, then
  `raise typer.Exit(code) from None`.
- Exit codes: 0 ok, 1 failure, 2 usage/config error, 3 stopped early (re-run to
  resume), 130 interrupted.
- A `--json` flag when output is likely to be piped into another tool.
