---
name: ember-system
description: Apply the Ember design system to a UI. Use only when the user asks you to build or skin a UI with ember-system.
---

1. Read `${CLAUDE_SKILL_DIR}/design-guide.md` — canonical rules for components, variables, typography, and themes.
2. Read `${CLAUDE_SKILL_DIR}/ember.css` — all component CSS and the full variable reference.
3. Pick a theme file from `${CLAUDE_SKILL_DIR}/themes/` based on user preference, or ask if unclear.
4. Copy `${CLAUDE_SKILL_DIR}/ember.css` and the chosen theme file(s) into the project directory alongside the HTML file.
5. Link the copied files with relative `<link>` tags. For auto light/dark switching, copy both theme files and link each with a `media="(prefers-color-scheme: ...)"` attribute.

The generated project must be standalone — reference only files copied into the project, never paths inside `${CLAUDE_SKILL_DIR}`.

`${CLAUDE_SKILL_DIR}/design-demo.html` contains worked examples of every component. Read it if the guide and CSS alone don't make a pattern clear.
