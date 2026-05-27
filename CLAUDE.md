# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website for Ryan Fraser (PhD student, Economics & Business, University of Michigan), built on **AcademicPages** — a Jekyll theme that is itself a fork of Minimal Mistakes. Most of `_layouts/`, `_includes/`, `_sass/`, and `assets/js/` is upstream theme code; the site-specific content and configuration is a small subset (see "Where to edit").

The site owner is non-technical (no web/dev background). Prefer low-maintenance solutions and explain changes in plain terms.

## Build & deploy

- **Hosting:** GitHub Pages, classic auto-build (there is no Actions workflow). The git remote is `RCFraser/rcfraser.github.io`; pushing to the published branch (currently `main`) triggers GitHub's built-in Jekyll build and serves at **https://rcfraser.github.io**. `baseurl` is `""` (root of the domain).
- **There is no separate build step, no CI, and no staging.** The loop is: edit → commit → push → GitHub rebuilds the live site.
- **Local preview is not currently set up** — Ruby/Bundler is not installed on this machine (`bundle` is absent). The `Gemfile` is ready for it, so if local preview is wanted: `bundle install`, then `bundle exec jekyll serve` (http://localhost:4000). Jekyll does **not** reload `_config.yml` changes without restarting the server.
- **Plugins run in GitHub Pages "safe mode":** only the whitelisted plugins in `_config.yml` (`jekyll-feed`, `jekyll-sitemap`, `jekyll-redirect-from`, `jemoji`, `jekyll-gist`, `jekyll-paginate`) take effect on the live build. Plugins outside that set are silently ignored.

## CV (downloadable PDF)

The CV is a **PDF compiled from `cv/cv.tex`**, which is the single source of truth. There is no HTML CV page.

- Build: `pdflatex -interaction=nonstopmode -output-directory=files cv/cv.tex`, then rename `files/cv.pdf` → `files/Ryan_Fraser_CV.pdf` and delete the stray aux files (`.aux/.log/.out` are gitignored). MiKTeX is installed locally.
- The nav "CV" link (`_data/navigation.yml`) and the About-page link (`_pages/about.md`) both point at `/files/Ryan_Fraser_CV.pdf`.
- **To update the CV:** edit `cv/cv.tex`, recompile, recommit the PDF. Do not reintroduce an HTML CV page.

## Where to edit (content vs. theme)

Site content/config — edit freely:
- `_config.yml` — site title/URL, the `author:` block (powers the sidebar profile and which social icons appear), collections, and per-type `defaults`.
- `_data/navigation.yml` — the top-nav links.
- `_pages/about.md` — the homepage (`permalink: /`).
- `_publications/`, `_talks/`, `_teaching/` — content, one Markdown file per item (see "Collections").
- `cv/cv.tex`, `files/` (downloads), `images/` (headshot `profile.jpg`, favicons).

Theme internals — change cautiously; these are upstream AcademicPages code:
- `_layouts/`, `_includes/`, `_sass/`, `assets/js/`.

## How pages render

Layout inheritance: `compress` (HTML-minify wrapper) → `default` (the HTML skeleton: head, masthead, `{{ content }}`, footer, scripts) → one of:
- **`single`** — standard content page (author sidebar + article body). The default for `_pages`, `_publications`, `_teaching`, `_portfolio`, `_posts` (assigned via `defaults` in `_config.yml`, so individual files rarely set `layout`).
- **`archive`** — listing page (sidebar + title + the page's own body). The collection-index pages override their front matter to this and put their own Liquid `for` loops in the body.
- **`talk`** — per-talk page (shows talk type / venue / location). Default for `_talks`.

`{% include base_path %}` sets `base_path = site.url + site.baseurl`; internal links and assets are written `{{ base_path }}/...`. The author sidebar appears whenever a page sets `author_profile: true` (`sidebar.html` → `author-profile.html`, driven by the `author:` block in `_config.yml`).

## Collections (the content model)

Four collections, all output to `/:collection/:path/`. Each `.md` file is one item with YAML front matter.

- **`_publications/` is repurposed as "Works in Progress."** Indexed by `_pages/publications.html`, whose `permalink` is **`/works-in-progress/`** (also the nav label), rendered through `archive-single.html`. Front matter in use: `title`, `collection: publications`, `permalink`, `date` (used only for sorting — it is not displayed), `excerpt` (the coauthor line). Because these are unpublished they deliberately omit `venue`: `archive-single.html` only prints "Published in *venue*, year" when a `venue` exists. Optional `citation` / `paperurl` / `slidesurl` / `bibtexurl` fields unlock "Download …" links. To later split published papers from working papers, uncomment `publication_category` in `_config.yml`.
- **`_talks/`** — indexed by `_pages/talks.html` (`/talks/`) via `archive-single-talk.html`; each talk page uses the `talk` layout (`talk_type`, `venue`, `location`).
- **`_teaching/`** — indexed by `_pages/teaching.html` (`/teaching/`) via `archive-single.html` (`type`, `venue`, `date`).
- **`_portfolio/`, `_posts/`** — available from the theme but unused.

## Theming

`site_theme` in `_config.yml` selects a palette (`default`, `air`, `sunrise`, `mint`, `dirt`, `contrast`), implemented in `_sass/_themes.scss`. A light/dark toggle (the sun icon in the masthead) is wired by `assets/js/theme.js`. `assets/css/main.scss` is the Sass entry point Jekyll compiles to `/assets/css/main.css`.

## Notes

- `docs/buying-a-domain.md` documents upgrading from `rcfraser.github.io` to a custom domain (recommended `ryan-c-fraser.com`, via Cloudflare DNS + a root `CNAME` file). No `CNAME` exists yet — the site is still on the github.io subdomain.
- `_config.yml` force-includes `_pages` and `files` via `include:`, which is why PDFs in `files/` are published.
