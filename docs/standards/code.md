# Code

- **Layout: `<repo>/<package>/`, no `src/`.** `python-template/python_template/`.
  Declare it in `[tool.hatch.build.targets.wheel] packages`.
- **Entry points are routers.** `cli.py` and `web.py` only parse input and call a
  function that lives in its own module. Logic never sits in a command or a route,
  and domain modules never import typer or fastapi.
- `from __future__ import annotations` at the top of every module.
- `@dataclass(frozen=True)` for value and config objects. pydantic only at the edge,
  to validate YAML/JSON coming in.
- One base exception for expected, user-facing failures (`AppError`, optionally with
  a `.hint`). Raise subclasses deep down; catch once at the top and print a message,
  not a traceback. Example: `braindump/braindump/core/errors.py`.
- Timezone-aware datetimes only: `datetime.now(UTC)`. Ruff `DTZ` enforces it.
- Every `noqa` / `ty: ignore` names the rule and why: `# noqa: S603 -- args are ours`.
- Leave a seam where tests need to swap something out (a module-level alias, a
  parameter, a factory argument) instead of reaching for `unittest.mock`. Example:
  `imgen/src/imgen/client.py` (`_run = replicate.run`).
