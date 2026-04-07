# Component: Sheet

> **TL;DR:** A slide-out panel anchored to a screen edge. 8 exported parts: Sheet, SheetTrigger, SheetClose, SheetContent, SheetHeader, SheetFooter, SheetTitle, SheetDescription. Built on Radix Dialog. SheetContent supports `side` prop (`top`, `right`, `bottom`, `left`) with per-side slide animations. Overlay with fade animation. Optional close button. Used for navigation drawers, detail panels, and mobile menus.

## Metadata

- **Component Name:** Sheet
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-dialog`

## Behavior

A slide-out panel that overlays content from a screen edge, built on Radix Dialog.

- SheetContent slides in from the specified `side` (`top`, `right`, `bottom`, `left`; default `right`)
- Right/left panels render at `w-3/4 sm:max-w-sm` with full height and a vertical border (`border-l` or `border-r`)
- Top/bottom panels render at full width with `h-auto` and a horizontal border (`border-b` or `border-t`)
- Overlay renders with `bg-black/50` and fade in/out animation
- Content uses `bg-background` with `shadow-lg` and per-side slide animations
- Close button is absolute-positioned at `top-4 right-4`, renders XIcon with `opacity-70` and `hover:opacity-100`; controlled by `showCloseButton` prop (default `true`)
- SheetHeader uses `flex flex-col gap-1.5 p-4`
- SheetFooter uses `mt-auto flex flex-col gap-2 p-4`
- SheetTitle uses `font-semibold text-foreground`
- SheetDescription uses `text-sm text-muted-foreground`
- Sheet, SheetTrigger, and SheetClose are direct re-exports of Radix Dialog primitives

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Sheet` — Root container. Manages open/close state. Re-export of Radix Dialog Root.
- `SheetTrigger` — Trigger element that opens the sheet. Re-export of Radix Dialog Trigger.
- `SheetClose` — Close element that dismisses the sheet. Re-export of Radix Dialog Close.
- `SheetContent` — Slide-out panel content. Renders inside a portal with overlay. Supports `side` and `showCloseButton` props.
- `SheetHeader` — Header section inside the sheet panel.
- `SheetFooter` — Footer section inside the sheet panel. Pushed to bottom via `mt-auto`.
- `SheetTitle` — Heading text for the sheet.
- `SheetDescription` — Descriptive text below the title.

### Props

#### Sheet

Inherits all Radix Dialog Root props (controlled/uncontrolled open state).

#### SheetTrigger

Inherits all Radix Dialog Trigger props.

#### SheetClose

Inherits all Radix Dialog Close props.

#### SheetContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| side | `"top" \| "right" \| "bottom" \| "left"` | `"right"` | Edge the sheet slides in from |
| showCloseButton | `boolean` | `true` | Whether to render the close button |
| className | `string` | — | Additional CSS classes |

#### SheetHeader

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### SheetFooter

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### SheetTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### SheetDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

#### Side Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `side="right"` | Slides in from right edge, `w-3/4 sm:max-w-sm`, full height, `border-l` | Detail panels, settings |
| `side="left"` | Slides in from left edge, `w-3/4 sm:max-w-sm`, full height, `border-r` | Navigation drawers, sidebars |
| `side="top"` | Slides in from top edge, full width, `h-auto`, `border-b` | Notifications, announcements |
| `side="bottom"` | Slides in from bottom edge, full width, `h-auto`, `border-t` | Mobile menus, action sheets |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"sheet"` | Sheet |
| `[data-slot]` | `"sheet-trigger"` | SheetTrigger |
| `[data-slot]` | `"sheet-close"` | SheetClose |
| `[data-slot]` | `"sheet-portal"` | SheetPortal (internal) |
| `[data-slot]` | `"sheet-overlay"` | SheetOverlay (internal) |
| `[data-slot]` | `"sheet-content"` | SheetContent |
| `[data-slot]` | `"sheet-header"` | SheetHeader |
| `[data-slot]` | `"sheet-footer"` | SheetFooter |
| `[data-slot]` | `"sheet-title"` | SheetTitle |
| `[data-slot]` | `"sheet-description"` | SheetDescription |
| `[data-state]` | `"open"`, `"closed"` | SheetOverlay, SheetContent (Radix) |

### CSS Variables (Radix)

Radix Dialog does not expose CSS variables. Animations and positioning are handled via Tailwind classes.

## Composition Patterns

### Basic Sheet (Right)

```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button variant="outline">Open Settings</Button>
  </SheetTrigger>
  <SheetContent>
    <SheetHeader>
      <SheetTitle>Settings</SheetTitle>
      <SheetDescription>
        Manage your account settings and preferences.
      </SheetDescription>
    </SheetHeader>
    <div className="p-4">
      <p>Settings content here.</p>
    </div>
    <SheetFooter>
      <Button>Save Changes</Button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

### Left Navigation Drawer

```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button variant="ghost" size="icon">
      <MenuIcon />
    </Button>
  </SheetTrigger>
  <SheetContent side="left">
    <SheetHeader>
      <SheetTitle>Navigation</SheetTitle>
    </SheetHeader>
    <nav className="flex flex-col gap-2 p-4">
      <a href="#">Dashboard</a>
      <a href="#">Projects</a>
      <a href="#">Settings</a>
    </nav>
  </SheetContent>
</Sheet>
```

### Bottom Action Sheet

```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>Show Actions</Button>
  </SheetTrigger>
  <SheetContent side="bottom">
    <SheetHeader>
      <SheetTitle>Actions</SheetTitle>
      <SheetDescription>Choose an action to perform.</SheetDescription>
    </SheetHeader>
    <div className="flex flex-col gap-2 p-4">
      <Button variant="outline">Edit</Button>
      <Button variant="outline">Duplicate</Button>
      <Button variant="destructive">Delete</Button>
    </div>
  </SheetContent>
</Sheet>
```

### Without Close Button

```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>Open</Button>
  </SheetTrigger>
  <SheetContent showCloseButton={false}>
    <SheetHeader>
      <SheetTitle>Custom Close</SheetTitle>
    </SheetHeader>
    <div className="p-4">
      <p>Use your own close mechanism.</p>
    </div>
    <SheetFooter>
      <SheetClose asChild>
        <Button>Done</Button>
      </SheetClose>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

## Accessibility

- **WAI-ARIA Pattern:** Dialog (modal)

### ARIA Roles

- SheetContent renders with `role="dialog"` (inherited from Radix Dialog Content)
- SheetTitle is associated via `aria-labelledby` (Radix Dialog Title)
- SheetDescription is associated via `aria-describedby` (Radix Dialog Description)
- SheetOverlay provides modal backdrop; clicking it closes the sheet

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Escape` | Closes the sheet |
| `Tab` | Moves focus to next focusable element within the sheet |
| `Shift + Tab` | Moves focus to previous focusable element within the sheet |

Focus is trapped within the sheet while open. Focus returns to the trigger element when the sheet is closed.

## HTML

### Standalone HTML Structure

```html
<!-- Trigger -->
<button data-slot="sheet-trigger">Open Sheet</button>

<!-- Portal (appended to body) -->
<div data-slot="sheet-portal">
  <div data-slot="sheet-overlay" data-state="open"></div>
  <div data-slot="sheet-content" role="dialog" data-state="open">
    <div data-slot="sheet-header">
      <div data-slot="sheet-title">Sheet Title</div>
      <div data-slot="sheet-description">Sheet description text.</div>
    </div>
    <!-- body content -->
    <div data-slot="sheet-footer">
      <button>Save</button>
    </div>
    <button data-slot="sheet-close" aria-label="Close">
      <svg><!-- X icon --></svg>
    </button>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* SheetOverlay */
.SheetOverlay {
  position: fixed;
  inset: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.5);
}

.SheetOverlay[data-state="open"] {
  animation: fadeIn 150ms ease-in;
}

.SheetOverlay[data-state="closed"] {
  animation: fadeOut 150ms ease-out;
}

/* SheetContent — base */
.SheetContent {
  position: fixed;
  z-index: 50;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  background-color: var(--background);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
              0 4px 6px -4px rgba(0, 0, 0, 0.1);
  transition: transform 150ms ease-in-out;
}

/* SheetContent — right */
.SheetContent[data-side="right"] {
  top: 0;
  right: 0;
  bottom: 0;
  width: 75%;
  max-width: 24rem;
  border-left: 1px solid var(--border);
}

/* SheetContent — left */
.SheetContent[data-side="left"] {
  top: 0;
  left: 0;
  bottom: 0;
  width: 75%;
  max-width: 24rem;
  border-right: 1px solid var(--border);
}

/* SheetContent — top */
.SheetContent[data-side="top"] {
  top: 0;
  left: 0;
  right: 0;
  height: auto;
  border-bottom: 1px solid var(--border);
}

/* SheetContent — bottom */
.SheetContent[data-side="bottom"] {
  bottom: 0;
  left: 0;
  right: 0;
  height: auto;
  border-top: 1px solid var(--border);
}

/* SheetContent — close button */
.SheetContent .CloseButton {
  position: absolute;
  top: 1rem;
  right: 1rem;
  opacity: 0.7;
  cursor: pointer;
}

.SheetContent .CloseButton:hover {
  opacity: 1;
}

/* SheetHeader */
.SheetHeader {
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
  padding: 1rem;
}

/* SheetFooter */
.SheetFooter {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 1rem;
}

/* SheetTitle */
.SheetTitle {
  font-weight: 600;
  color: var(--foreground);
}

/* SheetDescription */
.SheetDescription {
  font-size: 0.875rem;
  color: var(--muted-foreground);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| SheetOverlay | `bg-black/50 fixed inset-0 z-50 data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=closed]:animate-out data-[state=closed]:fade-out-0` | Modal backdrop with fade animation |
| SheetContent (base) | `bg-background fixed z-50 flex flex-col gap-6 shadow-lg transition ease-in-out` | Panel base styling |
| SheetContent (right) | `inset-y-0 right-0 w-3/4 border-l sm:max-w-sm data-[state=open]:slide-in-from-right data-[state=closed]:slide-out-to-right` | Right-anchored panel |
| SheetContent (left) | `inset-y-0 left-0 w-3/4 border-r sm:max-w-sm data-[state=open]:slide-in-from-left data-[state=closed]:slide-out-to-left` | Left-anchored panel |
| SheetContent (top) | `inset-x-0 top-0 h-auto border-b data-[state=open]:slide-in-from-top data-[state=closed]:slide-out-to-top` | Top-anchored panel |
| SheetContent (bottom) | `inset-x-0 bottom-0 h-auto border-t data-[state=open]:slide-in-from-bottom data-[state=closed]:slide-out-to-bottom` | Bottom-anchored panel |
| Close button | `absolute top-4 right-4 opacity-70 hover:opacity-100` | Dismiss control |
| SheetHeader | `flex flex-col gap-1.5 p-4` | Header layout |
| SheetFooter | `mt-auto flex flex-col gap-2 p-4` | Footer layout pushed to bottom |
| SheetTitle | `font-semibold text-foreground` | Title typography |
| SheetDescription | `text-sm text-muted-foreground` | Description typography |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | SheetContent background color | `shadcn` |
| `--foreground` | SheetTitle text color | `shadcn` |
| `--muted-foreground` | SheetDescription text color | `shadcn` |
| `--border` | SheetContent edge border color (implicit from `border-l`, `border-r`, `border-t`, `border-b`) | `shadcn` |
| `--ring` | Focus ring color (implicit) | `shadcn` |
| `--secondary` | Secondary interactive elements (implicit) | `shadcn` |

## Theme Support

### Component Structure

Component is organized into orientation variants and sub-components:

Sheet:
| Variant Property | Values |
|-----------------|--------|
| Orientation | Right, Left, Top, Bottom |

.Sheet Close (sub-component):
| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Hover, Focus |

.Sheet Example Content (sub-component — no variant properties, placeholder)

- Header uses gap 6px between title and description
- Footer uses gap 8px between buttons, mt-auto

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Sheet panel background |
| `--foreground` | `--foreground` | Title text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--primary` | `--primary` | Primary button background |
| `--border` | `--border` | Panel edge border |
| `--radius` | `--radius` | Border radius |
| `p-6` (24px) | `padding` | Panel padding |
| `gap-1.5` (6px) | `gap` | Header title/description gap |
| `gap-2` (8px) | `gap` | Footer button gap |
| `font-sans` | `--font-sans` | Font family |
| `font-semibold` (600) | `font-weight` | Title font weight |
| `text-lg` (18px) | `font-size` | Title text size |
| `text-sm` (14px) | `font-size` | Description and button text size |

### Theme Behavior

- SheetContent uses `--background` — adapts to light/dark via theme
- SheetTitle uses `--foreground` — adapts to light/dark via theme
- SheetDescription uses `--muted-foreground` — adapts to light/dark via theme
- Border uses `--border` (implicit) — adapts to light/dark via theme
- Overlay uses fixed `bg-black/50` — consistent across themes
- All spacing, typography, and animation tokens are mode-independent
