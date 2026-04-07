# Component: Spinner

> **TL;DR:** A loading indicator. 1 part: Spinner. No external primitive — renders a `Loader2Icon` from Lucide with `animate-spin` at `size-4`. Includes `role="status"` and `aria-label="Loading"` for accessibility. No CSS variable dependencies — inherits current text color. Used for inline loading states and button loading indicators.

## Metadata

- **Component Name:** Spinner
- **Source:** shadCN
- **Dependencies:** (none — uses `lucide-react` icon only)

## Behavior

A simple spinning icon that indicates a loading or processing state.

- Renders a `Loader2Icon` SVG from Lucide with `animate-spin` for continuous rotation
- Default size is `size-4` (16px)
- Inherits the current text color from its parent — no explicit color set
- Accepts `className` for size and color customization
- Accepts all SVG props for additional configuration
- Includes `role="status"` and `aria-label="Loading"` for screen reader accessibility

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Spinner` — Loading indicator. Spinning Lucide icon with accessibility attributes.

### Props

#### Spinner

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes (size, color overrides) |
| ...props | `SVGProps` | — | All SVG element props are forwarded |

### Variants

No named variants. Size and color are controlled via `className`.

### Data Attributes

No `data-slot` attributes — Spinner renders an SVG element directly.

### CSS Variables (Radix)

Not applicable — Spinner does not use Radix primitives.

## Composition Patterns

### Inline Loading

```tsx
<Spinner />
```

### Button Loading State

```tsx
<Button disabled>
  <Spinner />
  Saving...
</Button>
```

### Custom Size

```tsx
<Spinner className="size-6" />
```

### Custom Color

```tsx
<Spinner className="text-muted-foreground" />
```

### Centered Loading

```tsx
<div className="flex items-center justify-center p-8">
  <Spinner className="size-8" />
</div>
```

## Accessibility

- **WAI-ARIA Pattern:** Status

### ARIA Roles

- Renders with `role="status"` to indicate a live region with advisory information
- `aria-label="Loading"` provides an accessible name for screen readers
- Screen readers will announce "Loading" when the spinner appears in the DOM

### Keyboard Behavior

No keyboard interaction — Spinner is a non-interactive status indicator.

## HTML

### Standalone HTML Structure

```html
<svg role="status" aria-label="Loading" class="animate-spin size-4">
  <!-- Loader2 icon paths -->
</svg>
```

## CSS

### Raw CSS

```css
/* Spinner */
.Spinner {
  width: 1rem;
  height: 1rem;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Spinner | `animate-spin size-4` | Spinning animation at 16px |

## CSS Variable Dependencies

No CSS variable dependencies. Spinner inherits the current `color` from its parent element.

## Theme Support

### Component Structure

Component is organized into a variant matrix:

| Variant Property | Values |
|-----------------|--------|
| Rotation | 0, 90, 180, 270 |
| Size | 3, 4, 5, 6, 8 |

- Spinner → Lucide Icons / loader-circle (single icon layer)
- Rotation variants represent static snapshots of the spin animation at 0°, 90°, 180°, 270°
- Size variants map to Tailwind size tokens: 3 (12px), 4 (16px), 5 (20px), 6 (24px), 8 (32px)
- No color tokens applied — icon inherits `currentColor` from parent context

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `size-4` (16px) | `width` / `height` | Default spinner size |
| `size-3` (12px) | `width` / `height` | Small spinner size |
| `size-5` (20px) | `width` / `height` | Medium spinner size |
| `size-6` (24px) | `width` / `height` | Large spinner size |
| `size-8` (32px) | `width` / `height` | Extra-large spinner size |

No color variables — Spinner inherits `currentColor`. No spacing or typography tokens beyond sizing.

### Theme Behavior

- Spinner inherits `currentColor` from its parent — adapts to any context automatically
- No explicit theme variables are referenced
- Animation (`animate-spin`) is mode-independent
- Size token (`size-4`) is mode-independent
