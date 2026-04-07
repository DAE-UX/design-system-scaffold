# Component: Popover

> **TL;DR:** A floating panel anchored to a trigger element. 7 parts: Popover, PopoverTrigger, PopoverContent, PopoverAnchor, PopoverHeader, PopoverTitle, PopoverDescription. Built on Radix UI primitive. Content renders via Portal, `w-72`, `sideOffset=4`, `align="center"`. Fade+zoom+slide animations. PopoverHeader/Title/Description are layout helpers.

## Metadata

- **Component Name:** Popover
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A floating panel that appears anchored to a trigger element, built on Radix UI's Popover primitive.

- Content renders via Radix Portal for proper stacking context
- Default width is `w-72` (18rem)
- Default `sideOffset` is `4` and `align` is `"center"`
- Content uses `origin-(--radix-popover-content-transform-origin)` for transform origin
- Fade+zoom+slide animations on open/close
- PopoverHeader is a flex column layout helper with `gap-1` and `text-sm`
- PopoverTitle renders with `font-medium`
- PopoverDescription renders with `text-muted-foreground`
- PopoverAnchor allows anchoring the popover to a different element than the trigger
- Content has `z-50` for stacking, rounded corners, border, shadow, and padding

### Anti-Patterns

#### Do
- Use only for supplemental, non-essential information
- Always show a dismiss button when interactive elements are included
- Only trigger from user interactions with text or buttons
- Always use a header when dismissible

#### Don't
- Don't use for help content
- Don't use for content requiring extended reading — use a modal
- Don't launch a modal from within a popover
- Don't launch a popover from within a popover

Sources: shadCN, Radix UI

## API

### Parts

- `Popover` — Root provider. Manages open/close state.
- `PopoverTrigger` — Element that toggles the popover.
- `PopoverContent` — Floating content panel with animations.
- `PopoverAnchor` — Optional anchor element for positioning.
- `PopoverHeader` — Layout helper for title and description.
- `PopoverTitle` — Heading text inside the popover.
- `PopoverDescription` — Descriptive text inside the popover.

### Props

#### Popover

Inherits Radix UI Popover root props (open, onOpenChange, defaultOpen, modal).

#### PopoverTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Merge props onto child element |
| className | `string` | — | Additional CSS classes |

#### PopoverContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| align | `"start" \| "center" \| "end"` | `"center"` | Content alignment relative to trigger |
| sideOffset | `number` | `4` | Distance from trigger in pixels |
| className | `string` | — | Additional CSS classes |

#### PopoverAnchor

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PopoverHeader

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PopoverTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PopoverDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

No named variants — Popover has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"popover-trigger"`, `"popover-content"`, `"popover-anchor"`, `"popover-header"`, `"popover-title"`, `"popover-description"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | PopoverContent |

### CSS Variables (Radix)

| Variable | Purpose |
|----------|---------|
| `--radix-popover-content-transform-origin` | Transform origin for content animations |

## Composition Patterns

### Basic Popover

```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">Open Popover</Button>
  </PopoverTrigger>
  <PopoverContent>
    <PopoverHeader>
      <PopoverTitle>Dimensions</PopoverTitle>
      <PopoverDescription>Set the dimensions for the layer.</PopoverDescription>
    </PopoverHeader>
    <div className="grid gap-2">
      <Label htmlFor="width">Width</Label>
      <Input id="width" defaultValue="100%" />
    </div>
  </PopoverContent>
</Popover>
```

### With Custom Anchor

```tsx
<Popover>
  <PopoverAnchor asChild>
    <div className="inline-block">Anchor element</div>
  </PopoverAnchor>
  <PopoverTrigger asChild>
    <Button>Toggle</Button>
  </PopoverTrigger>
  <PopoverContent>
    <p>Content anchored to a different element.</p>
  </PopoverContent>
</Popover>
```

### With Alignment

```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button>Align Start</Button>
  </PopoverTrigger>
  <PopoverContent align="start">
    <p>Aligned to the start of the trigger.</p>
  </PopoverContent>
</Popover>
```

## Accessibility

- **WAI-ARIA Pattern:** Dialog (non-modal)

### ARIA Roles

Radix UI Popover provides built-in `dialog` role on the content element. Trigger is associated with the content via `aria-haspopup` and `aria-expanded`.

### Keyboard Behavior

- `Enter` / `Space` — Toggles the popover on the trigger
- `Escape` — Closes the popover
- `Tab` — Moves focus within the popover content
- Focus is trapped within the popover when open (if modal)

## HTML

### Standalone HTML Structure

```html
<!-- Trigger -->
<button data-slot="popover-trigger" aria-expanded="false">Open Popover</button>

<!-- Content (rendered in Portal) -->
<div data-slot="popover-content" data-state="open" role="dialog">
  <div data-slot="popover-header">
    <div data-slot="popover-title">Title</div>
    <div data-slot="popover-description">Description text.</div>
  </div>
  <div>
    <!-- Popover body content -->
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* PopoverContent */
.PopoverContent {
  z-index: 50;
  width: 18rem;
  transform-origin: var(--radix-popover-content-transform-origin);
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 1rem;
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  outline: hidden;
}

/* PopoverHeader */
.PopoverHeader {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  font-size: 0.875rem;
}

/* PopoverTitle */
.PopoverTitle {
  font-weight: 500;
}

/* PopoverDescription */
.PopoverDescription {
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| PopoverContent | `z-50 w-72 origin-(--radix-popover-content-transform-origin) rounded-md border bg-popover p-4 text-popover-foreground shadow-md outline-hidden` | Floating content panel |
| PopoverHeader | `flex flex-col gap-1 text-sm` | Header layout |
| PopoverTitle | `font-medium` | Title typography |
| PopoverDescription | `text-muted-foreground` | Description color |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Content background color | `shadcn` |
| `--popover-foreground` | Content text color | `shadcn` |
| `--border` | Content border color (implicit from `border` class) | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |

## Theme Support

### Component Structure

Component organizes Popover into two sub-components:

Popover:
- Single variant showing a floating panel with placeholder content slot

.Popover Example Content:
- Single variant showing a form layout example (labels, inputs, grid)

- Container: flex column, p 16px, rounded
- Background: popover, border
- Shadow: shadow-md
- Width: 367px


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--popover` | `--popover` | Popover background |
| `--border` | `--border` | Popover border |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--radius` | `--radius` | Popover border radius |
| `p-4` (16px) | `padding` | Popover padding |
| `font-sans` | `--font-sans` | Font family |
| `shadow-md` | `box-shadow` | Shadow |

### Theme Behavior

- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Border uses `--border` (implicit) — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Shadow is fixed (`shadow-md`) — same in both modes
- All spacing and typography tokens are mode-independent
