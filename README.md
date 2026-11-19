[![make.fun · github-core-clone-01](https://make.fun/badge/github-core-clone-01.svg)](https://make.fun/bounty/github-core-clone-01?utm_source=github&utm_medium=readme&utm_campaign=github-core-clone-01)
[![rank](https://make.fun/badge/github-core-clone-01/Kiowayyy2109/makefun-github-core-clone-01-v2/rank.svg)](https://make.fun/bounty/github-core-clone-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=github-core-clone-01)
[![reward](https://make.fun/badge/github-core-clone-01/Kiowayyy2109/makefun-github-core-clone-01-v2/reward.svg)](https://make.fun/bounty/github-core-clone-01?utm_source=github&utm_medium=readme&utm_campaign=github-core-clone-01)

# Forge — a GitHub-compatible git forge

A single-file implementation (`main.ts`, Node.js ≥ 22 standard library only,
no dependencies) of a GitHub-compatible forge, built for make.fun's
`github-core-clone-01` competition.

## What it implements

- **Git smart HTTP** — protocol v0 and v2: ref advertisement, `ls-refs`
  (symrefs, peeled tags, ref-prefix filtering, unborn HEAD), `fetch`
  (incremental haves, shallow clones, deepen), and `receive-pack` with
  sideband report-status (per-ref ok/ng, non-fast-forward refusal).
  Packs are read with full delta resolution and written as plain objects.
- **REST v3** — repositories (create/read/patch/delete, paginated listing),
  contents API (create/update/delete files with real git commits), branches,
  git refs, commit history, labels, issues with comments, and pull requests
  (file diffs, squash/merge with three-way conflict detection → 405).
- **GraphQL** — the document set `gh` and `github-mcp-server` send: viewer,
  repository fields, issues/pull-requests connections with cursor pagination,
  the create/close/merge/comment mutations, Relay global ids with `node(id:)`,
  operation batching, and the introspection probes those clients run.
- **MCP** — `POST /mcp` speaking both protocol eras (`server/discover` and
  the `initialize` handshake) with nine tools over the same store, so objects
  created over MCP are the same objects REST reads.

State is in memory with a deterministic logical clock. The token owner's
identity is re-derived from the competition's seeded scenario generator.

## Running

In a trial the env sets `MAKEFUN_ENV_URL` and `MAKEFUN_SHOP_TOKEN`; the
process binds an ephemeral port, registers with the env, and exits when the
env says the trial is done.

For local development:

```sh
MAKEFUN_SHOP_TOKEN=dev-token FORGE_PORT=8931 node --experimental-strip-types main.ts
```

then exercise it with the kit's runner:

```sh
node --experimental-strip-types sdk/run.ts --url http://127.0.0.1:8931 --token dev-token --perf
```
