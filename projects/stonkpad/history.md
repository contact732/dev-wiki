---
updated: 2026-09-25
tags: [stonkblend]
---

# History

Times in UTC unless marked local (the owner is at UTC−3).

## 2026-09-22 → 23: built as StonkPad, launched as StonkPair

Spec and build plan; launch path, keeper, indexer and site built with CI
green (validator proofs and a browser e2e); deployed to Railway as StonkPair
on stonkpair.fun with five test coins.

## 2026-09-24 morning: relaunch as StonkBlend

- Paused, renamed, relaunched on stonkblend.fun on a fresh database behind a
  password at the edge; the old `railway` database later dropped on the
  owner's word (`DROP DATABASE railway WITH (FORCE)`).
- The owner's design (OTC-style) adopted class for class; hero rebuilt as
  the OTC banner with real figures; the CA button publishes the desk's
  address; coin cards carry the basket's logos piled to the right, one
  size, first on top; the board's Filter is OTC's panel; the launch form's
  dev buy is STONK-only with presets 50/100/150/200; the /design mirror
  kept usable.
- Rulings: **no coin charts and no trading** on the site (`6611c22`); **no
  placeholder drawings**; **no fake numbers** (dividends total and wallet
  lookup made real; revenue pills, "where it sits", launch X link removed).
- Keeper hardening: polling confirms instead of a websocket (a 429 on the
  websocket had killed it for two hours), watchdogs, restart policy in
  `railway.json`, the container entrypoint made CRLF-proof, the ops wallet's
  self-refuel (`335b201`).

## 2026-09-24 afternoon: "0 STONK loaded"

- Diagnosed: StonkFun forwards the creator's half only past a $5 floor and
  its ledger lags the chain 40 min–2 h. The owner: "I want to front it".
- `d6e47d2` fronting from the ops wallet (cap 20/coin, reserve 200, FIFO
  repayment), rounds spaced five minutes; `c436b09` the coin page's "On the
  curve" is our own ledger; `0096900` the **Next round** countdown fed by the
  keeper's `/config`; MIN_ROUND_STONK set to 10 by the owner.
- Pre-IPO legs: Manifest order-book routes refused in simulation
  (`0x1771`); `281f701` retries a failed leg with 3× slippage and
  `excludeDexes=Manifest` (verified live 19:04Z and 19:05Z).
- `b346566` the dust bug: a split's residue (1 raw STONK) was carried like
  a leg and ran an empty round every five minutes with eight refused quotes;
  `LEG_MIN_STONK` floor added.

## 2026-09-24 evening: the volume night

Dozens of launches, thousands of trades, hundreds of holders.

- **21:00Z** the keeper's watchdog killed the process: one sequential tick
  over 29 coins passed 8 minutes. Railway showed "Completed" and did not
  restart it. Down until the owner restarted it at 21:33Z; the indexer's
  own watchdog killed the catch-up at 21:41Z; stopgaps INTERVAL_MS 600000
  and POLL_MS 300000 while the fixes were built.
- Review written (`caf0649`, `docs/reviews/2026-09-24-volume-review.md`).
- The ops wallet burned 5.16 → 1.40 SOL in 95 minutes on top-ups (rent for
  1,155 holder accounts, 2.4 SOL parked as slack); later hit 0.007 SOL twice;
  the owner sent SOL. Payout floor raised to 50 STONK (owner's number).
- SPYGOLD leak: eight repayments refused for lack of SOL in the payout
  wallet, then the round swapped the 20 STONK due back.
- **2b796a3** (23:11 local 19:11) the big fix: coins in parallel, progress
  watchdog, batched accruals, Pre legs routed right, repay top-up, STONK
  payout floor, top-ups sized to accounts that need opening, duplicate-send
  guard, the site's negative caches.
- Three coins at once blew Jupiter's rate limit (every price read 429,
  forty rounds failed at the payout step): **946ebe1** paced queue, 429
  retries, memoised prices; concurrency set to 2.
- 22:20Z Cloudflare flagged `https://stonkblend.fun/` as suspected
  phishing. The owner bought stonkblends.fun.

## 2026-09-25: the go

- The owner set Restart Policy = Always in Railway.
- An interrupted command had pushed and deployed the indexer concurrency
  (`ee06652`); reverted on the owner's word (`dc2c9fa`), then staged again
  with the rest.
- Staged and tested, pushed on "go push all" at 02:38Z: `ead484f` indexer
  two coins at once, `f47f138` per-leg carry (migration 0009) + sweep
  script, `310ea29` domain move, `a09e698` runbook, `ebad8ba` graduated
  market cap.
- 02:40Z keeper deployed (migration confirmed), 02:47Z web, 02:49Z edge
  with all four domains; every page 200 on stonkblends.fun, redirects and
  metadata verified.
- Graduated caps still dashes → Jupiter answers only 50 ids a request;
  `64ba540` batches of fifty, deployed 03:03Z; ELONCOIN shows $47.8K.
- Sweep dry run: 16 wallets, 2.01 SOL, awaiting the owner's word.
- Meme baskets planned (`11a299f`), then built and pushed on "build then push": `37b9ee8` (assets table, hourly sync, registry, Memes tab, launch check, impact guard) and `9c72885` (the launch page request-time only: the first web deploy failed at prerender for want of a database address). Live 04:04Z with 235 memes offered.

## Numbers at the end of the night

42 coins at 21:25Z, 62 by 23:20Z; ~8,000 trades; 536 payouts to 293 wallets
in two hours; 190 advances and 76 repayments; ops income 790 STONK in two
hours against 4.04 SOL of top-ups.
