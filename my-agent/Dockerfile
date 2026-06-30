# my-agent — personal-assistant Telegram bot (Sam).
# Lives in the homelab repo (not the app fork); build context is the sibling
# ../../my-agent source tree. Long-poll worker: no inbound port, no build step.
FROM node:24-bookworm-slim

# System deps the agent's `bash` tool and chart feature rely on:
#  - git/curl/ca-certificates: the agent shells out freely (AGENTS.md)
#  - python3: backing interpreter for `uv run --with matplotlib` chart rendering
RUN apt-get update && apt-get install -y --no-install-recommends \
      git curl ca-certificates python3 \
    && rm -rf /var/lib/apt/lists/*

# uv — the agent renders charts via `uv run --with matplotlib --with numpy ...`
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# pnpm 10, pinned via package.json's packageManager field
RUN corepack enable

WORKDIR /app

# Dependencies first for layer caching
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

# App source
COPY . .

# Persistent disk: workspace + memory live here, mounted as a named volume.
# config.ts puts memory at $WORKSPACE_ROOT/memory, so one volume covers both.
ENV WORKSPACE_ROOT=/data/workspace
RUN mkdir -p /data/workspace && chown -R node:node /data /app

# Drop root: the model-driven bash tool runs as an unprivileged user.
USER node

CMD ["pnpm", "run", "start:prod"]
