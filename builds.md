---
updated: 2026-10-08
tags: [retro]
---

# builds

Every project on one board, one row each. A project here lives a day or two,
so the row is the record: brand, repo, dates, state. The checklist for
getting one live is [[launch]]; the rules below are the ones that save time.
Map: [[home]].

## The board

State checked 2026-10-08. Repos are folders under `~/Documents/`.

| repo | brand (earlier names) | built | live at | state 2026-10-08 | page |
| --- | --- | --- | --- | --- | --- |
| tcat-site | Taowanese Inu | 09-22 | tinu.win | live, coin live | [[ideas/taowanese-cat-site]] |
| compound-inu | Compound Inu | 09-22 → 23 | compoundinu.fun | live, coin live; git has no commits | [[ideas/compound-inu]] |
| stonkpad | StonkBasket (StonkPair, StonkBlend) | 09-22 → 25 | stonkbasket.win | answered 404 today; branch `stonkbasket` unmerged; new wallets unfunded | [[projects/stonkpad/index]] |
| pennypair | PennyPair | 09-25 | pennypair.fun, app.pennypair.fun | live, tracker minted, no coin, proof never run | [[projects/pennypair/index]] |
| ddosinu | DDOSINU | 09-26, 39 min | ddosinu.fun, stonklotto.fun | live, coin live; not under git | [[projects/ddosinu/index]] |
| paidpair | PaidPair | 09-26, 7 h 48 min | none | built, nothing deployed, no remote | [[projects/paidpair/index]] |
| paidblend | DivvyX (PaidPairs, Share) | 09-26 → 28 | divvyxsolana.fun | live behind a password, payouts pretend | [[projects/paidblend/index]] |
| cardstrat | PokeRaffle (PokeStrategy) | 09-29 → 30 | pokeraffle.win | live behind a password, real card buys ran, strategies wiped 10-05 | [[design/reskin/index]] |
| pokechart | PokeChart | 09-29 → 30 | workers.dev | gated pretend front end | [[design/reskin/index]] |
| short | Short | 09-29 | none | spec approved, build state unverified | |
| stonkfuse | StonkFuse | 09-30 | stonkfuse.fun | live and open, $FUSE live since 10-01, real money | [[design/reskin/index]] |
| agencypad | AGENCY (clone) | 10-03 | none | built, gated until rebrand, nothing deployed | [[design/reskin/index]] |
| holderpad | Callout (VoicePad) | 10-04 → 07 | calloutsol.fun | live, gate open, sample data, engine not deployed | |
| mining | Wordle (Mining) | 10-06 → 07 | wordlesol.fun | live and open in test mode; no desk token, no edge key, no coin | |
| routed | Shared (Routed) | 10-06 | useshared.fun | live, UI only, a test CA on the hero button | [[design/reskin/index]] |
| btc-launchpad | (research) | 10-06 | none | research only | |
| desk404 | OTC.exe | 10-07 | workers.dev | live and functional, no domain, nothing ever sent; not under git | [[design/reskin/index]] |
| sentients | SENTIENTS (copy) | 10-07 | workers.dev | gated static copy; back end build started 10-08 | [[design/reskin/sentients]] |
| holdapi | $API (copy of holdapi.lol) | 10-09 | none | front end identical, back end built + audited (267 tests), nothing deployed | [[design/reskin/holdapi]] |
| lisa | L.I.S.A copy (Locomotive) | 10-10 | quiet-ember-7314.lingering-wood-d572.workers.dev | password gated preview, identical copy, own inbox | [[design/reskin/lisa]] |
| clone | clone (piergorelli.com mirror, renamed) | 10-20 | none | local mirror of the Astro build renamed to clone, start.cmd on port 8790, nothing deployed | [[design/reskin/index]] |
| elysium | Elysium copy (thebenezer) | 10-10 | misty-fern-5183.lingering-wood-d572.workers.dev | password gated preview, identical copy | [[design/reskin/elysium]] |

Retros that exist: [[projects/pennypair/retro]], [[projects/ddosinu/retro]], [[projects/paidpair/retro]],
[[projects/paidblend/retro]], [[projects/stonkpad/history]]. New projects get a retro only when
something was learned that [[launch]] does not already say.

Dead-end domains, never a page, redirect or link: stonkpair.fun,
stonkblend.fun, stonkblends.fun, paidpairs.fun, sharesol.fun, pokestrategy.win.

## How a site gets made here

1. **Kit.** The owner says the idea; one message of questions with defaults
   ([[launch/kit]]). The domain is bought now ([[launch/domain]]).
2. **Spec, then plan.** Written into the repo under `docs/superpowers/`.
   45 to 60 minutes.
3. **The look is a copy.** Capture the reference site, serve its stylesheet
   verbatim, write markup with its class names ([[design/reskin/index]]). The
   engine is the last project's, cloned with a rename script.
4. **Build by helpers.** One task, one implementer, one reviewer. Tasks on
   separate folders run side by side. Review how the pieces fit halfway.
5. **Deploy gated the same hour** ([[launch/hosting]], [[launch/gate]]).
6. **Face in a second chat**: logo, banners, X content ([[launch/logo]],
   [[launch/banners]], [[launch/x-content]]).
7. **Checks, then the owner opens the gate and publishes the CA**
   ([[launch/checks]], [[launch/coin]]).
8. **Changes.** One owner message, one small change, one deploy. This is
   where the time goes.
9. **Shutdown** when it is done ([[launch/shutdown]]).

## Where the time went, across the first five

| cause | seen in | total cost |
| --- | --- | --- |
| numbers and words changed after launch, each a deploy | Share | ~4.5 h |
| design built in code before it was agreed as a picture | PennyPair hero, Share video | ~4 h |
| problems found only at the final review | PaidPair, PennyPair | ~2.5 h |
| guessing instead of asking (the animal, the target, the product) | DDOSINU, PennyPair | ~1 h |
| the permission checker refusing actions | all | ~45 min |
| name and domain changed after deploy | Share, StonkBlend, PokeRaffle | ~1 h, plus dead links |
| two chats on one checkout or one rule | PaidPair, Share, Callout, PokeRaffle | ~1 h |
| waiting on a key, a domain, a sign-in | PennyPair, DDOSINU | ~1.5 h |

## The rules

### Before the first line

1. One message of questions, each with a default. Then start on whatever
   does not depend on the answers.
2. The owner's kit, asked for up front ([[launch/kit]]).
3. Name and domain are settled before anything is launched.
4. Read the project's `OPS.md` before touching its design.

### In the spec

5. Write the money rule as one line that must always be true (earned = paid
   + pot, per coin page) and test it before any page is drawn.
6. Every number the owner may want to try is a desk setting, never a
   constant. Every sentence on the site reads the setting.
7. Prove the one risky unknown by hand first. Build around it after.

### In the build

8. Review how the pieces fit halfway, not only at the end.
9. One git worktree per chat. One chat owns the rules; others read.
10. The reviewer runs the tests the implementer named.
11. Every helper's brief carries the exact commit trailer and the
    frozen-look rule.
12. Read a memory note before writing it. Add, never replace.
13. Commit only your own files in a shared checkout; never `git add -A`.

### In design and motion

14. Pictures first. A still of the hero, a storyboard of the video, four
    previews a second. Code and full renders come after a yes.
15. Ask for a reference before the first frame, and copy its moves one by
    one. Identical means identical.
16. Art starts from the character picture as reference, with the "no" list
    in the prompt.
17. Repeat the target back in five words before changing it.

### At deploy

18. A deploy checklist in `OPS.md`, copied from the last project.
19. Commands for the owner are PowerShell, and say "make a new file" when
    the file is new.
20. Commit after every deploy. Deploy only from a clean worktree.
21. Read the live state of a gate, a wallet or a variable before speaking
    about it or flipping it.

## What already works, keep doing it

- Cloning the last site that worked, with a rename script.
- Site and art in two chats at once.
- Screenshots at desktop and phone width before the owner looks.
- Helpers side by side on separate folders: a whole product's code in 40
  minutes.
- Every ruling made for the owner written down with its cost if wrong.
- A backup before anything is cleared; a repro script before a bug is fixed.
- Money actions wait for the owner's word, every time.
