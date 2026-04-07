# Component: Native Select

> **TL;DR:** A styled wrapper around the native HTML `<select>` element. 3 parts: NativeSelect, NativeSelectOption, NativeSelectOptGroup. Renders a native `select` with a wrapper div for a chevron icon. Supports `default` and `sm` sizes. Focus ring, error via `aria-invalid`. Dark mode `bg-input/30` with hover `bg-input/50`. Chevron icon absolute positioned right. No external primitive — pure shadCN.

## Metadata

- **Component Name:** NativeSelect
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A styled wrapper around the native HTML `<select>` element, providing consistent styling while preserving native browser behavior for option selection, keyboard navigation, and mobile pickers.

- Wrapper div is `relative w-fit` with group class `group/native-select`
- Native `select` uses `appearance-none` to remove default browser styling
- Chevron icon (ChevronDownIcon) is absolute positioned to the right of the select
- Supports `default` (h-9) and `sm` (h-8) sizes via `data-size` attribute
- Focus state uses `focus-visible:border-ring` with a 3px ring (`ring-ring/50`)
- Error state via `aria-invalid` applies destructive border and ring colors
- Dark mode applies `bg-input/30` with hover `bg-input/50`
- Disabled state reduces opacity to 50% on the wrapper and disables pointer events on the select
- Selection highlight uses `bg-primary` / `text-primary-foreground`
- Placeholder text uses `text-muted-foreground`

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `NativeSelect` — Wrapper div. Relative container with group class for the select and chevron icon.
- `NativeSelectOption` — Native `<option>` element.
- `NativeSelectOptGroup` — Native `<optgroup>` element for grouping options.

### Props

#### NativeSelect

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm"` | `"default"` | Select height size |
| className | `string` | — | Additional CSS classes |

#### NativeSelectOption

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NativeSelectOptGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="default"` | Standard height (`h-9`) | General form selects |
| `size="sm"` | Compact height (`h-8`, `py-1`) | Dense layouts, inline selects |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"native-select-wrapper"`, `"native-select"`, `"native-select-option"`, `"native-select-optgroup"` | All parts |
| `[data-size]` | `"default"`, `"sm"` | NativeSelect (select element) |

### CSS Variables (Radix)

Not applicable — NativeSelect does not use Radix primitives.

## Composition Patterns

### Basic Native Select

```tsx
<NativeSelect>
  <NativeSelectOption value="">Select a fruit</NativeSelectOption>
  <NativeSelectOption value="apple">Apple</NativeSelectOption>
  <NativeSelectOption value="banana">Banana</NativeSelectOption>
  <NativeSelectOption value="orange">Orange</NativeSelectOption>
</NativeSelect>
```

### With OptGroup

```tsx
<NativeSelect>
  <NativeSelectOption value="">Select a food</NativeSelectOption>
  <NativeSelectOptGroup label="Fruits">
    <NativeSelectOption value="apple">Apple</NativeSelectOption>
    <NativeSelectOption value="banana">Banana</NativeSelectOption>
  </NativeSelectOptGroup>
  <NativeSelectOptGroup label="Vegetables">
    <NativeSelectOption value="carrot">Carrot</NativeSelectOption>
    <NativeSelectOption value="broccoli">Broccoli</NativeSelectOption>
  </NativeSelectOptGroup>
</NativeSelect>
```

### Small Size

```tsx
<NativeSelect size="sm">
  <NativeSelectOption value="option1">Option 1</NativeSelectOption>
  <NativeSelectOption value="option2">Option 2</NativeSelectOption>
</NativeSelect>
```

### Error State

```tsx
<NativeSelect aria-invalid="true">
  <NativeSelectOption value="">Required field</NativeSelectOption>
  <NativeSelectOption value="option1">Option 1</NativeSelectOption>
</NativeSelect>
```

## Accessibility

- **WAI-ARIA Pattern:** Native HTML select — inherits browser accessibility

### ARIA Roles

No custom ARIA roles needed. The native `<select>` element provides built-in accessibility including `listbox` role, option announcement, and form integration.

### Keyboard Behavior

Native `<select>` keyboard behavior is preserved:
- `Space` / `Enter` — Opens the dropdown
- `Arrow Up` / `Arrow Down` — Navigates options
- `Home` / `End` — Jumps to first/last option
- `Escape` — Closes the dropdown
- Type-ahead — Jumps to matching option

## HTML

### Standalone HTML Structure

```html
<div data-slot="native-select-wrapper">
  <select data-slot="native-select" data-size="default">
    <option data-slot="native-select-option" value="">Select an option</option>
    <optgroup data-slot="native-select-optgroup" label="Group">
      <option data-slot="native-select-option" value="1">Option 1</option>
      <option data-slot="native-select-option" value="2">Option 2</option>
    </optgroup>
  </select>
  <!-- ChevronDownIcon -->
  <svg>...</svg>
</div>
```

## CSS

### Raw CSS

```css
/* NativeSelect wrapper */
.NativeSelectWrapper {
  position: relative;
  width: fit-content;
}

.NativeSelectWrapper:has(select:disabled) {
  opacity: 0.5;
}

/* NativeSelect (select element) */
.NativeSelect {
  height: 2.25rem;
  width: 100%;
  min-width: 0;
  appearance: none;
  border-radius: 0.375rem;
  border: 1px solid var(--input);
  background-color: transparent;
  padding: 0.5rem 2.25rem 0.5rem 0.75rem;
  font-size: 0.875rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: color, box-shadow;
  outline: none;
}

.NativeSelect::selection {
  background-color: var(--primary);
  color: var(--primary-foreground);
}

.NativeSelect::placeholder {
  color: var(--muted-foreground);
}

.NativeSelect:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

.NativeSelect[aria-invalid="true"] {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

.NativeSelect:disabled {
  pointer-events: none;
  cursor: not-allowed;
}

/* Small size */
.NativeSelect[data-size="sm"] {
  height: 2rem;
  padding-top: 0.25rem;
  padding-bottom: 0.25rem;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  .NativeSelect {
    background-color: color-mix(in srgb, var(--input) 30%, transparent);
  }

  .NativeSelect:hover {
    background-color: color-mix(in srgb, var(--input) 50%, transparent);
  }

  .NativeSelect[aria-invalid="true"] {
    box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 40%, transparent);
  }
}

/* Chevron icon */
.NativeSelectChevron {
  pointer-events: none;
  position: absolute;
  top: 50%;
  right: 0.875rem;
  width: 1rem;
  height: 1rem;
  transform: translateY(-50%);
  color: var(--muted-foreground);
  opacity: 0.5;
  user-select: none;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| NativeSelect wrapper | `group/native-select relative w-fit has-[select:disabled]:opacity-50` | Relative container with disabled state |
| NativeSelect (select) | `h-9 w-full min-w-0 appearance-none rounded-md border border-input bg-transparent px-3 py-2 pr-9 text-sm shadow-xs transition-[color,box-shadow] outline-none` | Base select styling |
| NativeSelect (selection) | `selection:bg-primary selection:text-primary-foreground` | Text selection highlight |
| NativeSelect (placeholder) | `placeholder:text-muted-foreground` | Placeholder text color |
| NativeSelect (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| NativeSelect (error) | `aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40` | Error state |
| NativeSelect (disabled) | `disabled:pointer-events-none disabled:cursor-not-allowed` | Disabled state |
| NativeSelect (sm) | `data-[size=sm]:h-8 data-[size=sm]:py-1` | Small size variant |
| NativeSelect (dark) | `dark:bg-input/30 dark:hover:bg-input/50` | Dark mode background |
| ChevronDownIcon | `pointer-events-none absolute top-1/2 right-3.5 size-4 -translate-y-1/2 text-muted-foreground opacity-50 select-none` | Chevron icon positioning |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Border color and dark mode background | `shadcn` |
| `--ring` | Focus ring color | `shadcn` |
| `--destructive` | Error border and ring color | `shadcn` |
| `--primary` | Text selection background | `shadcn` |
| `--primary-foreground` | Text selection foreground | `shadcn` |
| `--muted-foreground` | Placeholder and chevron icon color | `shadcn` |

## Theme Support

### Component Structure

EMPTY: None recorded.

### CSS Variable Mapping

EMPTY: None recorded.

### Theme Behavior

- Border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error state uses `--destructive` — adapts to light/dark via theme
- Text selection uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Placeholder and chevron use `--muted-foreground` — adapts to light/dark via theme
- Dark mode applies `--input` at 30% opacity for background, 50% on hover
- Shadow is fixed (`shadow-xs`) — same in both modes
- All spacing and typography tokens are mode-independent
