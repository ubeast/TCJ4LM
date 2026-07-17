# j4lm-pylib

> Questions or comments? Contact Michael Schertz — michael.schertz.civ@mail.mil

Shared Python utility library used across TC4JLM products.

## Overview

Reusable utility functions and modules for use across products.

## Structure

```
├── src/j4lm_pylib/       # verified, ready for any product to use
│   └── __init__.py
├── parking_lot/             # new code, not yet reviewed
├── tests/
└── README.md
```

## Where code goes

| Location | What goes here | How it moves out |
|---|---|---|
| `src/j4lm_pylib/` | Verified, reviewed code — safe for any product to use | This is the destination — nothing moves out of here except into a product that imports it |
| `parking_lot/` | Code you wrote that you think could be reusable | Reviewed and tested, then promoted into `src/j4lm_pylib/` |

If something sits in `parking_lot/` untouched for a long stretch (~6 months),
revisit it — either promote it or remove it.

## Review

Changes here affect every product that depends on this library. Required
reviewers will be enforced via `CODEOWNERS` once it's added — see
Planned below.

## Planned / not yet implemented

This repo is intentionally minimal right now while the packaging/publish
flow gets tested against our actual GitLab instance. Once verified, expect:

- **`CODEOWNERS`** — required reviewers by path
- **`pyproject.toml`** — package metadata, dependencies, tool config (ruff, mypy, pytest)
- **`.gitlab-ci.yml`** — lint, type-check, test, build, and publish stages
- **`.gitignore`** — standard Python/tooling exclusions
- **Wheel packaging** — building and publishing this library to the GitLab
  Package Registry so products can install a pinned version
- **`docs/`** — architecture/design notes, if needed beyond this README

Don't build against any of the above until it's actually added — this
section is a heads-up, not documentation of something that exists yet.
