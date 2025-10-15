# Restore Democracy — Hugo Site

This repository contains a Hugo-powered news site using the `hugo-news` theme.

## How the homepage is generated
- The homepage content comes from `content/_index.md`.
- The layout and visual structure are provided by the theme/layout templates (e.g., files under `themes/hugo-news/layouts/`).
- If you need to customize the homepage template explicitly, create `layouts/index.html` in this project (or override the relevant theme templates).

## About the former `homepage.html` prototype
- That file was a static prototype/mockup used for quick design/layout experiments and has been removed to avoid confusion.
- Hugo does not read files from the repository root for rendering; it was never part of the build.
- The live homepage is generated from `content/_index.md` and the theme’s templates.
- If you want to create a new prototype outside Hugo in the future, keep it under a separate folder (e.g., `/prototypes/`) or a gist, not in the repo root.

## Development
- Run `hugo server` to start a local development server.
- Public output is generated in the `public/` directory when you run `hugo`.

## Step-by-step: How Hugo builds the homepage
1. Read configuration
   - Hugo loads site config from `config/_default/config.yaml` (and other config files if present) to determine baseURL, theme, taxonomies, params, and output settings.
2. Identify the homepage content
   - The homepage is the special “home” node (Kind = home). If `content/_index.md` exists (it does in this repo), its front matter and body become `.Title`, `.Params`, and `.Content` for the home page.
   - If `content/_index.md` were absent, the home page would still render, but `.Content` would be empty unless the template adds listings or other content.
3. Find the homepage layout (template lookup order)
   - Hugo searches for a template for Kind=home in this order:
     1) `layouts/index.html` (project override)
     2) `layouts/_default/home.html`
     3) `layouts/_default/list.html`
     4) `themes/<theme>/layouts/index.html`
     5) `themes/<theme>/layouts/_default/home.html`
     6) `themes/<theme>/layouts/_default/list.html`
   - In this project, you have `layouts/index.html` (project-level) and the theme provides `themes/hugo-news/layouts/_default/home.html` and `list.html`. The highest-priority match present is used.
4. Apply base template and partials (if used)
   - Most layouts extend a base template via `baseof.html` and `{{ define "main" }}` blocks.
   - The theme includes `themes/hugo-news/layouts/_default/baseof.html` which pulls in partials like `partials/head.html`, `partials/header.html`, `partials/footer.html`, and assets.
5. Render homepage content
   - Within the selected layout’s `{{ define "main" }}` block, Hugo injects `.Content` from `content/_index.md` (Markdown converted to HTML) when referenced.
   - The layout may also include lists of recent articles, sections, or taxonomies using `.Site.RegularPages`, `.Paginate`, `.Site.Taxonomies`, etc., depending on the template.
6. Pagination (if the template enables it)
   - If the chosen layout calls `{{ $p := .Paginate ... }}`, Hugo creates paginated pages under `/page/<n>/` and exposes `$p.Pages` for rendering.
7. Asset inclusion
   - CSS/JS referenced by the layout/partials are included. In this theme, styles and scripts live under `themes/hugo-news/assets` or `themes/hugo-news/static` and/or your project’s `static/` (none project-level here). Files under `static/` are copied as-is to `public/`.
8. Generate output files
   - Hugo writes the rendered homepage to `public/index.html`. If RSS or JSON outputs are configured for home, it also writes `public/index.xml` or `public/index.json` based on output formats.
9. Theme override mechanics (how to customize)
   - To change homepage structure, add `layouts/index.html` in this repo; it will override the theme’s home template.
   - To tweak shared chrome, override partials by creating files under `layouts/partials/` with the same names as in the theme. To change the base skeleton, override `layouts/_default/baseof.html`.

Quick references in this repo
- Content source: `content/_index.md`
- Project override (if present): `layouts/index.html`
- Theme base: `themes/hugo-news/layouts/_default/baseof.html`
- Theme home/list: `themes/hugo-news/layouts/_default/home.html` and `themes/hugo-news/layouts/_default/list.html`
- Built output: `public/index.html` (and possibly `public/index.xml`)
