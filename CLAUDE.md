# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## CRITICAL: Only edit files in this repository

When working in this project, **only read and edit files in this repository**. Never read or touch the installed skill at `~/.claude/skills/ember-system/` — it may have a different version of the files and is not what we are developing here.

## What this project is

`ember-system` is a Claude Code skill — a reusable design system for styling plain HTML/CSS/JS applications. It ships a retro-raised style design system as an artifact to be copied into projects and applied by AI agents.

There is no build system, no package manager, no test suite, and no CI. The project is pure HTML/CSS files and Markdown documentation.

## File layout

```
skills/ember-system/
├── SKILL.md           # Claude Code skill definition (how agents invoke the skill)
├── design-guide.md    # Canonical reference for all design decisions and component patterns
├── design-demo.html   # Browser-viewable demo of every component + live theme switcher
├── ember.css          # All component CSS; uses only CSS custom properties, no hardcoded colors
└── themes/
    ├── umber-light.css / umber-dark.css         # Warm terracotta on cream (reference theme)
    ├── solarized-light.css / solarized-dark.css # Based on Ethan Schoonover's Solarized palette
    └── corporate-blues-light.css / corporate-blues-dark.css  # Navy/sky blue; dark sidebar variant
```

## How the skill works

When an agent applies ember-system to a project:
1. Read `design-guide.md` and `ember.css` to understand the system.
2. Copy `ember.css` and the chosen theme file(s) into the target project directory.
3. Link them with relative `<link>` tags — **never** reference the skill directory path.
4. For automatic light/dark switching, copy both variants and use `media="(prefers-color-scheme: ...)"`.

The generated project must be standalone — no runtime references to `${CLAUDE_SKILL_DIR}` or any path outside the project.

## CSS architecture

All component CSS references CSS custom properties exclusively — no hardcoded colors. Theme files define every variable on `:root`. Theme switching is drop-in with no component changes.

**Key variable categories:** `--accent-*`, `--page-bg` / `--container-bg` / `--surface-bg`, `--text-active` / `--text-secondary` / `--text-muted` / `--ui-chrome`, `--surface-border` / `--surface-bottom`, `--sidebar-*`, `--header-*`, semantic status (`--error`, `--warning`, `--success`, `--info`).

**Critical light/dark asymmetry:** `--surface-bottom` is darker than `--surface-bg` in light themes (shadow edge) but lighter in dark themes (highlight edge). `--accent-dark` follows the same inversion. This is intentional — see `design-guide.md`.

**Invariant:** `--surface-bg` must always be lighter than `--container-bg` so cards lift above panels.

## Depth technique

Raised surfaces use thick bottom borders — not box-shadow — to simulate physical keycap depth:

```css
border-bottom: 3px solid var(--surface-bottom);
```

Buttons get a pressed state with `border-bottom-width: 1px; transform: translateY(2px)` on `:active`.

Flat/recessed elements (form inputs, panels) use uniform 1px borders with no thick bottom.

## Adding or modifying themes

A theme file sets all CSS custom properties on `:root`. Use an existing theme as a template. The `design-guide.md` documents what each variable controls.
