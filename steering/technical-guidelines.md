# Technical Guidelines — Design System Skill

> **TL;DR:** Framework-agnostic technical rules for the design system. Architecture stack: CSS variables (theming layer) → Tailwind CSS v4 (utility framework) → Radix UI (accessible primitives) → shadCN UI (pre-styled components) → your application. Theming uses OKLCH CSS variables in `:root` / `.dark` with background/foreground naming convention. Tailwind v4 uses `@theme` directive for design tokens. Breakpoints: 8-tier scale from 320px (xs) to 2560px (4xl). All components must support light/dark modes via CSS variables — no hardcoded colors.

## Purpose

Framework-agnostic technical rules, architecture patterns, and configuration standards for the design system.

**Use for:** Understanding the architecture stack, theming conventions, breakpoint system, CSS variable patterns, and build/setup requirements.

**Do not use for:** shadCN-specific setup and CLI (see `shadcn.md`), Storybook generation (see `storybook.md`), component behavior (see `design-system/components/`), theme values (see `themes/`).

---

## Architecture Stack

```
┌─────────────────────────────────┐
│  Your Application               │
├─────────────────────────────────┤
│  shadCN UI Components           │  ← Pre-styled, composable
│  (Tailwind classes + CSS vars)  │
├─────────────────────────────────┤
│  Radix UI Primitives            │  ← Accessible, unstyled
│  (Behavior + ARIA)              │
├─────────────────────────────────┤
│  Tailwind CSS v4                │  ← Utility framework
│  (@theme design tokens)         │
├─────────────────────────────────┤
│  CSS Variables                  │  ← Theming layer
│  (:root / .dark)                │
└─────────────────────────────────┘
```

---

## Theming Implementation

Source: `https://ui.shadcn.com/docs/theming`

### CSS Variable Convention

shadCN uses a background/foreground naming convention:

- `--primary` → background color of the component
- `--primary-foreground` → text color on that background

The background suffix is omitted. Usage:

```html
<div class="bg-primary text-primary-foreground">Hello</div>
```

### Color Format

shadCN v4 uses OKLCH color format for all CSS variables:

```css
--primary: oklch(0.205 0 0);
--primary-foreground: oklch(0.985 0 0);
```

### Variable Placement

Variables are defined in `:root` (light mode) and `.dark` (dark mode):

```css
:root {
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
}
```

### Register Variables as Tailwind Utilities

Use the `@theme inline` directive to make CSS variables available as Tailwind utility classes:

```css
@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
}
```

The `inline` keyword is required because these reference other CSS variables.

### Add New Colors

1. Define the variable in `:root` and `.dark`
2. Register with `@theme inline`

```css
:root {
  --warning: oklch(0.84 0.16 84);
  --warning-foreground: oklch(0.28 0.07 46);
}

.dark {
  --warning: oklch(0.41 0.11 46);
  --warning-foreground: oklch(0.99 0.02 95);
}

@theme inline {
  --color-warning: var(--warning);
  --color-warning-foreground: var(--warning-foreground);
}
```

### Base Color Options

| Base Color | Description |
|------------|-------------|
| Neutral | Pure grayscale (no hue) |
| Stone | Warm gray with slight brown undertone |
| Zinc | Cool gray with slight blue undertone |
| Mauve | Gray with purple undertone |
| Olive | Gray with green undertone |
| Mist | Gray with blue-green undertone |
| Taupe | Gray with warm brown undertone |

### Variable Reference (Default Neutral, OKLCH)

| CSS Variable | Light (`:root`) | Dark (`.dark`) |
|-------------|-----------------|----------------|
| `--radius` | `0.625rem` | `0.625rem` |
| `--background` | `oklch(1 0 0)` | `oklch(0.145 0 0)` |
| `--foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| `--card` | `oklch(1 0 0)` | `oklch(0.205 0 0)` |
| `--card-foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| `--popover` | `oklch(1 0 0)` | `oklch(0.269 0 0)` |
| `--popover-foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| `--primary` | `oklch(0.205 0 0)` | `oklch(0.922 0 0)` |
| `--primary-foreground` | `oklch(0.985 0 0)` | `oklch(0.205 0 0)` |
| `--secondary` | `oklch(0.97 0 0)` | `oklch(0.269 0 0)` |
| `--secondary-foreground` | `oklch(0.205 0 0)` | `oklch(0.985 0 0)` |
| `--muted` | `oklch(0.97 0 0)` | `oklch(0.269 0 0)` |
| `--muted-foreground` | `oklch(0.556 0 0)` | `oklch(0.708 0 0)` |
| `--accent` | `oklch(0.97 0 0)` | `oklch(0.371 0 0)` |
| `--accent-foreground` | `oklch(0.205 0 0)` | `oklch(0.985 0 0)` |
| `--destructive` | `oklch(0.577 0.245 27.325)` | `oklch(0.704 0.191 22.216)` |
| `--destructive-foreground` | `oklch(0.985 0 0)` | `oklch(0.985 0 0)` |
| `--border` | `oklch(0.922 0 0)` | `oklch(1 0 0 / 10%)` |
| `--input` | `oklch(0.922 0 0)` | `oklch(1 0 0 / 15%)` |
| `--ring` | `oklch(0.708 0 0)` | `oklch(0.556 0 0)` |
| `--chart-1` | `oklch(0.646 0.222 41.116)` | `oklch(0.488 0.243 264.376)` |
| `--chart-2` | `oklch(0.6 0.118 184.704)` | `oklch(0.696 0.17 162.48)` |
| `--chart-3` | `oklch(0.398 0.07 227.392)` | `oklch(0.769 0.188 70.08)` |
| `--chart-4` | `oklch(0.828 0.189 84.429)` | `oklch(0.627 0.265 303.9)` |
| `--chart-5` | `oklch(0.769 0.188 70.08)` | `oklch(0.645 0.246 16.439)` |
| `--sidebar` | `oklch(0.985 0 0)` | `oklch(0.205 0 0)` |
| `--sidebar-foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| `--sidebar-primary` | `oklch(0.205 0 0)` | `oklch(0.488 0.243 264.376)` |
| `--sidebar-primary-foreground` | `oklch(0.985 0 0)` | `oklch(0.985 0 0)` |
| `--sidebar-accent` | `oklch(0.97 0 0)` | `oklch(0.269 0 0)` |
| `--sidebar-accent-foreground` | `oklch(0.205 0 0)` | `oklch(0.985 0 0)` |
| `--sidebar-border` | `oklch(0.922 0 0)` | `oklch(1 0 0 / 10%)` |
| `--sidebar-ring` | `oklch(0.708 0 0)` | `oklch(0.439 0 0)` |

Source: `https://ui.shadcn.com/docs/theming` — default Neutral base color values.

---

## Tailwind CSS v4 Configuration

Source: `https://tailwindcss.com/docs/theme`

### The @theme Directive

Tailwind CSS v4 uses the `@theme` directive to define design tokens as theme variables that generate corresponding utility classes.

```css
@import "tailwindcss";

@theme {
  --color-mint-500: oklch(0.72 0.11 178);
}
```

This creates utility classes (`bg-mint-500`, `text-mint-500`) and a CSS variable (`var(--color-mint-500)`).

### @theme vs :root

| Directive | Purpose | Generates Utilities |
|-----------|---------|--------------------:|
| `@theme` | Design tokens that map to utility classes | Yes |
| `:root` | Regular CSS variables without utility class generation | No |

### Theme Variable Namespaces

| Namespace | Utility Classes | Example |
|-----------|----------------|---------|
| `--color-*` | Color utilities | `bg-red-500`, `text-sky-300` |
| `--font-*` | Font family | `font-sans` |
| `--text-*` | Font size | `text-xl` |
| `--font-weight-*` | Font weight | `font-bold` |
| `--tracking-*` | Letter spacing | `tracking-wide` |
| `--leading-*` | Line height | `leading-tight` |
| `--breakpoint-*` | Responsive variants | `sm:*` |
| `--container-*` | Container queries + sizing | `@sm:*`, `max-w-md` |
| `--spacing-*` | Spacing and sizing | `px-4`, `max-h-16` |
| `--radius-*` | Border radius | `rounded-sm` |
| `--shadow-*` | Box shadow | `shadow-md` |
| `--inset-shadow-*` | Inset box shadow | `inset-shadow-xs` |
| `--drop-shadow-*` | Drop shadow filter | `drop-shadow-md` |
| `--blur-*` | Blur filter | `blur-md` |
| `--perspective-*` | Perspective | `perspective-near` |
| `--aspect-*` | Aspect ratio | `aspect-video` |
| `--ease-*` | Transition timing | `ease-out` |
| `--animate-*` | Animation | `animate-spin` |

### Customization Patterns

```css
/* Extend: add new variables alongside defaults */
@theme { --font-script: Great Vibes, cursive; }

/* Override a single value */
@theme { --breakpoint-sm: 30rem; }

/* Override entire namespace (clear defaults first) */
@theme { --color-*: initial; --color-white: #fff; --color-primary: #3f3cbb; }

/* Fully custom theme (clear all defaults) */
@theme { --*: initial; --spacing: 4px; --font-body: Inter, sans-serif; }
```

### @theme Variants

| Variant | Behavior | Use Case |
|---------|----------|----------|
| `@theme` | Generates utilities for used variables only | Default |
| `@theme inline` | Inlines variable value instead of referencing | When referencing other CSS variables |
| `@theme static` | Generates CSS variables for all defined variables | When all variables must be available |

### Share Themes Across Projects

```css
/* packages/brand/theme.css */
@theme { --*: initial; --spacing: 4px; --color-lagoon: oklch(0.72 0.11 221.19); }

/* project CSS */
@import "tailwindcss";
@import "../brand/theme.css";
```

---

## Radix UI Primitives

Source: `https://www.radix-ui.com/primitives/docs/overview/getting-started`

```bash
npm install radix-ui@latest
```

Radix components use a compound component pattern:

```jsx
import { Popover } from "radix-ui";

const PopoverDemo = () => (
  <Popover.Root>
    <Popover.Trigger>More info</Popover.Trigger>
    <Popover.Portal>
      <Popover.Content>
        Some content
        <Popover.Arrow />
      </Popover.Content>
    </Popover.Portal>
  </Popover.Root>
);
```

Radix primitives are unstyled, accessible (WAI-ARIA), controllable (controlled + uncontrolled), and customizable.

### How shadCN Wraps Radix

shadCN adds: Tailwind CSS classes, CSS variable references for theming, composition patterns with defaults, TypeScript types and forwarded refs.

### Controlled vs Uncontrolled

| Pattern | Usage | When to Use |
|---------|-------|-------------|
| Uncontrolled | `<Accordion.Root defaultValue="item-1">` | Simple cases, no external state |
| Controlled | `<Accordion.Root value={value} onValueChange={setValue}>` | Parent needs to control state |

---

## Breakpoints

Source: shadCN breakpoint documentation

### Breakpoint Scale

| Breakpoint | Width | Tailwind Variable | Device Context |
|------------|-------|-------------------|----------------|
| `xs` | `320px` | `--breakpoint-xs` | Smallest phones in portrait |
| `sm` | `544px` | `--breakpoint-sm` | Large phones landscape; small tablets portrait |
| `md` | `768px` | `--breakpoint-md` | Tablets in portrait |
| `lg` | `1012px` | `--breakpoint-lg` | Tablets in landscape |
| `xl` | `1280px` | `--breakpoint-xl` | Small desktops / laptops |
| `2xl` | `1400px` | `--breakpoint-2xl` | Standard desktops (1366–1440px) |
| `3xl` | `1600px` | `--breakpoint-3xl` | Large desktops |
| `4xl` | `2560px` | `--breakpoint-4xl` | Ultra-wide / QHD monitors |

### Tailwind Configuration

```css
@theme {
  --breakpoint-xs: 320px;
  --breakpoint-sm: 544px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1012px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1400px;
  --breakpoint-3xl: 1600px;
  --breakpoint-4xl: 2560px;
}
```

### Device Class Reference

| Device Class | Portrait Range | Landscape Range | Breakpoints |
|-------------|---------------|-----------------|-------------|
| Phones | 320–414px | 568–915px | `xs` (portrait), `sm` (landscape) |
| Tablets | 768–820px | 1024–1366px | `md` (portrait), `lg` / `xl` (landscape) |
| Desktops | 1280px+ | — | `xl`, `2xl`, `3xl`, `4xl` |

---

## CSS File Structure

After initialization, your CSS file contains:

```css
@import "tailwindcss";

:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  /* ... all light mode variables */
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  /* ... all dark mode variables */
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  /* ... utility registrations */
}
```

---

## Known Constraints

### Tailwind v4

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| `tailwind.config.js` not used | Theme config moves to CSS | Use `@theme` directive |
| `@theme` must be top-level | Cannot nest under selectors | Define all at root level |
| Only used variables generated by default | Unused variables not in output | Use `@theme static` |
| `@theme inline` required for var references | Variable resolution may fail | Always use `inline` with `var()` |

### Radix UI

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Primitives are unstyled | Must add all visual styling | shadCN provides styling layer |
| Compound component pattern required | Cannot use flat API | Follow `Root > Trigger > Content` |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| shadCN UI | `https://ui.shadcn.com` | Theming, CSS variable convention |
| Tailwind CSS | `https://tailwindcss.com` | @theme directive, design tokens, utility classes |
| Radix UI | `https://www.radix-ui.com` | Accessible primitives, component architecture |
