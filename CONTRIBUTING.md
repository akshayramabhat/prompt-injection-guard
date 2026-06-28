# Contributing

Thanks for helping out. This is a small library on purpose, so changes are easy
to review.

## Ground rules

- **No runtime dependencies.** The library stays pure standard library. A PR that
  adds a dependency will be declined.
- Keep the surface small. New helpers need a clear injection-defense purpose.
- Be honest about limits. If a function only handles the common case, say so in
  its docstring and in the README, the way `strip_urls` does.

## Develop

```bash
pip install -e . pytest
pytest -q
```

Add a test for any behavior change. The suite runs on Python 3.8, 3.11, and 3.13
in CI.

## Pull requests

- One focused change per PR.
- Update the README and `CHANGELOG.md` if behavior changes.
- Make sure `pytest -q` passes before you open the PR.
