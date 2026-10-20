---
updated: 2026-09-25
tags: [stonkblend]
---

# Architecture

## Repositories

| repo | what | notes |
| --- | --- | --- |
| `~/Documents/stonkpad` (contact732/stonkpad) | the product: `web/`, `keeper/`, `indexer/`, `packages/db`, `packages/launchlab`, `edge/`, `ops/`, `docs/` | worktree `~/Documents/stonkpad-live-chart` (branch `live-chart`) is where the volume-night work was done and pushed to `master` |
| `~/Documents/stonkblend` | the owner's design: `build.py` generates the static site into `site/`; a preview Worker on workers.dev | the app vendors the design's CSS, script, fonts and logos; the design's Worker must never list the domain |
| `~/Documents/dev` | this wiki | public |

## Services on Railway (project `stonkpad`, env `production`)

| service | image | what |
| --- | --- | --- |
| `web` | `web/Dockerfile` | the Next 16 app; reached only through the edge Worker (checks `x-edge-key`) |
| `Postgres` | Railway's | database `stonkblend` (the old `railway` database was dropped) |
| `keeper` | `ops/Dockerfile` | one container running the **indexer and the keeper** under `ops/supervisor.mjs`; if either exits the container exits |

The indexer owns the schema and runs migrations at boot; the keeper expects
the tables. Private networking is IPv6; the site reaches the keeper at
`http://keeper.railway.internal:8789` for `/payout` and `/config`.

Restart policy: `railway.json` says ALWAYS but Railway did not honour it on
2026-09-24 (the service showed "Completed" after an exit). The owner set
Restart Policy = Always in the dashboard on 2026-09-25.

## The edge (Cloudflare)

Worker `stonkblend-edge` (`edge/worker.js`, `edge/wrangler.jsonc`) owns four
custom domains: stonkblends.fun, www.stonkblends.fun, stonkblend.fun,
www.stonkblend.fun. It is a plain reverse proxy to the Railway origin, with:

- one canonical host, `stonkblends.fun`; every other host and plain http
  redirect to it with a 301, except `/api/coins/<mint>/metadata.json`, which
  every host serves itself (coins launched on the first domain carry its
  address in their on-chain URI);
- `x-forwarded-for` from `cf-connecting-ip`, `x-forwarded-proto: https`,
  `x-forwarded-host`, and the `x-edge-key` secret the origin requires;
- an optional password gate (`SITE_PASSWORD` secret; removed 2026-09-24
  19:24Z, the site is public), an `OFFLINE` switch, no caching of its own.

Cloudflare's "Suspected Phishing" interstitial (abuse report of 2026-09-24
on `https://stonkblend.fun/`) is shown by Cloudflare before the Worker runs,
on that one URL only.

## Packages

- `packages/db` — drizzle schema, migrations (`0000` … `0009_round_legs`),
  `createDb`/`migrateDb` (pglite in tests, postgres-js in production).
- `packages/launchlab` — the StonkFun/LaunchLab program bindings, pool
  reading, `jupiterPrices` (batches of 50 ids), `fetchWithTimeout`,
  constants (`STONK_MINT`, `SOL_MINT`, pool statuses).
- `web` — the site. `keeper` — rounds. `indexer` — the chain readers.

## Tables

| table | what | written by |
| --- | --- | --- |
| `coins` | one row per launched coin: mint, pool, payout wallet, basket, launch slot, graduation, CPMM pool | site (launch confirm) / indexer |
| `launches` | the launch form's record, logo, X link, confirmation | site |
| `trades` | every curve trade with its fee (`fee_b`) | indexer |
| `arrivals` | every STONK arrival in a payout wallet from anyone but the keeper | indexer |
| `holders` | current balances per coin, with the slot read | indexer |
| `curves` | one row per coin: the pool's reserves, supply, status, updated_at | indexer |
| `prices` | USD per mint per minute: STONK, SOL, every basket stock, every graduated coin | indexer (and the keeper's rounds) |
| `cursors` | per-stream walk positions | indexer |
| `rounds` | one row per round: stonk_in, ops_out, bought, holders_paid, carry_out, **legs** (0009), status, note | keeper |
| `sends` | every transaction the keeper signs, recorded before broadcast: topup, ops, swap, payout, advance, repay, refuel | keeper |
| `accruals` | what each holder is owed per stock | keeper |
| `payouts` | what each holder was paid, per round | keeper |
| `alerts` | fee_lag, payout_mismatch, ops_low, advance_low; shown on the desk only | indexer / keeper |
| `settings` | the desk's published values (contract address, X handle) | desk |
| `tracked_coins` | coins the desk publishes that were not launched here | desk / indexer |

## Data flow in one picture

chain ⟶ indexer (trades, arrivals, holders, curve, prices) ⟶ Postgres ⟶
keeper (settle fronting → round: ops slice → swaps → accruals → payouts) ⟶
chain; Postgres ⟶ web (pages, 5 s cache per key) ⟶ edge ⟶ visitor.
