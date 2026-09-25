---
updated: 2026-09-25
tags: [stonkblend]
---

# The site

`web/`, Next 16, server-rendered, every read through a 5-second in-memory
cache per key (`cache.ts`). The look is the owner's design, vendored class
for class; the app fills the numbers. Rules of the frozen design are in
[[stonkblend/decisions]].

## Pages

| route | what it shows | where the figures come from |
| --- | --- | --- |
| `/` | hero (coins launched, stock bought for holders, paid to holders, rounds run; the CA button), tip strip, the coin board with the OTC filter panel | `listCoins`, payout stats, `latestPrices`, market cap per card |
| `/explore` | the board alone, paged, with the filter panel (Pays out in / By kind / By stock) and search and sort | same keys as Home (`page:coins`, `page:stonk-usd`, `page:coin-usd`) |
| `/coin/[mint]` | header (logo, ticker, CA copy), market cap, price, 24 h volume and change, holders; holder payouts split ("On the curve" = our ledger of what StonkFun still holds, "In the pool"), the fronted sentence, the **Next round** countdown; rounds, payouts, arrivals; the basket pile | `getCoin`, `coinExtras`, `stonkfunFees` (their API, cached a minute, failures kept a minute), `keeperConfig` (`/config`, fallback in `KEEPER_FALLBACK`) |
| `/launch` | the three-step form: coin, basket, first buy (STONK presets) | `/api/launch/quote`, `/prepare`, `/confirm` |
| `/dividends` | what is waiting, eligible holders, rounds, the top payer and its largest holders with what each is owed and was paid | `queries-dividends` |
| `/rewards` | payout stats, recent payouts, coins ranked by what they bought for holders | `queries-rewards` |
| `/revenue` | the site's own figures and its charts (the only charts on the site) | `revenue-view` |
| `/docs` | the API walk-through (prepare/confirm sample names stonkblends.fun) | static |
| `/about`, `/terms` | static | |
| `/desk` | the owner's page behind the desk token: alerts ("What has gone wrong"), published settings (contract address, X handle), tracked coins | `alerts`, `settings` |
| `/design/…` | the frozen design, served as-is for diffing | `web/public/design/` |

API: `/api/launch/{quote,prepare,confirm}`, `/api/coins/[mint]/metadata.json`
and `/logo` (public, also on the old domain), `/api/mint/[mint]`,
`/api/wallet/[wallet]/accruals`, `/api/alerts` (desk), `/api/config`,
`/api/rpc` (server-side forward to the Helius RPC so the key never reaches
the browser).

## Market cap

- On the curve (status 0) or migrating (status 1): the curve's marginal
  price × supply × STONK's USD price, from the `curves` row, only when that
  row is under 10 minutes old (`CURVE_MAX_AGE_MS`); else a dash.
- Graduated (status 2 or `graduatedAt` set): Jupiter's USD price for the
  coin itself (the indexer writes it every minute) × the supply the curve
  row holds, only when the price row is under 10 minutes old
  (`freshPrices`, `graduatedCapUsd`); never from the frozen curve. Volume
  and change stay dashes, since trades in the new pool are not indexed.

## What is deliberately not on the site

No coin charts anywhere, no trading, no placeholder drawings, no fake
numbers (owner's rulings of 2026-09-24). A figure the tables cannot give is
a dash marked `data-placeholder="true"`.

## The design pipeline

Design changes are made in `~/Documents/stonkblend/build.py`, built with
`python build.py`, frozen with `python tools/mirror_design.py`, and copied
into the app with `npm run sync-design` in `web/`; markup changes are then
ported by hand into `web/src/components/design/`. The design's own Worker
is a workers.dev preview and must never list the domain (it took the domain
back once, 2026-09-24 12:23Z).

## Tests

Unit (vitest) in `web/test` (305), `keeper/test` (129), `indexer/test`
(66), `packages/*`; Playwright `web/e2e/pages.spec.ts` (24) against a fresh
build with a seeded pglite database and no network. `next build` rewrites
`web/tsconfig.json`; check it out after an e2e run.
