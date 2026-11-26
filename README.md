# Renovate's Gleam manager skips a gleam.toml that has a path dependency

`gleam.toml` requires `gleam_stdlib` from Hex, below its latest major version (1.x), and `local_dep` by path:

```toml
[dependencies]
gleam_stdlib = ">= 0.40.0 and < 1.0.0"
local_dep = { path = "./local_dep" }
```

## Current behavior

Renovate extracts no dependencies from `gleam.toml`, and so never proposes `gleam_stdlib` 1.x. With `LOG_LEVEL=debug` it logs:

```text
DEBUG: Error parsing Gleam package file content (repository=..., packageFile=gleam.toml)
```

The Gleam manager's schema only accepts string requirements, so the `{ path = "..." }` table fails the whole file. Gleam also accepts `{ version = "..." }` and `{ git = "...", ref = "..." }` tables, which fail the same way.

## Expected behavior

Renovate proposes `gleam_stdlib` 1.x, and skips `local_dep` as a path dependency.
