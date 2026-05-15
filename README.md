# Design System Scaffold

## About this project

This is a design system power that separates the rules and guidance of a design system from the technology underneath it. The power contains all guidelines information needed for Kiro to construct front-end experiences in shadCN — styles, patterns, paradigms, accessibility, and composition rules — without the need to manage and maintain an underlying technology stack. This ensures standards are met: consistency, centralized solutions to common problems, and more efficient execution against user needs, without the UI framework overhead. This is a prepackaged version of the design system scaffold using a curated set of rules and standards. If you would like to build off of these standards with your own rules, standards, and technologies check out our [design system power builder](https://github.com/DAE-UX/design-system-power-builder).

Use it for prototyping, development, or as a stand-in for the design-oriented aspects of a design system.

Use it for prototyping, development, or as a stand-in for the design-oriented aspects of a design system.

## Getting started

Install as a Kiro Power:

```bash
cp -r design-system-scaffold/ ~/.kiro/powers/design-system-scaffold/
```

Or install via Kiro UI: Command Palette → "Powers: Configure" → "Install from folder".

Once installed, ask the agent to generate a theme, set up a Storybook, prototype a UI layout, look up component specs, or check accessibility requirements.

## What you can do

| Capability | Description | Example prompt |
|-----------|-------------|----------------|
| Look up component specs | Get behavior, API, variants, accessibility, and styling for any of the 57 components. | "Show me the Button component spec" |
| Generate a new theme | Create a complete theme file with all CSS variables for light and dark modes. | "Generate a new theme called finops" |
| Generate a Storybook | Set up Storybook with theme switching, story files for all components, and accessibility testing. | "Generate a Storybook for my project" |
| Prototype a UI layout | Compose pages from templates, patterns, and components with proper theming. | "Build a dashboard layout with sidebar navigation" |
| Check accessibility | Verify WCAG 2.2 conformance for components and layouts. | "Review this form for accessibility issues" |
| Write UI copy | Apply voice, tone, and microcopy standards to interface text. | "Write error messages for this form" |
| Run a design heuristic review | Evaluate UI against 54 design heuristics across 9 domains. | "Review this page against design heuristics" |

## Technologies

| Technology | Role |
|-----------|------|
| shadCN UI | Pre-styled component library (57 components) |
| Tailwind CSS v4 | Utility-first CSS framework with `@theme` design tokens |
| Radix UI | Accessible, unstyled component primitives |
| Storybook | Component development and documentation environment |
| Motion | Animation library for complex interactions |
| Lucide | Icon library (shadCN default) |
| Mantine | Translation layer for additional component patterns |
| Pretext | Text measurement library |

## Project structure

```text
design-system-scaffold/
├── POWER.md                    — Root with YAML frontmatter and routing table
├── README.md                   — This file
└── steering/
    ├── design-system.md        — Design system library (parent record + indexes)
    ├── components/             — 57 component specs
    ├── patterns/               — Data Table, Date Picker
    ├── templates/              — Calendar, Dashboard, Login, Sidebar
    ├── default-theme.md        — New York base theme (light + dark)
    ├── design-guidelines.md    — 54 design heuristics across 9 domains
    ├── ui-guidelines.md        — Tactical UI composition rules
    ├── copy-guidelines.md      — Voice, tone, writing standards
    ├── glossary.md             — Terminology definitions
    ├── technical-guidelines.md — Architecture, theming, Tailwind, breakpoints
    ├── shadcn.md               — shadCN setup and CLI
    ├── storybook.md            — Storybook generation and configuration
    ├── motion.md               — Motion animation library
    ├── lucide.md               — Lucide icon library
    ├── mantine.md              — Mantine translation layer
    ├── pretext.md              — Pretext text measurement
    ├── mcp.md                  — MCP server setup
    └── workflows.md            — Generate Theme and Generate Storybook workflows
```

## License

MIT
