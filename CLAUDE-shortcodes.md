# Claude Code Task: Build Dossier-Style Hugo Shortcodes for restore-democracy.org

## Goal
Create a set of Hugo shortcodes that reproduce the editorial "Dossier" article format
used in the article widget design. These shortcodes allow content authors to write
structured, visually rich articles in markdown without touching HTML directly.

All shortcode templates go in `layouts/shortcodes/`.
All CSS goes in `static/css/dossier-shortcodes.css` (separate from dossier.css so
it's easy to maintain independently).
Inject the new CSS file by appending its `<link>` tag to `layouts/partials/dossier-head.html`
(which should already exist from the previous task).

---

## Shortcodes to build

### 1. `case-card` — wraps a single case study

**Usage:**
```
{{< case-card number="01" title="The Faked Throuple Attack on Thomas Massie" platforms="X (Twitter) · Facebook · Kentucky primary targeting" >}}
  ...inner content and nested shortcodes...
{{< /case-card >}}
```

**Renders:**
- A bordered card (dark surface, gold border, border-radius)
- A header row containing:
  - A monospace pill badge showing "CASE 01" in gold/amber
  - The case title in Playfair Display
  - The platforms string in JetBrains Mono, small, muted, below the title
- A body area below the header for inner content (description text + verdict shortcode)

**Template file:** `layouts/shortcodes/case-card.html`

```html
<div class="dossier-case-card">
  <div class="dossier-case-header">
    <span class="dossier-case-number">CASE {{ .Get "number" }}</span>
    <div class="dossier-case-meta">
      <div class="dossier-case-title">{{ .Get "title" }}</div>
      <div class="dossier-case-platforms">{{ .Get "platforms" }}</div>
    </div>
  </div>
  <div class="dossier-case-body">
    {{ .Inner | markdownify }}
  </div>
</div>
```

---

### 2. `verdict` — the two-column misdirection/reality box

**Usage:**
```
{{< verdict
  misdirection="A salacious, viral fabrication engineered to trigger disgust and tribal betrayal."
  reality="The entire scenario was AI-generated. Facebook ran fine-print disclosure. X ran it with no label at all."
>}}
```

**Renders:**
- A two-column grid (stacks to single column on mobile)
- Left column: amber/warning toned box labeled "WHAT THE AD CLAIMED"
- Right column: blue/info toned box labeled "WHAT ACTUALLY HAPPENED"
- Both boxes: rounded corners, label in JetBrains Mono uppercase, body in Crimson Pro

**Template file:** `layouts/shortcodes/verdict.html`

```html
<div class="dossier-verdict-row">
  <div class="dossier-verdict-box dossier-verdict-misdirection">
    <div class="dossier-verdict-label">What the ad claimed</div>
    <p class="dossier-verdict-text">{{ .Get "misdirection" }}</p>
  </div>
  <div class="dossier-verdict-box dossier-verdict-reality">
    <div class="dossier-verdict-label">What actually happened</div>
    <p class="dossier-verdict-text">{{ .Get "reality" }}</p>
  </div>
</div>
```

---

### 3. `section-label` — the small monospace section divider

**Usage:**
```
{{< section-label text="Case studies" >}}
```

**Renders:**
- JetBrains Mono, 10-11px, uppercase, letter-spacing, muted gold color
- Margin above and below
- Optional: a short horizontal rule or thin line above it

**Template file:** `layouts/shortcodes/section-label.html`

```html
<div class="dossier-section-label">{{ .Get "text" }}</div>
```

---

### 4. `pullquote` — the gold left-border pull quote

**Usage:**
```
{{< pullquote >}}
Disinformation has outgrown "fake news." The current weapon of choice is synthetic video
that blurs the line between hyperbole, satire, and outright fabrication.
{{< /pullquote >}}
```

**Renders:**
- Gold left border (3px)
- Italic Playfair Display, slightly larger than body text
- Dark surface background, subtle padding
- Rounded right corners

**Template file:** `layouts/shortcodes/pullquote.html`

```html
<div class="dossier-pullquote">
  <p>{{ .Inner | markdownify }}</p>
</div>
```

---

### 5. `platform-table` — the platform accountability breakdown table

**Usage:**
```
{{< platform-table >}}
| Platform | Primary delivery method | Current moderation approach |
|---|---|---|
| **X (Twitter)** | Unlabeled viral video | Post-upload Community Notes only |
| **Facebook** | Micro-targeted dark ads | Fine-print disclosure, easily ignored |
| **Instagram** | Reels and AI avatars | Weak synthetic persona detection |
| **YouTube** | Pre-roll in local races | Minimal scrutiny at municipal level |
{{< /platform-table >}}
```

**Renders:**
- Wraps a standard markdown table in a styled container
- Dark surface background, gold header border-bottom
- JetBrains Mono for header row, Crimson Pro for body rows
- Responsive: horizontal scroll wrapper on mobile

**Template file:** `layouts/shortcodes/platform-table.html`

```html
<div class="dossier-platform-table-wrap">
  {{ .Inner | markdownify }}
</div>
```

---

### 6. `kicker` — the small label above the article title (optional, for use in content)

**Usage:**
```
{{< kicker text="AI & Disinformation · Week of May 26, 2026" >}}
```

**Renders:**
- JetBrains Mono, uppercase, muted, with a short horizontal rule before the text
- Sits above the article title in the content flow

**Template file:** `layouts/shortcodes/kicker.html`

```html
<div class="dossier-kicker">{{ .Get "text" }}</div>
```

---

## CSS to write: `static/css/dossier-shortcodes.css`

Write this file with clear section comments. Use the dossier color variables as
CSS custom properties defined at `:root` so they're available throughout:

```css
:root {
  --dossier-gold: #c9a84c;
  --dossier-gold-hover: #e2bf72;
  --dossier-bg: #0f0f0f;
  --dossier-surface: #1a1a1a;
  --dossier-text: #f0ece4;
  --dossier-text-muted: #a09880;
  --dossier-border: rgba(201, 168, 76, 0.2);
  --dossier-border-strong: rgba(201, 168, 76, 0.5);
  --dossier-amber-bg: rgba(201, 168, 76, 0.08);
  --dossier-amber-border: rgba(201, 168, 76, 0.3);
  --dossier-blue-bg: rgba(56, 139, 221, 0.08);
  --dossier-blue-border: rgba(56, 139, 221, 0.3);
  --dossier-blue-text: #7ab3e8;
}
```

### Required CSS sections:

**`.dossier-case-card`**
- border: 0.5px solid var(--dossier-border)
- border-radius: 10px
- margin: 1.5rem 0
- overflow: hidden
- background: var(--dossier-surface)

**`.dossier-case-header`**
- padding: 1rem 1.25rem 0.85rem
- border-bottom: 0.5px solid var(--dossier-border)
- display: flex, align-items: flex-start, gap: 1rem

**`.dossier-case-number`**
- font-family: JetBrains Mono
- font-size: 11px, uppercase, letter-spacing: 0.12em
- color: var(--dossier-gold)
- background: var(--dossier-amber-bg)
- border: 0.5px solid var(--dossier-amber-border)
- border-radius: 4px, padding: 2px 7px
- white-space: nowrap, flex-shrink: 0, margin-top: 3px

**`.dossier-case-title`**
- font-family: Playfair Display
- font-size: 19px, font-weight: 700, line-height: 1.25
- color: var(--dossier-text)

**`.dossier-case-platforms`**
- font-family: JetBrains Mono
- font-size: 10px, color: var(--dossier-text-muted)
- letter-spacing: 0.08em, margin-top: 4px

**`.dossier-case-body`**
- padding: 1.1rem 1.25rem
- Crimson Pro, 16px body text inside

**`.dossier-verdict-row`**
- display: grid
- grid-template-columns: 1fr 1fr
- gap: 1rem, margin-top: 0.75rem
- @media (max-width: 600px): grid-template-columns: 1fr

**`.dossier-verdict-box`**
- border-radius: 6px, padding: 0.85rem 1rem

**`.dossier-verdict-misdirection`**
- background: var(--dossier-amber-bg)
- border: 0.5px solid var(--dossier-amber-border)

**`.dossier-verdict-reality`**
- background: var(--dossier-blue-bg)
- border: 0.5px solid var(--dossier-blue-border)

**`.dossier-verdict-label`**
- font-family: JetBrains Mono
- font-size: 10px, uppercase, letter-spacing: 0.1em, margin-bottom: 0.5rem
- .misdirection label: color var(--dossier-gold)
- .reality label: color var(--dossier-blue-text)

**`.dossier-verdict-text`**
- font-family: Crimson Pro, font-size: 15px, line-height: 1.55
- color: var(--dossier-text), margin: 0

**`.dossier-section-label`**
- font-family: JetBrains Mono
- font-size: 10px, uppercase, letter-spacing: 0.14em
- color: var(--dossier-text-muted)
- margin: 2.5rem 0 1rem

**`.dossier-pullquote`**
- border-left: 3px solid var(--dossier-gold)
- border-radius: 0 6px 6px 0
- background: var(--dossier-surface)
- padding: 0.75rem 1.25rem, margin: 2rem 0
- p: Playfair Display, italic, 19px, line-height 1.5, color var(--dossier-text)

**`.dossier-platform-table-wrap`**
- overflow-x: auto (for mobile)
- margin: 1.5rem 0
- table: width 100%, border-collapse collapse
- th: JetBrains Mono, 10px, uppercase, letter-spacing, muted color,
      border-bottom: 0.5px solid var(--dossier-border), padding 0.5rem 0.75rem, text-align left
- td: Crimson Pro, 16px, padding 0.65rem 0.75rem,
      border-bottom: 0.5px solid var(--dossier-border), color var(--dossier-text)
- tr:last-child td: no border-bottom
- strong inside td: Crimson Pro 600, color var(--dossier-text)

**`.dossier-kicker`**
- font-family: JetBrains Mono
- font-size: 11px, uppercase, letter-spacing: 0.12em
- color: var(--dossier-text-muted)
- display: flex, align-items: center, gap: 10px, margin-bottom: 1.2rem
- ::before pseudo: content '', width 28px, height 1px, background var(--dossier-text-muted), opacity 0.5

---

## Example article showing all shortcodes in use

After building the shortcodes, create a test article at
`content/articles/test-dossier-shortcodes/index.md` using this content
to verify everything renders correctly:

```markdown
---
title: "Shortcode Test: Dossier Format"
date: 2026-05-29T00:00:00Z
type: "articles"
draft: true
author: "Theta"
categories: ["Test"]
tags: ["shortcodes", "dossier"]
featured_image: ""
comments: false
---
---

{{< kicker text="AI & Disinformation · Test Article" >}}

For years, political disinformation meant grainy screenshots and chain emails.
This week's landscape represents something qualitatively different.

{{< pullquote >}}
Disinformation has outgrown "fake news." The current weapon of choice is synthetic
video that blurs the line between hyperbole, satire, and outright fabrication.
{{< /pullquote >}}

{{< section-label text="Case studies" >}}

{{< case-card number="01" title="The Faked Throuple Attack on Thomas Massie" platforms="X (Twitter) · Facebook · Kentucky primary targeting" >}}
Perhaps the most brazenly deceptive ad to hit social feeds this month targeted
Representative Thomas Massie right before his high-profile primary defeat in Kentucky.

{{< verdict
  misdirection="A salacious fabrication engineered to trigger disgust and tribal betrayal — designed to spread before anyone could fact-check it."
  reality="The entire scenario was AI-generated. Facebook ran a blink-and-miss-it fine-print disclosure. X ran it with no label at all."
>}}
{{< /case-card >}}

{{< section-label text="Platform accountability" >}}

{{< platform-table >}}
| Platform | Primary delivery method | Moderation approach |
|---|---|---|
| **X (Twitter)** | Unlabeled viral video | Post-upload Community Notes only |
| **Facebook** | Micro-targeted dark ads | Fine-print disclosure, easily ignored |
| **Instagram** | Reels and AI avatars | Weak synthetic persona detection |
| **YouTube** | Pre-roll in local races | Minimal scrutiny at municipal level |
{{< /platform-table >}}
```

Run `hugo server` and navigate to `/articles/test-dossier-shortcodes/` to verify.
Set `draft: false` only after confirming the render looks correct.

---

## Constraints
- Do NOT modify any files inside `themes/hugo-news/`
- Do NOT modify existing content files
- Shortcode templates must use Hugo's `.Inner` and `.Get` correctly
- The `verdict` shortcode uses named params (`.Get "misdirection"`) not positional
- All shortcodes must be mobile-safe (test at 375px)
- CSS must not conflict with existing dossier.css — use the `.dossier-` prefix on all classes
- Add `/* === SECTION NAME === */` comments throughout dossier-shortcodes.css

## Report back when done
1. List every file created
2. Confirm the test article renders correctly at `/articles/test-dossier-shortcodes/`
3. Note any Hugo version compatibility issues (especially with `.Inner | markdownify`)
4. Flag any shortcode nesting issues (verdict inside case-card) and how resolved
