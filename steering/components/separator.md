# Component: Separator

> **TL;DR:** A visual divider between content sections. 1 part: Separator. Built on Radix UI primitive. Supports `horizontal` and `vertical` orientations. Decorative by default. Simple `bg-border` line.

## Metadata

- **Component Name:** Separator
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A visual divider that separates content into distinct sections, built on Radix UI's Separator primitive.

- Renders as a `div` with `shrink-0` and `bg-border`
- Horizontal orientation (default): `h-px w-full` — a 1px tall line spanning full width
- Vertical orientation: `h-full w-px` — a 1px wide line spanning full height
- `decorative` prop defaults to `true` — when true, the separator is hidden from assistive technology
- When `decorative` is `false`, the separator has `role="separator"` and is announced by screen readers
- Orientation is controlled via `data-[orientation]` attribute selectors

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Separator` — The divider element. A single line with orientation-based sizing.

### Props

#### Separator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"horizontal" \| "vertical"` | `"horizontal"` | Divider direction |
| decorative | `boolean` | `true` | Whether the separator is purely decorative (hidden from assistive technology) |
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `orientation="horizontal"` | Full-width, 1px tall line | Dividing stacked content sections |
| `orientation="vertical"` | Full-height, 1px wide line | Dividing side-by-side content |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"separator"` | Separator |
| `[data-orientation]` | `"horizontal"`, `"vertical"` | Separator |

### CSS Variables (Radix)

Not applicable — Separator does not use custom Radix CSS variables.

## Composition Patterns

### Horizontal Separator

```tsx
<div>
  <p>Content above</p>
  <Separator />
  <p>Content below</p>
</div>
```

### Vertical Separator

```tsx
<div className="flex h-5 items-center gap-4">
  <span>Left</span>
  <Separator orientation="vertical" />
  <span>Right</span>
</div>
```

### Non-Decorative Separator

```tsx
<Separator decorative={false} />
```

## Accessibility

- **WAI-ARIA Pattern:** Separator (implicit via Radix)

### ARIA Roles

When `decorative={true}` (default), the separator has `role="none"` and is hidden from assistive technology. When `decorative={false}`, it has `role="separator"` with `aria-orientation`.

### Keyboard Behavior

No keyboard interaction — Separator is a static visual element. It is not focusable or interactive.

## HTML

### Standalone HTML Structure

```html
<!-- Decorative (default) -->
<div data-slot="separator" data-orientation="horizontal" role="none"></div>

<!-- Non-decorative -->
<div data-slot="separator" data-orientation="vertical" role="separator" aria-orientation="vertical"></div>
```

## CSS

### Raw CSS

```css
/* Separator */
.Separator {
  flex-shrink: 0;
  background-color: var(--border);
}

/* Horizontal */
.Separator[data-orientation="horizontal"] {
  height: 1px;
  width: 100%;
}

/* Vertical */
.Separator[data-orientation="vertical"] {
  height: 100%;
  width: 1px;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Separator | `shrink-0 bg-border` | Base divider styling |
| Separator (horizontal) | `data-[orientation=horizontal]:h-px data-[orientation=horizontal]:w-full` | Full-width 1px line |
| Separator (vertical) | `data-[orientation=vertical]:h-full data-[orientation=vertical]:w-px` | Full-height 1px line |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Separator line color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into orientation variants:

| Variant Property | Values |
|-----------------|--------|
| Horizontal | True, False |

- Separator → Line (1px height, full width, rendered as vector stroke)
- Horizontal=True: `w-full`, `h-0` (1px line via border/stroke)
- Horizontal=False (vertical): `h-full`, `w-0` (1px line via border/stroke)

No additional sub-components or nested layers.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

The Separator component in the design tool renders as a simple vector line stroke. No explicit CSS variable tokens are surfaced in the design context output — the line color is derived from the `--border` variable at the theme level.

| Token | CSS Variable | Purpose |
|-------------|-------------|---------|
| (stroke color) | `--border` | Separator line color (inferred from theme) |

### Theme Behavior

- Separator uses `--border` — adapts to light/dark via theme
- All sizing tokens (1px) are mode-independent
