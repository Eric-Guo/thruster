# Repository Guidelines

## Project Structure & Module Organization
- Core proxy services live in `internal/`, with middleware, caching, and server orchestration split by file (e.g., `logging_middleware.go`, `server.go`).
- The command-line entrypoint is `cmd/thrust/main.go`; keep CLI-specific logic there and call into `internal` packages.
- Ruby gem packaging sits in `lib/` alongside `thruster.gemspec`; update these when shipping binaries or version bumps.
- Test fixtures (images, text samples) live under `internal/fixtures/`; reuse or extend them instead of adding ad-hoc assets.

## Build, Test, and Development Commands
- `go run ./cmd/thrust` launches the proxy against your local Puma service—ideal for smoke checks.
- `make build` compiles the Go binary into `bin/`, while `make dist` cross-builds into `dist/` for linux/darwin on amd64/arm64.
- `make test` (or `go test ./...`) runs the full Go suite; pair with `make bench` when validating performance-sensitive changes.
- Use `rake package` after `rake clobber` only when preparing release gems; normal development does not require Ruby tasks.

## Coding Style & Naming Conventions
- Format Go sources with `gofmt` (tabs for indentation) and keep imports grouped; run `go fmt ./...` before committing.
- Exported functions, types, and fields use CamelCase; unexported items stay lowerCamelCase to mirror existing modules.
- Tests follow table-driven patterns in `*_test.go`; name helpers with the suffix `Helper` for clarity.
- Ruby supporting files should follow standard two-space indentation and keep version constants in `lib/thruster/version.rb`.

## Testing Guidelines
- Place new tests beside the code in `internal/` and mirror file names (e.g., `variant.go` ⇢ `variant_test.go`).
- Prefer focused scenarios over broad integration tests; lean on the existing fixtures for cache, upstream, and sendfile cases.
- Run `go test -run <Regex> ./internal/...` for targeted debugging, and ensure `make test` passes before opening a PR.
- Capture benchmarks with `make bench` when touching hot paths like caching or proxy handlers.

## Commit & Pull Request Guidelines
- Follow the repo norm of concise, imperative commit subjects (e.g., `Document LOG_REQUESTS in README`).
- Reference related issues or context in the body, and summarize behavior changes plus test coverage.
- PRs should outline intent, implementation notes, and manual/automated test results; attach config diffs or logs if relevant.
- Confirm code is formatted, tests are green, and release metadata (`CHANGELOG.md`, `lib/thruster/version.rb`) stays consistent when versioning.
