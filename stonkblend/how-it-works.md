---
updated: 2026-09-25
tags: [stonkblend]
---

# How StonkBlend works, end to end

## 1. A launch

A launcher opens stonkblends.fun/launch, connects a wallet, names the coin,
uploads a logo, picks a basket of one to five stocks (the picker's tabs:
xStocks 24, Sunrise 71, PreStocks 7, Tessera 2 — 104 tokens, the allowlist
in `web/src/lib/stocks.ts`), optionally an X link, and a first buy in STONK
(presets 50, 100, 150, 200 STONK; no SOL option, no percentages).

The site's `POST /api/launch/prepare` builds the StonkFun LaunchLab
transaction: the coin's creator is set to the coin's **payout wallet**, which
the keeper derives as `HMAC-SHA512(master secret, mint)`; the site asks the
keeper for that public key over Railway's private network (`GET /payout`).
The coin's on-chain metadata URI points at
`https://stonkblends.fun/api/coins/<mint>/metadata.json`, which the site
serves (name, symbol, logo, external link). The wallet signs; the site polls
for confirmation (`POST /api/launch/confirm`) and records the coin.

## 2. Trades and fees

Every trade on the coin's StonkFun curve pays a 1 % fee in STONK. StonkFun's
own off-chain ledger gives the creator half of it (0.5 % of volume) and
forwards it from StonkFun's fee wallet to the creator — our payout wallet —
once the coin's share clears a $5 floor. The first forward comes about 30 s
after StonkFun "adopts" the coin (~5 min after launch); later ones can lag
hours. There is nothing to claim or enable.

The indexer walks each coin's pool for trades (recording each trade's fee)
and each payout wallet for arrivals, keeps the holders table from the chain,
reads the curve for the market cap, and writes prices.

## 3. Fronting

Because StonkFun pays late, the keeper **fronts** the creator's share from
the ops wallet's STONK the moment trades exist: owed = Σ trade fees ÷ 2 −
Σ arrivals, sent into the payout wallet as an `advance`, at most 20 STONK
outstanding per coin, never dipping the ops wallet under its 200 STONK
reserve. When StonkFun's forward lands, the keeper takes that much back
(`repay`) before the round reads the wallet. See [[stonkblend/keeper]].

## 4. A round

About every five minutes per coin (when the payout wallet holds at least 10
STONK, or a leg left over from a failed swap): the keeper takes the 10 % ops
slice off the new STONK, swaps the rest through Jupiter into equal legs of
the basket (a pre-IPO leg routes around the Manifest order book; a failed
leg is retried with more slippage, and if it still fails its STONK is bought
on its own next round), credits every holder their pro-rata share of each
stock bought, and pays out every holder whose accrued share of a stock is
worth at least 50 STONK. A payout opens the holder's stock token account if
needed (rent 0.00214 SOL, paid by the payout wallet, topped up from ops).

## 5. What the holder sees

Stocks arrive in the holder's wallet as the xStock / Sunrise / PreStock
token itself. The coin page shows every round, payout and fee arrival, the
next round's countdown, what StonkFun still holds, and the market cap. The
Dividends page shows what is waiting and the largest holders of the top
payer; Rewards ranks coins by what they bought for holders; Revenue shows
the site's own figures.

## 6. Money in, money out (for the operator)

- In: the ops slice, 10 % of every round's new STONK, into the ops wallet.
- Out: SOL for every swap and payout fee and for holders' account rent;
  STONK fronted (returned when StonkFun pays).
- The ops wallet refuels its own SOL by selling its STONK when under 1 SOL,
  aiming for 3 SOL.
- The payout floor keeps rent a small part of what a payout delivers: at the
  old 0.005 SOL floor, 43 % of a smallest payout was rent.
