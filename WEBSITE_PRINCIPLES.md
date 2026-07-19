# Website architecture playbook

For whoever (human or AI) maintains this site next. It documents *why* things are built the way they are.

## Stack

- **Hugo (extended)**, no theme module. All templates are hand-written and live in `layouts/`.
  - This is a deliberate departure from the Hugo Blox reference stack. Blox's Tailwind pipeline needs a
    Node/PostCSS toolchain, and this site overrides the base shell, navbar, footer, search, and every page
    template anyway — so Blox would have been downloaded weight that never renders. A pure-Hugo build gives
    the same design with a simpler, more robust CI (Hugo + Pagefind only; no Go modules, no Tailwind build).
  - Styling is a single hand-written stylesheet, `assets/css/custom.css`, using semantic CSS custom
    properties — **not** utility classes. Change the palette in the `:root` block at the top.
- **Pagefind** for search (built in CI against `public/`). No search server.
- **GitHub Pages** via GitHub Actions (`.github/workflows/deploy.yml`).
- **User site** (`byolkim.github.io`): `baseURL` is the domain root, so there is no `/REPO/` subpath.
  Templates still use `relURL` without leading slashes, and content uses the `staticrel` shortcode, so the
  site would survive a move to a project subpath.

## Content model

- Every publication / talk / software item is a leaf bundle: `content/<section>/<slug>/index.md`.
  The **folder name is the slug and the URL** (`permalinks: :contentbasename`). Never rename a live folder.
- Shared front matter: `title`, `date`, `authors`, `publication_types`, `publication` (venue), `tags`,
  `links` (name+url). Talks add `venues` and `kind_note`; in-prep work adds `status: in_preparation`.
- `data/research_areas.json` maps areas → tags. `data/coauthors.json` holds resolved People links.
  `data/featured_publications.yaml` is a spotlight hook (currently the in-prep paper).
- `i18n/en.yaml` centralizes every UI label. Rename buttons there, not in templates.

## Key templates

- `layouts/landing/list.html` — homepage: hero (photo left / text right, stacks on mobile) + research-area
  accordions (native `<details>`, so they work without JS). The homepage intro is inline here and must stay
  distinct from `content/bio/index.md`.
- `layouts/publication/list.html` — the Writings page. Server-renders every item with `data-tab`,
  `data-year`, `data-areas`, `data-search`; a vanilla-JS IIFE does tab/area/year/text filtering, sorting,
  live count, hash sync (`#articles` …), `?area=` deep-links, per-item BibTeX copy, and BibTeX export of the
  visible set. All content is in the DOM at load, so it degrades to a full list if JS fails.
- `layouts/_partials/related_finder.html` — the automatic "See also". Explicit `related_*` front matter
  wins; the rest is scored by shared title tokens (+2, stop-words stripped), shared authors (+1), shared
  tags (+2); threshold 4, relaxed to 2 when there are <3 explicit picks; capped at 8; title-deduped. No
  script, no `related_map.json` to go stale.
- `layouts/authors/list.html` — People. Co-author set is discovered from content at build time; links come
  from `data/coauthors.json`. Names-only presentation (Byol is a solo researcher, not a lab). **Auto-linked
  and fallible — every link needs owner review** (see the maintainer note in the template and UPDATING.md).
- `layouts/_partials/item_single.html` — shared detail page for publication/talk/software: breadcrumbs,
  authors (co-authors linked out via coauthors.json), venue, talk venues, links, See also.

## Conventions worth keeping

- Forced light mode; the theme toggle is hidden in CSS.
- Type labels everywhere (`[Paper]`, `[Presentation]`, `[Software]`, `[Working paper]`).
- No process/sourcing commentary in rendered copy; flags for the owner go in `<!-- HTML comments -->`.
- Favicon is a generated "BK" monogram on the slate accent (`static/favicon*`), matching `--color-accent`.
- The mysite footer credit and discovery metadata are gated behind `params.mysite.credit` / `.discovery`.
