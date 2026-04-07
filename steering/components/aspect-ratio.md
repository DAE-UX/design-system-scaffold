# Component: Aspect Ratio

> **TL;DR:** A layout utility that displays content within a desired width-to-height ratio. Built on Radix UI AspectRatio primitive via shadCN. Single part (`AspectRatio`), single prop (`ratio`). No interactive behavior, no ARIA roles, no keyboard interaction. Commonly used to constrain images and videos to consistent dimensions.

## Metadata

- **Component Name:** Aspect Ratio
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

Displays content within a desired width-to-height ratio. The component constrains its children to maintain the specified aspect ratio regardless of container width.

- Accepts any numeric ratio via the `ratio` prop (e.g., `16 / 9`, `1 / 1`, `9 / 16`)
- Defaults to `1` (square) when no ratio is provided
- Content fills the constrained area — use CSS (`object-cover`, `object-fit`) on children to control how content fills the space
- Purely a layout utility — no interactive state, no animations
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use to maintain consistent aspect ratios for media content.
- Use standard ratios (16/9, 4/3, 1/1) for predictable layouts.

#### Don't
- Don't use for text-only content.
- Don't nest interactive elements that depend on the container's dimensions.

Sources: shadCN, Radix UI

## API

### Parts

- `AspectRatio` — Root container. Wraps `AspectRatioPrimitive.Root`. Constrains children to the specified ratio.

### Props

#### AspectRatio

| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| ratio | `number` | `1` | Yes | Width-to-height ratio (e.g., `16 / 9` = 1.778) |
| className | `string` | — | No | Additional CSS classes |
| asChild | `boolean` | `false` | No | Merge props onto child element instead of rendering a default div |

### Variants

No variants — AspectRatio is a single-purpose layout utility. The `ratio` prop controls the aspect ratio directly.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"aspect-ratio"` | Root |

### CSS Variables (Radix)

No component-specific Radix CSS variables. AspectRatio uses the padding-bottom technique internally to maintain the ratio.

## Composition Patterns

### Landscape (16:9)

```tsx
<AspectRatio ratio={16 / 9} className="rounded-lg bg-muted">
  <Image
    src="..."
    alt="Landscape photo"
    fill
    className="h-full w-full rounded-lg object-cover"
  />
</AspectRatio>
```

### Square (1:1)

```tsx
<AspectRatio ratio={1 / 1} className="rounded-lg bg-muted">
  <Image
    src="..."
    alt="Square photo"
    fill
    className="h-full w-full rounded-lg object-cover"
  />
</AspectRatio>
```

### Portrait (9:16)

```tsx
<AspectRatio ratio={9 / 16} className="rounded-lg bg-muted">
  <Image
    src="..."
    alt="Portrait photo"
    fill
    className="h-full w-full rounded-lg object-cover"
  />
</AspectRatio>
```

### With Dark Mode Image Adjustment

```tsx
<AspectRatio ratio={16 / 9} className="rounded-lg bg-muted">
  <Image
    src="..."
    alt="Photo"
    fill
    className="h-full w-full rounded-lg object-cover dark:brightness-[0.2] dark:grayscale"
  />
</AspectRatio>
```

### With Video

```tsx
<AspectRatio ratio={16 / 9}>
  <iframe
    src="..."
    title="Video"
    className="h-full w-full rounded-lg"
    allowFullScreen
  />
</AspectRatio>
```

## Accessibility

- **WAI-ARIA Pattern:** None — AspectRatio is a layout utility with no semantic role

### ARIA Roles

No ARIA roles — AspectRatio renders a presentational `div` container. The content inside (images, videos) should have appropriate `alt` text or labels.

### Keyboard Behavior

No keyboard interaction — AspectRatio is a static layout container.

## HTML

### Standalone HTML Structure

```html
<div style="position: relative; width: 100%; padding-bottom: 56.25%;">
  <div style="position: absolute; inset: 0;">
    <img src="..." alt="Description" style="width: 100%; height: 100%; object-fit: cover;" />
  </div>
</div>
```

The `padding-bottom` percentage is calculated as `(1 / ratio) * 100`. For 16:9, that's `(1 / 1.778) * 100 = 56.25%`.

## CSS

### Raw CSS

```css
/* AspectRatio container */
.AspectRatio {
  position: relative;
  width: 100%;
  padding-bottom: calc(100% / var(--ratio, 1));
}

/* Inner content wrapper */
.AspectRatio > * {
  position: absolute;
  inset: 0;
}
```

Note: Radix handles the ratio calculation internally. The CSS above illustrates the underlying technique.

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| AspectRatio (common usage) | `rounded-lg bg-muted` | Rounded corners and placeholder background |
| Child image | `h-full w-full rounded-lg object-cover` | Fill container and maintain crop |
| Child image (dark mode) | `dark:brightness-[0.2] dark:grayscale` | Optional dark mode image treatment |

Note: The shadCN AspectRatio component itself adds no Tailwind classes — it's a thin wrapper around the Radix primitive. Styling is applied via `className` at the usage site.

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Placeholder background (when used with `bg-muted`) | `shadcn` |

Note: AspectRatio has minimal CSS variable dependencies. The `bg-muted` class is commonly applied at the usage site but is not built into the component itself.

## Theme Support

### Component Structure

EMPTY: None recorded.

### CSS Variable Mapping

EMPTY: None recorded.

### Theme Behavior

- AspectRatio itself is theme-agnostic — it's a layout utility with no color or style tokens
- Background color, when applied via `className="bg-muted"`, adapts to light/dark via theme
- Dark mode image adjustments (brightness, grayscale) are applied at the usage site, not in the component
- All spacing and sizing is ratio-driven, not theme-dependent
