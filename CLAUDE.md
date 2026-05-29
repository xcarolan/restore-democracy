# Claude Code Task: Apply "Dossier" Theme to restore-democracy.org

## Goal
Apply a dark, editorial "Dossier" aesthetic to the restore-democracy.org Hugo site using
CSS overrides and partial template injection only. Do NOT modify any files inside
`themes/hugo-news/`. All changes go in the project-level `static/`, `layouts/partials/`,
and `assets/` directories so the theme remains upgradeable.

---

## Design spec

### Colors
- Background (page): `#0f0f0f`
- Background (cards/surfaces): `#1a1a1a`
- Background (nav): `#111111`
- Accent / gold: `#c9a84c`
- Accent hover: `#e2bf72`
- Text primary: `#f0ece4`
- Text secondary: `#a09880`
- Border: `rgba(201, 168, 76, 0.2)`
- Border strong: `rgba(201, 168, 76, 0.5)`

### Typography (load from Google Fonts)
- Headlines: `Playfair Display` (700)
- Body: `Crimson Pro` (400, 400 italic, 600)
- Labels / meta / tags / nav: `JetBrains Mono` (400, 500)

### Aesthetic rules
- No red or blue anywhere — remove or neutralize any partisan color usage in the theme
- Article titles use Playfair Display
- Body text uses Crimson Pro at 18px, line-height 1.75
- All meta (author, date, tags) use JetBrains Mono at 11-12px, uppercase, letter-spacing
- Tag/category pills: dark background, gold border, gold text, monospace font
- "Read More" buttons: gold border, gold text, transparent background, monospace font
- Nav links: monospace, uppercase, letter-spacing, gold on hover
- Cards: dark surface (#1a1a1a), subtle gold border, no box-shadow
- Featured image: full width, slight overlay to blend into dark background
- Horizontal rules: gold, 1px, low opacity
- Links in body text: gold, no underline by default, underline on hover

---

## Step-by-step instructions

### Step 1: Audit the theme
Read and understand the following files before writing any CSS:
- `themes/hugo-news/layouts/_default/baseof.html`
- `themes/hugo-news/layouts/_default/home.html`
- `themes/hugo-news/layouts/_default/single.html` (article page)
- `themes/hugo-news/layouts/partials/head.html`
- `themes/hugo-news/layouts/partials/header.html`
- `themes/hugo-news/layouts/partials/footer.html`
- Any CSS files under `themes/hugo-news/assets/css/` or `themes/hugo-news/static/css/`

List every meaningful CSS class and HTML structure you find. This is the foundation
for writing accurate overrides — do not guess at class names.

### Step 2: Create the font + CSS injection partial

Create `layouts/partials/dossier-head.html`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Crimson+Pro:ital,wght@0,400;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="/css/dossier.css">
```

### Step 3: Inject the partial into head

Check if `layouts/partials/head.html` already exists at the project level.
- If YES: append `{{ partial "dossier-head.html" . }}` before the closing `</head>` tag.
- If NO: create `layouts/partials/head.html` by copying the theme's version
  (`themes/hugo-news/layouts/partials/head.html`) and appending the partial call.

### Step 4: Write the CSS override file

Create `static/css/dossier.css` using the class names discovered in Step 1.

The file must cover at minimum:

**Global / reset**
- `body`: dark background, primary text color, Crimson Pro font
- `a`: gold color, no underline; hover underline
- `hr`: gold, 1px, 20% opacity
- `h1, h2, h3, h4`: Playfair Display
- Selection highlight: gold background, dark text

**Navigation / header**
- Nav background: #111111
- Site title: Playfair Display, gold
- Nav links: JetBrains Mono, uppercase, letter-spacing, text-secondary; gold on hover
- Border-bottom: 1px solid gold at low opacity

**Homepage**
- Page background: #0f0f0f
- Featured/hero article card: dark surface, gold border
- Article card grid: dark surface cards, gold border, no shadow
- Card titles: Playfair Display
- Card meta (author, date): JetBrains Mono, text-secondary, small, uppercase
- Category/tag pills: dark bg, gold border, gold text, JetBrains Mono, small
- "Read More" button: transparent bg, gold border, gold text, JetBrains Mono,
  uppercase, letter-spacing; lighten on hover

**Article / single page**
- Article title: Playfair Display, large (2.2rem+), text-primary
- Byline: JetBrains Mono, small, text-secondary
- Body text: Crimson Pro, 18px, 1.75 line-height
- Body `h2`, `h3`: Playfair Display, gold accent left-border or gold color
- Body `strong`: Crimson Pro 600
- Body `blockquote`: gold left border (3px), italic, indented
- Body `table`: dark surface, gold header border-bottom, monospace headers
- Body `code`, `pre`: dark surface, monospace, gold text
- Featured image: width 100%, display block
- Tags section: same pill style as homepage

**Footer**
- Dark background (#111111), gold border-top
- Text: text-secondary, JetBrains Mono, small

### Step 5: Verify
Run `hugo server` and check:
- [ ] Homepage renders with dark background and gold accents
- [ ] Article page body text is readable in Crimson Pro
- [ ] Nav and tags use JetBrains Mono
- [ ] No blue or red partisan colors remain visible
- [ ] Featured images display correctly against dark background
- [ ] Mobile layout is not broken (check at 375px width)

If any theme CSS is using `!important` to force colors, match it in dossier.css.
If the theme loads CSS via Hugo Pipes (asset bundling), you may need to increase
specificity by prefixing selectors with `body` or `html body` rather than relying
on source order alone.

### Step 6: Report back
When done, provide:
1. A list of every file created or modified
2. Any class names or theme patterns that were unexpected or required workarounds
3. Any mobile or specificity issues found and how they were resolved
4. A note if any theme files HAD to be modified (with justification)

---

## Constraints
- Do NOT edit any file inside `themes/hugo-news/`
- Do NOT change content files in `content/`
- Do NOT change `config/` files unless a font or CSS path needs to be registered
- Keep the CSS file well-commented by section so Sylvester can maintain it easily
- Preserve all existing Hugo functionality (pagination, tags, categories, authors)
