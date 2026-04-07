# Theme: Default (New York)

## About

The default theme uses shadCN's New York style with the Neutral base color. It serves as the baseline for all theme generation and as the only built-in theme for external and skill-builder outputs. All values are sourced from the official shadCN theming documentation.

Source: `https://ui.shadcn.com/docs/theming`

## shadCN CSS Variables

Standard shadCN variables that components reference. These follow the `--name` / `--name-foreground` convention and are registered as Tailwind utilities via `@theme inline`.

### Core Variables

| CSS Variable | Light (oklch) | Light (hex) | Dark (oklch) | Dark (hex) |
|-------------|---------------|-------------|--------------|------------|
| `--background` | `oklch(1 0 0)` | `#ffffff` | `oklch(0.145 0 0)` | `#0a0a0a` |
| `--foreground` | `oklch(0.145 0 0)` | `#0a0a0a` | `oklch(0.985 0 0)` | `#fafafa` |
| `--card` | `oklch(1 0 0)` | `#ffffff` | `oklch(0.205 0 0)` | `#171717` |
| `--card-foreground` | `oklch(0.145 0 0)` | `#0a0a0a` | `oklch(0.985 0 0)` | `#fafafa` |
| `--popover` | `oklch(1 0 0)` | `#ffffff` | `oklch(0.269 0 0)` | `#262626` |
| `--popover-foreground` | `oklch(0.145 0 0)` | `#0a0a0a` | `oklch(0.985 0 0)` | `#fafafa` |
| `--primary` | `oklch(0.205 0 0)` | `#171717` | `oklch(0.922 0 0)` | `#e5e5e5` |
| `--primary-foreground` | `oklch(0.985 0 0)` | `#fafafa` | `oklch(0.205 0 0)` | `#171717` |
| `--secondary` | `oklch(0.97 0 0)` | `#f5f5f5` | `oklch(0.269 0 0)` | `#262626` |
| `--secondary-foreground` | `oklch(0.205 0 0)` | `#171717` | `oklch(0.985 0 0)` | `#fafafa` |
| `--muted` | `oklch(0.97 0 0)` | `#f5f5f5` | `oklch(0.269 0 0)` | `#262626` |
| `--muted-foreground` | `oklch(0.556 0 0)` | `#737373` | `oklch(0.708 0 0)` | `#a3a3a3` |
| `--accent` | `oklch(0.97 0 0)` | `#f5f5f5` | `oklch(0.371 0 0)` | `#404040` |
| `--accent-foreground` | `oklch(0.205 0 0)` | `#171717` | `oklch(0.985 0 0)` | `#fafafa` |
| `--destructive` | `oklch(0.577 0.245 27.325)` | `#dc2626` | `oklch(0.704 0.191 22.216)` | `#ef4444` |
| `--destructive-foreground` | `oklch(0.985 0 0)` | `#fafafa` | `oklch(0.985 0 0)` | `#fafafa` |
| `--border` | `oklch(0.922 0 0)` | `#e5e5e5` | `oklch(1 0 0 / 10%)` | `rgba(255,255,255,0.1)` |
| `--input` | `oklch(0.922 0 0)` | `#e5e5e5` | `oklch(1 0 0 / 15%)` | `rgba(255,255,255,0.15)` |
| `--ring` | `oklch(0.708 0 0)` | `#a3a3a3` | `oklch(0.556 0 0)` | `#737373` |

| CSS Variable | Value |
|-------------|-------|
| `--radius` | `0.625rem` |

### Chart Variables

| CSS Variable | Light (oklch) | Light (hex) | Dark (oklch) | Dark (hex) |
|-------------|---------------|-------------|--------------|------------|
| `--chart-1` | `oklch(0.646 0.222 41.116)` | `#e76e50` | `oklch(0.488 0.243 264.376)` | `#2563eb` |
| `--chart-2` | `oklch(0.6 0.118 184.704)` | `#2a9d8f` | `oklch(0.696 0.17 162.48)` | `#2eb88a` |
| `--chart-3` | `oklch(0.398 0.07 227.392)` | `#264653` | `oklch(0.769 0.188 70.08)` | `#e8a838` |
| `--chart-4` | `oklch(0.828 0.189 84.429)` | `#e9c46a` | `oklch(0.627 0.265 303.9)` | `#a855f7` |
| `--chart-5` | `oklch(0.769 0.188 70.08)` | `#e8a838` | `oklch(0.645 0.246 16.439)` | `#e54666` |

### Sidebar Variables

| CSS Variable | Light (oklch) | Light (hex) | Dark (oklch) | Dark (hex) |
|-------------|---------------|-------------|--------------|------------|
| `--sidebar` | `oklch(0.985 0 0)` | `#fafafa` | `oklch(0.205 0 0)` | `#171717` |
| `--sidebar-foreground` | `oklch(0.145 0 0)` | `#0a0a0a` | `oklch(0.985 0 0)` | `#fafafa` |
| `--sidebar-primary` | `oklch(0.205 0 0)` | `#171717` | `oklch(0.488 0.243 264.376)` | `#2563eb` |
| `--sidebar-primary-foreground` | `oklch(0.985 0 0)` | `#fafafa` | `oklch(0.985 0 0)` | `#fafafa` |
| `--sidebar-accent` | `oklch(0.97 0 0)` | `#f5f5f5` | `oklch(0.269 0 0)` | `#262626` |
| `--sidebar-accent-foreground` | `oklch(0.205 0 0)` | `#171717` | `oklch(0.985 0 0)` | `#fafafa` |
| `--sidebar-border` | `oklch(0.922 0 0)` | `#e5e5e5` | `oklch(1 0 0 / 10%)` | `rgba(255,255,255,0.1)` |
| `--sidebar-ring` | `oklch(0.708 0 0)` | `#a3a3a3` | `oklch(0.439 0 0)` | `#525252` |

### Typography Variables

| CSS Variable | Value |
|-------------|-------|
| `--font-sans` | `Geist` |
| `--font-mono` | `Geist Mono` |


---

## Tailwind Mapping

Tailwind utilities map directly to the shadCN CSS variables above via the `@theme inline` directive in shadCN v4:

| Tailwind Utility Pattern | CSS Variable | Example |
|-------------------------|-------------|---------|
| `bg-{name}` | `--{name}` | `bg-primary` → `var(--primary)` |
| `text-{name}` | `--{name}` (foreground) | `text-primary-foreground` → `var(--primary-foreground)` |
| `border-{name}` | `--{name}` | `border-border` → `var(--border)` |
| `ring-{name}` | `--{name}` | `ring-ring` → `var(--ring)` |

Source: `https://ui.shadcn.com/docs/theming`

## Global Styles

### Body

- Background: `var(--background)` — `oklch(1 0 0)` / `#ffffff` (light), `oklch(0.145 0 0)` / `#0a0a0a` (dark)
- Text color: `var(--foreground)` — `oklch(0.145 0 0)` / `#0a0a0a` (light), `oklch(0.985 0 0)` / `#fafafa` (dark)
- Font family: `var(--font-sans)` — `Geist`

### Surfaces

- Card: `oklch(1 0 0)` / `#ffffff` (light), `oklch(0.205 0 0)` / `#171717` (dark)
- Popover: `oklch(1 0 0)` / `#ffffff` (light), `oklch(0.269 0 0)` / `#262626` (dark)
- Muted surfaces: `oklch(0.97 0 0)` / `#f5f5f5` (light), `oklch(0.269 0 0)` / `#262626` (dark)
- Sidebar: `oklch(0.985 0 0)` / `#fafafa` (light), `oklch(0.205 0 0)` / `#171717` (dark)

### Typography

- Font family: Geist (sans-serif)
- Font family monospace: Geist Mono (monospace)
- Text sizes follow Tailwind scale (`text-xs` through `text-xl`)
- Font weights: `font-normal` (400), `font-medium` (500), `font-semibold` (600), `font-bold` (700)

## Usage Notes

This is the default shadCN New York theme with Neutral base color. It contains only standard shadCN contract variables — no custom extensions (status, focus, gradients, shadows, agentic). Use this as a base when generating new themes via the Generate Theme workflow.

Variable hierarchy:
1. shadCN CSS Variables — standard component theming contract
2. Tailwind utilities — mapped from shadCN variables via `@theme inline`
