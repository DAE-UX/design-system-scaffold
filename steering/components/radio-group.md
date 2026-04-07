# Component: Radio Group

> **TL;DR:** A set of mutually exclusive radio options. 2 parts: RadioGroup, RadioGroupItem. Built on Radix UI primitive. Group is `grid gap-3`. Item is a 16px circle with border, CircleIcon indicator (8px, `fill-primary`). Focus ring, error via `aria-invalid`. Dark mode `bg-input/30`.

## Metadata

- **Component Name:** RadioGroup
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A set of mutually exclusive radio options where only one item can be selected at a time, built on Radix UI's RadioGroup primitive.

- RadioGroup renders as a `grid` with `gap-3`
- RadioGroupItem is a 16px (`size-4`) circle with border and shadow
- Selected item shows a CircleIcon indicator (8px, `fill-primary`) centered via absolute positioning
- Focus state uses `focus-visible:border-ring` with a 3px ring (`ring-ring/50`)
- Error state via `aria-invalid` applies destructive border color
- Disabled state reduces opacity to 50% and sets `cursor-not-allowed`
- Dark mode applies `bg-input/30` on the item
- Item uses `shrink-0` to prevent flex shrinking
- Item text color is `text-primary` for the indicator

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `RadioGroup` — Root container. Grid layout for radio items.
- `RadioGroupItem` — Individual radio option. Circle with indicator on selection.

### Props

#### RadioGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Controlled selected value |
| defaultValue | `string` | — | Default selected value |
| onValueChange | `(value: string) => void` | — | Callback when selection changes |
| disabled | `boolean` | `false` | Disable all items |
| className | `string` | — | Additional CSS classes |

#### RadioGroupItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Value for this radio option |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

### Variants

No named variants — RadioGroup has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"radio-group"`, `"radio-group-item"` | All parts |
| `[data-state]` | `"checked"`, `"unchecked"` | RadioGroupItem |
| `[data-disabled]` | `""` | RadioGroupItem (when disabled) |

### CSS Variables (Radix)

Not applicable — RadioGroup does not use custom Radix CSS variables.

## Composition Patterns

### Basic Radio Group

```tsx
<RadioGroup defaultValue="option-one">
  <div className="flex items-center gap-2">
    <RadioGroupItem value="option-one" id="option-one" />
    <Label htmlFor="option-one">Option One</Label>
  </div>
  <div className="flex items-center gap-2">
    <RadioGroupItem value="option-two" id="option-two" />
    <Label htmlFor="option-two">Option Two</Label>
  </div>
</RadioGroup>
```

### With Descriptions

```tsx
<RadioGroup defaultValue="comfortable">
  <div className="flex items-start gap-2">
    <RadioGroupItem value="default" id="r1" />
    <div className="grid gap-1">
      <Label htmlFor="r1">Default</Label>
      <p className="text-sm text-muted-foreground">Standard spacing</p>
    </div>
  </div>
  <div className="flex items-start gap-2">
    <RadioGroupItem value="comfortable" id="r2" />
    <div className="grid gap-1">
      <Label htmlFor="r2">Comfortable</Label>
      <p className="text-sm text-muted-foreground">More breathing room</p>
    </div>
  </div>
</RadioGroup>
```

### Disabled State

```tsx
<RadioGroup disabled>
  <div className="flex items-center gap-2">
    <RadioGroupItem value="option-one" id="d1" />
    <Label htmlFor="d1">Disabled Option</Label>
  </div>
</RadioGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** Radiogroup (implicit via Radix)

### ARIA Roles

Radix UI RadioGroup provides built-in `radiogroup` role on the root and `radio` role on each item. Items include `aria-checked` state.

### Keyboard Behavior

- `Tab` — Moves focus into the radio group (focuses selected item, or first item if none selected)
- `Arrow Down` / `Arrow Right` — Moves focus and selection to the next item
- `Arrow Up` / `Arrow Left` — Moves focus and selection to the previous item
- `Space` — Selects the focused item (if not already selected)

## HTML

### Standalone HTML Structure

```html
<div data-slot="radio-group" role="radiogroup">
  <div>
    <button data-slot="radio-group-item" role="radio" aria-checked="true" data-state="checked">
      <span>
        <svg><!-- CircleIcon --></svg>
      </span>
    </button>
    <label>Option One</label>
  </div>
  <div>
    <button data-slot="radio-group-item" role="radio" aria-checked="false" data-state="unchecked">
    </button>
    <label>Option Two</label>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* RadioGroup */
.RadioGroup {
  display: grid;
  gap: 0.75rem;
}

/* RadioGroupItem */
.RadioGroupItem {
  aspect-ratio: 1;
  width: 1rem;
  height: 1rem;
  flex-shrink: 0;
  border-radius: 9999px;
  border: 1px solid var(--input);
  color: var(--primary);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.RadioGroupItem:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

.RadioGroupItem:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.RadioGroupItem[aria-invalid="true"] {
  border-color: var(--destructive);
}

@media (prefers-color-scheme: dark) {
  .RadioGroupItem {
    background-color: color-mix(in srgb, var(--input) 30%, transparent);
  }
}

/* RadioGroupIndicator */
.RadioGroupIndicator {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* CircleIcon */
.RadioGroupCircle {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0.5rem;
  height: 0.5rem;
  transform: translate(-50%, -50%);
  fill: var(--primary);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| RadioGroup | `grid gap-3` | Grid layout |
| RadioGroupItem | `aspect-square size-4 shrink-0 rounded-full border border-input text-primary shadow-xs` | Circle radio button |
| RadioGroupItem (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| RadioGroupItem (disabled) | `disabled:cursor-not-allowed disabled:opacity-50` | Disabled state |
| RadioGroupItem (error) | `aria-invalid:border-destructive` | Error state |
| RadioGroupItem (dark) | `dark:bg-input/30` | Dark mode background |
| RadioGroupIndicator | `relative flex items-center justify-center` | Indicator container |
| CircleIcon | `absolute top-1/2 left-1/2 size-2 -translate-x-1/2 -translate-y-1/2 fill-primary` | Selected indicator |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Item border color and dark mode background | `shadcn` |
| `--primary` | Item text color and circle indicator fill | `shadcn` |
| `--ring` | Focus ring color | `shadcn` |
| `--destructive` | Error border color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into composite and sub-components:

Radio Group (composite):
| Variant Property | Values |
|-----------------|--------|
| Error | True, False |
| Disabled | True, False |
| Orientation | Vertical, Horizontal |

.Radio Group Item (sub-component):
| Variant Property | Values |
|-----------------|--------|
| Type | Default, Card |
| Disabled | True, False |
| Error | True, False |
| Orientation | Left, Right |
| Checked | True, False |

.Radio Group Radio Toggle (sub-component):
| Variant Property | Values |
|-----------------|--------|
| Checked | True, False |
| State | Default, Hover, Focus, Disabled |

- Item → Radio Toggle (16×16, shadow-xs) + Content (label + description)
- Content uses flex column with gap 6px, label is font-medium foreground, description is font-normal muted-foreground
- Item uses gap 12px between toggle and content

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Toggle base background (at 30% opacity) and border |
| `--foreground` | `--foreground` | Label text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `size-4` (16px) | `width` / `height` | Toggle size |
| `rounded-full` | `border-radius` | Toggle rounding |
| `gap-3` (12px) | `gap` | Gap between toggle and content |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Label font weight |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-xs` | `box-shadow` | Toggle shadow |

### Theme Behavior

- Item border uses `--input` — adapts to light/dark via theme
- Indicator uses `--primary` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error state uses `--destructive` — adapts to light/dark via theme
- Dark mode applies `--input` at 30% opacity for background
- Shadow is fixed (`shadow-xs`) — same in both modes
- All spacing and border-radius tokens are mode-independent
