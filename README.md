# ember-system

A Claude Code skill that applies a consistent visual style to HTML tools you build with AI.

## What it is

When you ask Claude Code to build a one-off HTML tool, the result works but looks generic. This skill shows one way to fix that. Instead of writing CSS from scratch each time, Claude reads a design system — components, variables, typography rules — and applies it to whatever you're building.

I built this whole skill from a nice touch that Claude made when styling keyboard shortcuts. I really like the "raised" style, and figured I'd try to get Claude to restyle the app I was building around it. Then I realised I could reuse it by getting Claude to help me make a simple [design system](https://en.wikipedia.org/wiki/Design_system) that I could use to reskin HTML applications I made.

This repository is intended as a demonstration. You can build a skill like this for yourself, based on your tastes.

Pick websites whose colour palettes or visual style you like. Paste screenshots into Claude and ask it to help you extract a design system from them. Use that system as the basis for a skill. Every HTML tool you build after that will look the way you want it to — without any styling work on your part.

## This skill's style

You might of course like this style too, in which case, you can reuse my work!

![Umber theme](demo-umber.png)
*Umber theme — warm terracotta on cream*

![Corporate Blues theme](demo-blues.png)
*Corporate Blues theme — navy sidebar, white content area*

## Using the skill

Copy `skills/ember-system` into `~/.claude/skills/`:

```sh
cp -r skills/ember-system ~/.claude/skills/ember-system
```

Then ask Claude to build or style something:

> Build a task tracker. Use ember-system with the Umber theme.

Claude will read the design guide, pick up the CSS, and produce styled HTML that links `ember.css` and the chosen theme file.

## What's in the skill

```
skills/ember-system/
├── SKILL.md          # Instructions Claude reads to invoke the skill
├── ember.css         # All component CSS
├── design-guide.md   # Canonical rules for components, variables, and layout
├── design-demo.html  # Worked examples of every component
└── themes/
    ├── umber-light.css
    ├── umber-dark.css
    ├── solarized-light.css
    ├── solarized-dark.css
    ├── corporate-blues-light.css
    └── corporate-blues-dark.css
```

`ember.css` contains no colour values — all colours come from CSS custom properties that the theme files set on `:root`. To add a new theme, create a file that defines those variables. The design guide lists every variable and its role.
