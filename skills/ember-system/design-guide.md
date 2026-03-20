# Design Guide

This guide defines the visual design system. Component patterns, variable roles, and layout rules apply to any theme. Apply a theme by linking a theme file from `themes/` — available themes are listed at the end.

---

## Feel

The UI should feel like a well-made physical object: clear structure, satisfying tactile feedback, and focused calm. Interactive elements look like keycaps. Buttons push down when pressed. Container areas are quiet trays that hold things. One accent colour ties everything together.

Avoid:
- Drop shadows as the primary depth technique (use borders instead)
- Multiple accent colours competing for attention
- Pastel washes as backgrounds

---

## Raised Surface Treatment

The thick bottom border creates depth — not box-shadow. Apply it to elements that invite action or can be physically moved. When everything is raised, nothing is.

**Good candidates:**
- Buttons (primary and secondary)
- Cards and list items
- `<kbd>` keycap labels
- Dropdown and select triggers (the visible control, not the open menu)
- Clickable filter chips and tag pills
- Segmented control segments (especially the selected one)
- Draggable items (raised treatment reinforces the "lifted" state)
- Toasts and notification banners (they float above the page)
- Modal dialogs (a distinct surface above the rest of the UI)
- Assignee/user chips in selectors

**Leave flat:**
- Form inputs and textareas (recessed; they receive content)
- Container panels and columns
- Read-only metadata, stat blocks, table rows
- Section labels and dividers

```css
background: var(--surface-bg);
border: 1px solid var(--surface-border);
border-bottom: 3px solid var(--surface-bottom);
border-radius: 6px;
```

### Primary buttons

Primary buttons use the accent as background with white text. On hover, darken the background — don't use opacity:

```css
.btn-primary {
  background: var(--accent);
  border-color: var(--accent-dark);
  border-bottom-color: var(--accent-dark);
  color: #fff;
}

.btn-primary:hover {
  background: var(--accent-hover);
  border-color: var(--accent-dark-hover);
  border-bottom-color: var(--accent-dark-hover);
}
```

### Button press feedback

When a button is active (pressed), collapse the bottom border and nudge it down:

```css
button:active {
  border-bottom-width: 1px;
  transform: translateY(2px);
}
```

---

## Variables

All components use CSS custom properties. Each theme file sets these on `:root`. Each variable has a defined role — never substitute one for another.

### Accent

| Variable             | Role                                                                 |
|----------------------|----------------------------------------------------------------------|
| `--accent`           | Primary accent: title, active section header, count badge, links, checkbox fill |
| `--accent-hover`     | Accent background on hover — a darkened version of `--accent`        |
| `--accent-dark`      | Button borders and borders on accent-filled elements                 |
| `--accent-dark-hover`| Darkened `--accent-dark` used on hover for button borders            |

### Backgrounds

| Variable               | Role                                                              |
|------------------------|-------------------------------------------------------------------|
| `--page-bg`            | Body background — the outermost surface                          |
| `--container-bg`       | Default panel and column background — slightly darker than page  |
| `--focus-container-bg` | Active or focused panel/nav item — warmer/deeper than `--container-bg`|
| `--surface-bg`         | Raised elements: cards, buttons, `<kbd>` — lightest surface      |

### Text

| Variable          | Role                                                                  |
|-------------------|-----------------------------------------------------------------------|
| `--text-active`   | All readable body text, card titles, nav links                        |
| `--text-secondary`| Notes, captions, all-caps section and panel headers                   |
| `--text-muted`    | De-emphasised items only — timestamps, completed items                |
| `--ui-chrome`     | Status lines, shortcut hints — the quietest readable text             |
| `--error`         | Error messages and error callout border/title                         |
| `--warning`       | Warning callout border and title                                      |
| `--success`       | Success callout border and title                                      |
| `--info`          | Info callout border and title                                         |

### Borders

| Variable           | Role                                                                                                     |
|--------------------|----------------------------------------------------------------------------------------------------------|
| `--surface-border` | Top and side borders on raised elements (cards, buttons, `<kbd>`)                                        |
| `--surface-bottom` | Thick bottom border on raised elements — darker than `--surface-bg` in light themes to create a shadow edge; *lighter* than `--surface-bg` in dark themes to create a highlight edge instead |
| `--drag-outline`   | Drop-target outline — typically matches `--accent`                                                       |

Two variable relationships differ between light and dark themes:

| Variable | Light theme | Dark theme | Why |
|---|---|---|---|
| `--surface-bottom` vs `--surface-bg` | darker (shadow edge) | lighter (highlight edge) | A darker bottom in dark mode creates a harsh hole; a lighter one glows |
| `--accent-dark` vs `--accent` | darker | lighter | Button borders must be visible; darkening accent in dark mode makes them disappear |

In both light and dark themes, `--surface-bg` must be lighter than `--container-bg` so cards lift above the panel.

### Chrome (sidebar and header)

Chrome variables let the sidebar and header differ visually from the content area. A dark sidebar on a light background (like Corporate Blues) changes only these variables — no component CSS differs.

| Variable           | Role                                      |
|--------------------|-------------------------------------------|
| `--sidebar-bg`     | Sidebar background                        |
| `--sidebar-text`   | Sidebar text and nav link colour          |
| `--sidebar-border` | Sidebar right border and dividers         |
| `--header-bg`      | Page header background                    |
| `--header-text`    | Page header text colour                   |
| `--header-border`  | Page header bottom border                 |

Example — same component CSS, different chrome variables:

| Variable group | Umber (unified chrome) | Corporate Blues (split chrome) |
|---|---|---|
| `--sidebar-bg` | `#ede8e0` (matches container) | `#3a5878` (dark navy) |
| `--header-bg` | `#f7f3ee` (matches page) | `#ffffff` (white) |

---

## Typography

System font stack throughout. No web fonts.

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  color: var(--text-active);
}
```

Use `monospace` for app titles to give them a distinct, tool-like character:

```css
h1 {
  font-family: monospace;
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--accent);
}
```

| Element          | Size       | Weight | Notes                                       |
|------------------|------------|--------|---------------------------------------------|
| Page title (h1)  | `1.25rem`  | 600    | Monospace, `var(--accent)`                  |
| Card / item text | `0.875rem` | 400    | `var(--text-active)` throughout             |
| Section header   | `0.75rem`  | 700    | All caps, `letter-spacing: 0.08em`, `var(--text-secondary)` |
| Secondary text   | `0.75rem`  | 400    | `var(--text-secondary)`, `white-space: pre-wrap` |
| UI chrome        | `0.75rem`  | 400    | `var(--ui-chrome)` — status lines, hints    |

---

## Cards and List Items

Cards are the primary content unit. They use the raised surface treatment.

```css
.card {
  background: var(--surface-bg);
  border-radius: 6px;
  border: 1px solid var(--surface-border);
  border-bottom: 3px solid var(--surface-bottom);
  padding: 10px 12px;
  margin-bottom: 8px;
  font-size: 0.875rem;
  color: var(--text-active);
  line-height: 1.4;
  word-break: break-word;
}
```

Secondary text (notes, captions) sits below the primary text, smaller and more muted:

```css
.secondary {
  margin-top: 4px;
  font-size: 0.75rem;
  color: var(--text-secondary);
  line-height: 1.4;
  white-space: pre-wrap;
}

.secondary a {
  color: var(--accent);
  text-decoration: none;
}

.secondary a:hover {
  text-decoration: underline;
}
```

---

## Container Panels

Panels are quiet trays. No border at rest — background colour alone defines them.

```css
.panel {
  background: var(--container-bg);
  border-radius: 4px;
  padding: 12px;
}
```

The active/focus panel gets a warmer tint, and its header uses the accent colour:

```css
.panel.active {
  background: var(--focus-container-bg);
}

.panel.active .panel-header {
  color: var(--accent);
}

.panel.active .panel-header .count {
  background: var(--accent);
  color: #fff;
}
```

Inactive panel headers use secondary text colour; count badges are muted:

```css
.panel-header {
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--text-secondary);
}

.panel-header .count {
  background: var(--container-bg);
  border-radius: 10px;
  padding: 1px 7px;
  font-size: 0.7rem;
  font-weight: 600;
  color: var(--text-muted);
}
```

When a panel is a drop target, show a solid accent outline:

```css
.panel.drag-over {
  background: var(--focus-container-bg);
  outline: 2px solid var(--drag-outline);
  outline-offset: -1px;
}
```

---

## Sidebar Nav

The sidebar uses `--sidebar-bg`, `--sidebar-text`, and `--sidebar-border`. Nav links must have `color` set explicitly — `<a>` tags do not inherit `color` from `body` and fall back to the browser's default blue without it.

```css
.sidebar {
  background: var(--sidebar-bg);
  border-right: 1px solid var(--sidebar-border);
}

.sidebar-nav li a {
  display: block;
  padding: 6px 16px;
  color: var(--sidebar-text);  /* must be explicit — <a> doesn't inherit */
  text-decoration: none;
  border-left: 3px solid transparent;
}

.sidebar-nav li a.active {
  color: var(--accent);
  border-left-color: var(--accent);
  background: var(--focus-container-bg);
  font-weight: 500;
}
```

Section labels within the sidebar use the shared all-caps header style (see Typography).

---

## Header Layout

The page header uses `--header-bg`, `--header-text`, and `--header-border`. Equal outer columns keep the centred element truly centred regardless of content width on either side.

```css
header {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: baseline;
  background: var(--header-bg);
  border-bottom: 1px solid var(--header-border);
  color: var(--header-text);
}
```

Right-side content (status, buttons) sits in a flex row aligned to the right:

```css
.header-right {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 8px;
}
```

---

## Keyboard Shortcut Hints

Render keyboard shortcuts with `<kbd>`. Style them as physical keycaps:

```html
<span class="shortcut">Quick entry <kbd>⌃⌥Space</kbd></span>
```

```css
.shortcut {
  font-size: 0.75rem;
  color: var(--ui-chrome);
  display: flex;
  align-items: center;
  gap: 6px;
}

kbd {
  font-family: monospace;
  font-size: 0.75rem;
  background: var(--surface-bg);
  border: 1px solid var(--surface-border);
  border-bottom: 3px solid var(--surface-bottom);
  border-radius: 5px;
  padding: 2px 8px;
  white-space: nowrap;
}
```

---

## Checkboxes

Use fully custom checkboxes so they match the raised-surface style and use the accent colour. Apply globally to all `input[type="checkbox"]`:

```css
input[type="checkbox"] {
  appearance: none;
  -webkit-appearance: none;
  width: 18px;
  height: 18px;
  border: 1.5px solid var(--surface-border);
  border-bottom: 2px solid var(--surface-bottom);
  border-radius: 4px;
  background: var(--surface-bg);
  cursor: pointer;
  position: relative;
  flex-shrink: 0;
}

input[type="checkbox"]:checked {
  background: var(--accent);
  border-color: var(--accent-dark);
  border-bottom-color: var(--accent-dark);
}

input[type="checkbox"]:checked::after {
  content: '';
  position: absolute;
  left: 5px;
  top: 2px;
  width: 5px;
  height: 9px;
  border: 2px solid #fff;
  border-top: none;
  border-left: none;
  transform: rotate(40deg);
}
```

---

## Form Inputs

Inputs and textareas sit recessed against their background — they receive content rather than invite action. Use a flat border (no thick bottom border) so they contrast with the raised buttons around them. Forms sit directly on the page or panel background; do not wrap them in a card.

```css
input[type="text"],
textarea {
  background: var(--surface-bg);
  border: 1px solid var(--surface-border);
  border-radius: 5px;
  padding: 7px 10px;
  font-family: inherit;
  font-size: 0.8125rem;
  color: var(--text-active);
  width: 100%;
}
```

On focus, switch the border to accent to show which field is active:

```css
input[type="text"]:focus,
textarea:focus {
  outline: none;
  border-color: var(--accent);
}
```

Textareas are resizable vertically, with a minimum height tall enough to show a couple of lines:

```css
textarea {
  resize: vertical;
  min-height: 80px;
  line-height: 1.5;
}
```

Form labels use the all-caps section header style (see Typography):

```css
label {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
```

---

## Callouts

Callouts use a left border to signal semantic meaning — no background fill, no raised treatment, no `box-shadow`. The border alone carries the semantic signal.

Four variants, each using a dedicated semantic colour variable:

```css
.callout {
  border-left: 4px solid var(--surface-border);
  padding: 10px 14px;
  font-size: 0.8125rem;
  color: var(--text-active);
}

.callout-title {
  font-weight: 600;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  margin-bottom: 2px;
}

.callout-info    { border-left-color: var(--info); }
.callout-success { border-left-color: var(--success); }
.callout-warning { border-left-color: var(--warning); }
.callout-error   { border-left-color: var(--error); }

.callout-info    .callout-title { color: var(--info); }
.callout-success .callout-title { color: var(--success); }
.callout-warning .callout-title { color: var(--warning); }
.callout-error   .callout-title { color: var(--error); }
```

---

## Error Messages

Errors appear inline near the relevant UI (e.g. in the header status bar or inside a related card), not below the main content:

```css
.error {
  color: var(--error);
  font-size: 0.75rem;
}
```

---

## Prose

For long-form text — documentation, help pages, detail views, READMEs rendered in the UI — wrap your HTML in `.prose`. It restores sensible spacing and sizing on plain semantic elements without touching backgrounds or borders, so it works on the bare page or inside a card or panel unchanged.

```html
<div class="prose">
  <h2>Section heading</h2>
  <p>Body text with a <a href="#">link</a> and <code>inline code</code>.</p>
  <ul>
    <li>List item</li>
  </ul>
  <pre><code>code block</code></pre>
  <blockquote>A callout-style quote.</blockquote>
</div>
```

### What `.prose` handles

| Element | Treatment |
|---|---|
| `h1`–`h3` | Scaled weights in `--text-active`; `1.75em` top margin, none on first child |
| `h4` | Small-caps style, `--text-secondary` — acts like a section label within prose |
| `p` | `0.75em` top margin; `line-height: 1.7` for comfortable reading |
| `a` | `--accent` colour, underline on hover |
| `strong` / `em` | Bold / italic; inherit colour |
| `ul` / `ol` | Left-padded with disc/decimal markers; `0.3em` gap between items |
| `code` (inline) | Monospace, `--surface-bg` background, 1px border — keycap look without thick bottom |
| `pre` + `code` | Flat recessed block (`--container-bg` background, 1px border) — same philosophy as form inputs |
| `blockquote` | Left border (`--surface-border`) + italic text in `--text-secondary` — same pattern as callouts |
| `hr` | 1px `--surface-border` top border; vertical breathing room |

### Sizing

`.prose` sets `font-size: 0.9375rem` (slightly larger than the UI default of `0.875rem`) and `max-width: 68ch`. The wider measure and looser line height distinguish reading text from dense UI data. Override either on the container if you need a tighter fit.

### Embedding prose in components

`.prose` controls only spacing and typography. The same class works without modification on the bare page background or inside a `.card`, `.detail-card`, or `.panel`. Do not add separate padding inside `.prose`; rely on the parent component's padding instead.

---

## Themes

Link `ember.css` and one theme file. For automatic light/dark switching, use two link tags with `media` queries:

```html
<link rel="stylesheet" href="ember.css">
<link rel="stylesheet" href="themes/umber-light.css" media="(prefers-color-scheme: light)">
<link rel="stylesheet" href="themes/umber-dark.css"  media="(prefers-color-scheme: dark)">
```

Available themes:

| File                         | Description                                              |
|------------------------------|----------------------------------------------------------|
| `umber-light.css`            | Warm terracotta on cream. The reference theme.           |
| `umber-dark.css`             | Dark brown ground with terracotta accents.               |
| `solarized-light.css`        | Solarized palette, light variant.                        |
| `solarized-dark.css`         | Solarized palette, dark variant.                         |
| `corporate-blues-light.css`  | Navy and sky blue on white; dark sidebar chrome.         |
| `corporate-blues-dark.css`   | Dark navy ground with steel blue accents; dark sidebar.  |
| `monochroma-light.css`       | Pure greyscale on light grey; chrome recedes for media.  |
| `monochroma-dark.css`        | Pure greyscale on deep charcoal; chrome recedes for media.|
