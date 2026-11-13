# Contributor guide

## Scope

`one-conv` reads and searches saved AI conversations, relays messages between live
agent sessions, and exposes selected local CLIs through remote MCP connectors. Keep
cloud history, local history, live messaging, controller, accounting, and tuning
concerns in their existing modules under `one_conv/`; `one_conv_cli.py` remains the
top-level CLI and compatibility layer.

## Setup

- Use Python 3.12 or newer and `uv`.
- Run the CLI on Windows with `bin\one-conv.cmd --help`; on macOS or Linux use
  `./bin/one-conv --help`.
- Browser-session discovery, Keychain login, launchd, systemd, tmux, and archive
  freeing have platform-specific paths. Preserve their explicit platform guards.

## Tests

Each test is a PEP 723 script with pinned dependencies. Run a focused file while
developing:

```sh
uv run --no-project --with-requirements tests/conftest.py python -m pytest tests/test_send_images.py
```

Run the complete offline suite before merging:

```sh
uv run --no-project --with-requirements tests/conftest.py python -m pytest -n auto tests
```

Add regression coverage beside the behavior being changed. Tests must use temporary
state and stub transports; never contact real accounts, hubs, browser sessions, or
provider APIs.

## Safety and conventions

- Keep provider mutations allowlisted in their transport methods and never retry a
  send or upload whose upstream outcome may be uncertain.
- Validate and bound external input before persisting or relaying it. Parameterize
  SQLite queries and keep private state in the existing protected database helpers.
- Do not log credentials, cookies, tokens, response bodies, transcript contents, or
  local paths that a cloud peer should not see.
- Keep README, `SKILL.md`, provider docs, extension metadata, and command help aligned
  when a public command or capability changes.
- Do not commit generated caches, local live state, archives, databases, credentials,
  or build artifacts.
