# design

How UI/UX gets made for anything that reaches **building** in [[ideas]]. This is
the rulebook; the per-project shape is [[templates/design-brief]], copied into
the project's repo as `DESIGN-BRIEF.md` before the first component. Map: [[home]].

## The stack — five skills, in this order

Distilled 2026-09-22. The order is precedence: when two layers disagree, the
higher one wins. Each layer has one job.

| # | skill | job |
| --- | --- | --- |
| 1 | [Impeccable](https://github.com/pbakaus/impeccable) | the process: mode, direction contract, craft floor, bounded review |
| 2 | [Frontend design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) (Anthropic) | the taste method: ground in subject, two-pass plan→critique, AI-tell calibration, copy as design |
| 3 | [Taste](https://github.com/Leonxlnx/taste-skill) | the anti-slop hard rules: design read, dials, real design systems, layout discipline |
| 4 | [UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | the data: searchable styles, palettes, font pairs, 119 UX rules, token generation |
| 5 | [Emil Kowalski](https://github.com/emilkowalski/skills) | the feel: motion decision framework, component micro-craft |

Layer 3 scopes itself to Persuade surfaces (landings, portfolios, redesigns —
not dashboards); layers 1, 2, 4, 5 apply everywhere.

## The one law

**Direction before code.** A design that exists only as a mood is not decided.

## The process

### 1 · Mode (Impeccable)

Picked per *surface*, not per product. **Operate** — visitor completes a task
(the screener, the dashboard: almost everything this vault births; scanability
and native expectations outrank expression, the tool disappears into the
task). **Persuade** — visitor decides and acts (the landing page). **Read** —
docs. **Experience** — rare here.

### 2 · Brief (Impeccable + Frontend design)

Fill [[templates/design-brief]]:

- **Ground in the subject.** Name the concrete subject, audience, and the
  design's one job. The subject's world is where distinctive choices come
  from — a degen momentum tool and a compliance dashboard share nothing.
- **Scene picks the theme.** One sentence — who, where, under what light —
  forces light/dark. Never category habit.
- **Design read, one line** (Taste): "Reading this as: <surface> for
  <audience>, with a <vibe> language, leaning toward <system/aesthetic>."
  Then set the three dials: VARIANCE / MOTION / DENSITY, 1–10 (dashboards run
  low variance, low motion, high density).
- **Direction contract** (Impeccable): THESIS (the idea this surface owns and
  the category default it refuses) · OWN-WORLD (recognizable with content
  removed) · STORY · FIRST VIEWPORT.

### 3 · Plan, then check it for genericness (Frontend design)

Two passes before code. First a compact token plan: palette as 4–6 named
values, faces and roles, layout as ASCII wireframe, principles. Then the
calibration pass: if any part is what you'd produce for *any* similar brief —
cream+serif+terracotta, near-black+one neon accent, broadsheet hairlines, the
SaaS card kit, eyebrow-above-every-heading chrome — revise it and say why.
Someone should not be able to guess the aesthetic from the category alone.

### 4 · System (Taste + UI UX Pro Max)

- **Real system when the brief names one** (Taste): enterprise → Fluent/
  Carbon, Shopify → Polaris, gov → USWDS, modern SaaS you own → shadcn/ui,
  indie → Tailwind v4. Use the official package; never recreate it by hand.
  One system per project.
- Otherwise generate and persist tokens:

      python3 ~/.claude/skills/ui-ux-pro-max/scripts/search.py \
        "<product> <keywords>" --design-system --persist -p "<Project>" \
        --density 8 --output-dir <repo-root>

  `design-system/<slug>/MASTER.md` becomes the repo's source of truth. Fix
  priority when reviewing: accessibility → touch → performance → style →
  layout → type/color → animation → forms → nav → charts. Raw hex in a
  component is a bug.

### 5 · Build — the merged craft floor

**Typography**
- One well-tuned family usually carries a product UI; two max, clearly
  distinct. Fixed rem scale, ratio 1.125–1.2. Body 16px, lh ≥1.5, measure
  <80ch (65–75 for prose); tables run dense.
- Serif is not the default for "creative/premium" — sans display is the
  default for the same reason black is in fashion. Fraunces and Instrument
  Serif are banned as reflexes. Emphasis inside a headline = italic/bold of
  the *same* family, never a swapped-in serif word, never one recolored word.
- No all-caps tracked labels by default; no label above content that the
  content doesn't need. `tabular-nums` on every number column.

**Color**
- Restrained floor for Operate: neutrals + ONE accent, locked page-wide
  (color consistency lock — no blue CTA appearing in section 7 of a
  warm-grey site). Accent marks actions, selection, state — never decoration.
- **Red and green are spent** in market products — price direction only; the
  accent is neither. Saturation <80%; no AI-purple glow; one gray family.
- Full state vocabulary standardized once; second neutral for panels;
  contrast 4.5:1 / 3:1 verified on every button and form; OKLCH tokens.
- One theme per page, locked. Sections never invert mid-scroll.

**Layout**
- Structural responsiveness (collapse the sidebar), mobile-first, no
  horizontal scroll, `min-h-[100dvh]` never `h-screen`, Grid over flex math.
- Spacing rhythm: tight in groups, generous between, more above a heading
  than below. Shape lock: one corner-radius scale for the page, or a written
  rule ("buttons pill, cards 16, inputs 8") followed everywhere.
- A layout family appears at most once per page; max 2 consecutive
  image/text zigzags; hero fits the first viewport with ≤4 text elements;
  nav one line, ≤80px.
- Cards only when elevation means hierarchy — otherwise borders, dividers,
  whitespace. Long lists get a different component (group, tabs, cards,
  marquee — once), not a longer list.

**Components**
- Every interactive element ships all states: default, hover, focus, active,
  disabled, loading (skeletons matching layout, not spinners), error, empty
  (teaches the interface). Touch targets ≥44px.
- Icons from one real library (Phosphor, Radix, Tabler...), one stroke
  weight, never hand-rolled SVG paths, never emoji-as-icon.
- Standard affordances in Operate mode; overlays escape clipping ancestors;
  modal is the last resort.
- Real images, not div-based fake screenshots or gradient-blob heroes; a
  text-only page is incomplete work, not minimalism.

**Motion (Emil — the decision framework, in order)**
1. *Frequency:* seen 100+ times/day (keyboard actions, **streaming ticks**) —
   never animate; a live cell gets at most a ~100ms flash. Occasional
   (modals, toasts) — standard. First-run — delight allowed.
2. *Purpose:* feedback, state, spatial continuity, or preventing a jarring
   change — one sentence, or drop it (motion must be motivated).
3. *Easing:* enter/exit → ease-out (`cubic-bezier(0.23,1,0.32,1)`); on-screen
   movement → ease-in-out (`cubic-bezier(0.77,0,0.175,1)`); **ease-in banned**.
4. *Duration:* buttons 100–160ms, dropdowns 150–250ms, modals ≤300ms; exit
   faster than enter.
- `scale(0.97)` on `:active`; nothing enters from `scale(0)`; popovers scale
  from their trigger; transitions over keyframes for rapid-fire UI; stagger
  30–80ms; only `transform`/`opacity`; continuous values via motion values,
  never `useState`; respect `prefers-reduced-motion`; gate hover behind
  `@media (hover: hover)`.
- One authored moment per surface. No per-section fade-ups, no page-load
  choreography in a tool, max one marquee per page.

**Copy is design material (Frontend design)**
- User's language, not the system's; active voice; a CTA names exactly what
  happens ("Save changes", not "Submit") and keeps its name through the flow.
- One label per intent per page (no "Get in touch" + "Contact us" + "Let's
  talk"). Errors name the problem and the recovery, never apologize. Empty
  states invite action.
- Self-audit every visible string before ship; fake-precise numbers are
  real, labeled mock, or deleted.

**Browser surfaces** — theme selection color, caret, focus rings, scrollbars,
underline offset, tabular numerals. Defaults belong to no design system.

### 6 · Review — bounded, then stop

Build fully → one batched round (desktop 1440 + mobile 390, real content, all
states) → fix in one batch → one confirm round → **stop**. The artifact is
Emil's | Before | After | Why | table. Sweep: contrast both themes, keyboard
path, empty/error/loading real, no CLS on data arrival, eyebrow count ≤
sections/3, CTA never wraps, copy audit. Chanel rule: look in the mirror,
remove one accessory. Watch motion the next day, slow, once.

## When to reach for which layer

| situation | layer |
| --- | --- |
| new surface, no direction | 1 · `/impeccable` (shape → direction contract) |
| plan feels generic / AI-default | 2 · frontend-design calibration pass |
| landing page / portfolio hard rules | 3 · taste (dials, layout discipline, pre-flight) |
| need palettes, fonts, UX rules, tokens | 4 · ui-ux-pro-max search |
| component feels dead; building motion | 5 · emil (review table, animate) |

## The install

All five live in `~/.claude/skills` (impeccable, frontend-design,
taste-skill, ui-ux-pro-max, and Emil's set), Impeccable's four agents in
`~/.claude/agents`. Installed 2026-09-22 by git clone / release zip — this
machine has no Node, so `npx` installers and `/plugin` don't apply. To
update: re-clone the repo (Impeccable: latest `universal.zip` from its GitHub
releases) and copy over the same folders. In a new project, start with
`/impeccable init`.

The repos are the source of truth and move fast — refresh this distillate
when a re-read shows the law changed.
