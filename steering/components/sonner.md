# Component: Sonner

> **TL;DR:** A toast notification system. 1 exported part: Toaster. Wraps the Sonner library's Toaster with theme integration via `next-themes` and custom status icons. Maps CSS variables for background, text, border, and border-radius. Provides success, info, warning, error, and loading icon overrides. Used for transient feedback messages.

## Metadata

- **Component Name:** Sonner
- **Source:** shadCN
- **Dependencies:** `sonner`, `next-themes`

## Behavior

A toast notification provider that wraps the Sonner library with theme-aware styling and custom icons.

- Integrates with `next-themes` via `useTheme()` to pass the current theme to Sonner
- Overrides default Sonner icons with custom Lucide icons (all `size-4`):
  - Success: `CircleCheckIcon`
  - Info: `InfoIcon`
  - Warning: `TriangleAlertIcon`
  - Error: `OctagonXIcon`
  - Loading: `Loader2Icon` with `animate-spin`
- Maps design system CSS variables to Sonner's internal variables via the `style` prop:
  - `--normal-bg` → `var(--popover)`
  - `--normal-text` → `var(--popover-foreground)`
  - `--normal-border` → `var(--border)`
  - `--border-radius` → `var(--radius)`
- Applies `className="toaster group"` to the Sonner Toaster
- All additional Sonner Toaster props are forwarded

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Toaster` — Toast notification provider. Wraps Sonner's Toaster with theme and icon overrides.

### Props

#### Toaster

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| ...props | `ComponentProps<typeof SonnerToaster>` | — | All Sonner Toaster props are forwarded |

Sonner Toaster props include `position`, `duration`, `richColors`, `closeButton`, `expand`, `visibleToasts`, etc. See Sonner documentation for full API.

### Variants

No named variants. Toast types (success, info, warning, error, loading) are controlled via the `toast()` function API from Sonner.

### Data Attributes

No `data-slot` attributes. Sonner manages its own DOM attributes internally.

### CSS Variables (Radix)

Not applicable — Sonner does not use Radix primitives.

## Composition Patterns

### Provider Setup

```tsx
// In your root layout
import { Toaster } from "@/components/ui/sonner"

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Toaster />
      </body>
    </html>
  )
}
```

### Trigger Toasts

```tsx
import { toast } from "sonner"

// Success
toast.success("Changes saved successfully.")

// Error
toast.error("Something went wrong.")

// Info
toast.info("New update available.")

// Warning
toast.warning("This action cannot be undone.")

// Loading
toast.loading("Saving changes...")

// With description
toast.success("File uploaded", {
  description: "Your file has been uploaded successfully.",
})

// With action
toast("Event created", {
  action: {
    label: "Undo",
    onClick: () => console.log("Undo"),
  },
})
```

## Accessibility

- **WAI-ARIA Pattern:** Alert (status messages)

### ARIA Roles

- Sonner renders toasts within a region with `role="status"` and `aria-live="polite"` for non-urgent toasts
- Error and warning toasts use `aria-live="assertive"` for immediate announcement
- Each toast is announced to screen readers when it appears

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Escape` | Dismisses the focused toast (Sonner default) |

Toast interactions (action buttons, close buttons) follow standard button keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<!-- Toaster container (rendered by Sonner) -->
<section aria-label="Notifications" tabindex="-1">
  <ol>
    <li role="status" aria-live="polite" data-sonner-toast>
      <div>
        <svg><!-- status icon --></svg>
        <div>Toast message</div>
      </div>
    </li>
  </ol>
</section>
```

## CSS

### Raw CSS

```css
/* Sonner variable mapping (applied via style prop) */
.toaster {
  --normal-bg: var(--popover);
  --normal-text: var(--popover-foreground);
  --normal-border: var(--border);
  --border-radius: var(--radius);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Toaster | `toaster group` | Container className for Sonner |

Sonner manages its own internal styling. The design system integration is achieved via CSS variable mapping in the `style` prop rather than Tailwind classes.

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Toast background color (mapped to `--normal-bg`) | `shadcn` |
| `--popover-foreground` | Toast text color (mapped to `--normal-text`) | `shadcn` |
| `--border` | Toast border color (mapped to `--normal-border`) | `shadcn` |
| `--radius` | Toast border radius (mapped to `--border-radius`) | `shadcn` |

## Theme Support

### Component Structure

Component organizes Sonner (Toast) into variant states and examples:

| Variant Property | Values |
|-----------------|--------|
| Type | Default |

- Content area: Title (`font-medium`, `text-sm`) + Description (`font-normal`, `text-sm`)
- Action area: Button sub-component (`bg-primary`, `text-primary-foreground`, `rounded-4px`, `text-xs`, `font-medium`)
- Container uses shadow-lg, rounded, border
- Background: popover


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--popover` | `--popover` | Toast background |
| `--foreground` | `--foreground` | Title text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--border` | `--border` | Toast border |
| `--primary` | `--primary` | Action button background |
| `--radius` | `border-radius` | Toast border radius |
| `p-4` (16px) | `padding` | Toast padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Title font weight |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Toast background uses `--popover` — adapts to light/dark via theme (passed through `useTheme()`)
- Toast text uses `--popover-foreground` — adapts to light/dark via theme
- Toast border uses `--border` — adapts to light/dark via theme
- Toast border radius uses `--radius` — consistent across themes
- Theme is passed to Sonner via `next-themes` `useTheme()` hook, ensuring toasts match the current application theme
