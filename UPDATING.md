# Updating your website

This is your site's owner's guide. You edit plain-text Markdown files on GitHub, commit, and the
site rebuilds and redeploys itself in a few minutes. You never have to run anything locally.

Every item — a paper, a talk, a piece of software — is a folder under `content/` containing a file
named `index.md`. The **folder name is the permanent web address** for that item, so once the site is
live, don't rename a folder (change the `title:` inside instead).

---

## Please review the People page first

The **People** page (`content/authors/`) is generated automatically from the co-authors listed on your
papers, talks, and software. The external links next to each name were found by an automated web search
and **may point at the wrong person or a stale page.** Please read the page and fix anything wrong.

- Links live in `data/coauthors.json`, keyed by the exact author name. Example:
  ```json
  "Song Liu": { "url": "https://anewgithubname.github.io/", "type": "website" }
  ```
- These names are currently **unlinked** because no confident match was found — add a line for anyone you
  can place: **Chen Xu, Ty O Easley, Zhen Ren, Federico Pineda**.
- To remove a wrong link, delete that name's line; the name still appears, just without a link.

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
   publication_types: ["journal_article"]   # or ["conference_paper"], ["report"], ["software"]
   publication: "Journal Name, Vol(Issue): pages"
   tags: ["conformal prediction", "high-dimensional data"]
   links:
     - name: "Publisher's Version"
       url: "https://doi.org/..."
   ---
   ```
3. To attach a PDF, drop it in `static/files/` and add a link with `url: "files/your.pdf"`
   (no leading slash).
4. Commit. The "See also" links and the Writings filters update themselves — you don't maintain them.

**Work in preparation:** set `publication_types: ["report"]` and add `status: "in_preparation"`. It then
appears under the *In Preparation* tab.

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

## Update your bio

Edit `content/bio/index.md`. The short homepage introduction is separate — it lives in
`layouts/landing/list.html` (search for "hero__intro"). Keep the two different.

## Research areas (homepage accordions + Writings filter)

Edit `data/research_areas.json`. Each area has a `name`, a `description`, and a list of `tags`. An item
shows up under an area when any of its `tags` matches. Tag your content consistently and it sorts itself.

## Teaching / student project galleries

Edit `content/teaching/index.md`. The two "gallery coming soon" boxes are placeholders — replace them
with your students' project write-ups or links when they're ready.

## Add a featured image to a paper

Drop a file named `featured.jpg` (or `.png`) inside that paper's folder, next to `index.md`. It is picked
up automatically.

## The two GaryKing.org/mysite options

Both are currently **on**. To change them, edit `hugo.yaml` under `params.mysite:`
- `credit: false` removes the small "Created using GaryKing.org/mysite" line in the footer.
- `discovery: false` removes the invisible discovery metadata in the page source.

## Downloading citations

The Writings page has a **Download BibTeX** button (exports whatever is currently filtered) and a **Cite**
button on each row (copies that one entry). These are generated from your front matter — nothing to keep
in sync.
