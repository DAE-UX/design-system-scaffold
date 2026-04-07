# Component: Drawer

> **TL;DR:** A panel that slides in from the edge of the screen. 10 parts built on the vaul library. Supports four directions: top, bottom, left, right. Bottom drawer includes a drag handle. Includes overlay backdrop, header, footer, title, and description. Bottom/top drawers have max 80vh height and centered text; left/right drawers have 75% width (max sm). Swipe-to-dismiss on touch devices.

## Metadata

- **Component Name:** Drawer
- **Source:** shadCN (vaul)
- **Dependencies:** vaul

## Behavior

A panel that slides in from the edge of the screen, built on the vaul library.

- DrawerTrigger opens the drawer; supports `asChild`
- DrawerContent renders via Portal with an Overlay backdrop
- Content direction is controlled by vaul's `direction` prop on the Drawer root
- Bottom drawer: slides up from bottom, rounded top corners, border-top, max 80vh, includes drag handle
- Top drawer: slides down from top, rounded bottom corners, border-bottom, max 80vh
- Right drawer: slides in from right, border-left, 75% width (max-w-sm on sm+)
- Left drawer: slides in from left, border-right, 75% width (max-w-sm on sm+)
- Drag handle (pill-shaped muted bar) is only visible for bottom direction
- DrawerHeader text is centered for top/bottom directions, left-aligned on md+
- DrawerFooter is pushed to the bottom via `mt-auto`
- Overlay is fixed full-screen with `bg-black/50` and fade animations
- Swipe-to-dismiss on touch devices (vaul built-in)
- Supports controlled mode via `open` and `onOpenChange` on Drawer root
- Supports `shouldScaleBackground` to scale the page content behind the drawer
- DrawerClose can be used standalone or with `asChild`
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Persist user preference for drawer open/closed across navigation
- Use the footer for persistent actions or contextual information

#### Don't
- Don't put critical content in a drawer — it should be supplemental
- Don't trigger a drawer from a modal
- Don't overwhelm users with information — keep drawer content concise
- Don't overload the footer

Sources: shadCN, Radix UI

## API

### Parts

- `Drawer` — Root. Manages open/close state and direction. Wraps vaul Drawer.Root.
- `DrawerTrigger` — Button that opens the drawer. Supports `asChild`.
- `DrawerPortal` — Portal wrapper for rendering outside the DOM tree.
- `DrawerOverlay` — Full-screen backdrop with fade animation.
- `DrawerContent` — Sliding panel. Direction-aware positioning and styling.
- `DrawerHeader` — Container for title and description.
- `DrawerFooter` — Container for action buttons. Pushed to bottom via `mt-auto`.
- `DrawerTitle` — Drawer heading text.
- `DrawerDescription` — Drawer helper/description text.
- `DrawerClose` — Close trigger. Supports `asChild`.

### Props

#### Drawer

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |
| direction | `"top" \| "bottom" \| "left" \| "right"` | `"bottom"` | Slide direction |
| shouldScaleBackground | `boolean` | — | Scale page content behind drawer |
| dismissible | `boolean` | `true` | Allow dismissing by swiping/clicking overlay |

#### DrawerContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### DrawerTrigger / DrawerClose

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element |

### Variants

No CVA variants. Direction-based styling is handled via vaul's `data-vaul-drawer-direction` attribute.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"drawer"`, `"drawer-trigger"`, `"drawer-portal"`, `"drawer-overlay"`, `"drawer-content"`, `"drawer-header"`, `"drawer-footer"`, `"drawer-title"`, `"drawer-description"`, `"drawer-close"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Overlay, Content |
| `[data-vaul-drawer-direction]` | `"top"`, `"bottom"`, `"left"`, `"right"` | Content |

### CSS Variables (Radix)

Not applicable — Drawer uses vaul, not Radix primitives.

## Composition Patterns

### Basic (Bottom)

```tsx
<Drawer>
  <DrawerTrigger asChild>
    <Button variant="outline">Open Drawer</Button>
  </DrawerTrigger>
  <DrawerContent>
    <div className="mx-auto w-full max-w-sm">
      <DrawerHeader>
        <DrawerTitle>Move Goal</DrawerTitle>
        <DrawerDescription>Set your daily activity goal.</DrawerDescription>
      </DrawerHeader>
      <div className="p-4">
        {/* Content */}
      </div>
      <DrawerFooter>
        <Button>Submit</Button>
        <DrawerClose asChild>
          <Button variant="outline">Cancel</Button>
        </DrawerClose>
      </DrawerFooter>
    </div>
  </DrawerContent>
</Drawer>
```

### Right Side

```tsx
<Drawer direction="right">
  <DrawerTrigger asChild>
    <Button variant="outline">Open Right</Button>
  </DrawerTrigger>
  <DrawerContent>
    <DrawerHeader>
      <DrawerTitle>Settings</DrawerTitle>
      <DrawerDescription>Adjust your preferences.</DrawerDescription>
    </DrawerHeader>
    <div className="p-4">
      {/* Settings form */}
    </div>
  </DrawerContent>
</Drawer>
```

### Controlled

```tsx
const [open, setOpen] = React.useState(false)

<Drawer open={open} onOpenChange={setOpen}>
  <DrawerTrigger asChild>
    <Button>Open</Button>
  </DrawerTrigger>
  <DrawerContent>
    <DrawerHeader>
      <DrawerTitle>Controlled Drawer</DrawerTitle>
    </DrawerHeader>
  </DrawerContent>
</Drawer>
```

## Accessibility

- **WAI-ARIA Pattern:** [Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| DrawerContent | `dialog` (implicit via vaul) | Modal dialog semantics |
| DrawerTitle | heading | Accessible name for the drawer |
| DrawerDescription | description | Accessible description |
| DrawerOverlay | presentation | Backdrop, not interactive |
| DrawerClose | `button` | Close trigger |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Escape | Closes the drawer |
| Tab | Cycles focus within the drawer (focus trap) |
| Shift + Tab | Cycles focus backward within the drawer |

## HTML

### Standalone HTML Structure

```html
<!-- Overlay -->
<div data-slot="drawer-overlay" data-state="open"></div>
<!-- Content (bottom direction) -->
<div data-slot="drawer-content" data-state="open" data-vaul-drawer-direction="bottom" role="dialog">
  <!-- Drag handle (bottom only) -->
  <div class="drag-handle"></div>
  <div data-slot="drawer-header">
    <h2 data-slot="drawer-title">Drawer Title</h2>
    <p data-slot="drawer-description">Description text.</p>
  </div>
  <div>
    <!-- Body content -->
  </div>
  <div data-slot="drawer-footer">
    <button>Submit</button>
    <button data-slot="drawer-close">Cancel</button>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* DrawerOverlay */
.DrawerOverlay {
  position: fixed;
  inset: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.5);
}

/* DrawerContent (base) */
.DrawerContent {
  position: fixed;
  z-index: 50;
  display: flex;
  height: auto;
  flex-direction: column;
  background-color: var(--background);
}

/* Bottom direction */
.DrawerContent[data-vaul-drawer-direction="bottom"] {
  inset-inline: 0;
  bottom: 0;
  margin-top: 6rem;
  max-height: 80vh;
  border-top-left-radius: 0.5rem;
  border-top-right-radius: 0.5rem;
  border-top: 1px solid var(--border);
}

/* Top direction */
.DrawerContent[data-vaul-drawer-direction="top"] {
  inset-inline: 0;
  top: 0;
  margin-bottom: 6rem;
  max-height: 80vh;
  border-bottom-left-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
  border-bottom: 1px solid var(--border);
}

/* Right direction */
.DrawerContent[data-vaul-drawer-direction="right"] {
  inset-block: 0;
  right: 0;
  width: 75%;
  border-left: 1px solid var(--border);
}

@media (min-width: 640px) {
  .DrawerContent[data-vaul-drawer-direction="right"] { max-width: 24rem; }
}

/* Left direction */
.DrawerContent[data-vaul-drawer-direction="left"] {
  inset-block: 0;
  left: 0;
  width: 75%;
  border-right: 1px solid var(--border);
}

@media (min-width: 640px) {
  .DrawerContent[data-vaul-drawer-direction="left"] { max-width: 24rem; }
}

/* Drag handle (bottom only) */
.DragHandle {
  margin: 1rem auto 0;
  display: none;
  height: 0.5rem;
  width: 100px;
  flex-shrink: 0;
  border-radius: 9999px;
  background-color: var(--muted);
}

.DrawerContent[data-vaul-drawer-direction="bottom"] .DragHandle {
  display: block;
}

/* DrawerHeader */
.DrawerHeader {
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  padding: 1rem;
}

/* DrawerFooter */
.DrawerFooter {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 1rem;
}

/* DrawerTitle */
.DrawerTitle {
  font-weight: 600;
  color: var(--foreground);
}

/* DrawerDescription */
.DrawerDescription {
  font-size: 0.875rem;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Overlay | `fixed inset-0 z-50 bg-black/50` | Full-screen backdrop |
| Overlay (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0` | Fade in |
| Overlay (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0` | Fade out |
| Content (base) | `group/drawer-content fixed z-50 flex h-auto flex-col bg-background` | Base layout |
| Content (bottom) | `data-[vaul-drawer-direction=bottom]:inset-x-0 data-[vaul-drawer-direction=bottom]:bottom-0 data-[vaul-drawer-direction=bottom]:mt-24 data-[vaul-drawer-direction=bottom]:max-h-[80vh] data-[vaul-drawer-direction=bottom]:rounded-t-lg data-[vaul-drawer-direction=bottom]:border-t` | Bottom slide |
| Content (top) | `data-[vaul-drawer-direction=top]:inset-x-0 data-[vaul-drawer-direction=top]:top-0 data-[vaul-drawer-direction=top]:mb-24 data-[vaul-drawer-direction=top]:max-h-[80vh] data-[vaul-drawer-direction=top]:rounded-b-lg data-[vaul-drawer-direction=top]:border-b` | Top slide |
| Content (right) | `data-[vaul-drawer-direction=right]:inset-y-0 data-[vaul-drawer-direction=right]:right-0 data-[vaul-drawer-direction=right]:w-3/4 data-[vaul-drawer-direction=right]:border-l data-[vaul-drawer-direction=right]:sm:max-w-sm` | Right slide |
| Content (left) | `data-[vaul-drawer-direction=left]:inset-y-0 data-[vaul-drawer-direction=left]:left-0 data-[vaul-drawer-direction=left]:w-3/4 data-[vaul-drawer-direction=left]:border-r data-[vaul-drawer-direction=left]:sm:max-w-sm` | Left slide |
| Drag handle | `mx-auto mt-4 hidden h-2 w-[100px] shrink-0 rounded-full bg-muted group-data-[vaul-drawer-direction=bottom]/drawer-content:block` | Bottom drag indicator |
| Header | `flex flex-col gap-0.5 p-4 group-data-[vaul-drawer-direction=bottom]/drawer-content:text-center group-data-[vaul-drawer-direction=top]/drawer-content:text-center md:gap-1.5 md:text-left` | Header layout |
| Footer | `mt-auto flex flex-col gap-2 p-4` | Footer layout |
| Title | `font-semibold text-foreground` | Title typography |
| Description | `text-sm text-muted-foreground` | Description typography |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Content background | `shadcn` |
| `--border` | Content border (direction-dependent edge) | `shadcn` |
| `--foreground` | Title text color | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |
| `--muted` | Drag handle background | `shadcn` |

## Theme Support

### Component Structure

Drawer layout: flex column (overlay bg, justify-end) → Container
Container: flex column (background, rounded top corners) → Handle Container + optional Header + Content + optional Footer
Handle: centered bar (muted bg, 8px × 100px, rounded-full)
Header: flex row (px 24px, py 16px, gap 12px) → Title text (text-base, font-medium)
Content: flex column (p 16px)
Footer: flex column (p 16px, gap 8px) → Primary button + Secondary button

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--background)` | `--background` | Drawer container background |
| `var(--card-foreground)` | `--card-foreground` | Header title text |
| `var(--muted)` | `--muted` | Handle bar background |
| `var(--primary)` | `--primary` | Primary button background |
| `var(--primary-foreground)` | `--primary-foreground` | Primary button text |
| `--radius` | `--radius` | Container top border radius |
| `p-4` (16px) | `p-4` | Content padding |
| `px-6` (24px) | `px-6` | Header horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Header title weight |
| `text-base` (16px) | `text-base` | Header title size |

### Theme Behavior

- Content uses `--background` — adapts to light/dark via theme
- Border uses `--border` on the direction-dependent edge — adapts to light/dark via theme
- Title uses `--foreground` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Drag handle uses `--muted` — adapts to light/dark via theme
- Overlay is fixed `bg-black/50` — same in both modes
- All directional positioning and sizing is mode-independent
