---
status: building
born: 2026-09-22
last-signal: 2026-09-25
conviction: 2
like: 0
watches: [stonkfun, stonk, xstocks, stock]
tags: [idea]
---

StonkBlend is a Solana launchpad whose coins pay their holders in tokenized
stocks. Every coin is a real StonkFun launch paired to STONK, and a keeper
turns the coin's creator fees into a basket of xStocks the launcher picked,
then sends each holder their share automatically.

Live at stonkblend.fun behind a password since 2026-09-24. It launched first
as StonkPair on stonkpair.fun and was renamed the same week; stonkpair.fun is
a dead end on purpose and must never link to or redirect to the new site.

## Why now

- 2026-09-21 — memecoins pairing against tokenized stocks instead of SOL is a
  live meta, and STONK has been on the board since scan 1:
  [[narratives/stocks-on-chain]].
- 2026-09-24 — StonkFun forwards half of every trade's 1% fee to the pool's
  creator, off-chain, with no claim step. That creator is our payout wallet,
  so a coin earns from its first trade.

## Who else

- **StonkFun** — the launchpad underneath. Does NOT turn creator fees into
  stocks or pay holders; the creator keeps the fee.
- **Reflection / dividend tokens** — pay holders in the coin itself or SOL.
  Do NOT pay in stocks, and usually tax transfers to do it.
- **xStocks direct** — buy the stock token yourself. No meme, no launch, no
  holder payout.

## Scorecard

| axis | score | proof |
| --- | --- | --- |
| heat — attention now | 3 | stock-paired launches are a standing meta in the scans |
| gap — unserved | 3 | nobody named above pays holders in a stock basket |
| edge — why me faster | 4 | launch path, keeper, indexer and site all built and live in one week |
| money — who pays | 2 | 10% ops slice of each coin's fees, only once coins trade |

Conviction 2: built and running, no real coin has traded on it yet.

## What would kill it

- StonkFun changing or ending its creator fee forwarding. The whole payout
  loop runs on it.

## Where things live

- **The project wiki: [[stonkblend/index]]** — how it works, architecture,
  keeper, indexer, site, operations, history, decisions, plans.

- Product repo: `~/Documents/stonkpad` (GitHub contact732/stonkpad, private).
  Start with `OPS.md`, then `docs/PAID-SERVICES.md` for costs and limits.
- Design front end: `~/Documents/stonkblend` (static site, Cloudflare Worker).
- Hosting: Railway project `stonkpad` (web, Postgres, keeper), Pro plan,
  daily and weekly database backups.
- Chain: Helius, Developer plan. The RPC URL with its key is in Railway's
  `RPC_URL` variables and in `~/Documents/stonkpad/.secrets/helius-rpc-url.txt`
  (gitignored). Never put it in this wiki; the wiki repo is public.
- Every other secret (master secret, ops keypair, desk token, site password)
  lives in the stonkpad repo's gitignored files or in Railway and Cloudflare
  secrets, never here.

## Next step

Finish the cushion list in `docs/PAID-SERVICES.md`: Cloudflare Workers Paid,
about 1.3 SOL in the ops wallet before a big launch, and a Jupiter API key.

## Log

- 2026-09-22 — born as StonkPad; spec and build plan written.
- 2026-09-23 — launch path, keeper, indexer and site built with CI green;
  deployed to Railway as StonkPair on stonkpair.fun.
- 2026-09-24 — relaunched as StonkBlend on stonkblend.fun on a fresh
  database, password-gated at the edge. Railway moved to Pro with backups,
  Helius moved to Developer.
- 2026-09-24 — the volume night: dozens of launches, thousands of trades;
  fronting, pacing, per-coin watchdog, the 50 STONK floor. Cloudflare
  flagged the home page as phishing; the site moved to stonkblends.fun.
- 2026-09-25 — every fix pushed and live on the owner's go; graduated
  coins show a market cap; the wiki written: [[stonkblend/index]].
