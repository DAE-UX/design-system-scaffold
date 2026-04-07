# Component: Alert

> **TL;DR:** A callout component for displaying important messages. No external primitive dependency — pure shadCN. Supports `default` and `destructive` variants. Composed of `Alert`, `AlertTitle`, `AlertDescription`, and `AlertAction` parts. Accepts Lucide icons as first child. Supports RTL and custom color overrides via className.

## Metadata

- **Component Name:** Alert
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

Displays a callout message to attract user attention. Used for success confirmations, error messages, warnings, and informational notices.

- Static display component — no interactive state changes
- Accepts an optional icon as the first child (Lucide icons recommended)
- Supports `default` and `destructive` visual variants
- Supports RTL layout via the Direction component
- Custom colors can be applied via className overrides (e.g., `bg-amber-50 dark:bg-amber-950`)

### Anti-Patterns

#### Do
- Use alerts to convey severity and urgency within context
- When multiple alerts exist, order by urgency: error → warning → info → success

#### Don't
- Don't stack multiple alert types on the same page unless necessary
- Don't use alerts for non-contextual, persistent messaging — use a banner instead
- Don't omit the icon — it reinforces the severity level

Sources: shadCN, Radix UI

## API

### Parts

- `Alert` — Root container. Renders a `div` with `role="alert"`.
- `AlertTitle` — Displays the alert heading.
- `AlertDescription` — Displays the alert body content.
- `AlertAction` — Positions an action element (e.g., button) in the top-right corner of the alert.

### Props

#### Alert

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "destructive"` | `"default"` | Visual style variant |
| className | `string` | — | Additional CSS classes for custom styling |
| children | `ReactNode` | — | Icon, AlertTitle, AlertDescription, AlertAction |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Standard alert styling with border and background | Success messages, informational notices |
| `destructive` | Error/danger styling with destructive color tokens | Error messages, critical warnings |

### Data Attributes

No data attributes — Alert is a static display component without interactive state.

### CSS Variables (Radix)

Not applicable — Alert does not use Radix primitives.

## Composition Patterns

### Basic Alert (Icon + Title + Description)

```tsx
<Alert>
  <CheckCircle2Icon />
  <AlertTitle>Success! Your changes have been saved</AlertTitle>
  <AlertDescription>
    This is an alert with icon, title and description.
  </AlertDescription>
</Alert>
```

### Destructive Variant

```tsx
<Alert variant="destructive">
  <AlertCircleIcon />
  <AlertTitle>Unable to process your payment.</AlertTitle>
  <AlertDescription>
    Please verify your billing information and try again.
  </AlertDescription>
</Alert>
```

### With Action Button

```tsx
<Alert>
  <InfoIcon />
  <AlertTitle>Dark mode is now available</AlertTitle>
  <AlertDescription>
    Enable it under your profile settings to get started.
  </AlertDescription>
  <AlertAction>
    <Button variant="outline">Enable</Button>
  </AlertAction>
</Alert>
```

### Custom Colors via className

```tsx
<Alert className="bg-amber-50 dark:bg-amber-950">
  <AlertTriangleIcon />
  <AlertTitle>Your subscription will expire in 3 days.</AlertTitle>
  <AlertDescription>
    Renew now to avoid service interruption.
  </AlertDescription>
</Alert>
```

## Accessibility

- **WAI-ARIA Pattern:** Uses `role="alert"` on the root element

### ARIA Roles

| Element | Role | Notes |
|---------|------|-------|
| Alert (root) | `alert` | Automatically announces content to assistive technology |

### Keyboard Behavior

No keyboard interaction — Alert is a static display component.

## HTML

### Standalone HTML Structure

```html
<div role="alert" data-variant="default">
  <svg><!-- Lucide icon --></svg>
  <h5>Alert title</h5>
  <div>Alert description text.</div>
</div>
```

## CSS

### Raw CSS

```css
.Alert {
  position: relative;
  width: 100%;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  padding: 1rem;
}
.Alert[data-variant="default"] {
  background-color: var(--background);
  color: var(--foreground);
}
.Alert[data-variant="destructive"] {
  border-color: var(--destructive);
  color: var(--destructive);
}
.Alert > svg {
  position: absolute;
  left: 1rem;
  top: 1rem;
  width: 1rem;
  height: 1rem;
}
.AlertTitle {
  margin-bottom: 0.25rem;
  font-weight: 500;
  line-height: 1;
  letter-spacing: -0.01em;
}
.AlertDescription {
  font-size: 0.875rem;
  line-height: 1.625;
}
.AlertAction {
  position: absolute;
  right: 1rem;
  top: 0.875rem;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Alert (base) | `relative w-full rounded-lg border px-4 py-3 text-sm` | Layout and sizing |
| Alert (default) | `bg-background text-foreground` | Default color tokens |
| Alert (destructive) | `border-destructive/50 text-destructive dark:border-destructive` | Destructive color tokens |
| Alert > svg (icon) | `absolute left-4 top-4 size-4` | Icon positioning |
| AlertTitle | `mb-1 font-medium leading-none tracking-tight` | Title typography |
| AlertDescription | `text-sm [&_p]:leading-relaxed` | Description typography |
| AlertAction | `absolute right-4 top-3.5` | Action positioning |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Default variant background | `shadcn` |
| `--foreground` | Default variant text color | `shadcn` |
| `--border` | Alert border color (default variant) | `shadcn` |
| `--destructive` | Destructive variant text and border color | `shadcn` |
| `--radius` | Border radius | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Error |

Default layout: Icon wrapper (16×16) + Body (Title + Description, flex column, gap 4px)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--card` | `--card` | Alert background (default variant) |
| `--border` | `--border` | Alert border |
| `--foreground` | `--foreground` | Title text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--radius` (derived) | `--radius` | Border radius |
| `p-4` (16px) | `padding` | Alert padding |
| `gap-3` (12px) | `gap` | Gap between icon and body |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Title font weight |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Default variant uses `--background` / `--foreground` / `--border` — adapts to light/dark via theme
- Destructive variant uses `--destructive` for text and border — adapts to light/dark via theme
- Destructive border uses `--destructive/50` opacity in light mode, full `--destructive` in dark mode
- Icon inherits text color from the variant
- Background is transparent for destructive variant (inherits from parent)
- Custom colors via className override both light and dark values
