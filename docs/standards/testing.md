# Testing

- Write the failing test first. Assume the test is right; never weaken an assertion
  to make it pass.
- Hand-written fakes and `monkeypatch`, not `unittest.mock`. Inject fakes through a
  parameter, a factory argument or FastAPI `dependency_overrides`.
- An autouse fixture in `conftest.py` points every data path at `tmp_path`, so no
  test touches real data. `cecil/tests/conftest.py`.
- CLI: `typer.testing.CliRunner`. Set `_TYPER_FORCE_DISABLE_TERMINAL=1` in
  `conftest.py` so output has no ANSI codes.
- Web: `fastapi.testclient.TestClient`.
- No network in the default run. Tests that need it get `@pytest.mark.integration`,
  excluded via `addopts = "-m 'not integration'"`.
- Golden files for long rendered output; regenerate with `UPDATE_GOLDEN=1 uv run poe test`
  and review the diff. `typer-agentic/tests/conftest.py`.
