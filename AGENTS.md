# AGENTS.md

## Project overview

This repository is a **Stream Deck plugin** (`com.viruspc.codex-micro`) built with the
official [Elgato Stream Deck SDK](https://docs.elgato.com/streamdeck/sdk/introduction/getting-started/)
(`@elgato/streamdeck`) and TypeScript. It was scaffolded with the Stream Deck CLI
(`@elgato/cli`) plugin creation wizard.

### Layout
- `src/plugin.ts` — plugin entry point; registers actions and connects to Stream Deck.
- `src/actions/increment-counter.ts` — example "Counter" action (increments on key press).
- `com.viruspc.codex-micro.sdPlugin/` — the compiled plugin distributed to Stream Deck
  (`manifest.json`, images, property inspector UI, and `bin/plugin.js` build output).
- `rollup.config.mjs`, `tsconfig.json` — build configuration (Rollup + TypeScript).

## Commands
- Install dependencies: `npm install`
- Build the plugin: `npm run build` (Rollup bundles `src/plugin.ts` → `*.sdPlugin/bin/plugin.js`)
- Watch/rebuild while developing: `npm run watch` (also restarts the plugin in Stream Deck)
- Validate the plugin package: `npx streamdeck validate com.viruspc.codex-micro.sdPlugin`

## Cursor Cloud specific instructions

### Toolchain
- The Stream Deck SDK requires **Node.js 24+**. The committed
  `.cursor/environment.json` builds from `node:24` (`.cursor/Dockerfile`) and runs
  `npm install`, so fresh Cloud Agents get the correct Node version and dependencies
  automatically.
- If you are on a VM whose default `node` is older than 24 (e.g. a just-in-time run
  before the environment build), install and select Node 24 with nvm:
  `nvm install 24 && export PATH="$HOME/.nvm/versions/node/$(nvm version 24)/bin:$PATH"`.
  Note a shadowing `node` may exist earlier in `PATH` (e.g. `/exec-daemon/node`), so
  prepend the nvm bin directory as shown rather than relying on `nvm use` alone.

### Testing on Linux (no Stream Deck app)
- The Stream Deck **desktop app is macOS/Windows only** and is not available in this
  Linux VM, so `streamdeck create`/`link`/`restart` and full device runtime testing
  cannot be performed here. (`streamdeck create` itself also currently fails on Linux
  because it invokes Windows-only `reg.exe`; scaffold on macOS/Windows, or render the
  CLI templates directly, as was done to create this project.)
- To test plugin logic end-to-end on Linux, build the plugin and run it against a
  **mock Stream Deck WebSocket server**: launch `com.viruspc.codex-micro.sdPlugin/bin/plugin.js`
  with the registration args (`-port`, `-pluginUUID`, `-registerEvent registerPlugin`,
  `-info <json>`) from within the `*.sdPlugin` directory, perform the registration
  handshake, then send `deviceDidConnect` / `willAppear` / `keyDown` events and assert
  on the `setTitle` / `setSettings` commands the plugin sends back.
- Prefer unit-testing the action classes in `src/actions/` directly for fast feedback.
