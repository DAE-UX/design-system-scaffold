---
name: design-system-scaffold
displayName: Design System Power
description: Modular design system reference for component specs, theming, accessibility guidance, UI composition, and agent-executable workflows. Use for prototyping, development, or as a stand-in for the design-oriented aspects of a design system. Separates design rules from underlying technology so teams can manage standards centrally while engineering chooses their own stack.
keywords:
  - design-system
  - shadcn
  - tailwind
  - components
  - accessibility
  - theming
  - ui
  - storybook
  - prototyping
version: 1.1.0
author: AWS
---

# Design System Power

A design system skill that separates the rules and guidance of a design system from the technology underneath it. A design team manages the standards centrally — styles, patterns, paradigms, accessibility, and composition rules — while engineering teams choose and manage their own technology stack.

## What's included

- 57 component specs with behavior, API, variants, accessibility, HTML structure, CSS, and theme support
- 2 patterns (Data Table, Date Picker) showing functional compositions
- 4 templates (Calendar, Dashboard, Login, Sidebar) as layout blueprints
- Default theme (New York) with full light and dark mode values
- Design heuristics and UI composition guidelines
- Copywriting standards and terminology glossary
- Technical implementation guides for shadCN, Tailwind, Storybook, Motion, and Lucide
- Agent-executable workflows: Generate Theme and Generate Storybook

## How to use

Install as a Kiro Power:

```bash
cp -r kiro-design-system-power/ ~/.kiro/powers/kiro-design-system-power/
```

Or install via Kiro UI: Command Palette → "Powers: Configure" → "Install from folder".

Once installed, ask the agent to generate a theme, set up a Storybook, prototype a UI layout, look up component specs, or check accessibility requirements.

## Steering files

All content is in the `steering/` directory:

- `design-system.md` — Parent record and component/pattern/template indexes
- `components/` — 57 component specs
- `patterns/` — Data Table and Date Picker
- `templates/` — Calendar, Dashboard, Login, Sidebar
- `default-theme.md` — New York base theme
- `design-guidelines.md` — Design heuristics
- `ui-guidelines.md` — UI composition rules
- `copy-guidelines.md` — Voice, tone, writing standards
- `glossary.md` — Terminology
- `technical-guidelines.md` — Architecture, theming, Tailwind
- `shadcn.md` — shadCN setup
- `storybook.md` — Storybook configuration
- `motion.md` — Motion animation library
- `lucide.md` — Lucide icon library
- `mcp.md` — MCP server setup
- `workflows.md` — Generate Theme and Generate Storybook workflows

## License and support

This power is licensed under MIT.

- [Privacy Policy](https://aws.amazon.com/privacy/)
- [Support](https://github.com/DAE-UX/design-system-scaffold/issues)
