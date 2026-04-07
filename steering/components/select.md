# Component: Select

> **TL;DR:** A custom dropdown select with search, grouping, and scroll buttons. 10 parts: Select, SelectGroup, SelectValue, SelectTrigger, SelectContent, SelectItem, SelectLabel, SelectSeparator, SelectScrollUpButton, SelectScrollDownButton. Built on Radix UI primitive. Trigger has `default`/`sm` sizes. Content supports item-aligned (default) and popper positions. Items have check indicator on right. Scroll buttons with chevrons.

## Metadata

- **Component Name:** Select
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A custom dropdown select component with rich content support, built on Radix UI's Select primitive.

- SelectTrigger renders as a flex container with `justify-between` and `gap-2`
- Trigger supports `default` (h-9) and `sm` (h-8) sizes via `data-size` attribute
- Trigger has border, shadow, and focus ring; placeholder text uses `text-muted-foreground`
- Dark mode applies `bg-input/30` with hover `bg-input/50` on the trigger
- Error state via `aria-invalid` applies destructive border and ring colors
- SelectContent supports two position modes: `item-aligned` (default) and `popper`
- Content renders via Portal with `z-50`, border, shadow, and animations
- Content uses Radix CSS variables for available height, transform origin, trigger height, and trigger width
- SelectItem is a flex row with `gap-2`, focus uses `bg-accent` / `text-accent-foreground`
- Selected item shows a CheckIcon indicator absolute positioned on the right (`right-2`)
- Disabled items have `pointer-events-none` and `opacity-50`
- SelectLabel renders with `text-xs text-muted-foreground`
- SelectSeparator is a 1px `bg-border` line with negative horizontal margin
- SelectScrollUpButton and SelectScrollDownButton render chevron icons for overflow scrolling

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Select` — Root provider. Manages open/close and selection state.
- `SelectGroup` — Groups related items with an optional label.
- `SelectValue` — Displays the selected value in the trigger.
- `SelectTrigger` — Button that opens the dropdown.
- `SelectContent` — Dropdown content panel with items.
- `SelectItem` — Individual selectable option.
- `SelectLabel` — Group label text.
- `SelectSeparator` — Visual divider between groups or items.
- `SelectScrollUpButton` — Scroll indicator at the top of overflow content.
- `SelectScrollDownButton` — Scroll indicator at the bottom of overflow content.

### Props

#### Select

Inherits Radix UI Select root props (value, onValueChange, defaultValue, open, onOpenChange, disabled).

#### SelectTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm"` | `"default"` | Trigger height size |
| className | `string` | — | Additional CSS classes |

#### SelectContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| position | `"item-aligned" \| "popper"` | `"item-aligned"` | Content positioning strategy |
| className | `string` | — | Additional CSS classes |

#### SelectItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Value for this option |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### SelectLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### SelectSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="default"` | Standard trigger height (`h-9`) | General form selects |
| `size="sm"` | Compact trigger height (`h-8`) | Dense layouts |
| `position="item-aligned"` | Content aligns to selected item | Default behavior |
| `position="popper"` | Content positions like a popover | When item alignment is not desired |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"select-trigger"`, `"select-content"`, `"select-item"`, `"select-label"`, `"select-separator"`, `"select-scroll-up-button"`, `"select-scroll-down-button"` | All parts |
| `[data-size]` | `"default"`, `"sm"` | SelectTrigger |
| `[data-state]` | `"open"`, `"closed"` | SelectTrigger, SelectContent |
| `[data-placeholder]` | `""` | SelectTrigger (when no value selected) |
| `[data-disabled]` | `""` | SelectItem (when disabled) |

### CSS Variables (Radix)

| Variable | Purpose |
|----------|---------|
| `--radix-select-content-available-height` | Maximum content height |
| `--radix-select-content-transform-origin` | Content transform origin for animations |
| `--radix-select-trigger-height` | Trigger element height (for popper positioning) |
| `--radix-select-trigger-width` | Trigger element width (for popper positioning) |

## Composition Patterns

### Basic Select

```tsx
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Select a fruit" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="apple">Apple</SelectItem>
    <SelectItem value="banana">Banana</SelectItem>
    <SelectItem value="orange">Orange</SelectItem>
  </SelectContent>
</Select>
```

### With Groups

```tsx
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Select a food" />
  </SelectTrigger>
  <SelectContent>
    <SelectGroup>
      <SelectLabel>Fruits</SelectLabel>
      <SelectItem value="apple">Apple</SelectItem>
      <SelectItem value="banana">Banana</SelectItem>
    </SelectGroup>
    <SelectSeparator />
    <SelectGroup>
      <SelectLabel>Vegetables</SelectLabel>
      <SelectItem value="carrot">Carrot</SelectItem>
      <SelectItem value="broccoli">Broccoli</SelectItem>
    </SelectGroup>
  </SelectContent>
</Select>
```

### Small Size

```tsx
<Select>
  <SelectTrigger size="sm">
    <SelectValue placeholder="Size" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="sm">Small</SelectItem>
    <SelectItem value="md">Medium</SelectItem>
    <SelectItem value="lg">Large</SelectItem>
  </SelectContent>
</Select>
```

### Popper Position

```tsx
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Choose..." />
  </SelectTrigger>
  <SelectContent position="popper">
    <SelectItem value="a">Option A</SelectItem>
    <SelectItem value="b">Option B</SelectItem>
  </SelectContent>
</Select>
```

## Accessibility

- **WAI-ARIA Pattern:** Listbox (implicit via Radix)

### ARIA Roles

Radix UI Select provides built-in `listbox` role on the content and `option` role on items. Trigger uses `combobox` role with `aria-expanded` and `aria-haspopup`.

### Keyboard Behavior

- `Space` / `Enter` — Opens the dropdown on the trigger; selects the focused item
- `Arrow Up` / `Arrow Down` — Navigates between items
- `Home` / `End` — Jumps to first/last item
- `Escape` — Closes the dropdown
- Type-ahead — Jumps to matching item by typing

## HTML

### Standalone HTML Structure

```html
<button data-slot="select-trigger" role="combobox" aria-expanded="false" data-size="default">
  <span>Select a fruit</span>
  <svg><!-- ChevronDownIcon --></svg>
</button>

<!-- Content (rendered in Portal) -->
<div data-slot="select-content" role="listbox">
  <div data-slot="select-scroll-up-button">
    <svg><!-- ChevronUpIcon --></svg>
  </div>
  <div>
    <div data-slot="select-label">Fruits</div>
    <div data-slot="select-item" role="option" aria-selected="true">
      Apple
      <span><!-- CheckIcon --></span>
    </div>
    <div data-slot="select-item" role="option">Banana</div>
    <div data-slot="select-separator"></div>
  </div>
  <div data-slot="select-scroll-down-button">
    <svg><!-- ChevronDownIcon --></svg>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* SelectTrigger */
.SelectTrigger {
  display: flex;
  width: fit-content;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
  border-radius: 0.375rem;
  border: 1px solid var(--input);
  background-color: transparent;
  padding: 0.5rem 0.75rem;
  font-size: 0.875rem;
  white-space: nowrap;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.SelectTrigger[data-size="default"] {
  height: 2.25rem;
}

.SelectTrigger[data-size="sm"] {
  height: 2rem;
}

.SelectTrigger[data-placeholder] {
  color: var(--muted-foreground);
}

.SelectTrigger:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

.SelectTrigger[aria-invalid="true"] {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

@media (prefers-color-scheme: dark) {
  .SelectTrigger {
    background-color: color-mix(in srgb, var(--input) 30%, transparent);
  }

  .SelectTrigger:hover {
    background-color: color-mix(in srgb, var(--input) 50%, transparent);
  }

  .SelectTrigger[aria-invalid="true"] {
    box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 40%, transparent);
  }
}

/* SelectContent */
.SelectContent {
  position: relative;
  z-index: 50;
  max-height: var(--radix-select-content-available-height);
  min-width: 8rem;
  transform-origin: var(--radix-select-content-transform-origin);
  overflow-x: hidden;
  overflow-y: auto;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* SelectItem */
.SelectItem {
  position: relative;
  display: flex;
  width: 100%;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 2rem 0.375rem 0.5rem;
  font-size: 0.875rem;
}

.SelectItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.SelectItem[data-disabled] {
  pointer-events: none;
  opacity: 0.5;
}

/* SelectItemIndicator */
.SelectItemIndicator {
  position: absolute;
  right: 0.5rem;
  display: flex;
  width: 0.875rem;
  height: 0.875rem;
  align-items: center;
  justify-content: center;
}

/* SelectLabel */
.SelectLabel {
  padding: 0.375rem 0.5rem;
  font-size: 0.75rem;
  color: var(--muted-foreground);
}

/* SelectSeparator */
.SelectSeparator {
  pointer-events: none;
  margin: 0.25rem -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* ScrollButtons */
.SelectScrollButton {
  display: flex;
  cursor: default;
  align-items: center;
  justify-content: center;
  padding-top: 0.25rem;
  padding-bottom: 0.25rem;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| SelectTrigger | `flex w-fit items-center justify-between gap-2 rounded-md border border-input bg-transparent px-3 py-2 text-sm whitespace-nowrap shadow-xs` | Trigger button |
| SelectTrigger (sizes) | `data-[size=default]:h-9 data-[size=sm]:h-8` | Size variants |
| SelectTrigger (placeholder) | `data-[placeholder]:text-muted-foreground` | Placeholder color |
| SelectTrigger (dark) | `dark:bg-input/30 dark:hover:bg-input/50` | Dark mode background |
| SelectContent | `relative z-50 max-h-(--radix-select-content-available-height) min-w-[8rem] origin-(--radix-select-content-transform-origin) overflow-x-hidden overflow-y-auto rounded-md border bg-popover text-popover-foreground shadow-md` | Dropdown panel |
| SelectItem | `relative flex w-full cursor-default items-center gap-2 rounded-sm py-1.5 pr-8 pl-2 text-sm focus:bg-accent focus:text-accent-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50` | Selectable option |
| SelectItemIndicator | `absolute right-2 flex size-3.5 items-center justify-center` | Check icon container |
| SelectLabel | `px-2 py-1.5 text-xs text-muted-foreground` | Group label |
| SelectSeparator | `pointer-events-none -mx-1 my-1 h-px bg-border` | Divider line |
| ScrollButtons | `flex cursor-default items-center justify-center py-1` | Scroll indicators |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Trigger border color and dark mode background | `shadcn` |
| `--ring` | Focus ring color | `shadcn` |
| `--destructive` | Error border and ring color | `shadcn` |
| `--popover` | Content background color | `shadcn` |
| `--popover-foreground` | Content text color | `shadcn` |
| `--border` | Content border and separator color | `shadcn` |
| `--accent` | Item focus background | `shadcn` |
| `--accent-foreground` | Item focus text color | `shadcn` |
| `--muted-foreground` | Placeholder, label, and icon color | `shadcn` |
| `--primary` | Check indicator color (implicit) | `shadcn` |
| `--primary-foreground` | Selection text color (implicit) | `shadcn` |

## Theme Support

### Component Structure

Component is organized into the trigger component and sub-components:

Select (trigger):
| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Focus, Filled, Filled focus, Disabled |

.Select Menu Item (sub-component):
| Variant Property | Values |
|-----------------|--------|
| State | Default, Hover, Disabled |

.Select Menu Label (sub-component — no variant properties)

Select Menu (assembled dropdown — no variant properties)

- Trigger uses `px-3`, `py-2`, `min-w-[64px]`, `max-w-[448px]`, default width `240px`

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Trigger background (at 30% opacity) and border |
| `--muted-foreground` | `--muted-foreground` | Placeholder text color |
| `--foreground` | `--foreground` | Filled value text color |
| `--radius` (derived) | `border-radius` | Trigger border radius |
| `shadow-2xs` | `box-shadow` | Trigger shadow |
| `px-3` (12px) | `padding-inline` | Trigger horizontal padding |
| `py-2` (8px) | `padding-block` | Trigger vertical padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Trigger text size |

### Theme Behavior

- Trigger uses `--input` for border — adapts to light/dark via theme
- Content uses `--popover` / `--popover-foreground` / `--border` — adapts to light/dark via theme
- Item focus uses `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Labels and placeholder use `--muted-foreground` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error state uses `--destructive` — adapts to light/dark via theme
- Dark mode applies `--input` at 30%/50% opacity for trigger background
- Shadow is fixed (`shadow-xs` on trigger, `shadow-md` on content) — same in both modes
- All spacing and typography tokens are mode-independent
