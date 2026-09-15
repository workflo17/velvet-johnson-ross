# Velvet A. Johnson-Ross — website

Static site. No build step, no dependencies, no framework. Eight HTML pages, one
stylesheet, four images.

```
index.html        Home — hero, at-a-glance, focus areas, affiliations
about.html        Background — the full narrative, with quotes
experience.html   CV — consulting, prior roles, teaching, education, publications
work.html         Record — key figures, HAVP funding chart, record ledger
initiatives.html  Projects — The Eviction Fund in full, plus HOME
herhousing.html   #HerHousing — the podcast, its research and its budget
testimony.html    Her Feb 9 2021 testimony, reproduced in full
press.html        Articles featuring her, org coverage, all sources
assets/
  styles.css                The entire visual system
  velvet-headshot.jpg       504×756  — her own headshot (home hero)
  herhousing-city.png       601×296  — #HerHousing brand illustration
  velvet-portrait.jpg       877×877  — David Prize portrait (third-party)
  velvet-portrait-wide.png  1024×452 — wide crop of the same
vercel.json       Cache headers for assets
robots.txt        Allows indexing
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

## ⚠️ Before this goes public: photo rights

| Image | Source | Status |
|---|---|---|
| `velvet-headshot.jpg` | Her own #HerHousing deck | hers - used as the home hero |
| `herhousing-city.png` | Her own #HerHousing deck | hers |
| `velvet-portrait.jpg` / `-wide.png` | Photo by **Janick Gilpin**, courtesy of **The David Prize** | third-party |

The David Prize portrait is credited wherever it appears, but credit is not a licence.
It now sits only on `about.html`, off the home page. **Confirm permission from the
photographer and/or The David Prize before the site is publicly linked**, or remove it -
the site works without it, since the hero uses her own headshot.

---

## Deploying to Vercel

From this `site/` directory:

```bash
npx vercel deploy --prod
```

Or connect the folder to a Git repo and import it at vercel.com — framework preset
**Other**, build command **none**, output directory **.**

### Clean URLs (optional)

To serve `/about` instead of `/about.html`, add `"cleanUrls": true` to
`vercel.json`. Vercel will then 308-redirect the `.html` links. Left off by default
so the links behave identically locally and in production.

### Before launch

- [ ] Resolve photo rights (above)
- [ ] Replace the relative `og:image` paths with absolute URLs once the domain is
      known — e.g. `https://yourdomain.com/assets/velvet-portrait.jpg`. Relative
      OG paths are not reliably resolved by social/link-preview crawlers.
- [ ] Have Velvet read every page and confirm the framing is how she wants it

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
  artwork shot on white (the David Prize portrait, the #HerHousing illustration) dissolves
  into the page instead of showing as a white rectangle.
- `.portrait.mono` applies greyscale, for imagery that is monochrome by intent.

Her colour headshot uses neither, so it stays warm and full-colour.

### Chart colors

The two series hues on `work.html` are **validated**, not chosen by eye:

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
| An initiative | `work.html` | `<div class="init">` |
| A focus area | `index.html` | a `<div>` inside `.focus` |
| A role on the CV | `experience.html` | `<div class="role-block">` |
| A publication | `experience.html` | a `<div>` inside `.pubs` |

Record entries carry a typed chip. Available kinds:
`testimony` · `press` · `recognition` · `role` · `civic`

The nav, footer and `<head>` are duplicated across the eight pages — if you change
one, change all eight. (That duplication is the cost of having no build step; it is
deliberate.)

---

## Accuracy rules this site follows

- She was a **2022 David Prize finalist, not a winner**. No source shows a win —
  the Brooklyn winners that year were Geneva White and Mark Winston Griffith.
- **She publishes under two names** — her own and **Fannie Lou Diane**. Search both.
- The **Unlock NYC press list is coverage of the organization**, kept in its own
  labelled section. She does appear in several of those pieces herself - usually under
  **Fannie Lou Diane** - and the confirmed ones are promoted into "Featuring her".
- **HAVP launching is not claimed as her personal win.** She testified for the bill
  in 2021; the page states the outcome and lets the reader connect it.
- Her **age and home address are omitted** deliberately. Her cancer history appears
  only inside her own quoted testimony, in her framing.

Full research trail, including the open questions still worth asking her, is in
`../RESEARCH-NOTES.md`.
