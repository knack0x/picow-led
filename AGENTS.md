# AGENTS.md

## Build & Dev

- `make generate` runs `templ generate` then Tailwind CSS build — both outputs are gitignored (`*_templ.go`, `output.css`)
- Build order: `make generate` → `go build` (`make build` handles this)
- `make run` uses `gow` for hot reload watching `.go`, `.js`, `.css` files
- No test suite exists — don't look for tests

## Architecture

- Entrypoint: `main.go` → Echo server → `routes.Register()` → `services.NewRegistry(db)`
- `internal/`: handlers, routes, services, templates, env config
- `pkg/`: models, picow client, version (shared packages)
- Templates: `.templ` files in `internal/templates/` — generated Go code is ephemeral
- UI components from `github.com/templui/templui` and `github.com/knackwurstking/ui`

## Environment

| Variable         | Default   |
| ----------------- | --------- |
| `DB_PATH`         | `~/.picow-led` |
| `SERVER_ADDRESS`  | `:50835`  |
| `SERVER_PATH_PREFIX` | ``    |
| `VERBOSE`         | `true`    |

## macOS Deploy

- `make macos-install` installs to `/usr/local/bin` with launchd service
- Service expects `SERVER_PATH_PREFIX=/picow-led`
