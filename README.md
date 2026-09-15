# Velvet A. Johnson-Ross — website

Live at **https://velvetjohnsonross.com**.

Static site. No build step, no dependencies, no framework. Eight HTML pages, one
stylesheet, eight images.

```
index.html        Home — hero, at a glance, her own words, focus areas,
                  affiliations, and the index of every other page
about.html        Background — the narrative, the two names, Brooklyn, teaching
experience.html   CV — practice, prior roles, teaching, education, publications,
                  memberships, recognition, contact
work.html         Record — key figures, HAVP funding chart, 9-entry ledger, projects
initiatives.html  Projects — The Eviction Fund in full with budget, plus HOME
herhousing.html   #HerHousing — the podcast, its research and its budget
testimony.html    Her Feb 9 2021 testimony, reproduced in full
press.html        Articles featuring her, org coverage, all sources
assets/
  styles.css                 The entire visual system
  velvet-editorial.jpg       832×614 — home hero
  velvet-portrait-close.jpg  620×781 — Background lead
  sistersong-clipping.jpg    1200×639 — SisterSong membership clipping, Background
  hamer-quote.jpg            640×839 — Fannie Lou Hamer poster
  movement-collage.jpg       750×750 — protest collage
  raised-fists.jpg           562×562 — #HerHousing artwork
  justice-verb.jpg           777×482 — "Justice needs to be a verb"
  herhousing-city.jpg        560×276 — #HerHousing brand illustration
vercel.json       Cache headers
robots.txt        Allows indexing
sitemap.xml       All eight pages
```

## Theme

The palette and typography come from her own **#HerHousing pitch deck**. The deck's slide
master is a 45° linear gradient through the rose family — that gradient is the page ground
here, with content on a sheet above it so long-form reading holds.

| | Light | Dark |
|---|---|---|
| Ground | rose gradient `#AF4D79 → #A44670 → #D4A1B8` | deep plum gradient |
| Sheet | `#FFFDFC` | `#1B141A` |
| Accent | `#A4406E` | `#D98BAC` |

**Type:** Bodoni Moda (display), Pinyon Script (her name — standing in for the deck's
Zapfino), Gentium Book Plus (body, standing in for Plantagenet Cherokee), IBM Plex Sans
(UI), IBM Plex Mono (dates, bill numbers, figures).

**Theme switching:** a toggle sits at the right of the nav. It writes `data-theme` on
`<html>` and remembers the choice in `localStorage`. `?theme=light` and `?theme=dark`
force a mode by URL — useful for sharing a specific look.

---

## Navigation

Three devices, and they have to stay in step when a page is added or reordered.

**1. The top nav** is the same seven links on all eight pages. `#HerHousing` is
deliberately not in it — it is a sub-page of Projects, reached from the home-page
index, the Projects page, the Record page and the footer.

**2. The pager** at the foot of every page runs *both* directions and walks the pages
in this order, returning to the start:

```
index → about → experience → work → initiatives → herhousing → testimony → press → index
```

If you add a page, insert it in that chain — otherwise it is reachable only from the
top nav, which is the bug this replaced. (The old chain skipped Experience and Projects
entirely.)

**3. The on-page index** (`nav.onpage`) sits under the page header on every page with
more than two sections. Each entry points at a `<section id>`; the ids are slugs of the
`h2` text. If you rename an `h2`, update its `id` and the matching link, or use a
hand-written `id` as `#eviction-fund` and `#record` do.

The home page also carries a `nav.routes` index of all seven other pages — it is the
main way a visitor who scrolls rather than reads the nav gets anywhere.

---

## Design system

Everything lives in CSS custom properties in `assets/styles.css`. See **Theme** above
for the palette; the rest of the tokens follow the same pattern.

Three theme states are handled: bare `:root` (light), `prefers-color-scheme: dark`
guarded with `:not([data-theme="light"])`, and an explicit `[data-theme="dark"]` so the
toggle wins in both directions. Every token is declared on a bare `:root` first.

### Image treatment

Images opt in to two effects, so nothing is applied where it should not be:

- `.portrait.cut` uses `mix-blend-mode: multiply` against a fixed light photo field, so
  artwork shot on white dissolves into the page instead of showing as a white rectangle.
- `.portrait.mono` applies greyscale, for imagery that is monochrome by intent.

`.plate` sizes pictures by their own shape rather than the prose measure — a square
image constrained to `--measure` (66ch) rendered 572px tall and swallowed the viewport.

### Chart colors

The two series hues on `work.html` and `herhousing.html` are **validated**, not chosen
by eye:

| | Light | Dark |
|---|---|---|
| Series A (requested / Black households) | `#A4406E` | `#C96A96` |
| Series B (appropriated / white households) | `#C2891A` | `#C2891A` |

Rose against the deck's blue failed colorblind separation badly (ΔE 3.8 under protanopia —
magenta and blue collapse together). Rose against teal only reached 7.6. **Rose against
amber reaches ΔE 20.4** and passes all five checks in both themes: lightness band, chroma
floor, CVD separation, normal-vision floor, contrast against the surface.
**If you change these, re-validate rather than picking by eye.**

---

## Adding content

All repeated blocks are copy-paste patterns with comment markers in the HTML.

| To add | Page | Copy this block |
|---|---|---|
| A record entry | `work.html` | `<div class="entry">` — keep reverse-chronological, update the count in `.s-head` |
| An article featuring her | `press.html` | `<div class="press-row">` |
| An Unlock NYC article | `press.html` | an `<a>` inside `.press-compact` |
| An affiliation | `index.html` | `<div class="org-item">` |
| A project | `work.html` | `<a class="init">` — these link out to the Projects page |
| A focus area | `index.html` | a `<div>` inside `.focus` |
| A role on the CV | `experience.html` | `<div class="role-block">` |
| A publication | `experience.html` | a `<div>` inside `.pubs` |

Record entries carry a typed chip. Available kinds:
`testimony` · `press` · `recognition` · `role` · `civic`

The nav, footer, pager and `<head>` are duplicated across the eight pages — if you
change one, change all eight. (That duplication is the cost of having no build step; it
is deliberate.) A new **section** also needs an `id` and a matching entry in that page's
`nav.onpage`.

---

## Writing

House style, enforced across the site:

- **American spelling** — program, color, center, organizer, analyzed. (A mixed
  British/American text shipped once; it reads as unedited.)
- **Her words stay hers.** The testimony page is verbatim from the state record; resume
  bullets are her phrasing. Don't smooth either.
- **No editorial flourish in her voice.** Summarising closers ("the three threads are
  the same thread"), antithesis ("not X but Y") and metaphor stand-ins for facts were
  removed once already. Say what happened, with a date and a source.
- **Em dashes are for appositives**, not for stapling two thoughts together. If a
  sentence has two, it needs to be two sentences.
- **Captions describe the picture**, they do not editorialise about it.

---

## Accuracy rules this site follows

- She was a **2022 David Prize finalist, not a winner** — no source shows a win; the
  Brooklyn winners that year were Geneva White and Mark Winston Griffith.
- **She publishes under two names** — her own and **Fannie Lou Diane**. Search both.
- The **Unlock NYC press list is coverage of the organization**, kept in its own
  labelled section. She does appear in several of those pieces herself — usually under
  **Fannie Lou Diane** — and the confirmed ones are promoted into "Featuring her".
- **HAVP launching is not claimed as her personal win.** She testified for the bill
  in 2021; the page states the outcome and lets the reader connect it.
- Her **age and home address are omitted** deliberately. Her cancer history appears
  only inside her own quoted testimony, in her framing.

Full research trail, including the open questions still worth asking her, is in
`../RESEARCH-NOTES.md`.

---

## Deploying

The repo is Git-linked to the Vercel project `velvet-johnson-ross`; pushing to `main`
deploys. `velvetjohnsonross.com` is the canonical host and `www` redirects to it.

`vercel.json` caches `/assets/*.{jpg,png,webp,avif,woff2}` for a year and immutable, and
HTML and CSS `max-age=0, must-revalidate`. **Keep CSS out of the immutable bucket** —
`styles.css` has no content hash, so a long cache freezes returning visitors on old CSS.
