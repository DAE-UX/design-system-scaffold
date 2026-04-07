# Lucide Icons — Design System Skill

> **TL;DR:** Lucide React (`lucide-react`) is the icon library for this design system. It is shadCN's default icon library — configured via `iconLibrary: "lucide"` in `components.json`. Icons are tree-shakable React components that render inline SVGs. Use `currentColor` for theme-aware coloring (inherits from parent text color via CSS variables). Icons are decorative by default (`aria-hidden="true"`); add `aria-label` or `<title>` only when an icon conveys essential meaning on its own. See the Icon Semantics Map (TODO) for consistent icon-to-function mapping across the system.

## Purpose

Icon library reference and usage guidance for agents generating UI with the design system.

**Use for:** Selecting icons for components, understanding icon props, applying consistent icon semantics, accessibility patterns.

**Do not use for:** Component behavior (see `design-system/components/`), animation (see `motion.md`), theme colors (see `themes/`).

---

## Installation

```bash
npm install lucide-react
```

shadCN projects already include `lucide-react` when initialized with `iconLibrary: "lucide"` in `components.json`.

Source: `https://lucide.dev/guide/packages/lucide-react`

---

## Core Usage

### Import and Render

Each icon is a standalone React component. Only imported icons are bundled (tree-shakable).

```tsx
import { Camera, ChevronRight, MoreHorizontal } from "lucide-react"

function Example() {
  return (
    <div>
      <Camera />
      <ChevronRight />
      <MoreHorizontal />
    </div>
  )
}
```

### Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `size` | number | `24` | Width and height in pixels |
| `color` | string | `currentColor` | Stroke color — inherits from parent by default |
| `strokeWidth` | number | `2` | SVG stroke width |
| `absoluteStrokeWidth` | boolean | `false` | When true, stroke width stays constant regardless of icon size |

All standard SVG presentation attributes are also accepted as props.

```tsx
<Camera size={48} color="red" strokeWidth={1} />
```

### Theme-Aware Coloring

Icons use `currentColor` by default, which means they inherit the text color of their parent element. This integrates naturally with the design system's CSS variable theming:

```tsx
{/* Icon inherits --foreground color */}
<div className="text-foreground">
  <Camera />
</div>

{/* Icon inherits --muted-foreground color */}
<div className="text-muted-foreground">
  <Camera />
</div>

{/* Icon inherits --destructive color */}
<div className="text-destructive">
  <AlertTriangle />
</div>
```

Do NOT hardcode icon colors. Let them inherit from the parent's text color class so they respond to theme changes automatically.

---

## Global Styling

### CSS Approach

All Lucide icons have the class `lucide`. Target this class for global icon styling:

```css
.lucide {
  width: 16px;
  height: 16px;
  stroke-width: 1.5;
}
```

### Context Provider Approach

Use `LucideProvider` to set defaults for all child icons while still allowing per-icon overrides:

```tsx
import { LucideProvider, Home, Settings } from "lucide-react"

<LucideProvider size={20} strokeWidth={1.5}>
  <Home />           {/* 20px, strokeWidth 1.5 */}
  <Settings size={24} /> {/* 24px override, strokeWidth 1.5 */}
</LucideProvider>
```

### Recommended Defaults for This Design System

| Context | Size | Stroke Width | Rationale |
|---------|------|-------------|-----------|
| Inline with text (buttons, badges, breadcrumbs) | `16` | `2` | Matches shadCN component defaults |
| Standalone icons (empty states, navigation) | `24` | `2` | Default Lucide size |
| Large decorative (hero, empty state media) | `48` | `1.5` | Reduced stroke for visual balance at large sizes |

---

## Accessibility

Icons ship with `aria-hidden="true"` by default. This is correct for most cases — icons are decorative.

### When Icons Are Decorative (Most Cases)

No action needed. The icon is hidden from assistive technology.

```tsx
<button>
  <Camera /> {/* Decorative — button text provides meaning */}
  Take Photo
</button>
```

### When Icons Are the Only Content

If an icon is the sole content of an interactive element, add an accessible label to the parent:

```tsx
<button aria-label="Take photo">
  <Camera /> {/* No visible text — label on button */}
</button>
```

### When Icons Convey Essential Meaning Alone

Rare case — add `aria-label` or a `<title>` child to the icon itself:

```tsx
<AlertTriangle aria-label="Warning" />

{/* or */}

<AlertTriangle>
  <title>Warning</title>
</AlertTriangle>
```

---

## Icon Selection Rules

### Priority Chain

1. Check the Icon Semantics Map (TODO — see below) for the standard icon assigned to the function
2. If no mapping exists, search the Lucide library at `https://lucide.dev/icons/` for the most semantically appropriate icon
3. Prefer icons that are already used elsewhere in the design system for similar functions (consistency over novelty)
4. When multiple icons could work, choose the simplest and most universally recognizable one

### Consistency Rules

- The same function MUST use the same icon across all themes and all components
- Do NOT vary icons by theme — icon semantics are theme-agnostic
- If a shadCN component spec names a specific Lucide icon (e.g., `ChevronRight` for Breadcrumb separator), use that icon — the component spec is authoritative

### Icons Already Used in shadCN Components

These icons are referenced in component specs and should not be substituted:

| Icon | Component | Usage |
|------|-----------|-------|
| `ChevronRight` | Breadcrumb | Separator between items |
| `MoreHorizontal` | Data Table, Dropdown | Row actions trigger |
| `CalendarIcon` | Date Picker | Trigger button icon |
| `PanelLeftIcon` | Sidebar | Toggle button |
| `Loader2` | Spinner | Loading indicator (with `animate-spin`) |
| `Cross` / `X` | Dialog, Sheet | Close button |
| `Check` | Checkbox | Checked state indicator |
| `CircleCheck` | Sonner | Success toast icon |
| `Info` | Sonner | Info toast icon |
| `TriangleAlert` | Sonner, Alert | Warning/error icon |
| `Monitor` | Chart | Chart config icon example |
| `Bold`, `Italic`, `Underline` | Toggle Group | Text formatting toggles |

---

## Icon Semantics Map

EMPTY: None recorded.

This section will define the standard icon-to-function mapping for the design system, ensuring consistent symbol language across all themes and components. The map should cover:

- Navigation actions (home, back, forward, menu, close, search)
- CRUD operations (create, read, edit, delete, save, cancel)
- Status indicators (success, warning, error, info, loading)
- Common UI actions (filter, sort, expand, collapse, refresh, settings, share, download, upload)
- User actions (profile, logout, notifications, help)
- Data actions (copy, paste, link, unlink, archive)

Each entry should specify: function name, assigned Lucide icon, usage context, and any component-specific overrides.

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Icons are SVG-based | Cannot use icon fonts | This is intentional — SVGs are more accessible and tree-shakable |
| `currentColor` inheritance | Icon color depends on parent context | Ensure parent has appropriate text color class |
| CSS global styling overrides props | `size`/`color` props won't work if CSS targets `.lucide` | Use `LucideProvider` instead of CSS for global styling when per-icon overrides are needed |
| No filled variants in core | Some icons need filled state (e.g., favorited) | Use `lucide-lab` for filled variants or apply `fill="currentColor"` manually |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| Lucide React | `https://lucide.dev/guide/packages/lucide-react` | Overview, features |
| Lucide Getting Started | `https://lucide.dev/guide/react/getting-started` | Installation, props, basic usage |
| Lucide Accessibility | `https://lucide.dev/guide/react/advanced/accessibility` | aria-hidden, accessible labels |
| Lucide Global Styling | `https://lucide.dev/guide/react/advanced/global-styling` | CSS and LucideProvider approaches |
| Lucide Icon Search | `https://lucide.dev/icons/` | Browse and search all available icons |
