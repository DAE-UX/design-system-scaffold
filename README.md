# Design System Scaffold

## About this project

This is a design system power that separates the rules and guidance of a design system from the technology underneath it. The power contains all guidelines information needed for Kiro to construct front-end experiences in ShadCN — styles, patterns, paradigms, accessibility, and composition rules — without the need to manage and maintain an underlying technology stack. This ensure standards are met: consistency, centralized solutions to common problems, and more efficient execution against user needs, without the UI framework overhead. 

This is a prepackaged version of the design system scaffold using a curated set of rules and standards. If you would like to build off of these standards with your own rules, standards, and technologies check out our [design system power builder](https://github.com/DAE-UX/design-system-power-builder).

Use it for prototyping, development, or as a stand-in for the design-oriented aspects of a design system.

## Getting started

Install as a Kiro Power:

```bash
cp -r kiro-design-system-power/ ~/.kiro/powers/kiro-design-system-power/
```

Or install via Kiro UI: Command Palette → "Powers: Configure" → "Install from folder".

## What you can do

### Generate a theme

Create a new color theme with all the tokens your components need. The agent walks you through each variable group or accepts bulk imports from design tools or CSS files.

Trigger: "generate a new theme" or "create a theme."

### Generate a Storybook

Set up a complete Storybook with stories for every installed component, pattern, and template. Includes theme switching and light/dark mode in the toolbar.

Trigger: "generate a storybook" or "set up storybook."

### Prototype a UI layout

Describe what you need and the agent composes it from design system components using the active theme's CSS variables and design heuristics.

Trigger: Describe the UI you want to build.

### Look up component specs

Ask about any component's behavior, API, variants, accessibility requirements, or CSS variables.

Trigger: "how does the accordion work" or "what props does button have."

### Check accessibility requirements

Get WCAG 2.2 compliance guidance for any component or layout. Each component spec includes WAI-ARIA pattern references, ARIA roles, keyboard interaction rules, and contrast requirements.

Trigger: "check accessibility for dialog" or "what are the a11y requirements for tabs."

## Technologies

| Technology | How it's used |
|-----------|---------------|
| [shadCN](https://ui.shadcn.com) | Component library. All component specs are based on shadCN's API, variants, and composition patterns |
| [Tailwind CSS](https://tailwindcss.com) | Utility-first CSS framework. Theme variables map to Tailwind utilities via `@theme inline` |
| [Radix UI](https://radix-ui.com) | Headless primitives underneath shadCN components. Referenced for accessibility patterns |
| [Storybook](https://storybook.js.org) | Component development environment. The skill can generate a full Storybook from component specs |
| [Motion](https://motion.dev) | Animation library. Used when CSS transitions aren't sufficient |
| [Lucide](https://lucide.dev) | Icon library (shadCN default). Icon semantics are consistent across themes |

## Project structure

```
kiro-design-system-power/
├── POWER.md                    — Power metadata and overview
├── README.md                   — This file
└── steering/
    ├── design-system.md        — Parent record and indexes
    ├── components/             — 57 component specs
    ├── patterns/               — Data Table, Date Picker
    ├── templates/              — Calendar, Dashboard, Login, Sidebar
    ├── default-theme.md        — New York base theme (light + dark)
    ├── design-guidelines.md    — Design heuristics
    ├── ui-guidelines.md        — UI composition rules
    ├── copy-guidelines.md      — Voice, tone, writing standards
    ├── glossary.md             — Terminology
    ├── technical-guidelines.md — Architecture, theming, Tailwind
    ├── shadcn.md               — shadCN setup
    ├── storybook.md            — Storybook configuration
    ├── motion.md               — Motion animation library
    ├── lucide.md               — Lucide icon library
    ├── mcp.md                  — MCP server setup
    └── workflows.md            — Generate Theme and Generate Storybook
```

## Additional information

### Theme architecture

Components reference CSS variables (`var(--foreground)`, `var(--border)`) — never hardcoded color values. The default theme (New York) holds the resolved values. Create additional themes via the Generate Theme workflow.

### Design tool integration

You don't need a design tool to use this skill. If you use Figma, shadCN recommends several compatible component libraries — see `design-system.md` → Design Tool Libraries for the current list.

### License

MIT
