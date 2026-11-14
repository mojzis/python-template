# Calling Claude

Two ways, pick by where the code runs:

| situation | use |
| --- | --- |
| local tool, runs on the user's machine | `claude -p --output-format json` via a wrapper |
| deployed, needs prompt caching, structured output or high volume | `anthropic` SDK |

## `claude -p` wrapper

- One module (`claude_cli.py`) is the only place that spawns `claude`. It uses the
  local login, so no API key.
- The binary and model are overridable by env var, so tests point it at a stub
  script. Example: `braindump/braindump/core/claude_cli.py`.

## anthropic SDK

- One module owns the client. Cache the system prompt; use structured output
  instead of parsing prose. Example: `happytube/src/happytube/claude.py`.

## Both

- The model is a module constant (`MODEL = "claude-sonnet-5-5"`), not scattered strings.
- Prompts live in files next to the code (`prompts/<name>.md`), loaded with
  `importlib.resources`, so prompt changes are readable diffs.
- **Version every prompt.** A `PROMPT_VERSION = 3` constant beside the loader; bump
  it on any change to the prompt. Every stored result records `model`,
  `prompt_version` and a UTC timestamp. Re-runs skip items already done with the
  current version, so bumping the version is how you ask for a redo. Example:
  `happytube/src/happytube/channels/vet.py`, `happytube/docs/decisions/0005-agent-vetting.md`.
- Tests never call a real model: stub binary for the wrapper, a fake client passed
  in for the SDK.
