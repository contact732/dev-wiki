# market-scan

The tool. It answers one question on demand — what moved on Solana, BNB Chain
and Robinhood Chain in the last three days, and what does that suggest
building — then files everything into this vault by itself. The map of what
lands where is [[home]].

## Running it

```
cd ~/market-scan
python3 scan.py           the scan, filed into the vault
python3 scan.py --refile  regenerate the boards from stored data, no fetching
```

No installs, no keys. Data: [DefiLlama](https://defillama.com) for volume
history, [GeckoTerminal](https://www.geckoterminal.com) for pools. A run takes
about a minute because the free source limits how fast it answers; if it says
it gave up, wait a minute and run again. Raw terminal output of every run is
kept in `~/market-scan/reports/`; the registry is `~/market-scan/coins.json`.

## What earns tracking

Structure outranks size, in this order:

1. **Migrated today** — graduating is the event; no volume bar at all. Copies
   still on a curve are counted with it: a copied migration is one someone
   noticed.
2. **Narrative cluster** — a word shared by 3+ fresh coins puts the whole
   cluster on the board at a $10K bar. A group of small coins named around one
   thing is a meta being born.
3. **Anchor** — a non-major coin that 2+ fresh pairs quote against. Part of
   the story without being the story; its own size is beside the point.
4. Volume last: trending ≥ $1M/24h, or +100% in a day on ≥ $250K, or ≥ $250K
   on a first day. Majors and stables never qualify.

## Aging and birthdays

Seen by the latest scan = **hot**; within 7 days = **watching**; older =
**archived** — out of the meta, kept as history with its full sighting trail.
The `seen` count is the meta signal: persistence separates a meta from a
spike. Every coin gets a **launched** date — exact when the scan ever saw it
on a curve (a curve pool's creation is the launch), `~` for an upper bound,
`?` for unknown. Every curve pool crossed today is a birthday banked for
whatever migrates tomorrow, so this sharpens with use.

## Short and long trends

The trend is what launched inside the last 3 days — a coin born before that
can be *associated* with a trend without being in it. Narratives split by age:
**now** = born inside the window, the day-metas that live a day or two;
**running themes** = alive longer and still being seen, the older trends
day-metas run off (the board shows the lineage when it finds one). The run
note's Trend section groups the window's migrations by the narrative they
carry, day-metas first.

Latest: [[runs/2026-09-21]]
