# Website architecture playbook

For whoever (human or AI) maintains this site next. It documents *why* things are built the way they are.

## Stack

- **Hugo (extended)**, no theme module. All templates are hand-written and live in `layouts/`.
  - This is a deliberate departure from the Hugo Blox reference stack. Blox's Tailwind pipeline needs a
    Node/PostCSS toolchain, and this site overrides the base shell, navbar, footer, search, and every page
    template anyway — so Blox would have been downloaded weight that never renders. A pure-Hugo build gives
    the same design with a simpler, more robust CI (Hugo + Pagefind only; no Go modules, no Tailwind build).
  - Styling is a single hand-written stylesheet, `assets/css/custom.css`, using semantic CSS custom
    properties — **not** utility classes.
  - **Visual language is modeled on meethigher's "Starry" Hugo theme** (GPL-3.0): monospace type,
    `#2563eb` blue accent, cards with soft shadow + hover-lift, a deep-navy background, and a drifting
    star-particle canvas. We did not install the theme; the look is reproduced in our own CSS/JS so all
    academic features are preserved.
  - **Dark only.** `<html data-theme="dark">` is fixed in `layouts/_default/baseof.html`, and the star
    canvas (`#starry-canvas`, animated in `baseof.html`) is always on. There is **no** light/dark toggle.
    The CSS still defines a light palette under `:root` for completeness, but nothing exposes it; edit the
    `:root[data-theme="dark"]` block to change colors. (The canvas is disabled under
    `prefers-reduced-motion`.)
- **Pagefind** for search (built in CI against `public/`). No search server.
- **GitHub Pages** via GitHub Actions (`.github/workflows/deploy.yml`, Hugo pinned to `0.150.0`).
- **User site** (`byolkim.github.io`): `baseURL` is the domain root, so there is no `/REPO/` subpath.
  Templates use `relURL` without leading slashes and content uses the `staticrel` shortcode, so the site
  would survive a move to a project subpath.

## Content model

- Every publication / talk / software item is a leaf bundle: `content/<section>/<slug>/index.md`.
  The **folder name is the slug and the URL** (`permalinks: :contentbasename`). Never rename a live folder.
- Shared front matter: `title`, `date`, `authors`, `publication_types`, `publication` (venue), `tags`,
  `links` (name+url). Talks add `venues` and `kind_note`; in-prep work adds `status: in_preparation`.
- Data files:
  - `data/research_areas.json` maps areas → tags (drives the Research-page accordions and the Writings
    area filter).
  - `data/coauthors.json` holds resolved co-author homepage links (used on the publication/talk/software
    detail pages).
  - `data/teaching.yaml` drives the Teaching galleries.
  - `data/reading_ai.yaml` drives the "Teaching Mathematics in the Age of AI" reading list.
- `i18n/en.yaml` centralizes UI labels. Rename buttons there, not in templates.

## Key templates

- `layouts/landing/list.html` — **homepage**. A profile sidebar (photo, then name / title / institution,
  then an **EN / 한국어 / 日本語** language switch) beside a justified introduction. The intro has three
  inline language layers (`.lang.lang-en` / `-ko` / `-ja`); English is the default. The homepage intro is
  inline here and must stay distinct from `content/bio/index.md`.
- `layouts/research/list.html` — the **Research** page: one native `<details>` accordion per research area,
  each listing the items whose tags match (top 5, newest first) with a "View all in …" link to the filtered
  Writings page. Data comes from `data/research_areas.json`; no JS needed.
- `layouts/publication/list.html` — the **Writings** page. Server-renders every item with `data-tab`,
  `data-year`, `data-areas`, `data-search`; a vanilla-JS IIFE does tab/area/year/text filtering, sorting,
  live count, hash sync, `?area=` deep-links, per-item BibTeX copy, and BibTeX export of the visible set.
  All content is in the DOM at load, so it degrades to a full list if JS fails.
- `layouts/_partials/related_finder.html` — the automatic "See also". Explicit `related_*` front matter
  wins; the rest is scored by shared title tokens (+2, stop-words stripped), shared authors (+1), shared
  tags (+2); threshold 4, relaxed to 2 when there are <3 explicit picks; capped at 8; title-deduped.
- `layouts/_partials/item_single.html` — shared detail page for publication/talk/software: breadcrumbs,
  authors (co-authors linked out via `coauthors.json`), venue, talk venues, links, See also.
- `layouts/teaching/single.html` — the Teaching galleries from `data/teaching.yaml`, plus section-title
  links to the two standalone teaching pages: `content/teaching-notes/` (pedagogy essay) and
  `layouts/teaching-ai/single.html` (the AI reading list from `data/reading_ai.yaml`).
- `layouts/care-to-talk/single.html` and `layouts/advising/single.html` — the two Contact subpages, each
  with a Google-Form button whose URL is the page's `form_url:` front-matter field. "Care to talk?" also
  carries the two-language toggle (see below).
- `layouts/_default/single.html` — generic prose page (Bio, Contact). The Bio page appends the
  "Download C.V. (PDF)" button (from `params.cv`) after the content.

## Multilingual toggle

One CSS-driven mechanism serves both the homepage (three languages) and "Care to talk?" (two):

- A wrapper carries `data-lang` (default `"en"`); each language block has `class="lang lang-<code>"`.
  CSS hides the non-active layers: `[data-lang="en"] .lang-ko, [data-lang="en"] .lang-ja { display:none }`
  (and the symmetric rules). Switching languages is just flipping the wrapper's `data-lang`.
- The homepage uses a three-button switch (`.lang-btn[data-set-lang]`); "Care to talk?" uses a single
  cycling `.lang-toggle`. Both are ~5 lines of inline JS in their layouts.
- In Markdown content, wrap a language block with the `{{</* lang en */>}}…{{</* /lang */>}}` shortcode
  (`layouts/shortcodes/lang.html`), which emits the matching `.lang` layer.

## Conventions worth keeping

- Dark-only Starry aesthetic (deep navy, star canvas, monospace, blue accent) — no forced light mode, no
  toggle.
- Large text blocks are justified (`.prose`, `.hero__intro`); lists stay left-aligned.
- Type labels everywhere (`[Paper]`, `[Presentation]`, `[Software]`, `[Working paper]`).
- No process/sourcing commentary in rendered copy; flags for the owner go in `<!-- HTML comments -->`.
- Browser icons are in `static/` (`favicon.ico`, `favicon-32x32.png`, `apple-touch-icon.png`).
- The mysite footer credit and discovery metadata are gated behind `params.mysite.credit` / `.discovery`.
