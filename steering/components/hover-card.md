# Component: Hover Card

> **TL;DR:** A popup card that appears on hover. 3 parts: HoverCard, HoverCardTrigger, HoverCardContent. Built on Radix UI Hover Card primitive. Content renders via Portal with configurable alignment and offset. Enter/exit animations (fade, zoom, directional slide). Default width 256px. Used for previewing content like user profiles or link summaries without clicking.

## Metadata

- **Component Name:** Hover Card
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A popup card that appears when hovering over a trigger element, built on Radix UI Hover Card primitive.

- HoverCardTrigger opens the card on mouse enter; supports `asChild`
- HoverCardContent renders via Portal with configurable `sideOffset` (default 4px) and `align` (default `"center"`)
- Content has a default width of `w-64` (256px)
- Enter/exit animations: fade + zoom + directional slide based on `data-side`
- Open/close delay configurable via `openDelay` and `closeDelay` on root (Radix defaults: 700ms open, 300ms close)
- Supports controlled mode via `open` and `onOpenChange` on root
- Content uses `--radix-hover-card-content-transform-origin` for animation origin
- Does not trap focus — hover cards are for supplementary content, not interactive workflows

### Anti-Patterns

#### Do
- Use for previewing content on hover (user profiles, link previews).
- Keep content concise and scannable.
- Use for supplemental information only.

#### Don't
- Don't put critical actions inside hover cards — they're not accessible on touch devices.
- Don't use for content that requires interaction.
- Don't nest hover cards.

Sources: shadCN, Radix UI

## API

### Parts

- `HoverCard` — Root. Manages open/close state and delays.
- `HoverCardTrigger` — Hover target that opens the card. Supports `asChild`.
- `HoverCardContent` — Card panel. Rendered via Portal with animations.

### Props

#### HoverCard

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | — | Default open state (uncontrolled) |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |
| openDelay | `number` | `700` | Delay in ms before opening |
| closeDelay | `number` | `300` | Delay in ms before closing |

#### HoverCardTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element |
| className | `string` | — | Additional CSS classes |

#### HoverCardContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| sideOffset | `number` | `4` | Offset from trigger |
| side | `"top" \| "bottom" \| "left" \| "right"` | `"bottom"` | Preferred side |
| align | `"start" \| "center" \| "end"` | `"center"` | Alignment relative to trigger |
| className | `string` | — | Additional CSS classes |

### Variants

No variants — Hover Card has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"hover-card"`, `"hover-card-trigger"`, `"hover-card-content"`, `"hover-card-portal"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | HoverCardContent |
| `[data-side]` | `"top"`, `"bottom"`, `"left"`, `"right"` | HoverCardContent |

### CSS Variables (Radix)

| Variable | Description | Applies To |
|----------|-------------|------------|
| `--radix-hover-card-content-transform-origin` | Transform origin for animations | HoverCardContent |

## Composition Patterns

### Basic Hover Card

```tsx
<HoverCard>
  <HoverCardTrigger asChild>
    <a href="/user/shadcn">@shadcn</a>
  </HoverCardTrigger>
  <HoverCardContent>
    <div className="flex justify-between space-x-4">
      <Avatar>
        <AvatarImage src="..." />
        <AvatarFallback>SC</AvatarFallback>
      </Avatar>
      <div className="space-y-1">
        <h4 className="text-sm font-semibold">@shadcn</h4>
        <p className="text-sm">
          The React component library.
        </p>
      </div>
    </div>
  </HoverCardContent>
</HoverCard>
```

### Link Preview

```tsx
<HoverCard>
  <HoverCardTrigger asChild>
    <a href="https://example.com" className="underline">
      Example Site
    </a>
  </HoverCardTrigger>
  <HoverCardContent className="w-80">
    <div className="space-y-2">
      <h4 className="text-sm font-semibold">Example Site</h4>
      <p className="text-sm text-muted-foreground">
        A brief description of the linked content.
      </p>
    </div>
  </HoverCardContent>
</HoverCard>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Hover Card is supplementary content, not a required interaction

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| HoverCardTrigger | inherits from child element | No additional ARIA roles added |
| HoverCardContent | no implicit role | Supplementary content panel |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Tab | Focus moves through trigger normally; hover card does not trap focus |
| Escape | Closes the hover card if open |

## HTML

### Standalone HTML Structure

```html
<!-- Trigger -->
<a data-slot="hover-card-trigger" href="/user/shadcn">@shadcn</a>
<!-- Card (rendered in portal when hovered) -->
<div data-slot="hover-card-content" data-state="open" data-side="bottom">
  <div>
    <h4>@shadcn</h4>
    <p>The React component library.</p>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* HoverCardContent */
.HoverCardContent {
  z-index: 50;
  width: 16rem;
  transform-origin: var(--radix-hover-card-content-transform-origin);
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 1rem;
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  outline: none;
}

/* Open animation */
.HoverCardContent[data-state="open"] {
  animation: fadeIn, zoomIn;
}

/* Close animation */
.HoverCardContent[data-state="closed"] {
  animation: fadeOut, zoomOut;
}

/* Directional slide */
.HoverCardContent[data-side="bottom"] {
  animation: slideInFromTop;
}

.HoverCardContent[data-side="top"] {
  animation: slideInFromBottom;
}

.HoverCardContent[data-side="left"] {
  animation: slideInFromRight;
}

.HoverCardContent[data-side="right"] {
  animation: slideInFromLeft;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Content | `z-50 w-64 origin-(--radix-hover-card-content-transform-origin) rounded-md border bg-popover p-4 text-popover-foreground shadow-md outline-hidden` | Card panel |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Close animation |
| Content (slide) | `data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2` | Directional slide |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Content background | `shadcn` |
| `--popover-foreground` | Content text color | `shadcn` |
| `--border` | Content border (implicit from `border` class) | `shadcn` |

## Theme Support

### Component Structure

Single hover card container with content area. Layout: popover bg, border, rounded, shadow-md, p 16px, overflow clip, width 320px.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--popover)` | `--popover` | Card background |
| `var(--border)` | `--border` | Card border |
| `--radius` (derived) | `--radius` | Card border radius |
| `p-4` (16px) | `p-4` | Card padding |
| `shadow-md` | `shadow-md` | Card box shadow |

### Theme Behavior

- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Border uses `--border` (implicit) — adapts to light/dark via theme
- Shadow is fixed (`shadow-md`) — same in both modes
- All animations (fade, zoom, slide) are mode-independent
