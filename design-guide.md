# Design Guide

This guide defines the visual design system. It is theme-agnostic: the component patterns, variable roles, and layout rules apply to any theme. Apply a theme by dropping in a `:root` block — the themes are listed at the end of this guide.

---

## Feel

The UI should feel like a well-made physical object: clear structure, satisfying tactile feedback, and focused calm. Interactive elements look like keycaps. Buttons push down when pressed. Container areas are quiet trays that hold things. One accent colour ties everything together.

Avoid:
- Drop shadows as the primary depth technique (use borders instead)
- Multiple accent colours competing for attention
- Pastel washes as backgrounds

---

## Variables

All components use CSS custom properties. Set these on `:root` (or a `[data-theme]` attribute) to apply a theme. Each variable has a defined role — never substitute one for another.

### Accent

| Variable             | Role                                                                 |
|----------------------|----------------------------------------------------------------------|
| `--accent`           | Primary accent: title, active section header, count badge, links, checkbox fill |
| `--accent-hover`     | Accent background on hover — a darkened version of `--accent`        |
| `--accent-dark`      | Darker accent for text on light surfaces and for button borders      |
| `--accent-dark-hover`| Darkened `--accent-dark` used on hover for button borders            |

### Backgrounds

| Variable               | Role                                                              |
|------------------------|-------------------------------------------------------------------|
| `--page-bg`            | Body background — the outermost surface                          |
| `--container-bg`       | Default panel and column background — slightly darker than page  |
| `--focus-container-bg` | Active or highlighted panel — warmer/deeper than `--container-bg`|
| `--surface-bg`         | Raised elements: cards, buttons, `<kbd>` — lightest surface      |

In dark themes, `--surface-bg`, `--surface-bottom`, and `--accent-dark` invert their relationship to surrounding values — see Umber and Umber Dark in the Themes section for a worked example of a light/dark pair.

### Text

| Variable          | Role                                                                  |
|-------------------|-----------------------------------------------------------------------|
| `--text-active`   | All readable body text, card titles, nav links                        |
| `--text-secondary`| Notes, captions, all-caps section and panel headers                   |
| `--text-muted`    | De-emphasised items only — timestamps, completed items                |
| `--ui-chrome`     | Status lines, shortcut hints — the quietest readable text             |
| `--error`         | Error messages                                                        |

### Borders

| Variable           | Role                                                                                                     |
|--------------------|----------------------------------------------------------------------------------------------------------|
| `--surface-border` | Top and side borders on raised elements (cards, buttons, `<kbd>`)                                        |
| `--surface-bottom` | Thick bottom border on raised elements — darker than `--surface-bg` in light themes to create a shadow edge; can be *lighter* than `--surface-bg` in dark themes to create a highlight edge instead |
| `--drag-outline`   | Drop-target outline — typically matches `--accent`                                                       |

### Chrome (sidebar and header)

The chrome variables let the sidebar and header differ visually from the content area. In a theme with a dark sidebar on a light background (like Blues), only these variables change — the content area stays light.

| Variable          | Role                                      |
|-------------------|-------------------------------------------|
| `--sidebar-bg`    | Sidebar background                        |
| `--sidebar-text`  | Sidebar text and nav link colour          |
| `--sidebar-border`| Sidebar right border and dividers         |
| `--header-bg`     | Page header background                    |
| `--header-text`   | Page header text colour                   |
| `--header-border` | Page header bottom border                 |

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

In dark themes, `--surface-bg` should be lighter than `--container-bg` (cards lift up), and `--surface-bottom` can be lighter than `--surface-bg` to create a highlight edge rather than a dark shadow.

`<kbd>` elements use the same raised treatment as cards and buttons.

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

The sidebar uses `--sidebar-bg`, `--sidebar-text`, and `--sidebar-border`. The chrome variables let the sidebar differ from the content area — a theme can use a dark sidebar on a light background by setting only those three variables. Nav links must have `color` set explicitly — `<a>` tags do not inherit `color` from `body` and fall back to the browser's default blue without it.

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

The page header uses `--header-bg`, `--header-text`, and `--header-border`. Use CSS grid with equal outer columns so the centred element stays truly centred regardless of content width on either side.

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

## Error Messages

Errors appear inline near the relevant UI (e.g. in the header status bar), not below the main content:

```css
.error {
  color: var(--error);
  font-size: 0.75rem;
}
```

---

## Themes

Apply a theme by setting the variables below on `:root` or a `[data-theme]` attribute on `<html>`. Switch themes at runtime with `document.documentElement.dataset.theme = 'blues'`.

### Umber (default)

Warm terracotta on cream. The reference theme — the system was built around it.

```css
:root {
  --accent:             #c4622d;
  --accent-hover:       #a84f22;
  --accent-dark:        #5a4030;
  --accent-dark-hover:  #3a2010;
  --page-bg:            #f7f3ee;
  --container-bg:       #ede8e0;
  --focus-container-bg: #e8ddd4;
  --surface-bg:         #fdfcfa;
  --text-active:        #3a2e24;
  --text-secondary:     #8a7060;
  --text-muted:         #a89888;
  --ui-chrome:          #b8a898;
  --error:              #c0392b;
  --surface-border:     #ccc4b8;
  --surface-bottom:     #b8b0a4;
  --drag-outline:       #c4622d;
  --sidebar-bg:         #ede8e0;
  --sidebar-text:       #3a2e24;
  --sidebar-border:     #ccc4b8;
  --header-bg:          #f7f3ee;
  --header-text:        #3a2e24;
  --header-border:      #ccc4b8;
}
```

### Umber Dark

The same warm rust accent against deep brown backgrounds. Cards lift lighter; borders glow rather than shadow.

```css
[data-theme="umber-dark"] {
  --accent:             #c4622d;
  --accent-hover:       #a84f22;
  --accent-dark:        #e07840;
  --accent-dark-hover:  #c46030;
  --page-bg:            #1e1810;
  --container-bg:       #261f16;
  --focus-container-bg: #2e261a;
  --surface-bg:         #32281c;
  --text-active:        #e8d8c0;
  --text-secondary:     #a89070;
  --text-muted:         #786050;
  --ui-chrome:          #786050;
  --error:              #e05040;
  --surface-border:     #4a3c2c;
  --surface-bottom:     #5a4a36;
  --drag-outline:       #c4622d;
  --sidebar-bg:         #1e1810;
  --sidebar-text:       #e8d8c0;
  --sidebar-border:     #2e261a;
  --header-bg:          #1e1810;
  --header-text:        #e8d8c0;
  --header-border:      #2e261a;
}
```

### Solarized

The classic Solarized light palette. A cool blue accent on warm yellowed white.

```css
[data-theme="solarized"] {
  --accent:             #268bd2;
  --accent-hover:       #1a6fa8;
  --accent-dark:        #073642;
  --accent-dark-hover:  #052530;
  --page-bg:            #fdf6e3;
  --container-bg:       #eee8d5;
  --focus-container-bg: #e5ddc8;
  --surface-bg:         #fffdf7;
  --text-active:        #657b83;
  --text-secondary:     #586e75;
  --text-muted:         #93a1a1;
  --ui-chrome:          #93a1a1;
  --error:              #dc322f;
  --surface-border:     #d5ceb8;
  --surface-bottom:     #b5ae98;
  --drag-outline:       #268bd2;
  --sidebar-bg:         #eee8d5;
  --sidebar-text:       #657b83;
  --sidebar-border:     #d5ceb8;
  --header-bg:          #fdf6e3;
  --header-text:        #657b83;
  --header-border:      #d5ceb8;
}
```

### Solarized Dark

The dark counterpart to Solarized. Deep teal backgrounds, muted blue-grey text, same blue accent.

```css
[data-theme="solarized-dark"] {
  --accent:             #268bd2;
  --accent-hover:       #1a6fa8;
  --accent-dark:        #2aa198;
  --accent-dark-hover:  #1d8077;
  --page-bg:            #002b36;
  --container-bg:       #073642;
  --focus-container-bg: #0d3d4a;
  --surface-bg:         #0d4454;
  --text-active:        #839496;
  --text-secondary:     #93a1a1;
  --text-muted:         #586e75;
  --ui-chrome:          #586e75;
  --error:              #dc322f;
  --surface-border:     #1a5060;
  --surface-bottom:     #1f5c6e;
  --drag-outline:       #268bd2;
  --sidebar-bg:         #002b36;
  --sidebar-text:       #839496;
  --sidebar-border:     #073642;
  --header-bg:          #002b36;
  --header-text:        #839496;
  --header-border:      #073642;
}
```

### Corporate Blues

Clean navy and sky blue on white. The sidebar uses a dark navy chrome against the light content area.

```css
[data-theme="blues"] {
  --accent:             #176bad;
  --accent-hover:       #105a96;
  --accent-dark:        #243959;
  --accent-dark-hover:  #162540;
  --page-bg:            #ffffff;
  --container-bg:       #eaf0f7;
  --focus-container-bg: #d6e4f0;
  --surface-bg:         #ffffff;
  --text-active:        #1a2330;
  --text-secondary:     #3a5470;
  --text-muted:         #7aaac8;
  --ui-chrome:          #6ca8d2;
  --error:              #c0392b;
  --surface-border:     #96b8d4;
  --surface-bottom:     #5080a8;
  --drag-outline:       #176bad;
  --sidebar-bg:         #3a5878;
  --sidebar-text:       #deeaf5;
  --sidebar-border:     #2a4560;
  --header-bg:          #ffffff;
  --header-text:        #1a2330;
  --header-border:      #96b8d4;
}
```
