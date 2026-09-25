---
updated: 2026-09-25
tags: [stonkblend]
---

# The keeper

`keeper/src/`. One process, one loop (`loop.ts`), a health server on 8789
(`/health`, `/config`, `/payout`), a watchdog (`watchdog.ts`). Every
network call has a deadline (RPC 30 s, Jupiter 20 s). Every transaction is
recorded in `sends` before it is broadcast, confirmed by polling (no
websocket), and settled by `reconcile` if the process died in between.

## The tick

Every `INTERVAL_MS` (60 s): first `refuel` (below), then every coin, `COIN_CONCURRENCY`
(2) at a time. Each coin gets `runRound` (`round.ts`):

1. **Payout wallet check.** The wallet the keeper derives must equal the one
   recorded at launch, else a `payout_mismatch` alert and a skip.
2. **Reconcile** the coin's pending sends; if any is still in flight, skip.
3. **Close a crashed round** (a `running` row older than `LEASE_MAX_MS`,
   10 min): rebuild its carry from its sends, mark it failed.
4. **Basket sanity**, then **settle fronting** (`advance.ts`, below): repay
   what StonkFun has sent since the advance, then front what is still owed.
5. **Spacing:** no round within `ROUND_EVERY_MS` (5 min) of the last.
6. **What is in the wallet:** `carry` (the last round's leftover, already
   taxed) and `left` (the legs the last round could not buy, by stock, from
   `rounds.legs` less the swaps that landed since). New STONK = balance −
   carry. Ops slice = 10 % of new. Pool = balance − ops − Σ left, split into
   equal base legs; each stock's leg = base + its left part.
7. **Floors.** Under `MIN_ROUND_STONK` (10) the round waits, unless a leg
   left over of at least `LEG_MIN_STONK` (1) is waiting. Legs under 1 STONK
   are not swapped (Jupiter finds no route for dust). No round runs without
   a leg worth swapping.
8. **Holders** must be fresh (`HOLDERS_MAX_AGE_SLOTS` 3,000 ≈ 20 min) and
   non-empty after excluding the curve vault, the payout wallet and ops.
9. **Lease:** insert the `running` row with the planned legs; the database
   allows one running row per coin.
10. **Top-up** the payout wallet to 0.05 SOL if under 0.01.
11. **Ops slice** transfer to the ops wallet.
12. **Swaps**, one per leg, via Jupiter (`swap.ts`): pre-IPO mints (`Pre…`)
    exclude Manifest from the first try; any failed leg gets one retry with
    3× slippage and no order book; a pending leg is left alone.
13. **Accrue:** each stock's wallet balance beyond what is already owed is
    this round's pool, split by holder balance, written in batches of 500.
14. **Pay:** prices from Jupiter (memoised a minute; batches of 50 ids); a
    holder's accrual of a stock is paid when worth ≥ `THRESHOLD_STONK` (50)
    of STONK (or ≥ `THRESHOLD_SOL` when STONK has no price). The top-up for
    the payout is sized to the holder accounts that actually need opening
    (0.00214 SOL each) plus fees and slack, and only the difference is sent.
    Four lines a transaction; a failed batch is retried line by line.
15. **Finish:** carry_out = the wallet's STONK now; status done / partial /
    failed; the note lists what did not land.

## Fronting (`advance.ts`)

- earned = Σ `trades.fee_b` ÷ 2; forwarded = Σ `arrivals`; owed = earned −
  forwarded; outstanding = advances − repayments.
- Advance = min(owed − outstanding, cap room, float above the reserve), if ≥
  `ADVANCE_MIN_STONK` (0.5). Cap `ADVANCE_CAP_STONK` 20 per coin; reserve
  `ADVANCE_RESERVE_STONK` 200 in the ops wallet.
- Repay only from forwards that arrived after the advance (each advance
  records forwarded-before and repaid-before; FIFO), as far as the wallet
  holds them; the payout wallet is topped up with SOL first if it has none
  (SPYGOLD's eight refused repayments, 2026-09-24).
- `advance_low` alert when the float cannot cover a coin.

## Refuel (`refuel.ts`)

Before every tick: if the ops wallet holds under `OPS_SOL_FLOOR` (1 SOL) it
sells its own STONK through Jupiter for enough SOL to reach
`OPS_SOL_TARGET` (3 SOL), at most `REFUEL_MAX_BPS` (half) of its STONK per
tick, 3 % headroom; `ops_low` alert if still under. The sale needs a little
SOL for fees: at 0.007 SOL the refuel itself was refused (2026-09-24
22:09Z), so the owner seeds SOL when it gets that low.

## Pacing Jupiter (`pace.ts`)

Every Jupiter call (quotes, swaps, prices) leaves one queue,
`JUPITER_MIN_GAP_MS` (1,100 ms) apart; a 429 waits `Retry-After` or a
doubling backoff and is retried three times; `JUPITER_API_KEY` is sent as
`x-api-key` when set (a portal key raises the limit; then lower the gap).
Prices are memoised for a minute. Without this, three coins at once had
every price read answer 429 and forty rounds fail at the payout step.

## The watchdog

Counts a coin finishing as progress (`lastProgressAt`); exits with code 2
when nothing progressed for 3 × interval + 5 min (8 min at a 60 s tick).
Judging whole ticks killed the keeper on 2026-09-24 when a tick over 29
coins took longer than that. The same watchdog runs in the indexer.

## Sends and reconcile (`sends.ts`)

Kinds: `topup`, `ops`, `swap`, `payout`, `advance`, `repay`, `refuel`.
Recorded pending before broadcast; a signature already on record is never
sent twice (left to reconcile). Reconcile asks the chain for each pending
signature: confirmed → applied (a payout writes `payouts` rows and reduces
accruals, idempotently); failed → failed; expired with no trace → failed;
processed → still in flight.

## Knobs (Railway values on 2026-09-25)

| variable | value | meaning |
| --- | --- | --- |
| INTERVAL_MS | 60000 | the tick |
| ROUND_EVERY_MS | 300000 (default) | least time between a coin's rounds |
| MIN_ROUND_STONK | 10 | STONK a wallet needs before a round |
| LEG_MIN_STONK | 1 (default) | smallest leg worth a swap |
| OPS_BPS | 1000 (default) | ops slice, 10 % |
| THRESHOLD_STONK | 50 | payout floor per holder per stock |
| THRESHOLD_SOL | 0.155 | fallback floor without a STONK price |
| SLIPPAGE_BPS | 100 (default) | first-try slippage; retry ×3 |
| TOPUP_FLOOR_SOL / TOPUP_SOL | 0.01 / 0.05 (defaults) | payout wallet floor top-up |
| LEASE_MAX_MS | 600000 | a running round older than this is crashed |
| HOLDERS_MAX_AGE_SLOTS | 3000 (default) | holders freshness |
| OPS_SOL_FLOOR / OPS_SOL_TARGET | 1 / 3 | refuel |
| REFUEL_MAX_BPS | 5000 (default) | half the STONK per refuel |
| ADVANCE_CAP_STONK / MIN / RESERVE | 20 / 0.5 / 200 (defaults) | fronting |
| COIN_CONCURRENCY | 2 | coins per tick at once |
| JUPITER_MIN_GAP_MS | 1100 (default) | spacing of Jupiter calls |
| JUPITER_API_KEY | unset | portal key |
| PRIORITY_MICROLAMPORTS | 10000 (default) | priority fee bid |
| MAX_PRICE_IMPACT_PCT | 5 (default) | a leg whose Jupiter quote moves the price more than this is refused and carries (thin memes) |
