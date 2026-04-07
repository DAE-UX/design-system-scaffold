# Component: Dialog

> **TL;DR:** A modal window that overlays the page. 10 parts built on Radix UI Dialog primitive. Centered on screen with overlay backdrop. Includes header, footer, title, description, close button, trigger, and portal. Footer supports `showCloseButton` shorthand. Close button is optional via `showCloseButton` prop on Content. Enter/exit animations with fade + zoom. Traps focus and returns it on close.

## Metadata

- **Component Name:** Dialog
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A modal window that overlays the page content, requiring user interaction before returning to the main flow.

- DialogTrigger opens the dialog; can use `asChild` to render as any element
- DialogContent renders via Portal with an Overlay backdrop
- Content is centered on screen (50%/50% translate) with max-width constraints
- DialogOverlay is a fixed full-screen backdrop with `bg-black/50`
- DialogHeader arranges title and description vertically; centered on mobile, left-aligned on sm+
- DialogFooter arranges actions; column-reverse on mobile, row on sm+
- DialogTitle renders the heading text (semibold, lg)
- DialogDescription renders helper text (sm, muted)
- Close button (X icon) is rendered inside Content by default; controlled via `showCloseButton` prop
- DialogFooter also supports `showCloseButton` which renders an outline "Close" button
- DialogClose can be used standalone or with `asChild` for custom close triggers
- Content has enter/exit animations: fade + zoom (95% scale)
- Overlay has enter/exit animations: fade
- Focus is trapped inside the dialog when open
- Focus returns to trigger element on close
- Pressing Escape closes the dialog
- Clicking overlay closes the dialog
- Supports controlled mode via `open` and `onOpenChange` on Dialog root
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use action buttons to act on entire modal contents (Save, Delete, Done, Cancel)
- Use flash/toast messages to confirm results
- Keep text short and interactions minimal

#### Don't
- Don't use tabs or expanded sections inside modals
- Don't launch another modal from within a modal
- Don't chain a sequence of modals
- Don't put scrolling content in modals unless unavoidable

Sources: shadCN, Radix UI

## API

### Parts

- `Dialog` — Root. Manages open/close state.
- `DialogTrigger` — Button that opens the dialog. Supports `asChild`.
- `DialogPortal` — Portal wrapper for rendering outside the DOM tree.
- `DialogOverlay` — Full-screen backdrop with fade animation.
- `DialogContent` — Centered modal panel with close button.
- `DialogHeader` — Container for title and description.
- `DialogFooter` — Container for action buttons.
- `DialogTitle` — Dialog heading text.
- `DialogDescription` — Dialog helper/description text.
- `DialogClose` — Close trigger. Supports `asChild`.

### Props

#### Dialog

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | — | Default open state (uncontrolled) |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |
| modal | `boolean` | `true` | Whether to render as modal |

#### DialogTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element |
| className | `string` | — | Additional CSS classes |

#### DialogContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| showCloseButton | `boolean` | `true` | Show the X close button |
| className | `string` | — | Additional CSS classes |

#### DialogFooter

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| showCloseButton | `boolean` | `false` | Show an outline "Close" button |
| className | `string` | — | Additional CSS classes |

### Variants

No style variants — Dialog has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"dialog"`, `"dialog-trigger"`, `"dialog-portal"`, `"dialog-overlay"`, `"dialog-content"`, `"dialog-close"`, `"dialog-header"`, `"dialog-footer"`, `"dialog-title"`, `"dialog-description"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Overlay, Content, Close |

### CSS Variables (Radix)

Not applicable — Dialog does not expose Radix CSS variables for styling.

## Composition Patterns

### Basic with Form

```tsx
<Dialog>
  <DialogTrigger asChild>
    <Button variant="outline">Open Dialog</Button>
  </DialogTrigger>
  <DialogContent className="sm:max-w-[425px]">
    <DialogHeader>
      <DialogTitle>Edit profile</DialogTitle>
      <DialogDescription>
        Make changes to your profile here. Click save when you're done.
      </DialogDescription>
    </DialogHeader>
    <div className="grid gap-4">
      <div className="grid gap-3">
        <Label htmlFor="name">Name</Label>
        <Input id="name" defaultValue="Pedro Duarte" />
      </div>
    </div>
    <DialogFooter>
      <DialogClose asChild>
        <Button variant="outline">Cancel</Button>
      </DialogClose>
      <Button type="submit">Save changes</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

### Without Close Button

```tsx
<DialogContent showCloseButton={false}>
  <DialogHeader>
    <DialogTitle>Confirm Action</DialogTitle>
    <DialogDescription>This cannot be undone.</DialogDescription>
  </DialogHeader>
  <DialogFooter>
    <DialogClose asChild>
      <Button variant="outline">Cancel</Button>
    </DialogClose>
    <Button>Confirm</Button>
  </DialogFooter>
</DialogContent>
```

### Footer with Close Button Shorthand

```tsx
<DialogFooter showCloseButton>
  <Button type="submit">Save</Button>
</DialogFooter>
```

### Controlled

```tsx
const [open, setOpen] = React.useState(false)

<Dialog open={open} onOpenChange={setOpen}>
  <DialogTrigger asChild>
    <Button>Open</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Controlled Dialog</DialogTitle>
    </DialogHeader>
  </DialogContent>
</Dialog>
```

## Accessibility

- **WAI-ARIA Pattern:** [Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| DialogContent | `dialog` (implicit via Radix) | Modal dialog |
| DialogContent | `aria-modal="true"` | Set by Radix when modal |
| DialogContent | `aria-labelledby` | References DialogTitle |
| DialogContent | `aria-describedby` | References DialogDescription |
| DialogOverlay | presentation | Backdrop, not interactive |
| DialogClose | `button` | Close trigger |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Escape | Closes the dialog |
| Tab | Cycles focus within the dialog (focus trap) |
| Shift + Tab | Cycles focus backward within the dialog |
| Enter | Activates the focused button |
| Space | Activates the focused button |

## HTML

### Standalone HTML Structure

```html
<!-- Overlay -->
<div data-slot="dialog-overlay" data-state="open"></div>
<!-- Content -->
<div data-slot="dialog-content" data-state="open" role="dialog" aria-modal="true">
  <div data-slot="dialog-header">
    <h2 data-slot="dialog-title">Dialog Title</h2>
    <p data-slot="dialog-description">Dialog description text.</p>
  </div>
  <div>
    <!-- Body content -->
  </div>
  <div data-slot="dialog-footer">
    <button>Cancel</button>
    <button>Save</button>
  </div>
  <button data-slot="dialog-close" aria-label="Close">
    <svg><!-- X icon --></svg>
    <span class="sr-only">Close</span>
  </button>
</div>
```

## CSS

### Raw CSS

```css
/* DialogOverlay */
.DialogOverlay {
  position: fixed;
  inset: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.5);
}

/* DialogContent */
.DialogContent {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 50;
  display: grid;
  width: 100%;
  max-width: calc(100% - 2rem);
  transform: translate(-50%, -50%);
  gap: 1rem;
  border-radius: 0.5rem;
  border: 1px solid var(--border);
  background-color: var(--background);
  padding: 1.5rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  transition-duration: 200ms;
  outline: none;
}

@media (min-width: 640px) {
  .DialogContent { max-width: 32rem; }
}

/* DialogClose (X button) */
.DialogClose {
  position: absolute;
  top: 1rem;
  right: 1rem;
  border-radius: 0.125rem;
  opacity: 0.7;
  transition: opacity;
}

.DialogClose:hover { opacity: 1; }

.DialogClose:focus {
  box-shadow: 0 0 0 2px var(--ring), 0 0 0 4px var(--background);
  outline: none;
}

/* DialogHeader */
.DialogHeader {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  text-align: center;
}

@media (min-width: 640px) {
  .DialogHeader { text-align: left; }
}

/* DialogFooter */
.DialogFooter {
  display: flex;
  flex-direction: column-reverse;
  gap: 0.5rem;
}

@media (min-width: 640px) {
  .DialogFooter {
    flex-direction: row;
    justify-content: flex-end;
  }
}

/* DialogTitle */
.DialogTitle {
  font-size: 1.125rem;
  line-height: 1;
  font-weight: 600;
}

/* DialogDescription */
.DialogDescription {
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
| Content | `fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border bg-background p-6 shadow-lg duration-200 outline-none sm:max-w-lg` | Centered modal |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Enter animation |
| Content (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Exit animation |
| Close button | `absolute top-4 right-4 rounded-xs opacity-70 ring-offset-background transition-opacity hover:opacity-100 focus:ring-2 focus:ring-ring focus:ring-offset-2 focus:outline-hidden disabled:pointer-events-none` | X button |
| Close button (open) | `data-[state=open]:bg-accent data-[state=open]:text-muted-foreground` | Open state style |
| Close icon | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4` | Icon sizing |
| Header | `flex flex-col gap-2 text-center sm:text-left` | Header layout |
| Footer | `flex flex-col-reverse gap-2 sm:flex-row sm:justify-end` | Footer layout |
| Title | `text-lg leading-none font-semibold` | Title typography |
| Description | `text-sm text-muted-foreground` | Description typography |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Content background | `shadcn` |
| `--border` | Content border | `shadcn` |
| `--muted-foreground` | Description text, close button open state | `shadcn` |
| `--ring` | Close button focus ring | `shadcn` |
| `--accent` | Close button open state background | `shadcn` |

## Theme Support

### Component Structure

Dialog is organized into a base component, a close button sub-component, and example content:

Base: Dialog → Card Header (optional, with close button) + Content area + Card Footer (optional, with action buttons)

Dialog Close states: Enabled, Hover, Focus

Layout: Dialog container → Card Header (Title + Description + Close button) + Content + Card Footer (Cancel + Confirm buttons)
Close button: cross icon (24×24) at 70% opacity, positioned top-right
Content area: padding 32px vertical, 24px horizontal
Footer: flex row, justify-end, gap 8px

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Dialog background |
| `--border` | `--border` | Dialog border |
| `--card-foreground` | `--card-foreground` | Title text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--radius` | `--radius` | Dialog border radius |
| `p-6` (24px) | `padding` | Header/footer horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-semibold` (600) | `font-weight` | Title font weight |
| `text-lg` (18px) | `font-size` | Title font size |
| `text-sm` (14px) | `font-size` | Description font size |
| `shadow-md` | `box-shadow` | Dialog shadow |

### Theme Behavior

- Content uses `--background` / `--border` — adapts to light/dark via theme
- Overlay is fixed `bg-black/50` — same in both modes
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Close button focus ring uses `--ring` with `ring-offset-background` — adapts to light/dark via theme
- All animations (fade, zoom) are mode-independent
- Shadow is fixed (`shadow-lg`) — same in both modes
