---
status: live
updated: 2026-09-25
tags: [project, stonkblend]
---

# StonkBlend — project wiki

StonkBlend (stonkblends.fun) is a Solana launchpad whose coins pay their
holders in tokenized stocks. Every coin is a real StonkFun launch paired to
STONK; a keeper turns the coin's creator fees into the basket of stocks the
launcher picked and sends each holder their share, about every few minutes.
This folder is the full record: how it works, what runs where, every knob,
every change pushed, every incident, every plan.

The idea note that started it is [[ideas/stonkblend]]. The product repo is
`~/Documents/stonkpad` (GitHub contact732/stonkpad, private); its `OPS.md` is
the operator's manual and the source most of this wiki condenses. Nothing
secret is written here: this vault is public.

## The map

| page | what it answers |
| --- | --- |
| [[stonkblend/how-it-works]] | the whole loop, launch to payout, with the numbers |
| [[stonkblend/architecture]] | services, packages, tables, the edge, the design repo |
| [[stonkblend/keeper]] | the round, step by step: fronting, legs, floors, top-ups, refuel, pacing, watchdog |
| [[stonkblend/indexer]] | trades, arrivals, holders, curves, prices, graduation, alerts |
| [[stonkblend/site]] | every page, where each figure comes from, the frozen design's rules |
| [[stonkblend/operations]] | deploying, variables, logs, restarts, the sweep, domains, rollback |
| [[stonkblend/history]] | the timeline: what was pushed when, and every incident |
| [[stonkblend/decisions]] | the owner's rulings, which outrank any default |
| [[stonkblend/plans]] | what is planned and not built: meme baskets and the rest |

## Where it stands (2026-09-25 03:30Z)

- Live at **stonkblends.fun** since 2026-09-25 02:49Z. The first domain,
  stonkblend.fun, redirects there (Cloudflare flagged its home page as
  suspected phishing on 2026-09-24; a review was requested).
- 62 coins launched, 4 graduated, thousands of trades, hundreds of holders
  paid. Everything on the volume-night fix list is deployed; master is at
  `64ba540` plus one local plan commit.
- Payout floor 50 STONK a holder; two coins in flight at once in both the
  keeper and the indexer; Jupiter paced to one call a second.
- Still the owner's: a Jupiter API key (faster rounds), the Cloudflare
  dispute, and the word to sweep 2.01 SOL of parked top-ups back to ops.
