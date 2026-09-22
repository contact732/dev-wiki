# ideas

Runs are records; ideas are the product. This page is the rulebook, and
[[ideas.base]] is the board — open it to see every idea by status.

## The pipeline

Every idea is its own note in `ideas/`, copied from [[templates/idea]], and
carries a status property:

seed → testing → building → shipped, or dead at any point.

- **seed** — written down, nothing checked yet.
- **testing** — you did one thing to test it against the world.
- **building** — a repo exists. Anything with a face walks [[design]] first:
  copy [[templates/design-brief]] into the repo and commit a direction before
  the first component.
- **shipped** — it's live. The note stays as the record of why it was built.
- **dead** — killed. The note stays too, with a `Killed:` line at the top
  saying why, so it stays killed instead of coming back every month looking new.

## Where ideas come from, and when

Ideas are separate from collection: a scan records the market and never
touches this pipeline. When you want idea work, run

    cd ~/market-scan && python3 ideas.py

It folds the current day-metas into families (five `paired:*` legs are one
meta, not five candidates), calls each family's shape, says which are already
held by an idea or researched in a dossier, bumps `last-signal` on ideas whose
`watches:` words fired, and names anything silent past 7 days.

The other springs: the Signals in any run note, every narrative's Dev angle,
and **paid demand** — places where someone is already paying for tools, which
is evidence no scan can give:

- [Superteam Earn](https://superteam.fun/earn/bounties/) — Solana bounties,
  tooling tasks routinely $500–$5,000.
- Platform builder funds — e.g. PAIR's weekly Spotlight on Robinhood Chain
  pays launches it likes; launchpads fund tools that feed them.
- Ecosystem grant RFPs — a published RFP is a customer with a budget naming
  the product.

## The workshop: candidate → idea

A candidate line is a signal, not an idea. The distance between "family has
6 coins" and a dev project is where quality is made, and it is walked the
same way every time — `python3 ideas.py --brief <word>` prints this protocol
prefilled for one family:

1. **Shape** — is there a buildable surface, or only a trade? Pattern metas
   (pairing mechanics, launch venues, quote anchors) usually have one; word
   metas (a name people ape) usually do not. No surface: stop, log the word
   in its dossier, walk away. Most day-metas fail here, and should.
2. **Who else** — name the three closest existing products and what they do
   NOT do. The gap must survive naming them.
3. **Pain** — what users and creators ask for out loud (X search via web:
   `site:x.com <meta> "wish" OR "need" OR "why is there no"`).
4. **Paid demand** — is anyone paying already? Bounties, builder funds, RFPs.
5. **Score** — the template's card: heat / gap / edge / money, 1–5 each with
   one line of proof, then set conviction and write the note with `watches:`
   set to the family's words. `like:` stays yours.

An idea that skips 2 or 3 is a guess wearing a hat. Running the workshop is
a Claude-session act: hand it a brief and it searches, names names, fills
the card; you set `like`.

## Rating: two numbers, two questions

- `conviction` 1–5 — *will it work?* The evidence's number.
- `like` 1–5 — *do I want to build it?* Your number, no justification owed.
  0 means not yet rated. The board sorts by it, so rating is how you tell the
  system what to surface first.

They disagree on purpose: a conviction-5 like-1 idea is a business you don't
want; a conviction-1 like-5 idea is a toy worth one day. Both are useful to
know.

## The rules that keep it honest

1. An idea born in a run note gets promoted to its own note within a day, or
   it dies unrecorded. Surviving the night is the first filter.
2. Every scan that touches an idea adds one dated line to its Why now section,
   linking the run — and bumps its `last-signal` date. An idea whose
   last-signal is weeks old is going stale, whatever its conviction says.
3. Conviction is a 1–5 gut number you must write down. Changing it is fine;
   not having one is not.
4. Every idea names what would kill it, and the next step is always one act
   small enough for a day. An idea with no next step is dead and hasn't heard.

<!--ideas-->
## Candidates

Day-meta families with no idea attached, as of the data from 2026-09-21.
Refreshed only by `python3 ideas.py`, never by a collection run.
Workshop the pattern metas first; a word meta earns one only by surviving.

- **family** — 6 coins · bsc · 1 day · word meta: tradeable, rarely buildable · no dossier · `python3 ideas.py --brief family`
- **clip** — 4 coins · bsc+robinhood · 1 day · word meta: tradeable, rarely buildable · no dossier · `python3 ideas.py --brief clip`

### Already held

- pairing pattern — covered by [[ideas/pair-leg-radar]], [[ideas/robinhood-chain-analytics]]
- nvda — covered by [[ideas/pair-leg-radar]], [[ideas/robinhood-chain-analytics]]
- stock — covered by [[ideas/pair-leg-radar]], [[ideas/robinhood-chain-analytics]]
<!--/ideas-->
