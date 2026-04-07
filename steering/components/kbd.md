# Component: Kbd

> **TL;DR:** A keyboard shortcut indicator. 2 parts: Kbd, KbdGroup. No external primitive — renders a native `kbd` element. Inline-flex with muted background, minimum 20px width, centered content. Auto-sizes SVG icons to 12px. Special styling when inside tooltips (inverted colors). KbdGroup wraps multiple Kbd elements inline.

## Metadata

- **Component Name:** Kbd
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A keyboard shortcut indicator that displays key combinations in a styled inline element.

- Kbd renders as a native `kbd` element with `inline-flex` layout
- Minimum width of 20px (`min-w-5`) and height of 20px (`h-5`) for consistent sizing
- Content is centered with `items-center justify-center`
- Uses `font-sans` to override any monospace inheritance
- `pointer-events-none` and `select-none` — purely presentational
- SVG icons inside auto-size to 12px (`size-3`)
- When inside a tooltip (`[data-slot=tooltip-content]`), colors invert: `bg-background/20` and `text-background` (light), `bg-background/10` (dark)
- KbdGroup wraps multiple Kbd elements in an inline flex container with `gap-1`
- KbdGroup also renders as a `kbd` element for semantic correctness

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Kbd` — Individual key indicator. Renders as `kbd` element.
- `KbdGroup` — Container for multiple keys. Renders as `kbd` element with inline flex.

### Props

#### Kbd

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### KbdGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

No variants — Kbd has a single visual style (with contextual tooltip override).

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"kbd"`, `"kbd-group"` | All parts |

### CSS Variables (Radix)

Not applicable — Kbd does not use Radix primitives.

## Composition Patterns

### Single Key

```tsx
<Kbd>⌘</Kbd>
```

### Key Combination

```tsx
<KbdGroup>
  <Kbd>⌘</Kbd>
  <Kbd>K</Kbd>
</KbdGroup>
```

### With Text

```tsx
<p className="text-sm text-muted-foreground">
  Press <Kbd>⌘</Kbd> <Kbd>K</Kbd> to open the command palette.
</p>
```

### Inside Tooltip

```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Button variant="outline">Search</Button>
  </TooltipTrigger>
  <TooltipContent>
    Search <Kbd>⌘K</Kbd>
  </TooltipContent>
</Tooltip>
```

## Accessibility

- **WAI-ARIA Pattern:** None — uses native `kbd` element semantics

### ARIA Roles

No additional ARIA roles. The native `kbd` element provides semantic meaning for keyboard input.

### Keyboard Behavior

No keyboard interaction — Kbd is a presentational element.

## HTML

### Standalone HTML Structure

```html
<kbd data-slot="kbd">⌘</kbd>

<kbd data-slot="kbd-group">
  <kbd data-slot="kbd">⌘</kbd>
  <kbd data-slot="kbd">K</kbd>
</kbd>
```

## CSS

### Raw CSS

```css
/* Kbd */
.Kbd {
  pointer-events: none;
  display: inline-flex;
  height: 1.25rem;
  width: fit-content;
  min-width: 1.25rem;
  align-items: center;
  justify-content: center;
  gap: 0.25rem;
  border-radius: 0.125rem;
  background-color: var(--muted);
  padding: 0 0.25rem;
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--muted-foreground);
  user-select: none;
}

.Kbd svg:not([class*='size-']) {
  width: 0.75rem;
  height: 0.75rem;
}

/* Kbd inside tooltip */
[data-slot="tooltip-content"] .Kbd {
  background-color: color-mix(in srgb, var(--background) 20%, transparent);
  color: var(--background);
}

/* KbdGroup */
.KbdGroup {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Kbd | `pointer-events-none inline-flex h-5 w-fit min-w-5 items-center justify-center gap-1 rounded-sm bg-muted px-1 font-sans text-xs font-medium text-muted-foreground select-none` | Key indicator |
| Kbd (icons) | `[&_svg:not([class*='size-'])]:size-3` | Icon sizing |
| Kbd (in tooltip) | `[[data-slot=tooltip-content]_&]:bg-background/20 [[data-slot=tooltip-content]_&]:text-background dark:[[data-slot=tooltip-content]_&]:bg-background/10` | Tooltip context |
| KbdGroup | `inline-flex items-center gap-1` | Key group |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Background color | `shadcn` |
| `--muted-foreground` | Text color | `shadcn` |
| `--background` | Tooltip context text and background overlay | `shadcn` |
| `--font-sans` | Font family override | `shadcn` |

## Theme Support

### Component Structure

Kbd styles: Default, Reversed
Kbd Group types: Default, Separated

Kbd layout: optional Left Icon (12×12) + Label text + optional Right Icon (12×12)
Container: flex (gap 4px, h 20px, min-width 20px), muted bg, rounded, text-xs, font-medium, centered

Kbd Group (Default): inline flex with multiple Kbd children
Kbd Group (Separated): Kbd children with visual separators

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--muted` | `--muted` | Kbd background |
| `--muted-foreground` | `--muted-foreground` | Kbd text color |
| `--radius` (derived) | `--radius` | Kbd border radius |
| `h-5` (20px) | `height` | Kbd height |
| `font-sans` | `--font-sans` | Font family |
| `text-xs` (12px) | `font-size` | Text size |

### Theme Behavior

- Background uses `--muted` — adapts to light/dark via theme
- Text uses `--muted-foreground` — adapts to light/dark via theme
- Tooltip context differs: `bg-background/20` in light, `bg-background/10` in dark
- Font uses `--font-sans` to prevent monospace inheritance
- All sizing tokens are mode-independent
