---
updated: 2026-09-25
tags: [stonkblend]
---

# Plans

## Meme coins in the basket (planned 2026-09-25, not built)

Full plan: `docs/plans/2026-09-25-meme-baskets.md` in the product repo.

StonkFun's picker has a `custom` category, 416 launchable tokens today, its
memes plus majors it files there (WBTC, WETH, RAY, JUP, TRUMP, WIF, Bonk,
Fartcoin). Jupiter prices 408; 147 have over $100k of liquidity, 51 under
$1k. The plan:

1. A `basket_assets` table the indexer refreshes hourly from StonkFun's
   pairs plus Jupiter's price and liquidity; stocks move into it too.
2. A liquidity floor for memes ($25k, a knob): about 240 offered.
3. A Memes tab in the picker beside the four stock tabs (the one visible
   change, for the owner to approve); Explore's "By kind" gets Memes; logos
   from StonkFun's logo URLs, cached; ambiguous symbols show name and mint.
4. Launch validation reads the table.
5. A keeper guard: a leg whose quote shows over 5 % price impact is not
   bought that round and carries.
6. Tests, then a flagged rollout: one test launch with a meme basket, watch
   two rounds pay, then the tab goes on.

About one working day. Risks: rugs (the floor and the guard mean the leg
just does not fill), Token-2022 transfer-fee memes (holders receive less
than the accrual says), fast prices (the $-based floor handles it).

## The owner's open items

- A Jupiter API key in `JUPITER_API_KEY` (rounds every ~5 min instead of
  15–20 at 62 coins).
- The Cloudflare dispute on stonkblend.fun (review requested 2026-09-25).
- The word on the sweep (2.01 SOL).

## Ideas not yet planned

- Show the transfer-fee shortfall on PreStock payouts.
- Price graduated coins' 24 h volume from the CPMM pool.
- Recover parked SOL automatically at round end instead of by sweep.
- Split the indexer and keeper into their own services when the plan
  allows.
