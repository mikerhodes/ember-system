# Design Guide

This guide defines the visual style for Mike's internal web tools. Use it as the reference when starting a new app: read it, then apply it. The goal is a consistent family of tools that feel warm, calm, and focused — never corporate, never pastel, never flashy.

---

## CSS Custom Properties

Paste this `:root` block into every project. All examples below use these variables.

```css
:root {
  --accent:             #c4622d;
  --accent-dark:        #5a4030;
  --page-bg:            #f7f3ee;
  --container-bg:       #ede8e0;
  --focus-container-bg: #e8ddd4;
  --surface-bg:         #fdfcfa;
  --text-active:        #3a2e24;
  --text-muted:         #a89888;
  --text-secondary:     #8a7060;
  --ui-chrome:          #b8a898;
  --error:              #c0392b;
  --surface-border:     #ccc4b8;
  --surface-bottom:     #b8b0a4;
  --drag-outline:       #c4622d;
}
```

---

## Feel

The UI should feel like a well-made physical object: warm materials, clear structure, and satisfying tactile feedback. Interactive elements look like keycaps. Buttons push down when pressed. Container areas are quiet trays that hold things. One strong accent colour ties everything together.

Avoid:
- Cold blues and greys
- Drop shadows as the primary depth technique (use borders instead)
- Multiple accent colours competing for attention
- Pastel washes as backgrounds

---

## Colour Palette

### Accent

One accent colour used throughout. Warm rust/terracotta.

| Variable        | Value     | Usage                                                      |
|-----------------|-----------|------------------------------------------------------------|
| `--accent`      | `#c4622d` | Title, active section header, count badge, links, checkbox |
| `--accent-dark` | `#5a4030` | Text on light surfaces where the full accent is too loud   |

### Backgrounds

Barely-there warm whites. The accent does the work; backgrounds stay out of the way.

| Variable               | Value     | Usage                           |
|------------------------|-----------|---------------------------------|
| `--page-bg`            | `#f7f3ee` | Body background                 |
| `--container-bg`       | `#ede8e0` | Default panel/column background |
| `--focus-container-bg` | `#e8ddd4` | Active or highlighted panel     |
| `--surface-bg`         | `#fdfcfa` | Cards, buttons, kbd elements    |

### Text

| Variable          | Value     | Usage                                           |
|-------------------|-----------|-------------------------------------------------|
| `--text-active`   | `#3a2e24` | All readable body text, card titles, nav links  |
| `--text-muted`    | `#a89888` | Reserved for de-emphasised items only           |
| `--text-secondary`| `#8a7060` | Notes, captions, all-caps section/panel headers |
| `--ui-chrome`     | `#b8a898` | Timestamps, status lines, shortcut hints        |
| `--error`         | `#c0392b` | Error messages                                  |

### Borders

| Variable           | Value     | Usage                                   |
|--------------------|-----------|-----------------------------------------|
| `--surface-border` | `#ccc4b8` | Card and button top/side borders        |
| `--surface-bottom` | `#b8b0a4` | Thick bottom border for the raised look |
| `--drag-outline`   | `#c4622d` | Drop target outline                     |

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

## Raised Surface Treatment

The thick bottom border creates depth — not box-shadow. Apply it to elements that should feel like physical objects: things the user clicks, drags, or presses. Do not apply it to passive or recessed elements. When everything is raised, nothing is.

**Good candidates** — elements that invite action or can be physically moved:
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

**Leave flat** — elements that hold or display rather than act:
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

For `<kbd>` elements, use `box-shadow` instead of `border-bottom` to match the conventional keycap look:

```css
background: var(--surface-bg);
border: 1px solid var(--surface-border);
border-radius: 5px;
box-shadow: 0 2px 0 var(--surface-border);
padding: 2px 8px;
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
  background: #a84f22;
  border-color: #3a2010;
  border-bottom-color: #3a2010;
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
  background: #d8d0c8;
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
  background: #e0d8ce;
  outline: 2px solid var(--drag-outline);
  outline-offset: -1px;
}
```

---

## Sidebar Nav

The sidebar uses `--container-bg` as its background with a right border. Nav links must have `color` set explicitly — `<a>` tags do not inherit `color` from `body` and will fall back to the browser's default blue without it.

```css
.sidebar {
  background: var(--container-bg);
  border-right: 1px solid var(--surface-border);
}

.sidebar-nav li a {
  display: block;
  padding: 6px 16px;
  color: var(--text-active);  /* must be explicit — <a> doesn't inherit */
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

Use CSS grid with equal outer columns so the centred element stays truly centred regardless of content width on either side.

```css
header {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: baseline;
  margin-bottom: 20px;
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
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  font-size: 0.75rem;
  color: var(--accent-dark);
  background: var(--surface-bg);
  border: 1px solid var(--surface-border);
  border-radius: 5px;
  box-shadow: 0 2px 0 var(--surface-border);
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
  width: 15px;
  height: 15px;
  border: 1.5px solid var(--surface-border);
  border-bottom: 2px solid var(--surface-bottom);
  border-radius: 3px;
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
  left: 2px;
  top: 0px;
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

Inputs and textareas sit recessed in the page — they receive content rather than invite action. Use a flat border (no thick bottom border) so they contrast with the raised buttons and cards around them.

```css
input[type="text"],
textarea {
  background: var(--page-bg);
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

Textareas should be resizable vertically, with a minimum height tall enough to show a couple of lines:

```css
textarea {
  resize: vertical;
  min-height: 80px;
  line-height: 1.5;
}
```

Form labels use the all-caps section header style (see Typography) at `0.75rem` with a small gap above the input:

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

## Error Messages

Errors appear inline near the relevant UI (e.g. in the header status bar), not below the main content. They use a standard red that stands out without clashing with the warm palette:

```css
.error {
  color: var(--error);
  font-size: 0.75rem;
}
```

---

## Quick Reference

| Variable               | Value     |
|------------------------|-----------|
| `--accent`             | `#c4622d` |
| `--accent-dark`        | `#5a4030` |
| `--page-bg`            | `#f7f3ee` |
| `--container-bg`       | `#ede8e0` |
| `--focus-container-bg` | `#e8ddd4` |
| `--surface-bg`         | `#fdfcfa` |
| `--text-active`        | `#3a2e24` |
| `--text-muted`         | `#a89888` |
| `--text-secondary`     | `#8a7060` |
| `--ui-chrome`          | `#b8a898` |
| `--error`              | `#c0392b` |
| `--surface-border`     | `#ccc4b8` |
| `--surface-bottom`     | `#b8b0a4` |
| `--drag-outline`       | `#c4622d` |
