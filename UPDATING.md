# Updating your website

This is your site's owner's guide. You edit plain-text Markdown files on GitHub, commit, and the
site rebuilds and redeploys itself in a few minutes. You never have to run anything locally.

Every item — a paper, a talk, a piece of software — is a folder under `content/` containing a file
named `index.md`. The **folder name is the permanent web address** for that item, so once the site is
live, don't rename a folder (change the `title:` inside instead).

---

## Add a paper

1. In `content/publication/`, make a new folder with a short, hyphenated name, e.g.
   `content/publication/conformal-prediction-under-shift/`.
2. Inside it create `index.md`:
   ```yaml
   ---
   title: "Your paper title"
   date: 2026-09-01
   authors: ["Byol Kim", "Co Author"]
   publication_types: ["journal_article"]   # or ["conference_paper"], ["report"]
   publication: "Journal Name, Vol(Issue): pages"
   tags: ["conformal prediction", "high-dimensional data"]
   links:
     - name: "Publisher's Version"
       url: "https://doi.org/..."
   ---
   ```
3. To attach a PDF, drop it in `static/files/` and add a link with `url: "files/your.pdf"`
   (no leading slash).
4. Commit. The "See also" links, the Writings filters, and the Research-page area lists update
   themselves — you don't maintain them.

**Work in preparation:** set `publication_types: ["report"]` and add `status: "in_preparation"`. It then
appears under the *In Preparation* tab on the Writings page.

## Add a talk

Same idea, in `content/talk/`, with `publication_types: ["presentation"]`. If you gave the same talk at
several venues, list them:
```yaml
kind_note: "Invited seminar"
venues:
  - event: "Some Seminar"
    location: "Seoul"
    date: "May 2027"
```

## Add software

A folder in `content/software/` with `publication_types: ["software"]`, a one-line `summary:`, and a
`links:` entry pointing at the repository.

## Co-author links

Co-author names on publication / talk / software pages link out to their homepages when a URL is known.
Those URLs live in `data/coauthors.json`, keyed by the exact author name:
```json
"Song Liu": { "url": "https://example.github.io/", "type": "website" }
```
Add or edit a line to add or fix a link; delete a line to leave a name unlinked (the name still shows).

## Homepage introduction (three languages)

The homepage intro is **not** in `content/` — it lives in `layouts/landing/list.html`. It has three
language layers (English, 한국어, 日本語) that the visitor switches with the **EN / 한국어 / 日本語** buttons
under your photo; English is the default. To edit the text, find the `<div class="lang lang-en">`,
`lang-ko`, and `lang-ja` blocks and edit each language. The parenthetical citations link to your
publication pages. Keep this intro distinct from `content/bio/index.md` (the Bio page).

## Update your bio

Edit `content/bio/index.md`. The **Download C.V. (PDF)** button at the bottom of the page points at the
file named in `params.cv` in `hugo.yaml` (currently `files/byol-kim-cv.pdf`, i.e.
`static/files/byol-kim-cv.pdf`) — replace that PDF to update your C.V.

## Research areas (Research page + Writings filter)

Edit `data/research_areas.json`. Each area has a `name`, a `description`, and a list of `tags`. On the
**Research** page each area is an accordion listing the items whose `tags` match; the same tags drive the
area filter on the **Publications & Talks** (Writings) page. Tag your content consistently and it sorts
itself.

## Teaching

- **Galleries** (Design & Analysis of Experiments, Conformal Prediction, Conformal Prediction by Examples)
  are driven by `data/teaching.yaml`. Copy an existing block to add a project; put PDFs in
  `static/files/teaching/…` and thumbnails in `static/images/teaching/`. (Student evaluations are
  deliberately not published.)
- **"Notes on the pedagogical design"** is a prose page: `content/teaching-notes/index.md`.
- **"Teaching Mathematics in the Age of AI"** is a curated reading list driven by `data/reading_ai.yaml`
  (sections → entries with `title`, `author`, `date`, `url`; keep each section newest-first, and titles in
  sentence case).

## Contact page and its subpages

`content/contact/index.md` holds the Email / LinkedIn / Office cards and links to two subpages:
- **"Care to talk?"** — `content/care-to-talk/index.md`, an English/Korean page (visitor toggles with the
  한국어 / English button; English is the default). The "Fill out the form" button opens a Google Form set
  by `form_url:` in the page's front matter.
- **"Interested in having me as your thesis advisor?"** — `content/advising/index.md`, with its own Google
  Form (`form_url:`).

To change a form, replace the `form_url:` with your new Google Form share link.

## Colors and the dark theme

The site is **dark only** — a deep-navy background with a drifting star canvas, monospace type, and a blue
accent (`<html data-theme="dark">` is fixed in `layouts/_default/baseof.html`). To change colors, edit the
`:root[data-theme="dark"]` block at the top of `assets/css/custom.css` — e.g. `--color-primary` is the blue
accent.

## The two GaryKing.org/mysite options

Both are currently **on**. To change them, edit `hugo.yaml` under `params.mysite:`
- `credit: false` removes the small "Created using GaryKing.org/mysite" line in the footer.
- `discovery: false` removes the invisible discovery metadata in the page source.

## Downloading citations

The Writings page has a **Download BibTeX** button (exports whatever is currently filtered) and a **Cite**
button on each row (copies that one entry). These are generated from your front matter — nothing to keep
in sync.

## Local preview (optional)

You don't need this to publish. To preview locally you need **Hugo extended ≥ 0.150** (CI pins `0.150.0`):
```bash
hugo server        # then open http://localhost:1313
```
