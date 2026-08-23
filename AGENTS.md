# AGENTS.md

## Cursor Cloud specific instructions

### Current repository state
This repository is currently a **scaffold only**: it contains `README.md`, `LICENSE`,
and a Node/TypeScript-flavored `.gitignore`. There is **no application code, no
`package.json`/lockfile, and no build/lint/test tooling yet**. As a result there is
nothing to build, lint, test, or run until source is added.

### Intended stack
The repo name (`codex-micro-stream-deck-plugin`) and the Node-style `.gitignore`
indicate the intended product is a **Node.js / TypeScript Elgato Stream Deck plugin**
(built with the `@elgato/streamdeck` SDK and the `@elgato/cli` `streamdeck` tool).

### Preinstalled toolchain (available in the base image)
- Node.js `v22.x`, `npm`, `pnpm`, and `yarn` are all preinstalled.
- `python3` and `git` are available.
- The npm registry is reachable (egress is unrestricted for this run), so
  `npm install` of packages such as `@elgato/streamdeck` works out of the box.
- `nvm` is present; the base Node is already the correct major version, so no
  `nvm use` is normally required.

### How to work once code is added
- Once a `package.json` exists, install dependencies with the package manager that
  matches the committed lockfile (`package-lock.json` → `npm ci`, `pnpm-lock.yaml` →
  `pnpm install`, `yarn.lock` → `yarn install`). If there is no lockfile, use
  `npm install`.
- The startup **update script** is intentionally guarded and only runs
  `npm install` when a `package.json` is present, so it is a safe no-op on the
  current scaffold and starts installing dependencies automatically once a manifest
  is committed. Follow the package manager pinned by any future lockfile.
- Stream Deck plugins are typically developed with the Elgato CLI
  (`npx @elgato/cli` / `streamdeck`): scaffold with `streamdeck create`, build/watch
  with the project's `npm run build` / `npm run watch` scripts, and link/restart the
  plugin with `streamdeck link` / `streamdeck restart`. The Stream Deck desktop app
  itself is a macOS/Windows GUI and is **not** available in this Linux cloud VM, so
  full device-level runtime testing must happen on a host with Stream Deck installed;
  in the VM, validate by building the plugin and unit-testing the plugin logic.
