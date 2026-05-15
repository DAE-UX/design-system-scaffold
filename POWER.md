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
  - radix
  - storybook
  - workflows
  - heuristic-review
  - prototyping
author: AWS
version: 1.2.0
---

# Design System Scaffold

A design system power that separates the rules and guidance of a design system from the technology underneath it. A design team manages the standards centrally — styles, patterns, paradigms, accessibility, and composition rules — while engineering teams choose and manage their own technology stack.

## How to Use This Power

Before loading any steering file, identify the task type and load only what's needed:

| Task | Load these files |
|------|-----------------|
| Implement or modify a specific component | `steering/design-system.md` + `steering/components/[name].md` + `steering/default-theme.md` |
| Apply or validate the default theme | `steering/default-theme.md` + `steering/technical-guidelines.md` |
| Generate a new theme | `steering/workflows.md` → "Generate a New Theme" + `steering/default-theme.md` (structure reference) |
| Generate a Storybook | `steering/workflows.md` → "Generate a Storybook" + `steering/storybook.md` |
| Compose a layout or page | Relevant template file + component files for components used |
| Write UI copy or microcopy | `steering/copy-guidelines.md` + `steering/glossary.md` |
| Review against design heuristics | `steering/design-guidelines.md` + `steering/ui-guidelines.md` |
| Set up MCP connections | `steering/mcp.md` |
| Validate generated code quality | Use the Output Validation Checklist below |

**Theme selection:** Use the default theme unless the user specifies a custom theme.

**Context budget:** Load max 3–4 steering files per task. If more are needed, load sequentially as sub-tasks complete.

## Executable Workflows

| User says… | Workflow | Load |
|---|---|---|
| "generate a new theme", "create a theme for [X]" | Generate a New Theme | `steering/workflows.md` |
| "generate a Storybook", "set up Storybook" | Generate a Storybook | `steering/workflows.md` |

## Key Rules

1. Components reference CSS variables only — no hardcoded colors — because hardcoded values break theme switching and create maintenance debt when themes change.
2. Design heuristics are advisory; component specs and theme definitions are authoritative — because heuristics guide composition, not component design. Overriding specs with heuristics creates inconsistency.
3. Templates define layout structure only — no behavior, state management, or business logic — because templates are reusable blueprints; coupling logic to layout prevents reuse.
4. All components must support both light and dark modes — because users expect mode switching and single-mode components break the experience.
5. WCAG 2.2 conformance required for all components — because accessibility is a legal and ethical requirement, not optional polish.
6. Theme variable names are fixed — never rename or restructure tokens — because renaming breaks all components that reference those variables and creates silent failures.

## Output Validation Checklist

Before presenting any generated code or theme to the user, verify:

- [ ] All color values reference CSS variables — no hex, rgb, or oklch literals in component code
- [ ] Both light and dark mode values are present in any theme output
- [ ] All interactive components have focus-visible styles
- [ ] ARIA roles and labels are present on all non-obvious interactive elements
- [ ] Theme variable names exactly match the canonical list — no renames or new tokens
- [ ] Any empty sections encountered in specs were not synthesized — user was informed of the gap

## What's Included

- 57 component specs with behavior, API, variants, accessibility, HTML structure, CSS, and theme support
- 2 patterns (Data Table, Date Picker) showing functional compositions
- 4 templates (Calendar, Dashboard, Login, Sidebar) as layout blueprints
- Default theme (New York) with full light and dark mode values
- Design heuristics and UI composition guidelines
- Copywriting standards and terminology glossary
- Technical implementation guides for shadCN, Tailwind, Storybook, Motion, and Lucide
- Agent-executable workflows: Generate Theme and Generate Storybook

## Steering Files

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
- `mantine.md` — Mantine translation layer
- `pretext.md` — Pretext text measurement library
- `mcp.md` — MCP server setup
- `workflows.md` — Generate Theme and Generate Storybook workflows

## License and support

This power is licensed under MIT.

- [Privacy Policy](https://aws.amazon.com/privacy/)
- [Support](https://github.com/DAE-UX/design-system-scaffold/issues)
