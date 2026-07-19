# byolkim.github.io

Personal academic website for **Byol Kim**, Assistant Professor of Statistics, Sookmyung Women's University.

Built with [Hugo](https://gohugo.io/) (extended) and [Pagefind](https://pagefind.app/), deployed to
GitHub Pages via GitHub Actions.

- **To add or edit content**, see [`UPDATING.md`](UPDATING.md) — no local tools needed; edit Markdown on
  GitHub and it redeploys itself.
- **Architecture notes** are in [`WEBSITE_PRINCIPLES.md`](WEBSITE_PRINCIPLES.md).

## Local preview (optional)

Requires Hugo extended (≥ 0.148.2):

```bash
hugo server        # then open http://localhost:1313
```

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which builds the site with Hugo, generates the
Pagefind search index, and publishes to GitHub Pages.

> **One-time repo setting:** in **Settings → Pages**, set **Source** to **GitHub Actions**.
