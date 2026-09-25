---
updated: 2026-09-25
tags: [stonkblend]
---

# The indexer

`indexer/src/`. Runs in the keeper's container, starts first, owns the
schema (migrations at boot). Two schedules: a pass over every coin every
`POLL_MS` (60 s), and a price pass every `PRICE_MS` (60 s). Since 2026-09-25
a pass works on `INDEXER_CONCURRENCY` (2) coins at once; a sequential pass
over 62 coins took up to six minutes.

## Per coin, per pass (`tick.ts`)

Each stream runs in its own try, so one failing does not stop the others:

- **Trades**: signatures on the coin's pool since the cursor, oldest first,
  never before the launch slot; each transaction decoded (`decode.ts`) into a
  trade row with side, trader, amounts and the fee in STONK (`fee_b`). A
  transfer-fee mint's amounts honour the fee.
- **Arrivals**: STONK landing in the payout wallet from anyone but the
  keeper (an advance is never mistaken for a forward).
- **Holders** (`holders.ts`): re-read from the chain when a trade was
  inserted or every `HOLDERS_MS` (300000 on Railway): every owner's
  balance upserted with the slot, gone owners deleted, in one transaction; a
  read whose balances do not sum to the supply is refused. The keeper
  refuses a snapshot older than 3,000 slots.
- **Curve** (`curve.ts`) and **graduation** (`graduation.ts`): one pool read
  serves both; the curve row (reserves, supply, status, updated_at) is what
  the site prices a market cap from; status 2 marks the coin graduated with
  its CPMM pool.
- **Fee lag alert** (`alerts.ts`): a coin with trades and no arrival for
  `FEE_LAG_MS` (1 h) raises `fee_lag`; cleared on the next arrival.
- "Deferred": a walk that stopped at a signature the RPC listed but would
  not serve waits there; `/health` names the coin.

## The price pass (`prices.ts`)

One row per mint per minute: STONK, SOL, every basket stock, and every
graduated coin (so its market cap can be quoted from Jupiter's price for the
coin in its new pool). Jupiter answers only the first fifty ids of a
request, so `jupiterPrices` asks in batches of fifty (found 2026-09-25: 57
asked, 50 answered, the graduated coins last in line and unpriced).
Then `refreshTracked` re-asks StonkFun about the desk's tracked coins.

## Watchdog and health

The same progress watchdog as the keeper's (a coin finishing counts). `/health`
answers 503 when the last pass is older than `STALE_MS` (120 s).

## Knobs

| variable | value | meaning |
| --- | --- | --- |
| POLL_MS | 60000 | sleep between passes |
| PRICE_MS | 60000 (default) | price pass |
| HOLDERS_MS | 300000 | holders re-read cadence on a quiet coin |
| INDEXER_CONCURRENCY | 2 | coins per pass at once |
| FEE_LAG_MS | 3600000 (default) | fee lag alert |
| PAGE_LIMIT | 1000 (default) | signatures per RPC page |
| STALE_MS | 120000 (default) | health staleness |
