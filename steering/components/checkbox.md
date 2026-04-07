# Component: Checkbox

> **TL;DR:** A control that allows the user to toggle between checked and unchecked states. Single part (`Checkbox`) wrapping Radix UI Checkbox primitive. Supports checked, unchecked, and indeterminate states. Renders a 16px square with a check icon indicator. Pairs with Label and Field components for form usage. Supports controlled and uncontrolled modes, disabled state, and `aria-invalid` for error indication.

## Metadata

- **Component Name:** Checkbox
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A control that allows the user to toggle between checked and unchecked states.

- Renders as a 16px (`size-4`) square button with rounded corners (`rounded-[4px]`)
- Displays a `CheckIcon` (14px) when checked via Radix `Indicator`
- Supports three states: unchecked, checked, and indeterminate
- Controlled mode: use `checked` and `onCheckedChange` props
- Uncontrolled mode: use `defaultChecked` prop
- Disabled state: `cursor-not-allowed` and `opacity: 0.5`
- Focus-visible ring for keyboard navigation
- Supports `aria-invalid` for error/invalid state styling
- In dark mode, unchecked background uses `--input/30`; checked uses `--primary`
- Commonly paired with `Label` (via matching `id`/`htmlFor`) and `Field` components
- Can be wrapped in a `Label` for card-style checkbox patterns
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use checkboxes for selecting any number of options (zero, one, or several)
- Each checkbox is independent
- Use indeterminate state only on parent of nested group when some children are selected
- Use for toggling groups of elements on/off (progressive disclosure)

#### Don't
- Don't use for options requiring descriptions of both on/off states — use radio group or tiles
- Don't use for immediate-effect toggles (light/dark mode) — use switch
- Don't label a checkbox as optional

Sources: shadCN, Radix UI

## API

### Parts

- `Checkbox` — Root toggle control. Renders a `button` with Radix Checkbox primitive. Contains the `Indicator` internally.

### Props

#### Checkbox

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| checked | `boolean \| "indeterminate"` | — | Controlled checked state |
| defaultChecked | `boolean` | — | Default checked state (uncontrolled) |
| onCheckedChange | `(checked: boolean \| "indeterminate") => void` | — | Called when checked state changes |
| disabled | `boolean` | `false` | Disables the checkbox |
| required | `boolean` | `false` | Marks as required for form validation |
| name | `string` | — | Name for form submission |
| value | `string` | `"on"` | Value for form submission |
| className | `string` | — | Additional CSS classes |

### Variants

No style variants — Checkbox has a single visual style. Custom colors can be applied via className overrides on `data-[state=checked]`.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"checkbox"`, `"checkbox-indicator"` | Checkbox, Indicator |
| `[data-state]` | `"checked"`, `"unchecked"`, `"indeterminate"` | Checkbox |
| `[data-disabled]` | present when disabled | Checkbox |

### CSS Variables (Radix)

Not applicable — Checkbox does not expose Radix CSS variables.

## Composition Patterns

### Basic with Label

```tsx
<div className="flex items-center gap-3">
  <Checkbox id="terms" />
  <Label htmlFor="terms">Accept terms and conditions</Label>
</div>
```

### With Description

```tsx
<div className="flex items-start gap-3">
  <Checkbox id="terms-2" defaultChecked />
  <div className="grid gap-2">
    <Label htmlFor="terms-2">Accept terms and conditions</Label>
    <p className="text-sm text-muted-foreground">
      By clicking this checkbox, you agree to the terms and conditions.
    </p>
  </div>
</div>
```

### Disabled

```tsx
<div className="flex items-start gap-3">
  <Checkbox id="toggle" disabled />
  <Label htmlFor="toggle">Enable notifications</Label>
</div>
```

### Card-Style (Wrapped in Label)

```tsx
<Label className="flex items-start gap-3 rounded-lg border p-3 hover:bg-accent/50 has-[[aria-checked=true]]:border-blue-600 has-[[aria-checked=true]]:bg-blue-50">
  <Checkbox id="toggle-2" defaultChecked />
  <div className="grid gap-1.5 font-normal">
    <p className="text-sm leading-none font-medium">Enable notifications</p>
    <p className="text-sm text-muted-foreground">
      You can enable or disable notifications at any time.
    </p>
  </div>
</Label>
```

### Controlled

```tsx
const [checked, setChecked] = React.useState(false)

<Checkbox checked={checked} onCheckedChange={setChecked} />
```

### Invalid State

```tsx
<Checkbox aria-invalid />
```

## Accessibility

- **WAI-ARIA Pattern:** [Checkbox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/checkbox)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Checkbox | `checkbox` (implicit via Radix) | Native checkbox semantics |
| Checkbox (invalid) | `aria-invalid` | Triggers destructive ring styling |
| Checkbox + Label | `id` / `htmlFor` pairing | Required for accessible labeling |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Space | Toggles the checkbox state |
| Tab | Moves focus to the next focusable element |
| Shift + Tab | Moves focus to the previous focusable element |

## HTML

### Standalone HTML Structure

```html
<button data-slot="checkbox" data-state="unchecked" role="checkbox" aria-checked="false">
  <span data-slot="checkbox-indicator" style="display: none;">
    <svg><!-- check icon --></svg>
  </span>
</button>
```

### Checked State

```html
<button data-slot="checkbox" data-state="checked" role="checkbox" aria-checked="true">
  <span data-slot="checkbox-indicator">
    <svg><!-- check icon --></svg>
  </span>
</button>
```

### With Label

```html
<div>
  <button data-slot="checkbox" data-state="unchecked" role="checkbox" aria-checked="false" id="terms"></button>
  <label for="terms">Accept terms and conditions</label>
</div>
```

## CSS

### Raw CSS

```css
/* Checkbox */
.Checkbox {
  width: 1rem;
  height: 1rem;
  flex-shrink: 0;
  border-radius: 4px;
  border: 1px solid var(--input);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: box-shadow;
  outline: none;
}

/* Focus visible */
.Checkbox:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Disabled */
.Checkbox:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* Invalid */
.Checkbox[aria-invalid] {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

/* Checked */
.Checkbox[data-state="checked"] {
  border-color: var(--primary);
  background-color: var(--primary);
  color: var(--primary-foreground);
}

/* Indicator */
.CheckboxIndicator {
  display: grid;
  place-content: center;
  color: currentColor;
}

/* Indicator icon */
.CheckboxIndicator svg {
  width: 0.875rem;
  height: 0.875rem;
}
```

### Tailwind Mapping

| Element / State | Tailwind Classes | Purpose |
|-----------------|-----------------|---------|
| Checkbox (base) | `peer size-4 shrink-0 rounded-[4px] border border-input shadow-xs transition-shadow outline-none` | Base layout and border |
| Checkbox (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| Checkbox (disabled) | `disabled:cursor-not-allowed disabled:opacity-50` | Disabled state |
| Checkbox (invalid) | `aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40` | Error state |
| Checkbox (checked) | `data-[state=checked]:border-primary data-[state=checked]:bg-primary data-[state=checked]:text-primary-foreground` | Checked colors |
| Checkbox (dark unchecked) | `dark:bg-input/30` | Dark mode unchecked background |
| Checkbox (dark checked) | `dark:data-[state=checked]:bg-primary` | Dark mode checked background |
| Indicator | `grid place-content-center text-current transition-none` | Center the check icon |
| Indicator icon | `size-3.5` | 14px check icon |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Unchecked border color | `shadcn` |
| `--ring` | Focus-visible ring | `shadcn` |
| `--primary` | Checked background and border | `shadcn` |
| `--primary-foreground` | Checked icon color | `shadcn` |
| `--destructive` | Invalid state border and ring | `shadcn` |

## Theme Support

### Component Structure

Checkbox has three layout types and a raw toggle sub-component:

| Variant Property | Values |
|-----------------|--------|
| Type | Simple, Description, Card |
| Disabled | False, True |
| Destructive | False, True |
| Orientation | Left, Right |
| Checked | True, False |

Checkbox Group: Disabled (False/True), Error (False/True)

Checkbox Toggle states: Checked (False/Intermediate/True), State (Enabled/Focus)

Unchecked layout: 16×16 rounded rect with border and background fill, shadow
Checked layout: 16×16 rounded rect with primary fill + check icon (14×14, white)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--primary` | `--primary` | Checked state background |
| `--background` | `--background` | Unchecked state background |
| `--input` | `--input` | Unchecked state border |
| `size-4` (16px) | `size-4` | Toggle size |
| `shadow-2xs` | `shadow-2xs` | Toggle shadow |

### Theme Behavior

- Unchecked state uses `--input` for border — adapts to light/dark via theme
- Checked state uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- In dark mode, unchecked background gets subtle fill (`--input/30`)
- Invalid state uses `--destructive` — adapts to light/dark via theme (ring opacity differs: 20% light, 40% dark)
- Focus ring uses `--ring` — adapts to light/dark via theme
- Shadow is fixed (`shadow-xs`) — same in both modes
