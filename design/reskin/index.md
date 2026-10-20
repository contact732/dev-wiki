---
updated: 2026-10-20
tags: [reskin]
---

# reskin

Sites we copied one to one, so a project can wear their look. A copy here
is the reference's own markup, words, images and stylesheet, frozen; a
project then swaps in its own name and data. Different from
[[design/references]]: a reference gives a direction, a reskin is the whole skin.
Map: [[home]].

## The rule

- **Identical means identical.** Their markup, words, images and compiled
  stylesheet, verbatim. Only links change. Their name stays until the owner
  says rebrand.
- **No questions.** When the owner says "identical", nothing gets asked:
  clone everything, front end first, frozen sample data, nothing deployed.
- **Markup comes from the captured DOM, never redrawn from a screenshot.**
  Screenshots are the check, not the source.

## How a site gets copied

The road [[design/reskin/sentients]] took, reusable for the next one:

1. **Capture.** A Playwright script walks every route at 1440 and 390 and
   saves the full-page screenshot, the hydrated DOM, the page text and every
   same-origin response (their CSS, fonts, images, JSON). Then the
   interactive states: wallet popup, phone menu, copied toasts, expanders.
2. **Snapshot the data.** If the site has a JSON API (look in their client
   chunks for `/api/`), pull every endpoint once, plus one call per entity
   (each profile, post, coin). Pages with no JSON get their server HTML
   saved; the props sit in the RSC payload (`self.__next_f.push`), which is
   cleaner than scraping the DOM.
3. **Serve their stylesheet verbatim.** A Tailwind site ships one compiled
   sheet; copy it and its fonts as is, then write JSX with their exact class
   names. A class they never used does not exist, which keeps the copy honest.
4. **Port, don't redraw.** Procedural art (generated avatars), number and
   time formats, client behaviour (tabs, countdowns, copy buttons) are ported
   from their minified chunks and checked string for string.
5. **Convert their server render.** `~/Documents/holdapi/tools/rsc2jsx.mjs` turns the RSC payload into JSX,
   class for class and word for word; diff page text with `tools/textdiff.cjs`.
6. **Freeze time.** Relative times ("59s ago") count from the snapshot time,
   so the copy reads as the site did that minute.
7. **Build by helpers.** One shared shell first (header, phone bar, footer,
   wallet popup, shared cards), then one helper per group of pages, each
   checking its pages side by side with the reference at both widths.

## The shelf

| reference | our copy | folder | how far | note |
| --- | --- | --- | --- | --- |
| [sentientsai.xyz](https://sentientsai.xyz/) | SENTIENTS, all theirs | `~/Documents/sentients` | every page, front end, frozen data | password gated on workers.dev, [[design/reskin/sentients]] |
| [agencypad.fun](https://www.agencypad.fun/) | AGENCY | `~/Documents/agencypad` | whole product cloned, CSS verbatim, not deployed | rebrand is one file plus find and replace |
| [stonkfun.xyz](https://www.stonkfun.xyz/) | StonkFuse | `~/Documents/stonkfuse` | UI identical, StonkSplit functionality | live at stonkfuse.fun |
| [vaultstrat.com](https://vaultstrat.com/) | PokeRaffle | `~/Documents/cardstrat` | real clone, real coins | on pokeraffle.win; a new skin since on branch `reskin` |
| [bags.fm](https://bags.fm/) | PokeChart | `~/Documents/pokechart` | pretend front end | behind a password on workers.dev |
| [bags.fm](https://bags.fm/) | Short | `~/Documents/short` | the look only | password gated |
| MoonPay enterprise | PennyPair `/design` | `~/Documents/pennypair` | one page copy | pennypair.fun/design |
| [uxento.io](https://uxento.io/) | PennyPair landing and app, Shared | `~/Documents/pennypair-uxento`, `~/Documents/routed` | the look over our layouts | live |
| [desk404.fun](https://www.desk404.fun/) | OTC.exe | `~/Documents/desk404` | UI copy with our own icons and long text, sample tokens | live on workers.dev since 2026-10-07; never published under the Desk 404 name |
| [holdapi.lol](https://holdapi.lol/) | $API, all theirs | `~/Documents/holdapi` | every page identical, back end built and audited, not deployed | [[design/reskin/holdapi]] |
| [lisa.locomotive.ca/en](https://lisa.locomotive.ca/en) | L.I.S.A, all theirs | `~/Documents/lisa` | their files served as is (page, bundle, 3D model, voice clips, screen videos), own inbox for the forms | password gated on workers.dev since 2026-10-10, [[design/reskin/lisa]] |
| [elysium.thebenezer.com](https://elysium.thebenezer.com/) | Elysium, all theirs | `~/Documents/elysium` | their Vite build served as is (page, bundles, models, sounds, LUTs), GA loader dropped | password gated on workers.dev since 2026-10-10, [[design/reskin/elysium]] |
| [piergorelli.com](https://piergorelli.com/) | clone (renamed: name, domain, mark) | `~/Documents/clone` | whole Astro build served as is (26 pages, chunks, fonts, images); contact form lands on their thanks page | local only (`start.cmd`, port 8790), not deployed, 2026-10-20 |
| [agentx.family](https://agentx.family/) | none | | looked at, dropped for Sentients the same day | |
