# shadCN Setup and Usage — Design System Skill

> **TL;DR:** shadCN UI provides 57 pre-built components via CLI (`npx shadcn@latest`), configured through `components.json`. Not a traditional npm package — components are added directly to your project. Use `new-york` style (default is deprecated). Tailwind v4: leave `tailwind.config` blank. Supports namespaced registries for private/team components. For architecture, theming, and CSS variable details, see `technical-guidelines.md`.

## Purpose

shadCN-specific setup, CLI usage, configuration, and constraints.

**Use for:** Installing shadCN, configuring `components.json`, adding components, understanding CLI options, registry configuration.

**Do not use for:** General theming and CSS variables (see `technical-guidelines.md`), Storybook generation (see `storybook.md`), component behavior (see `design-system/components/`).

---

## Installation

shadCN is not a traditional npm package. Components are added directly to your project via CLI:

```bash
# Scaffold a new project (interactive — picks framework, icon library, theme)
npx shadcn@latest init

# Add a specific component
npx shadcn@latest add [component]

# Add multiple components
npx shadcn@latest add button card dialog
```

### Supported Frameworks

- Next.js
- Vite
- Laravel
- React Router
- Astro
- TanStack Start

---

## components.json Configuration

The `components.json` file controls how the CLI generates and places components. Optional if using copy-paste instead of CLI.

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide",
  "registries": {}
}
```

### Configuration Fields

| Field | Required | Description | Changeable After Init |
|-------|----------|-------------|-----------------------|
| `style` | Yes | Component style variant. Use `new-york` (default is deprecated). | No |
| `rsc` | Yes | React Server Components support. Adds `use client` directive when `true`. | Yes |
| `tsx` | Yes | TypeScript (`true`) or JavaScript (`false`). | Yes |
| `tailwind.config` | Yes | Path to `tailwind.config.js`. Leave blank for Tailwind v4. | Yes |
| `tailwind.css` | Yes | Path to CSS file importing Tailwind. | Yes |
| `tailwind.baseColor` | Yes | Base color palette (neutral, stone, zinc, gray, slate, mauve, olive, mist, taupe). | No |
| `tailwind.cssVariables` | Yes | `true` for CSS variable theming (recommended). | No |
| `tailwind.prefix` | No | Prefix for Tailwind utility classes (e.g., `tw-`). | Yes |
| `aliases.components` | Yes | Import alias for components directory. | Yes |
| `aliases.utils` | Yes | Import alias for utility functions. | Yes |
| `aliases.ui` | Yes | Import alias for UI components. Controls installation directory. | Yes |
| `aliases.lib` | Yes | Import alias for lib functions. | Yes |
| `aliases.hooks` | Yes | Import alias for hooks. | Yes |
| `iconLibrary` | No | Icon library to use (e.g., `lucide`). | Yes |

---

## Namespaced Registries

Multiple component registries can be configured for private or team-specific components:

```json
{
  "registries": {
    "@v0": "https://v0.dev/chat/b/{name}",
    "@acme": "https://registry.acme.com/{name}.json",
    "@private": {
      "url": "https://api.company.com/registry/{name}.json",
      "headers": {
        "Authorization": "Bearer ${REGISTRY_TOKEN}"
      }
    }
  }
}
```

Install from registries:

```bash
npx shadcn@latest add @v0/dashboard
npx shadcn@latest add @private/button
```

Environment variables in `${VAR_NAME}` format are automatically expanded.

---

## Build / Setup Steps

### Prerequisites

- Node.js (LTS recommended)
- A React framework (Next.js, Vite, React Router, Astro, Laravel, or TanStack Start)
- Package manager (npm, yarn, pnpm, or bun)

### Setup

1. Create or open your React framework project
2. Run `npx shadcn@latest init`
3. Follow interactive prompts (framework, icon library, base color, theme)
4. The CLI creates `components.json` and sets up CSS with theme variables
5. Add components: `npx shadcn@latest add accordion button card`

### Tailwind v4 Note

Tailwind CSS v4 does not use `tailwind.config.js`. Leave the `tailwind.config` field blank in `components.json`. All theme configuration happens in CSS via the `@theme` directive.

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| `style` cannot be changed after init | Must choose correctly at setup | Re-initialize project |
| `cssVariables` cannot be changed after init | Must choose correctly at setup | Delete and reinstall all components |
| `baseColor` cannot be changed after init | Affects entire color palette | Re-initialize project |
| Default style is deprecated | Must use `new-york` | Always specify `new-york` |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| shadCN UI | `https://ui.shadcn.com` | Component library, CLI, components.json |
| shadCN Installation | `https://ui.shadcn.com/docs/installation` | Setup guide |
| shadCN components.json | `https://ui.shadcn.com/docs/components-json` | Configuration reference |
