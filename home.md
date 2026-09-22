# home

The dev vault: watch the market, understand what's moving, turn that into
things worth building. Three layers, and one law — the tool writes numbers,
people and Claude write judgment, and neither ever overwrites the other. The
tool may refresh dated pointers inside written notes (frontmatter dates,
statuses, this line) — never words.

<!--scan-->As of [[runs/2026-09-21]]: 44 coins hot, 12 day-metas live.<!--/scan-->

## The map

| note | what it is | written by |
| --- | --- | --- |
| [[market-scan]] | the tool: what it answers, how to run it, its tracking rules | hand |
| [[runs/]] | one record per scan day — verdict, volume, movers, migrations, signals | tool |
| [[coins]] | the CA board: every tracked coin, hot → watching → archived | tool |
| [[research]] | who deserves a dossier next, in priority order | tool |
| `coins/` | one dossier per researched coin: catalysts, bull, bear, verdict | hand + Claude |
| `narratives/` | one note per story: what feeds it, what kills it, the dev angle | hand + Claude |
| [[ideas]] + [[ideas.base]] | the idea pipeline's rulebook and its board | hand |
| `ideas/` | one note per idea: seed → testing → building → shipped / dead | hand |
| [[design]] | how UI/UX gets made for anything that reaches building | hand + Claude |
| `templates/` | the blank shapes: idea, coin, narrative, design-brief | hand |

## The loop

1. Curious? Run the scan. Read the day's run note.
2. Anything worth keeping goes to `ideas/` within a day, or dies unrecorded.
3. When you want understanding, work [[research]] — or tell Claude to.
4. Narratives feed ideas; ideas become projects; runs older than a month are
   disposable records and can be deleted without loss.
5. Every run re-dates the coin and narrative notes it can know about, and
   [[research]] flags overdue verdicts and catalysts. Ideas are deliberately
   separate: collection never touches them — `python3 ideas.py`, run when you
   want ideas, proposes candidates, bumps watches and flags stale ones.

## Honest limits

X/Twitter has no free feed, so research is search-grounded snapshots taken
when the queue is worked, not live monitoring — that's why catalysts carry a
`check by` date. The scan's net is the free API's first pages, so migration
lists are a sample, not a census. And nothing the tool generates contains
opinion; nothing containing opinion is ever regenerated.
