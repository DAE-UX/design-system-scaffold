# Component: Alert Dialog

> **TL;DR:** A modal dialog that interrupts the user with important content and expects a response. Built on Radix UI AlertDialog primitive via shadCN. Supports `default` and `sm` sizes, optional media element, destructive action pattern, and RTL. Composed of 12 parts. Focus is automatically trapped. Esc closes the dialog.

## Metadata

- **Component Name:** Alert Dialog
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A modal dialog that interrupts the user with important content and expects a response. Used for destructive confirmations, irreversible actions, and critical decisions that require explicit user acknowledgment.

- Modal — blocks interaction with the rest of the page while open
- Focus is automatically trapped inside the dialog when open
- Can be controlled (`open`/`onOpenChange`) or uncontrolled (`defaultOpen`)
- Esc closes the dialog automatically and returns focus to the trigger
- Manages screen reader announcements via `Title` and `Description` components
- Supports two sizes: `default` (max-width `lg`) and `sm` (max-width `xs`)
- Supports optional media element (icon or image) via `AlertDialogMedia`
- Action and Cancel buttons render as `Button` components with configurable `variant` and `size`
- Cancel defaults to `outline` variant; Action defaults to `default` variant
- Supports RTL layout via the Direction component
- Portals content to `document.body` by default (configurable via `container` prop on Portal)

### Anti-Patterns

#### Do
- Use for destructive or irreversible actions that require explicit confirmation.
- Always provide both a confirm and cancel action.
- Use clear, specific action labels (not "OK"/"Cancel").

#### Don't
- Don't use for non-destructive confirmations — use a regular dialog.
- Don't chain alert dialogs.
- Don't auto-dismiss alert dialogs — require explicit user action.

Sources: shadCN, Radix UI

## API

### Parts

- `AlertDialog` — Root container. Wraps `AlertDialogPrimitive.Root`.
- `AlertDialogTrigger` — Button that opens the dialog. Supports `asChild` for custom trigger elements.
- `AlertDialogPortal` — Portals overlay and content into `document.body`.
- `AlertDialogOverlay` — Semi-transparent layer covering the page behind the dialog.
- `AlertDialogContent` — Contains the dialog content. Centered on screen with open/close animations.
- `AlertDialogHeader` — Layout container for title, description, and optional media. Grid-based layout.
- `AlertDialogFooter` — Layout container for action and cancel buttons.
- `AlertDialogTitle` — Accessible dialog title. Announced by screen readers on open.
- `AlertDialogDescription` — Accessible dialog description. Announced by screen readers on open.
- `AlertDialogMedia` — Optional container for an icon or image displayed in the header.
- `AlertDialogAction` — Confirms the action and closes the dialog. Renders as a `Button`.
- `AlertDialogCancel` — Cancels and closes the dialog. Renders as a `Button` with `outline` variant.

### Props

#### AlertDialog (Root)

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| defaultOpen | `boolean` | — | Initial open state (uncontrolled) |
| open | `boolean` | — | Controlled open state |
| onOpenChange | `(open: boolean) => void` | — | Callback when open state changes |

#### AlertDialogTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Merge props onto child element instead of rendering a default button |

#### AlertDialogPortal

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| forceMount | `boolean` | — | Forces mounting (useful with animation libraries) |
| container | `HTMLElement` | `document.body` | Target container for the portal |

#### AlertDialogOverlay

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Merge props onto child element |
| forceMount | `boolean` | — | Forces mounting (useful with animation libraries) |

#### AlertDialogContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm"` | `"default"` | Controls dialog width. `default` = max-width `lg`, `sm` = max-width `xs` |
| asChild | `boolean` | `false` | Merge props onto child element |
| forceMount | `boolean` | — | Forces mounting (useful with animation libraries) |
| onOpenAutoFocus | `(event: Event) => void` | — | Callback when focus moves into the dialog on open |
| onCloseAutoFocus | `(event: Event) => void` | — | Callback when focus moves back to trigger on close |
| onEscapeKeyDown | `(event: KeyboardEvent) => void` | — | Callback when Escape key is pressed |

#### AlertDialogAction

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `ButtonVariant` | `"default"` | Button visual variant (inherits from Button component) |
| size | `ButtonSize` | `"default"` | Button size (inherits from Button component) |
| asChild | `boolean` | `false` | Merge props onto child element |

#### AlertDialogCancel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `ButtonVariant` | `"outline"` | Button visual variant (inherits from Button component) |
| size | `ButtonSize` | `"default"` | Button size (inherits from Button component) |
| asChild | `boolean` | `false` | Merge props onto child element |

#### AlertDialogTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Merge props onto child element |

#### AlertDialogDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Merge props onto child element |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="default"` | Standard dialog width (max-width `lg` on sm+ screens). Header left-aligned on sm+ screens. | General confirmations, detailed messages |
| `size="sm"` | Compact dialog width (max-width `xs`). Header centered. Footer uses 2-column grid. | Simple yes/no confirmations, quick decisions |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-state]` | `"open" \| "closed"` | Trigger, Overlay, Content |
| `[data-size]` | `"default" \| "sm"` | Content |
| `[data-slot]` | Component identifier string | All parts |

### CSS Variables (Radix)

No component-specific Radix CSS variables. AlertDialog uses standard positioning (fixed, centered via translate) rather than Radix CSS variable-based sizing.

## Composition Patterns

### Basic Alert Dialog

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button variant="outline">Show Dialog</Button>
  </AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogTitle>Are you absolutely sure?</AlertDialogTitle>
      <AlertDialogDescription>
        This action cannot be undone. This will permanently delete your
        account and remove your data from our servers.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Continue</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### Small Size

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button variant="outline">Delete</Button>
  </AlertDialogTrigger>
  <AlertDialogContent size="sm">
    <AlertDialogHeader>
      <AlertDialogTitle>Delete item?</AlertDialogTitle>
      <AlertDialogDescription>
        This action cannot be undone.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Delete</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### With Media (Icon)

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button variant="outline">Show Dialog</Button>
  </AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogMedia>
        <TrashIcon />
      </AlertDialogMedia>
      <AlertDialogTitle>Delete your account?</AlertDialogTitle>
      <AlertDialogDescription>
        This will permanently remove all your data.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Delete</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### Destructive Action

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button variant="destructive">Delete Account</Button>
  </AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogTitle>Are you absolutely sure?</AlertDialogTitle>
      <AlertDialogDescription>
        This action cannot be undone. This will permanently delete your
        account and remove your data from our servers.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction variant="destructive">
        Yes, delete account
      </AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### Controlled (Async Form Submission)

```tsx
const [open, setOpen] = React.useState(false);

<AlertDialog open={open} onOpenChange={setOpen}>
  <AlertDialogTrigger asChild>
    <Button>Open</Button>
  </AlertDialogTrigger>
  <AlertDialogContent>
    <form
      onSubmit={(event) => {
        event.preventDefault();
        submitAsync().then(() => setOpen(false));
      }}
    >
      <AlertDialogHeader>
        <AlertDialogTitle>Confirm submission</AlertDialogTitle>
        <AlertDialogDescription>
          Are you sure you want to submit this form?
        </AlertDialogDescription>
      </AlertDialogHeader>
      <AlertDialogFooter>
        <AlertDialogCancel>Cancel</AlertDialogCancel>
        <button type="submit">Submit</button>
      </AlertDialogFooter>
    </form>
  </AlertDialogContent>
</AlertDialog>
```

### Small with Media

```tsx
<AlertDialog>
  <AlertDialogTrigger asChild>
    <Button variant="outline" size="sm">Remove</Button>
  </AlertDialogTrigger>
  <AlertDialogContent size="sm">
    <AlertDialogHeader>
      <AlertDialogMedia>
        <AlertCircleIcon />
      </AlertDialogMedia>
      <AlertDialogTitle>Remove item?</AlertDialogTitle>
      <AlertDialogDescription>
        This cannot be undone.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Remove</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

## Accessibility

- **WAI-ARIA Pattern:** [Alert and Message Dialogs](https://www.w3.org/WAI/ARIA/apg/patterns/alertdialog)
- Adheres to the WAI-ARIA Alert Dialog design pattern

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Content | `alertdialog` (implicit via Radix) | Identifies the element as an alert dialog |
| Title | `aria-labelledby` target | Provides accessible name for the dialog |
| Description | `aria-describedby` target | Provides accessible description for the dialog |
| Trigger | `button` | Opens the dialog |
| Action | `button` | Confirms the action |
| Cancel | `button` | Cancels and closes the dialog |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Space | Opens the dialog (on trigger) / Activates focused button |
| Enter | Opens the dialog (on trigger) / Activates focused button |
| Tab | Moves focus to the next focusable element within the dialog (trapped) |
| Shift + Tab | Moves focus to the previous focusable element within the dialog (trapped) |
| Esc | Closes the dialog and returns focus to the trigger |

## HTML

### Standalone HTML Structure

```html
<!-- Overlay -->
<div data-state="open" style="position: fixed; inset: 0; z-index: 50;">
</div>
<!-- Content -->
<div
  role="alertdialog"
  data-state="open"
  data-size="default"
  style="position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 50;"
>
  <!-- Header -->
  <div>
    <h2>Dialog title</h2>
    <p>Dialog description text.</p>
  </div>
  <!-- Footer -->
  <div>
    <button>Cancel</button>
    <button>Continue</button>
  </div>
</div>
```

### With Media

```html
<div
  role="alertdialog"
  data-state="open"
  data-size="default"
>
  <div>
    <!-- Media -->
    <div><svg><!-- icon --></svg></div>
    <h2>Dialog title</h2>
    <p>Dialog description text.</p>
  </div>
  <div>
    <button>Cancel</button>
    <button>Continue</button>
  </div>
</div>
```

### Small Size

```html
<div
  role="alertdialog"
  data-state="open"
  data-size="sm"
>
  <div>
    <h2>Dialog title</h2>
    <p>Dialog description text.</p>
  </div>
  <div>
    <button>Cancel</button>
    <button>Continue</button>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Overlay */
.AlertDialogOverlay {
  position: fixed;
  inset: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.5);
}

/* Content */
.AlertDialogContent {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 50;
  display: grid;
  width: 100%;
  max-width: calc(100% - 2rem);
  transform: translate(-50%, -50%);
  gap: 1rem;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  background-color: var(--background);
  padding: 1.5rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  transition-duration: 200ms;
}

/* Content size: default (sm+ screens) */
@media (min-width: 640px) {
  .AlertDialogContent[data-size="default"] {
    max-width: 32rem; /* lg */
  }
}

/* Content size: sm */
.AlertDialogContent[data-size="sm"] {
  max-width: 20rem; /* xs */
}

/* Header */
.AlertDialogHeader {
  display: grid;
  grid-template-rows: auto 1fr;
  place-items: center;
  gap: 0.375rem;
  text-align: center;
}

/* Header: default size on sm+ screens — left-aligned */
@media (min-width: 640px) {
  .AlertDialogContent[data-size="default"] .AlertDialogHeader {
    place-items: start;
    text-align: left;
  }
}

/* Footer */
.AlertDialogFooter {
  display: flex;
  flex-direction: column-reverse;
  gap: 0.5rem;
}

/* Footer: sm+ screens — row layout */
@media (min-width: 640px) {
  .AlertDialogFooter {
    flex-direction: row;
    justify-content: flex-end;
  }
}

/* Footer: sm size — 2-column grid */
.AlertDialogContent[data-size="sm"] .AlertDialogFooter {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

/* Title */
.AlertDialogTitle {
  font-size: 1.125rem;
  font-weight: 600;
}

/* Description */
.AlertDialogDescription {
  font-size: 0.875rem;
  color: var(--muted-foreground);
}

/* Media */
.AlertDialogMedia {
  margin-bottom: 0.5rem;
  display: inline-flex;
  width: 4rem;
  height: 4rem;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius);
  background-color: var(--muted);
}

/* Open/close animations */
.AlertDialogOverlay[data-state="open"] {
  animation: fadeIn 200ms ease-out;
}
.AlertDialogOverlay[data-state="closed"] {
  animation: fadeOut 200ms ease-in;
}
.AlertDialogContent[data-state="open"] {
  animation: fadeIn 200ms ease-out, zoomIn 200ms ease-out;
}
.AlertDialogContent[data-state="closed"] {
  animation: fadeOut 200ms ease-in, zoomOut 200ms ease-in;
}

@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
@keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }
@keyframes zoomIn { from { transform: translate(-50%, -50%) scale(0.95); } to { transform: translate(-50%, -50%) scale(1); } }
@keyframes zoomOut { from { transform: translate(-50%, -50%) scale(1); } to { transform: translate(-50%, -50%) scale(0.95); } }
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Overlay | `fixed inset-0 z-50 bg-black/50` | Full-screen semi-transparent backdrop |
| Overlay (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0` | Fade-in animation |
| Overlay (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0` | Fade-out animation |
| Content | `fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border bg-background p-6 shadow-lg duration-200` | Centered modal layout |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Close animation |
| Content (default size) | `data-[size=default]:sm:max-w-lg` | Default max-width on sm+ screens |
| Content (sm size) | `data-[size=sm]:max-w-xs` | Compact max-width |
| Header | `grid grid-rows-[auto_1fr] place-items-center gap-1.5 text-center` | Centered grid layout |
| Footer | `flex flex-col-reverse gap-2` | Stacked buttons (mobile) |
| Footer (sm+) | `sm:flex-row sm:justify-end` | Row layout on larger screens |
| Title | `text-lg font-semibold` | Dialog title typography |
| Description | `text-sm text-muted-foreground` | Subdued description text |
| Media | `mb-2 inline-flex size-16 items-center justify-center rounded-md bg-muted` | Icon/image container |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Content background color | `shadcn` |
| `--border` | Content border color | `shadcn` |
| `--muted` | Media container background | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |
| `--radius` | Content and media border radius | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Breakpoint | Medium and up, Small |

Default breakpoint layout:
- AlertDialogContent (flex column, gap 16px, padding 24px) + AlertDialogFooter (flex row, gap 8px, justify-end)
- AlertDialogContent → Title (text-lg, font-semibold) + Description (text-sm, muted-foreground)
- AlertDialogFooter → Secondary Button (outline variant) + Primary Button (primary variant)
- Container: max-width 512px, border-radius derived from `--radius`, border 1px solid `--border`, shadow-lg

Small breakpoint layout:
- Same structure, but content and footer are center-aligned
- Footer uses 2-column grid layout instead of flex row

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable / Tailwind Utility | Purpose |
|-------|--------------------------------|---------|
| `var(--background)` | `--background` / `bg-background` | Content background |
| `var(--border)` | `--border` / `border-border` | Content border color |
| `var(--foreground)` | `--foreground` / `text-foreground` | Title text color, secondary button text |
| `var(--muted-foreground)` | `--muted-foreground` / `text-muted-foreground` | Description text color |
| `var(--primary)` | `--primary` / `bg-primary` | Primary button background |
| `var(--primary-foreground)` | `--primary-foreground` / `text-primary-foreground` | Primary button text |
| `var(--input)` | `--input` / `border-input` | Secondary button border |
| `--radius` (derived) | `--radius` | Content and button border radius |
| `p-6` (24px) | `p-6` | Content padding |
| `gap-4` (16px) | `gap-4` | Gap between header and footer |
| `gap-2` (8px) | `gap-2` | Gap between title/description, button gap |
| `font-sans` | `--font-sans` / `font-sans` | Font family |
| `font-semibold` (600) | `font-semibold` | Title font weight |
| `font-medium` (500) | `font-medium` | Button text font weight |
| `text-lg` (18px) | `text-lg` | Title font size |
| `text-sm` (14px) | `text-sm` | Description and button font size |
| `shadow-lg` | `shadow-lg` | Content box shadow |

### Theme Behavior

- Content uses `--background` / `--border` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Media container uses `--muted` background — adapts to light/dark via theme
- Overlay uses `bg-black/50` (fixed opacity, not theme-variable-dependent)
- Action and Cancel buttons inherit theme behavior from the Button component
- Background is explicit (`--background`) on the content panel — does not inherit from parent
- All spacing, typography, and layout tokens are mode-independent
