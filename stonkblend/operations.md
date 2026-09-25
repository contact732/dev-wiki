---
updated: 2026-09-25
tags: [stonkblend]
---

# Operations

The operator's manual proper is `OPS.md` in the product repo; this is the
short form plus what was learned on 2026-09-24/25.

## Deploying

From the worktree (`~/Documents/stonkpad-live-chart`), after tests are
green and the commit is on master:

    railway up -p 45bfe9b1-469e-4dd4-9619-e9148f1f33c6 -e production -s keeper -c
    railway up -p 45bfe9b1-469e-4dd4-9619-e9148f1f33c6 -e production -s web -c

Order when a change adds a table: keeper first (it migrates at boot), then
web (it reads whole rows). Each deploy restarts the service; the keeper
finishes the coins in flight and starts no more; a round killed anyway is
closed after 10 minutes with its STONK carried.

Variables: `railway variable set --service keeper --skip-deploys "K=V" "K2=V2"`
stages several without a rebuild (each set without the flag rebuilds the
service); the next `railway up` picks them up. Never print the variables
(`railway variables` shows secrets); read a single non-secret one with
`railway run --service keeper -- node -e "console.log(process.env.K)"`.

The edge: `cd edge && npx --yes wrangler@4 deploy` (wrangler is not installed
locally; npx fetches it). Custom domains listed in `wrangler.jsonc` are
attached on deploy, which creates their DNS records and certificates; ones
not listed are detached.

## Watching

    railway logs --service keeper      # keeper and indexer, one stream
    railway logs --service web
    railway status                     # Online / Completed

Good signs: `INFO tick` every minute or two; `round N done`; `fronted` /
`repaid`. Bad signs: `watchdog: stuck`, `[supervisor] … exited`,
`answered 429`, `round N failed`, `Completed` in status. Skips are logged
once per change of reason (`skipped: below minimum` is normal for a quiet
coin).

Database reads from the laptop: a script under `railway run --service Postgres`
builds the connection from `PGUSER`, `PGPASSWORD`, `RAILWAY_TCP_PROXY_DOMAIN`
and `RAILWAY_TCP_PROXY_PORT` on database `stonkblend`; never write the
password anywhere.

## When a coin reads 0 STONK

1. Is the container alive (`railway status`, logs)?
2. StonkFun's ledger: `https://www.stonkfun.xyz/api/creator-fees?mint=&wallet=`
   — `accrued − forwarded` is theirs to send; they send past $5 and their
   ledger can lag the chain by hours. The keeper fronts up to 20 STONK
   meanwhile; the coin page's "On the curve" is our own count.
3. Holders: a coin whose only holder is the curve vault is skipped as "no
   eligible holders"; under 10 STONK it is "below minimum".

## The ops wallet

`EMePgPUz4Srs3tARuh47r9yWNQAisw615LVLVQATFgKa`. Holds SOL (fees, top-ups,
rent) and STONK (the fronting float and the ops income). Refuels its own SOL
from STONK below 1 SOL, aiming for 3; if it is ever near empty (under
~0.01 SOL) the refuel swap itself cannot pay its fee, so send it SOL. Never
sweep, pause or move wallets without the owner's word in the moment.

## The SOL sweep

`keeper/scripts/sweep-sol.ts` moves each payout wallet's SOL above 0.06 back
to ops, signed by the wallet itself (derived from the master secret in the
keeper's environment). Mints come from a file, since the keeper's database
URL is private-network only:

    railway run --service Postgres -- node <script that prints mints> > mints.txt
    railway run --service keeper -- npx tsx keeper/scripts/sweep-sol.ts --mints mints.txt --to <ops>          # dry run
    railway run --service keeper -- npx tsx keeper/scripts/sweep-sol.ts --mints mints.txt --to <ops> --send   # on the owner's word

Dry run on 2026-09-25 03:00Z: 16 wallets, 2.0142 SOL; not sent yet.

## Domains

- stonkblends.fun: the site (since 2026-09-25 02:49Z).
- stonkblend.fun: redirects to it; its home page carries Cloudflare's
  phishing interstitial (abuse report 2026-09-24, review requested
  2026-09-25); coin metadata JSON still served there.
- stonkpair.fun: dead end by the owner's ruling; nothing may point at it or
  from it.
- Moving domains again: add the new custom domains to `edge/wrangler.jsonc`,
  set `HOST` in `edge/worker.js`, `NEXT_PUBLIC_SITE_URL` on `web`, the docs
  sample, the design's `DOMAIN`, deploy edge and web, verify the redirects
  and the metadata JSON on every old host. Browsers cache old 301s; a
  visitor who once went www→old lands on the old home page.

## Rollback

`git revert` the commit, push, `railway up` keeper then web. Migrations so
far are additive (new columns with defaults); old code ignores them. The
edge rolls back by deploying the previous `edge/` folder.

## Secrets (names only)

Master secret (every payout wallet is `HMAC(master, mint)`), ops keypair,
desk token, `EDGE_KEY` (Worker secret + web variable), Helius `RPC_URL`,
StonkFun and Jupiter keys if any. In Railway/Cloudflare and gitignored
`.secrets/`; never in this wiki, never in a chat.
