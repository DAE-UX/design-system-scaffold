# Component: Tooltip

> **TL;DR:** A popup that displays information on hover or focus. 4 parts: TooltipProvider, Tooltip, TooltipTrigger, TooltipContent. Built on Radix Tooltip. Content renders in a Portal with an arrow. Inverted color scheme — `bg-foreground text-background`. Enter/exit animations with directional slide. `sideOffset` defaults to 0. TooltipProvider sets `delayDuration` to 0 by default. Commonly used for icon button labels, truncated text, and supplementary information.

## Metadata

- **Component Name:** Tooltip
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-tooltip`

## Behavior

A popup that displays supplementary information when hovering over or focusing on a trigger element.

- Built on Radix Tooltip primitive — manages open/close state, positioning, and accessibility
- TooltipProvider wraps the application (or section) and configures shared settings
- TooltipProvider sets `delayDuration` to 0 by default (instant show)
- TooltipContent renders inside a Portal for proper stacking
- Content uses inverted color scheme: `bg-foreground` background, `text-background` text
- Arrow is rendered inside TooltipContent: `bg-foreground fill-foreground`, rotated 45° square
- Content has `text-xs` font size, `px-3 py-1.5` padding, `rounded-md` corners
- Uses `text-balance` for balanced text wrapping
- `sideOffset` defaults to 0 (no gap between trigger and tooltip)
- Transform origin uses `--radix-tooltip-content-transform-origin` for natural animations
- Enter animation: fade-in + zoom-in-95 + directional slide (varies by side)
- Exit animation: fade-out + zoom-out-95
- Content is `w-fit` — sizes to content width

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `TooltipProvider` — Context provider. Configures shared tooltip settings (delay, skip delay). Wraps application or section.
- `Tooltip` — Root container for a single tooltip instance. Manages open/close state.
- `TooltipTrigger` — Element that triggers the tooltip on hover/focus.
- `TooltipContent` — Popup content with arrow. Renders in a Portal.

### Props

#### TooltipProvider

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| delayDuration | `number` | `0` | Delay in ms before tooltip appears |
| skipDelayDuration | `number` | `300` | Time in ms before skip delay resets |
| disableHoverableContent | `boolean` | `false` | Prevent hovering over tooltip content |

#### Tooltip

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | — | Initial open state |
| onOpenChange | `(open: boolean) => void` | — | Callback when open state changes |
| delayDuration | `number` | — | Override provider delay for this tooltip |

#### TooltipTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element |
| className | `string` | — | Additional CSS classes |

#### TooltipContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| sideOffset | `number` | `0` | Distance from trigger in px |
| side | `"top" \| "right" \| "bottom" \| "left"` | `"top"` | Preferred side |
| align | `"start" \| "center" \| "end"` | `"center"` | Alignment along the side |
| className | `string` | — | Additional CSS classes |

### Variants

No CVA variants — Tooltip has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"tooltip-provider"`, `"tooltip"`, `"tooltip-trigger"`, `"tooltip-content"` | All parts |
| `[data-state]` | `"open"`, `"closed"`, `"delayed-open"`, `"instant-open"` | TooltipContent (from Radix) |
| `[data-side]` | `"top"`, `"right"`, `"bottom"`, `"left"` | TooltipContent (from Radix) |

### CSS Variables (Radix)

| Variable | Description |
|----------|-------------|
| `--radix-tooltip-content-transform-origin` | Transform origin for enter/exit animations |

## Composition Patterns

### Basic Tooltip

```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button variant="outline" size="icon">
        <PlusIcon />
      </Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>Add to library</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

### With Side Offset

```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Button variant="ghost" size="icon">
      <InfoIcon />
    </Button>
  </TooltipTrigger>
  <TooltipContent sideOffset={4}>
    <p>More information</p>
  </TooltipContent>
</Tooltip>
```

### Different Sides

```tsx
<Tooltip>
  <TooltipTrigger>Hover me</TooltipTrigger>
  <TooltipContent side="right">
    <p>Tooltip on the right</p>
  </TooltipContent>
</Tooltip>
```

### With Custom Delay

```tsx
<TooltipProvider delayDuration={500}>
  <Tooltip>
    <TooltipTrigger>Slow tooltip</TooltipTrigger>
    <TooltipContent>
      <p>Appears after 500ms</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

## Accessibility

- **WAI-ARIA Pattern:** [Tooltip](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/)

### ARIA Roles

- TooltipContent renders with `role="tooltip"` (provided by Radix)
- TooltipTrigger is associated with content via `aria-describedby` (provided by Radix)
- Tooltip appears on both hover and focus for keyboard accessibility

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Tab` | Focus trigger, opens tooltip |
| `Escape` | Closes the tooltip |

Focus on the trigger element opens the tooltip. Moving focus away closes it.

## HTML

### Standalone HTML Structure

```html
<!-- Trigger -->
<button data-slot="tooltip-trigger" aria-describedby="tooltip-1">
  <svg><!-- icon --></svg>
</button>

<!-- Portal (appended to body) -->
<div data-slot="tooltip-content" id="tooltip-1" role="tooltip" data-state="open" data-side="top">
  <p>Tooltip text</p>
  <div class="tooltip-arrow"></div>
</div>
```

## CSS

### Raw CSS

```css
/* TooltipContent */
.TooltipContent {
  z-index: 50;
  width: fit-content;
  border-radius: 0.375rem;
  background-color: var(--foreground);
  padding: 0.375rem 0.75rem;
  font-size: 0.75rem;
  color: var(--background);
  text-wrap: balance;
  transform-origin: var(--radix-tooltip-content-transform-origin);
}

/* TooltipContent — enter animation */
.TooltipContent[data-state="open"],
.TooltipContent[data-state="delayed-open"],
.TooltipContent[data-state="instant-open"] {
  animation: fadeIn 150ms ease-in, zoomIn 150ms ease-in;
}

/* TooltipContent — exit animation */
.TooltipContent[data-state="closed"] {
  animation: fadeOut 150ms ease-out, zoomOut 150ms ease-out;
}

/* TooltipContent — directional slide */
.TooltipContent[data-side="top"] {
  animation: slideFromBottom 150ms ease-in;
}

.TooltipContent[data-side="bottom"] {
  animation: slideFromTop 150ms ease-in;
}

.TooltipContent[data-side="left"] {
  animation: slideFromRight 150ms ease-in;
}

.TooltipContent[data-side="right"] {
  animation: slideFromLeft 150ms ease-in;
}

/* Tooltip Arrow */
.TooltipArrow {
  z-index: 50;
  width: 0.625rem;
  height: 0.625rem;
  transform: translateY(calc(-50% - 2px)) rotate(45deg);
  border-radius: 2px;
  background-color: var(--foreground);
  fill: var(--foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| TooltipContent | `z-50 w-fit origin-(--radix-tooltip-content-transform-origin) rounded-md bg-foreground px-3 py-1.5 text-xs text-balance text-background` | Content styling |
| TooltipContent (enter) | `animate-in fade-in-0 zoom-in-95` | Enter animation |
| TooltipContent (exit) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Exit animation |
| TooltipContent (slide by side) | `data-[side=top]:slide-in-from-bottom-2 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2` | Directional slide |
| TooltipArrow | `z-50 size-2.5 translate-y-[calc(-50%_-_2px)] rotate-45 rounded-[2px] bg-foreground fill-foreground` | Arrow styling |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--foreground` | Content background, arrow background and fill | `shadcn` |
| `--background` | Content text color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into caret position variants:

| Variant Property | Values |
|-----------------|--------|
| Caret position | Top, Bottom, Right, Left |

- Content container: foreground bg, rounded, px 12px, py 6px, inline-flex centered
- Label text: background color, text-xs, font-normal, leading-4
- Caret: foreground bg, 10×10 square rotated 45°, rounded 2px, positioned at center of the relevant edge
- Optional Kbd sub-component: semi-transparent bg, rounded, min-w 20px, h 20px, text-xs, font-medium


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Tooltip background (inverted) |
| `--background` | `--background` | Tooltip text color (inverted) |
| `--radius` (derived) | `border-radius` | Content border radius |
| `px-3` (12px) | `padding-inline` | Content horizontal padding |
| `py-1.5` (6px) | `padding-block` | Content vertical padding |
| `size-2.5` (10px) | `size` | Caret size |
| `font-sans` | `--font-sans` | Font family |
| `text-xs` (12px) | `font-size` | Text size |

### Theme Behavior

- Content uses inverted colors: `--foreground` for background, `--background` for text — adapts to light/dark via theme
- Arrow matches content background (`--foreground`) — adapts to light/dark via theme
- In light mode: dark tooltip on light background; in dark mode: light tooltip on dark background
- Animations, spacing, and typography tokens are mode-independent