FROM node:24-bookworm-slim

# Git is required for source checkout and common tooling; the Stream Deck
# plugin build only needs Node.js (rollup/typescript are project devDependencies).
RUN apt-get update \
    && apt-get install -y --no-install-recommends git ca-certificates \
    && rm -rf /var/lib/apt/lists/*

USER node
