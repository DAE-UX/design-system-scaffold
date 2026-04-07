# Component: Badge

> **TL;DR:** A small status descriptor for UI elements. Single part (`Badge`) with 6 variants: default, secondary, destructive, outline, ghost, link. Uses CVA (class-variance-authority) for variant management. Supports `asChild` via Radix Slot for rendering as links. Accepts icons and spinners as children. No interactive state — purely visual.

## Metadata

- **Component Name:** Badge
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui (Slot only, for `asChild`), class-variance-authority

## Behavior

Displays a small status descriptor or label. Used for tags, statuses, counts, and categorization.

- Static display component — no interactive state changes
- 6 visual variants: `default`, `secondary`, `destructive`, `outline`, `ghost`, `link`
- Renders as a `span` by default; renders as child element when `asChild` is true (e.g., for links)
- When rendered as a link (`<a>`), hover styles are applied automatically via `[a&]:hover:` selectors
- Accepts icons as children — SVGs are automatically sized to 12px (`size-3`)
- Supports focus-visible ring for keyboard navigation
- Supports `aria-invalid` state for error indication
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use badges to label, categorize, or organize using text or numbers
- Place badges in proximity to the item they relate to
- Use consistent color coding across the application

#### Don't
- Don't include icons or imagery in badges — only text and numbers
- Don't use badges as interactive elements (they are static)
- Don't rely on color alone to indicate severity — supplement with text
- Don't use badges for "new feature" labels

Sources: shadCN, Radix UI

## API

### Parts

- `Badge` — Single-part component. Renders a `span` (or child element via `asChild`). Uses CVA for variant styling.

### Props

#### Badge

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "secondary" \| "destructive" \| "outline" \| "ghost" \| "link"` | `"default"` | Visual style variant |
| className | `string` | — | Additional CSS classes for custom styling |
| asChild | `boolean` | `false` | Render as child element (e.g., `<a>`) instead of `<span>` |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Primary background with primary foreground text | Primary status, active tags |
| `secondary` | Secondary background with secondary foreground text | Neutral tags, categories |
| `destructive` | Destructive background with white text | Error states, critical counts |
| `outline` | Border with foreground text, transparent background | Subtle tags, filter chips |
| `ghost` | No background or border, transparent | Minimal visual weight |
| `link` | Primary text with underline on hover | Clickable badge links |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"badge"` | Badge |
| `[data-variant]` | `"default" \| "secondary" \| "destructive" \| "outline" \| "ghost" \| "link"` | Badge |

### CSS Variables (Radix)

Not applicable — Badge does not use Radix primitives (only Radix Slot for `asChild`).

## Composition Patterns

### Basic Variants

```tsx
<Badge>Default</Badge>
<Badge variant="secondary">Secondary</Badge>
<Badge variant="destructive">Destructive</Badge>
<Badge variant="outline">Outline</Badge>
<Badge variant="ghost">Ghost</Badge>
```

### With Icon

```tsx
<Badge variant="secondary">
  <BadgeCheckIcon />
  Verified
</Badge>
```

### As Link

```tsx
<Badge asChild>
  <a href="/docs">
    Documentation
    <ArrowUpRightIcon />
  </a>
</Badge>
```

### Count Badge (Pill)

```tsx
<Badge className="h-5 min-w-5 rounded-full px-1 font-mono tabular-nums">
  8
</Badge>
```

### Custom Colors

```tsx
<Badge className="bg-green-50 text-green-700 dark:bg-green-800 dark:text-green-100">
  Active
</Badge>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Badge is a presentational/status component

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Badge | No implicit role | Rendered as `span`. When used as a link (`asChild` with `<a>`), inherits link semantics. |
| Badge (invalid) | `aria-invalid` | Triggers destructive ring styling for error indication |

### Keyboard Behavior

No keyboard interaction when rendered as `span`. When rendered as a link via `asChild`, standard link keyboard behavior applies. Focus-visible ring appears on keyboard focus.

## HTML

### Standalone HTML Structure

```html
<span data-slot="badge" data-variant="default">Badge text</span>
```

### With Icon

```html
<span data-slot="badge" data-variant="secondary">
  <svg><!-- icon --></svg>
  Badge text
</span>
```

## CSS

### Raw CSS

```css
.Badge {
  display: inline-flex;
  width: fit-content;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  gap: 0.25rem;
  overflow: hidden;
  border-radius: 9999px;
  border: 1px solid transparent;
  padding: 0.125rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 500;
  white-space: nowrap;
  transition: color, box-shadow;
}
.Badge > svg { pointer-events: none; width: 0.75rem; height: 0.75rem; }
.Badge:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}
.Badge[data-variant="default"] { background-color: var(--primary); color: var(--primary-foreground); }
.Badge[data-variant="secondary"] { background-color: var(--secondary); color: var(--secondary-foreground); }
.Badge[data-variant="destructive"] { background-color: var(--destructive); color: white; }
.Badge[data-variant="outline"] { border-color: var(--border); color: var(--foreground); }
.Badge[data-variant="link"] { color: var(--primary); text-underline-offset: 4px; }
```

### Tailwind Mapping

| Element / Variant | Tailwind Classes | Purpose |
|-------------------|-----------------|---------|
| Badge (base) | `inline-flex w-fit shrink-0 items-center justify-center gap-1 overflow-hidden rounded-full border border-transparent px-2 py-0.5 text-xs font-medium whitespace-nowrap` | Base layout |
| default | `bg-primary text-primary-foreground [a&]:hover:bg-primary/90` | Primary colors |
| secondary | `bg-secondary text-secondary-foreground [a&]:hover:bg-secondary/90` | Secondary colors |
| destructive | `bg-destructive text-white [a&]:hover:bg-destructive/90` | Destructive colors |
| outline | `border-border text-foreground [a&]:hover:bg-accent [a&]:hover:text-accent-foreground` | Border with hover |
| ghost | `[a&]:hover:bg-accent [a&]:hover:text-accent-foreground` | Transparent with hover |
| link | `text-primary underline-offset-4 [a&]:hover:underline` | Text with underline |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--primary` | Default variant background, link variant text | `shadcn` |
| `--primary-foreground` | Default variant text | `shadcn` |
| `--secondary` | Secondary variant background | `shadcn` |
| `--secondary-foreground` | Secondary variant text | `shadcn` |
| `--destructive` | Destructive variant background, invalid state ring | `shadcn` |
| `--border` | Outline variant border | `shadcn` |
| `--foreground` | Outline variant text | `shadcn` |
| `--accent` | Outline/ghost hover background | `shadcn` |
| `--accent-foreground` | Outline/ghost hover text | `shadcn` |
| `--ring` | Focus-visible ring | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Secondary, Outline, Destructive |
| Number | True, False |

Total: 8 variant combinations (4 types × 2 number modes)

Default layout: flex row (gap 4px, px 8px, py 2px, rounded from `--radius`, overflow clip)
Optional left icon (12×12px) + Label text (text-xs, font-medium, truncate) + Optional right icon (12×12px)
Number variants render as compact pill badges (no label text, icon-only or count display).

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable / Tailwind Utility | Purpose |
|-------|--------------------------------|---------|
| `var(--primary)` | `--primary` / `bg-primary` | Default variant background |
| `var(--primary-foreground)` | `--primary-foreground` / `text-primary-foreground` | Default variant text color |
| `--radius` (derived) | `rounded-md` | Badge border radius |
| `px-2` (8px) | `px-2` | Horizontal padding |
| `py-0.5` (2px) | `py-0.5` | Vertical padding |
| `gap-1` (4px) | `gap-1` | Gap between icon and label |
| `font-sans` | `--font-sans` / `font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Label font weight |
| `text-xs` (12px) | `text-xs` | Label font size |

### Theme Behavior

- Default variant uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Secondary variant uses `--secondary` / `--secondary-foreground` — adapts to light/dark via theme
- Destructive variant uses `--destructive` with `text-white`; in dark mode uses reduced opacity
- Outline variant uses `--border` / `--foreground` with `--accent` hover — adapts to light/dark via theme
- Ghost and link variants have no background — inherit from parent
- Focus ring uses `--ring` — adapts to light/dark via theme
- Custom colors via className override both light and dark values
