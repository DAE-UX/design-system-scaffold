# Component: Label

> **TL;DR:** An accessible label for form controls. 1 part: Label. Built on Radix UI Label primitive. Renders as a native `label` element with `htmlFor` support. Inline-flex with gap for icons/badges. Disabled state via parent `data-disabled` or peer `:disabled`. `select-none` to prevent accidental text selection during clicks.

## Metadata

- **Component Name:** Label
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

An accessible label element for form controls, built on Radix UI Label primitive.

- Renders as a native `label` element via Radix Label
- Supports `htmlFor` to associate with a form control
- Clicking the label focuses the associated control (native behavior)
- Uses `inline-flex` with `gap-2` for inline icons or badges alongside text
- `select-none` prevents accidental text selection when clicking to focus the control
- Disabled state: when parent has `data-disabled="true"`, label gets `pointer-events-none` and `opacity-50`
- Peer disabled: when a sibling input is `:disabled`, label gets `cursor-not-allowed` and `opacity-50`
- Typography: `text-sm`, `leading-none`, `font-medium`

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Label` — Label element. Renders as native `label` via Radix.

### Props

#### Label

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| htmlFor | `string` | — | ID of the associated form control |
| className | `string` | — | Additional CSS classes |

All native `label` props are supported.

### Variants

No variants — Label has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"label"` | Label |

### CSS Variables (Radix)

Not applicable — Label does not use Radix CSS variables.

## Composition Patterns

### Basic Label

```tsx
<Label htmlFor="email">Email</Label>
<Input id="email" type="email" />
```

### With Required Indicator

```tsx
<Label htmlFor="name">
  Name <span className="text-destructive">*</span>
</Label>
<Input id="name" required />
```

### With Badge

```tsx
<Label htmlFor="plan">
  Plan <Badge variant="secondary">Pro</Badge>
</Label>
```

## Accessibility

- **WAI-ARIA Pattern:** None — uses native `label` element semantics

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Label | native `label` | Provides accessible name for associated control via `htmlFor` |

### Keyboard Behavior

No keyboard interaction — Label is a presentational element. Clicking the label focuses the associated control (native browser behavior).

## HTML

### Standalone HTML Structure

```html
<label data-slot="label" for="email">Email</label>
<input id="email" type="email" />
```

## CSS

### Raw CSS

```css
/* Label */
.Label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  line-height: 1;
  font-weight: 500;
  user-select: none;
}

/* Disabled via parent */
[data-disabled="true"] .Label {
  pointer-events: none;
  opacity: 0.5;
}

/* Disabled via peer */
.Label:has(~ :disabled),
:disabled ~ .Label {
  cursor: not-allowed;
  opacity: 0.5;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Label | `flex items-center gap-2 text-sm leading-none font-medium select-none` | Label layout |
| Label (parent disabled) | `group-data-[disabled=true]:pointer-events-none group-data-[disabled=true]:opacity-50` | Disabled via parent |
| Label (peer disabled) | `peer-disabled:cursor-not-allowed peer-disabled:opacity-50` | Disabled via sibling |

## CSS Variable Dependencies

No CSS variable dependencies — Label uses only inherited text color.

## Theme Support

### Component Structure

Label is a simple text element. It is typically included within the Field component for form layouts.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Label text color |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Label font weight |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Label inherits text color from parent — adapts to light/dark via theme
- Disabled opacity (0.5) is mode-independent
- All typography tokens are mode-independent
