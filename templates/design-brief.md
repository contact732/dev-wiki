---
type: design-brief
project:
idea:            # [[ideas/<slug>]] this build came from
mode:            # operate | persuade | read | experience — per surface, see [[design]]
theme:           # dark | light | both — forced by the scene sentence below, not habit
status: draft    # draft → committed → built
date:
tags: [design]
---

Copy this into the project repo as `DESIGN-BRIEF.md` when an idea hits
**building**. Rules and laws live in [[design]]; this file holds only the
decisions. A block that reads like a mood is not decided yet.

## Scene

One sentence: who uses this, where, under what light, how often a day.
(This sentence picks the theme and the motion budget — write it honestly.)

## Task

What the user comes to do, in one line. The three states that matter most
(e.g. scanning the board, drilling one row, waiting on data). What must feel
instant.

## Direction contract

- **THESIS** — the one idea this surface owns, and the category default it
  refuses.
- **OWN-WORLD** — palette and component language, specific enough to be
  recognizable with all content removed.
- **STORY** — what the visitor understands, believes, and does.
- **FIRST VIEWPORT** — the exact composition: what is where, at what scale,
  where the primary action sits.

## Tokens

Generated MASTER.md at: `design-system/<slug>/MASTER.md`   (yes/no, date)

- Ground / surface / second neutral:
- Accent (not red, not green):
- State vocabulary confirmed (hover/focus/active/disabled/selected/loading/error/warn/success/info):
- Face(s) + scale ratio:

## Motion budget

The one authored moment:
Never animates (frequency law — list the hot paths, e.g. tick updates,
keyboard nav):

## Review

- [ ] batched round 1 (desktop 1440 + mobile 390, real data, all states)
- [ ] Before/After table written, fixes batched
- [ ] confirm round 2 — then stop
- [ ] pre-delivery sweep: contrast both themes, keyboard path, empty/error/
      loading real, no CLS on data arrival, tabular-nums, focus rings themed
- [ ] next-day motion watch, slow motion, once
