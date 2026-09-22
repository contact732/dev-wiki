---
status: seed
born: 2026-09-21
last-signal: 2026-09-21
conviction: 4
like: 0
watches: [stock, paired:MSTR, paired:MSTRX, paired:MSFTX, paired:AAPL, paired:NVDAX]
tags: [idea]
---

The cross-venue radar for stock-paired launches: which stock is the hot pair
leg *right now*, across StonkFun on Solana and PAIR/Pons/onlyup on Robinhood
Chain — one board no single launchpad can show, because each only sees itself.

Sharp wedge of the broader [[ideas/robinhood-chain-analytics]] play; the
scan's own `paired:*` detection is the working prototype.

## Why now

- 2026-09-21 — our scan holds five live `paired:*` day-metas (MSTR, MSTRX,
  MSFTX, AAPL, NVDAX) across both chains at once: [[runs/2026-09-21]],
  [[narratives/stocks-on-chain]]
- 2026-09-21 — Solana side industrialized: StonkFun's STONK ran to a ~$140M
  cap on its [Raydium LaunchLab integration](https://x.com/TheBlockCo/status/2096704326232342748),
  ~$300k revenue in week one, [Raydium calls it official](https://x.com/Raydium/status/2087250639533314163)
- 2026-09-21 — Robinhood side industrialized: [PAIR](https://www.globenewswire.com/news-release/2026/08/31/3353221/0/en/pair-launches-the-first-multipool-rwa-launchpad-on-robinhood-chain-pairing-new-tokens-with-baskets-of-tokenized-stocks-partners-with-aws-to-scale-its-infrastructure.html)
  pairs launches against baskets of up to 5 of 24 Robinhood Stock Tokens —
  $26M volume, 160k trades, 1,200+ tokens since Aug 26
- The meta now has two chains, 3+ venues, and zero cross-venue view. Traders
  rotate by stock leg (MSTR day, NVDA day); nothing shows the rotation.

## Who else

- **StonkFun's own rewards dashboard** — building analytics in-house, but
  only for StonkFun pairs on Solana. Does NOT see Robinhood Chain at all.
- **PAIR's platform stats** — its own volume/rewards numbers. Does NOT see
  Solana, or even Pons next door.
- **DexScreener / GeckoTerminal** — have every pool, but no concept of
  "stock-paired" as a class; you cannot ask "show me today's launches quoted
  in MSTR across chains." The grouping is the product, and nobody groups.

## Scorecard

| axis | score | proof |
| --- | --- | --- |
| heat — attention now | 5 | five paired:* metas live in one scan; STONK $140M; PAIR $26M in 3 weeks |
| gap — unserved | 4 | every venue dashboards itself; no cross-venue, no leg-rotation view anywhere named above |
| edge — why me faster | 5 | onlyuprh indexer already on Robinhood Chain, onlyupsol underway on Solana, and scan.py already detects pair legs — the hard part exists |
| money — who pays | 3 | PAIR's weekly Spotlight + builder fund; Superteam-style tooling bounties; or it's the discovery layer feeding onlyup launches — real but not yet recurring |

Conviction 4: the card's weakest leg is money, and the watch words will say
loudly if heat dies.

## What would kill it

- The pairing meta fading — the `watches:` words going silent answers this
  without argument. Robinhood Chain volume already cooled −20% window over
  window ([[runs/2026-09-21]]).
- DexScreener shipping a "stock-paired" filter — one feature from a giant
  erases the grouping gap. Check for it before each build week.

## Next step

One day: a static page off the scan's own stored data — pair legs ranked by
coins and volume, last 3 days, both chains. If reading it once doesn't beat
opening three dashboards, kill the idea before building the live version.

## Log

- 2026-09-21 — born from the first run of the workshop protocol
  (`ideas.py --brief paired:MSTR`); competition and demand searched same day.
