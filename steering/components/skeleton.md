# Component: Skeleton

> **TL;DR:** A placeholder loading indicator. 1 part: Skeleton. No external primitive — pure shadCN. Renders a pulsing rounded rectangle via `animate-pulse` with `bg-accent`. Used as a content placeholder while data is loading.

## Metadata

- **Component Name:** Skeleton
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A simple placeholder element that indicates content is loading.

- Renders as a `div` with `animate-pulse` for a pulsing opacity animation
- Uses `rounded-md` for consistent border radius
- Background color is `bg-accent`
- Accepts `className` for sizing and shape customization (width, height, rounded-full for circles, etc.)
- Purely decorative — no interactive behavior

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Skeleton` — Placeholder element. Pulsing animated div with accent background.

### Props

#### Skeleton

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes (used to set width, height, shape) |

### Variants

No named variants. Shape and size are controlled via `className`.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"skeleton"` | Skeleton |

### CSS Variables (Radix)

Not applicable — Skeleton does not use Radix primitives.

## Composition Patterns

### Text Placeholder

```tsx
<div className="flex flex-col gap-2">
  <Skeleton className="h-4 w-3/4" />
  <Skeleton className="h-4 w-1/2" />
</div>
```

### Card Placeholder

```tsx
<div className="flex flex-col gap-4">
  <Skeleton className="h-48 w-full rounded-xl" />
  <div className="flex flex-col gap-2">
    <Skeleton className="h-4 w-3/4" />
    <Skeleton className="h-4 w-1/2" />
  </div>
</div>
```

### Avatar Placeholder

```tsx
<div className="flex items-center gap-3">
  <Skeleton className="size-10 rounded-full" />
  <div className="flex flex-col gap-2">
    <Skeleton className="h-4 w-32" />
    <Skeleton className="h-3 w-24" />
  </div>
</div>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Skeleton is a decorative placeholder

### ARIA Roles

No implicit ARIA roles. Skeleton renders as a `div`. It is a decorative element and does not convey semantic meaning. If used within a loading region, the parent container should use `aria-busy="true"` and `role="status"` or `aria-live="polite"` to communicate loading state to assistive technologies.

### Keyboard Behavior

No keyboard interaction — Skeleton is a static decorative element.

## HTML

### Standalone HTML Structure

```html
<div data-slot="skeleton"></div>
```

## CSS

### Raw CSS

```css
/* Skeleton */
.Skeleton {
  border-radius: 0.375rem;
  background-color: var(--accent);
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Skeleton | `animate-pulse rounded-md bg-accent` | Pulsing placeholder with accent background |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--accent` | Skeleton background color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into type variants:

| Variant Property | Values |
|-----------------|--------|
| Type | Line, Card |

- Uses gap 16px between avatar and lines
- Uses gap 8px between line rows


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--accent` | `--accent` | Skeleton placeholder background |
| `rounded-full` | `border-radius` | Avatar circle rounding |
| `--radius` (derived) | `border-radius` | Line and card rounding |
| `gap-4` (16px) | `gap` | Gap between avatar and lines |
| `gap-2` (8px) | `gap` | Gap between line rows |

### Theme Behavior

- Skeleton uses `--accent` — adapts to light/dark via theme
- Animation (`animate-pulse`) is mode-independent
- All spacing and border-radius tokens are mode-independent
