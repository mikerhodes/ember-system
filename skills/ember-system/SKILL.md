---
name: ember-system
description: Apply the Ember design system to a UI. Use when the user asks you to build or skin a UI with ember-system.
disable-model-invocation: true
---

1. Read `${CLAUDE_SKILL_DIR}/design-guide.md` — canonical rules for components, variables, typography, and themes.
2. Read `${CLAUDE_SKILL_DIR}/ember.css` — all component CSS and the full variable reference.
3. Pick a theme file from `${CLAUDE_SKILL_DIR}/themes/` based on user preference, or ask if unclear.
4. Link `ember.css` and the chosen theme file. For auto light/dark switching, link both with `media="(prefers-color-scheme: ...)"`.

`${CLAUDE_SKILL_DIR}/design-demo.html` contains worked examples of every component. Read it if the guide and CSS alone don't make a pattern clear.
