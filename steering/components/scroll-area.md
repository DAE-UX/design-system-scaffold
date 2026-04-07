# Component: Scroll Area

> **TL;DR:** A custom scrollable container with styled scrollbars. 2 parts: ScrollArea, ScrollBar. Built on Radix UI primitive. Root is relative, Viewport is `size-full` with `rounded-[inherit]` and focus ring. ScrollBar supports vertical/horizontal orientation. Thumb is `rounded-full bg-border`.

## Metadata

- **Component Name:** ScrollArea
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A custom scrollable container that replaces native scrollbars with styled alternatives, built on Radix UI's ScrollArea primitive.

- Root is `relative` and wraps the viewport and scrollbar elements
- Viewport is `size-full` with `rounded-[inherit]` to match parent border radius
- Viewport has focus ring support: `focus-visible:ring-[3px] focus-visible:ring-ring/50`
- ScrollBar supports `vertical` and `horizontal` orientations
- Vertical ScrollBar is `h-full w-2.5` with `border-l border-l-transparent` for spacing
- Horizontal ScrollBar is `h-2.5` with `flex-col border-t border-t-transparent` for spacing
- ScrollBar uses `touch-none` to prevent touch scrolling on the bar itself, and `select-none` to prevent text selection
- ScrollBar thumb is `rounded-full bg-border` with `relative flex-1`
- ScrollBar has `transition-colors` for hover effects
- Scrollbar padding uses `p-px` for minimal spacing

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `ScrollArea` — Root container. Relative wrapper for viewport and scrollbars.
- `ScrollBar` — Custom scrollbar track with thumb. Supports vertical and horizontal orientations.

### Props

#### ScrollArea

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### ScrollBar

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"vertical" \| "horizontal"` | `"vertical"` | Scrollbar direction |
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| ScrollBar `orientation="vertical"` | Full-height, 2.5 unit wide scrollbar on the right | Default vertical scrolling |
| ScrollBar `orientation="horizontal"` | Full-width, 2.5 unit tall scrollbar on the bottom | Horizontal scrolling |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"scroll-area"`, `"scroll-bar"` | All parts |
| `[data-orientation]` | `"vertical"`, `"horizontal"` | ScrollBar |

### CSS Variables (Radix)

Not applicable — ScrollArea does not use custom Radix CSS variables.

## Composition Patterns

### Basic Scroll Area

```tsx
<ScrollArea className="h-72 w-48 rounded-md border">
  <div className="p-4">
    {/* Long content */}
  </div>
</ScrollArea>
```

### Horizontal Scrolling

```tsx
<ScrollArea className="w-96 whitespace-nowrap rounded-md border">
  <div className="flex w-max gap-4 p-4">
    {/* Wide content */}
  </div>
  <ScrollBar orientation="horizontal" />
</ScrollArea>
```

### Both Directions

```tsx
<ScrollArea className="h-72 w-96 rounded-md border">
  <div className="w-[800px] p-4">
    {/* Content wider and taller than container */}
  </div>
  <ScrollBar orientation="vertical" />
  <ScrollBar orientation="horizontal" />
</ScrollArea>
```

## Accessibility

- **WAI-ARIA Pattern:** Scrollbar (implicit via Radix)

### ARIA Roles

Radix UI ScrollArea provides built-in `scrollbar` role on the scrollbar elements with appropriate `aria-orientation` and `aria-controls` attributes.

### Keyboard Behavior

- Standard scroll behavior is preserved within the viewport
- `Tab` — Moves focus into/out of the scroll area
- `Arrow Keys` — Scrolls content when viewport is focused
- `Page Up` / `Page Down` — Scrolls by page
- `Home` / `End` — Scrolls to start/end

## HTML

### Standalone HTML Structure

```html
<div data-slot="scroll-area">
  <div style="overflow: hidden;">
    <!-- Viewport -->
    <div>
      <!-- Scrollable content -->
    </div>
  </div>
  <div data-slot="scroll-bar" data-orientation="vertical">
    <div><!-- Thumb --></div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* ScrollArea */
.ScrollArea {
  position: relative;
}

/* Viewport */
.ScrollAreaViewport {
  width: 100%;
  height: 100%;
  border-radius: inherit;
  transition: color, box-shadow;
  outline: none;
}

.ScrollAreaViewport:focus-visible {
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* ScrollBar (vertical) */
.ScrollBarVertical {
  display: flex;
  touch-action: none;
  padding: 1px;
  transition: color;
  user-select: none;
  height: 100%;
  width: 0.625rem;
  border-left: 1px solid transparent;
}

/* ScrollBar (horizontal) */
.ScrollBarHorizontal {
  display: flex;
  touch-action: none;
  padding: 1px;
  transition: color;
  user-select: none;
  height: 0.625rem;
  flex-direction: column;
  border-top: 1px solid transparent;
}

/* ScrollAreaThumb */
.ScrollAreaThumb {
  position: relative;
  flex: 1;
  border-radius: 9999px;
  background-color: var(--border);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ScrollArea | `relative` | Root container |
| Viewport | `size-full rounded-[inherit] transition-[color,box-shadow] outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50` | Scrollable viewport |
| ScrollBar (vertical) | `flex touch-none p-px transition-colors select-none h-full w-2.5 border-l border-l-transparent` | Vertical scrollbar track |
| ScrollBar (horizontal) | `h-2.5 flex-col border-t border-t-transparent` | Horizontal scrollbar track |
| ScrollAreaThumb | `relative flex-1 rounded-full bg-border` | Scrollbar thumb |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Scrollbar thumb color | `shadcn` |
| `--ring` | Viewport focus ring color | `shadcn` |

## Theme Support

### Component Structure

Scroll Area is a React-specific component. No design tool variant matrix applies.

Page content:
- Single Alert component (Type=Default) with icon (triangle-alert) + title ("React-specific component") + description ("This component is not here as it's only used in React.")
- No variant properties, no layer hierarchy for Scroll Area itself

### CSS Variable Mapping

No component-specific design tokens. CSS variable mappings are derived from the code implementation (see CSS Variable Dependencies above).

### Theme Behavior

- Scrollbar thumb uses `--border` — adapts to light/dark via theme
- Viewport focus ring uses `--ring` — adapts to light/dark via theme
- All spacing and border-radius tokens are mode-independent
