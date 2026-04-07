# Component: Combobox

> **TL;DR:** A filterable select with search input, single/multiple selection, and chip support. 16 parts + hook built on Base UI (`@base-ui/react`). InputGroup wrapper with optional trigger/clear buttons. Positioner with anchor-width matching. Groups, separators, labels. Multi-select via Chips with removable ChipRemove buttons. `autoHighlight`, `itemToStringValue` for object items. 11 CSS vars.

## Metadata

- **Component Name:** Combobox
- **Source:** shadCN (Base UI)
- **Dependencies:** @base-ui/react

## Behavior

A filterable select component built on Base UI's Combobox primitive, providing searchable dropdown selection with single and multiple modes.

- Combobox (Root) wraps the Base UI `ComboboxPrimitive.Root` — manages open/close, selection, and filtering state
- ComboboxInput renders inside an InputGroup wrapper with optional trigger chevron and clear button
- ComboboxTrigger renders a ChevronDownIcon button that opens the popup; hidden when clear button is visible
- ComboboxClear renders an XIcon button via InputGroupButton (ghost, icon-xs) to clear the current value
- ComboboxContent renders via Portal > Positioner > Popup with anchor-width matching and directional slide animations
- ComboboxList is a scrollable container with dynamic max-height based on available viewport space
- ComboboxItem renders with CheckIcon indicator absolute-positioned on the right; highlighted state uses accent colors
- ComboboxGroup organizes related items; ComboboxLabel provides group headings
- ComboboxEmpty displays centered text when no items match the filter; hidden by default, shown via `group-data-empty`
- ComboboxSeparator renders a 1px `bg-border` divider
- ComboboxChips renders a flex-wrap container for multi-select chips with focus ring and error state support
- ComboboxChip renders individual selection chips with optional ChipRemove (XIcon) button
- ComboboxChipsInput renders a minimal inline input inside the chips container for typing
- ComboboxCollection provides filtered list rendering
- ComboboxValue displays the selected value text
- `useComboboxAnchor` hook returns a ref for anchoring the popup to a custom element (used with chips)
- Content popup has open/close animations: fade-in, zoom-in-95, directional slide
- Dark mode applies `bg-input/30` on chips container and content's nested input-group
- Disabled items have `pointer-events-none` and `opacity-50`
- SVG icons inside items are auto-sized to 16px with `pointer-events: none`

### Anti-Patterns

#### Do
- Use when users need to search and select from a large list.
- Provide clear placeholder text indicating the expected input.
- Show a "no results" state when search yields nothing.

#### Don't
- Don't use for small lists (fewer than 7 items) — use select or radio group.
- Don't clear the search input on selection if users may need to make multiple selections.

Sources: shadCN, Radix UI

## API

### Parts

- `Combobox` — Root provider. Wraps Base UI ComboboxPrimitive.Root. Manages selection, filtering, open state.
- `ComboboxInput` — Search input wrapped in InputGroup. Supports `showTrigger` and `showClear` props.
- `ComboboxTrigger` — Chevron button that opens the popup. Renders ChevronDownIcon.
- `ComboboxClear` — Clear button that resets the value. Renders XIcon via InputGroupButton.
- `ComboboxContent` — Popup panel. Portal > Positioner > Popup with anchor-width matching.
- `ComboboxList` — Scrollable list container with dynamic max-height.
- `ComboboxItem` — Individual selectable option with CheckIcon indicator.
- `ComboboxGroup` — Groups related items.
- `ComboboxLabel` — Group heading text.
- `ComboboxCollection` — Filtered list rendering helper.
- `ComboboxEmpty` — Empty state message when no items match.
- `ComboboxSeparator` — Visual divider between groups.
- `ComboboxChips` — Multi-select chip container with focus ring.
- `ComboboxChip` — Individual selection chip with optional remove button.
- `ComboboxChipsInput` — Inline input inside chips container.
- `ComboboxValue` — Displays the selected value.
- `useComboboxAnchor` — Hook returning a ref for custom popup anchoring.

### Props

#### Combobox (Root)

Inherits all Base UI ComboboxPrimitive.Root props:

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `Value \| Value[]` | — | Controlled selected value |
| defaultValue | `Value \| Value[]` | — | Default selected value |
| onValueChange | `function` | — | Called when selection changes |
| inputValue | `string` | — | Controlled input value |
| defaultInputValue | `string` | — | Default input value |
| onInputValueChange | `function` | — | Called when input value changes |
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | `false` | Default open state |
| onOpenChange | `function` | — | Called when open state changes |
| multiple | `boolean` | `false` | Enable multiple selection |
| autoHighlight | `boolean` | `false` | Auto-highlight first matching item while filtering |
| filter | `function` | — | Custom filter function |
| filteredItems | `any[]` | — | Externally filtered items |
| items | `any[]` | — | Items to display |
| itemToStringLabel | `function` | — | Convert object value to display string |
| itemToStringValue | `function` | — | Convert object value to form submission string |
| disabled | `boolean` | `false` | Disable the combobox |
| required | `boolean` | `false` | Mark as required |
| readOnly | `boolean` | `false` | Prevent value changes |
| loopFocus | `boolean` | `true` | Loop keyboard focus through items |
| modal | `boolean` | `false` | Lock scroll and outside interactions when open |
| name | `string` | — | Form field name |

#### ComboboxInput

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| showTrigger | `boolean` | `true` | Show the chevron trigger button |
| showClear | `boolean` | `false` | Show the clear button |
| disabled | `boolean` | `false` | Disable the input |
| className | `string` | — | Additional CSS classes |
| placeholder | `string` | — | Input placeholder text |

#### ComboboxContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| side | `"top" \| "bottom" \| "left" \| "right"` | `"bottom"` | Popup placement side |
| sideOffset | `number` | `6` | Distance from anchor in pixels |
| align | `"start" \| "center" \| "end"` | `"start"` | Alignment along the side |
| alignOffset | `number` | `0` | Alignment offset in pixels |
| anchor | `Element \| Ref` | — | Custom anchor element (for chips pattern) |
| className | `string` | — | Additional CSS classes |

#### ComboboxItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `any` | — | Item value for selection |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### ComboboxChip

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| showRemove | `boolean` | `true` | Show the remove button |
| className | `string` | — | Additional CSS classes |

### Variants

No CVA style variants — Combobox has a single visual style. Multi-select mode is controlled via the `multiple` prop on Root.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"combobox-value"`, `"combobox-trigger"`, `"combobox-trigger-icon"`, `"combobox-clear"`, `"combobox-content"`, `"combobox-list"`, `"combobox-item"`, `"combobox-item-indicator"`, `"combobox-group"`, `"combobox-label"`, `"combobox-collection"`, `"combobox-empty"`, `"combobox-separator"`, `"combobox-chips"`, `"combobox-chip"`, `"combobox-chip-remove"`, `"combobox-chip-input"` | All parts |
| `[data-highlighted]` | Present when item is highlighted | ComboboxItem |
| `[data-selected]` | Present when item is selected | ComboboxItem |
| `[data-disabled]` | Present when disabled | ComboboxItem, ComboboxInput |
| `[data-chips]` | `"true"` when anchor is provided | ComboboxContent |
| `[data-side]` | `"top"`, `"bottom"`, `"left"`, `"right"` | ComboboxContent |
| `[data-open]` | Present when popup is open | ComboboxContent |
| `[data-closed]` | Present when popup is closed | ComboboxContent |
| `[data-empty]` | Present when list is empty | ComboboxList |
| `[data-pressed]` | Present when pressed | ComboboxTrigger |

### CSS Variables (Radix)

Not applicable — Combobox uses Base UI, not Radix primitives. Base UI Positioner exposes:

| Variable | Description |
|----------|-------------|
| `--anchor-width` | Width of the anchor element |
| `--anchor-height` | Height of the anchor element |
| `--available-width` | Available width between anchor and viewport edge |
| `--available-height` | Available height between anchor and viewport edge |
| `--transform-origin` | Transform origin for animations |

## Composition Patterns

### Basic Single Select

```tsx
<Combobox>
  <ComboboxInput placeholder="Select a framework..." />
  <ComboboxContent>
    <ComboboxList>
      <ComboboxEmpty>No results found.</ComboboxEmpty>
      <ComboboxItem value="next">Next.js</ComboboxItem>
      <ComboboxItem value="remix">Remix</ComboboxItem>
      <ComboboxItem value="astro">Astro</ComboboxItem>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### With Clear Button

```tsx
<Combobox>
  <ComboboxInput placeholder="Search..." showClear />
  <ComboboxContent>
    <ComboboxList>
      <ComboboxEmpty>No matches.</ComboboxEmpty>
      <ComboboxItem value="apple">Apple</ComboboxItem>
      <ComboboxItem value="banana">Banana</ComboboxItem>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### With Groups

```tsx
<Combobox>
  <ComboboxInput placeholder="Select produce..." />
  <ComboboxContent>
    <ComboboxList>
      <ComboboxEmpty>No results.</ComboboxEmpty>
      <ComboboxGroup>
        <ComboboxLabel>Fruits</ComboboxLabel>
        <ComboboxItem value="apple">Apple</ComboboxItem>
        <ComboboxItem value="banana">Banana</ComboboxItem>
      </ComboboxGroup>
      <ComboboxSeparator />
      <ComboboxGroup>
        <ComboboxLabel>Vegetables</ComboboxLabel>
        <ComboboxItem value="carrot">Carrot</ComboboxItem>
        <ComboboxItem value="spinach">Spinach</ComboboxItem>
      </ComboboxGroup>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Multiple Select with Chips

```tsx
const anchor = useComboboxAnchor()

<Combobox multiple>
  <ComboboxChips ref={anchor}>
    {(value) => (
      <ComboboxChip key={value} value={value}>
        {value}
      </ComboboxChip>
    )}
    <ComboboxChipsInput placeholder="Add items..." />
  </ComboboxChips>
  <ComboboxContent anchor={anchor}>
    <ComboboxList>
      <ComboboxEmpty>No results.</ComboboxEmpty>
      <ComboboxItem value="react">React</ComboboxItem>
      <ComboboxItem value="vue">Vue</ComboboxItem>
      <ComboboxItem value="svelte">Svelte</ComboboxItem>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

## Accessibility

- **WAI-ARIA Pattern:** [Combobox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| ComboboxInput | `role="combobox"` | Search input with popup |
| ComboboxInput | `aria-expanded` | Reflects open state |
| ComboboxInput | `aria-controls` | References the listbox |
| ComboboxInput | `aria-activedescendant` | References the highlighted item |
| ComboboxInput | `aria-autocomplete="list"` | Indicates filtering behavior |
| ComboboxInput | `aria-invalid` | Set when in error state |
| ComboboxList | `role="listbox"` | Container for options |
| ComboboxItem | `role="option"` | Selectable option |
| ComboboxItem | `aria-selected` | Set when item is selected |
| ComboboxItem | `aria-disabled` | Set when item is disabled |
| ComboboxGroup | `role="group"` | Groups related options |
| ComboboxLabel | `role="presentation"` | Group heading |
| ComboboxChips | `role="list"` | Container for selected chips |
| ComboboxChip | `role="listitem"` | Individual chip |
| ComboboxChipRemove | `aria-label="Remove"` | Remove button label |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Down | Open popup (if closed) or move to next item |
| Arrow Up | Move to previous item |
| Enter | Select the highlighted item |
| Escape | Close the popup |
| Backspace | Remove last chip (multi-select, when input is empty) |
| Home | Move to first item |
| End | Move to last item |
| Tab | Close popup and move focus |

## HTML

### Standalone HTML Structure

```html
<!-- Single select -->
<div data-slot="combobox">
  <div data-slot="input-group">
    <input data-slot="combobox-input" role="combobox"
           aria-expanded="true" aria-controls="listbox-id"
           aria-activedescendant="item-1" aria-autocomplete="list"
           placeholder="Select a framework..." />
    <button data-slot="combobox-trigger">
      <svg data-slot="combobox-trigger-icon"><!-- chevron --></svg>
    </button>
  </div>
  <!-- Portal -->
  <div data-slot="combobox-content" data-side="bottom" data-open>
    <div data-slot="combobox-list" role="listbox" id="listbox-id">
      <div data-slot="combobox-empty" style="display: none;">No results found.</div>
      <div data-slot="combobox-group" role="group">
        <div data-slot="combobox-label">Frameworks</div>
        <div data-slot="combobox-item" role="option" aria-selected="true" data-highlighted id="item-1">
          <span>Next.js</span>
          <svg data-slot="combobox-item-indicator"><!-- check --></svg>
        </div>
        <div data-slot="combobox-item" role="option" aria-selected="false">
          <span>Remix</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Multi-select with chips -->
<div data-slot="combobox">
  <div data-slot="combobox-chips" role="list">
    <div data-slot="combobox-chip" role="listitem">
      <span>React</span>
      <button data-slot="combobox-chip-remove" aria-label="Remove">
        <svg><!-- x icon --></svg>
      </button>
    </div>
    <input data-slot="combobox-chip-input" role="combobox"
           placeholder="Add items..." />
  </div>
  <!-- Portal -->
  <div data-slot="combobox-content" data-chips="true" data-side="bottom" data-open>
    <div data-slot="combobox-list" role="listbox">
      <div data-slot="combobox-item" role="option" aria-selected="true" data-selected>
        <span>React</span>
        <svg data-slot="combobox-item-indicator"><!-- check --></svg>
      </div>
      <div data-slot="combobox-item" role="option" aria-selected="false">
        <span>Vue</span>
      </div>
    </div>
  </div>
</div>
```


## CSS

### Raw CSS

```css
/* Combobox Content (Popup) */
.ComboboxContent {
  overflow: hidden;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  color: var(--popover-foreground);
  box-shadow: var(--shadow-md);
}

/* Content open/close animations */
.ComboboxContent[data-open] {
  animation: fadeIn 150ms ease, zoomIn95 150ms ease;
}
.ComboboxContent[data-closed] {
  animation: fadeOut 150ms ease, zoomOut95 150ms ease;
}
.ComboboxContent[data-side="bottom"] { animation-name: fadeIn, zoomIn95, slideFromTop; }
.ComboboxContent[data-side="top"] { animation-name: fadeIn, zoomIn95, slideFromBottom; }
.ComboboxContent[data-side="left"] { animation-name: fadeIn, zoomIn95, slideFromRight; }
.ComboboxContent[data-side="right"] { animation-name: fadeIn, zoomIn95, slideFromLeft; }

/* Content with chips anchor */
.ComboboxContent[data-chips="true"] .InputGroup {
  border: none;
  padding: 0;
  height: auto;
}

/* ComboboxList */
.ComboboxList {
  max-height: var(--available-height);
  scroll-padding: 0.25rem;
  overflow-x: hidden;
  overflow-y: auto;
  padding: 0.25rem;
}

/* ComboboxItem */
.ComboboxItem {
  position: relative;
  display: flex;
  width: 100%;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem;
  padding-right: 2rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.ComboboxItem[data-highlighted] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.ComboboxItem[data-disabled] {
  pointer-events: none;
  opacity: 0.5;
}

/* Item indicator (CheckIcon) */
.ComboboxItemIndicator {
  position: absolute;
  right: 0.5rem;
  display: flex;
  width: 1rem;
  height: 1rem;
  align-items: center;
  justify-content: center;
}

/* Item icons */
.ComboboxItem svg {
  pointer-events: none;
  flex-shrink: 0;
}
.ComboboxItem svg:not([class*='size-']) {
  width: 1rem;
  height: 1rem;
}

/* ComboboxLabel (group heading) */
.ComboboxLabel {
  padding: 0.375rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--muted-foreground);
}

/* ComboboxEmpty */
.ComboboxEmpty {
  display: none;
  padding: 1.5rem 0;
  text-align: center;
  font-size: 0.875rem;
}
.ComboboxList[data-empty] .ComboboxEmpty {
  display: block;
}

/* ComboboxSeparator */
.ComboboxSeparator {
  margin: 0 -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* ComboboxChips */
.ComboboxChips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.25rem;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  padding: 0.25rem;
  background-color: transparent;
}

.ComboboxChips:focus-within {
  outline: 2px solid var(--ring);
  outline-offset: -1px;
}

.ComboboxChips[aria-invalid="true"] {
  border-color: var(--destructive);
}

/* ComboboxChip */
.ComboboxChip {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  border-radius: 0.25rem;
  border: 1px solid var(--border);
  padding: 0.125rem 0.375rem;
  font-size: 0.75rem;
  background-color: transparent;
}

/* ComboboxChipRemove */
.ComboboxChipRemove {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 0.75rem;
  height: 0.75rem;
  border-radius: 9999px;
  opacity: 0.7;
}
.ComboboxChipRemove:hover {
  opacity: 1;
}

/* ComboboxChipsInput */
.ComboboxChipsInput {
  flex: 1;
  min-width: 4rem;
  border: none;
  background-color: transparent;
  font-size: 0.875rem;
  outline: none;
}
.ComboboxChipsInput::placeholder {
  color: var(--muted-foreground);
}

/* Dark mode overrides */
@media (prefers-color-scheme: dark) {
  .ComboboxChips {
    background-color: var(--input) / 0.3;
  }
  .ComboboxContent[data-chips="true"] .InputGroup {
    background-color: var(--input) / 0.3;
  }
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Content | `overflow-hidden rounded-md border bg-popover text-popover-foreground shadow-md` | Popup container |
| Content (open) | `data-[open]:animate-in data-[open]:fade-in-0 data-[open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[closed]:animate-out data-[closed]:fade-out-0 data-[closed]:zoom-out-95` | Close animation |
| Content (side) | `data-[side=bottom]:slide-in-from-top-2 data-[side=top]:slide-in-from-bottom-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2` | Directional slide |
| Content (chips) | `data-[chips=true]:[&_[data-slot=input-group]]:border-none data-[chips=true]:[&_[data-slot=input-group]]:p-0 data-[chips=true]:[&_[data-slot=input-group]]:h-auto` | Chips anchor overrides |
| List | `max-h-[var(--available-height)] scroll-py-1 overflow-x-hidden overflow-y-auto p-1` | Scrollable list |
| Item | `relative flex w-full cursor-default items-center gap-2 rounded-sm px-2 py-1.5 pr-8 text-sm outline-hidden select-none` | Item layout |
| Item (highlighted) | `data-[highlighted]:bg-accent data-[highlighted]:text-accent-foreground` | Highlighted state |
| Item (disabled) | `data-[disabled]:pointer-events-none data-[disabled]:opacity-50` | Disabled state |
| Item indicator | `absolute right-2 flex size-4 items-center justify-center` | Check icon position |
| Item icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4` | Icon sizing |
| Label | `px-2 py-1.5 text-xs font-medium text-muted-foreground` | Group heading |
| Empty | `hidden py-6 text-center text-sm group-data-[empty]:block` | Empty state |
| Separator | `-mx-1 h-px bg-border` | Divider |
| Chips | `flex flex-wrap gap-1 rounded-md border border-border p-1 bg-transparent focus-within:outline-2 focus-within:outline-ring focus-within:-outline-offset-1 aria-invalid:border-destructive` | Chips container |
| Chips (dark) | `dark:bg-input/30` | Dark mode background |
| Chip | `inline-flex items-center gap-1 rounded border border-border px-1.5 py-0.5 text-xs bg-transparent` | Individual chip |
| ChipRemove | `inline-flex items-center justify-center size-3 rounded-full opacity-70 hover:opacity-100` | Remove button |
| ChipsInput | `flex-1 min-w-16 border-0 bg-transparent text-sm outline-hidden placeholder:text-muted-foreground` | Inline input |


## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Content background | `shadcn` |
| `--popover-foreground` | Content text color | `shadcn` |
| `--border` | Content border, separator, chips border, chip border | `shadcn` |
| `--accent` | Highlighted item background | `shadcn` |
| `--accent-foreground` | Highlighted item text | `shadcn` |
| `--muted-foreground` | Group label, empty state, placeholder, icon color | `shadcn` |
| `--ring` | Chips container focus ring | `shadcn` |
| `--destructive` | Chips container error border | `shadcn` |
| `--input` | Dark mode chips/input-group background (at 30% opacity) | `shadcn` |
| `--shadow-md` | Content box shadow | `shadcn` |
| `--anchor-width` | Positioner anchor width matching (Base UI) | `base-ui` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Grouped | False, True |
| State | Enabled, Hover, Focus, Disabled |

Total: 8 variant combinations

Field layout: flex row (h 36px, background, border, rounded, shadow-sm, px 16px, py 8px) → optional Avatar (20px) + Content text (text-sm, font-medium, truncate) + Chevrons icon (16×16px)

Menu: Dropdown list with search field and menu items
Menu Item types: Simple, Checkbox, Icon, Avatar

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--foreground)` | `--foreground` | Label text, field content text |
| `var(--muted-foreground)` | `--muted-foreground` | Help text, placeholder text |
| `var(--input)` | `--input` | Field border color |
| `--radius` (derived) | `--radius` | Field border radius |
| `px-4` (16px) | `px-4` | Field horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Label and content font weight |
| `text-sm` (14px) | `text-sm` | All text size |
| `shadow-sm` | `shadow-sm` | Field box shadow |

### Theme Behavior

- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Highlighted items use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Group labels, placeholders, and icons use `--muted-foreground` — adapts to light/dark via theme
- Borders use `--border` throughout (content, chips, chip, separator) — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error state uses `--destructive` — adapts to light/dark via theme
- Dark mode applies `--input` at 30% opacity on chips container and content input-group background
- Positioner width matches anchor via `--anchor-width` (Base UI variable, theme-independent)
- All spacing, typography, and layout tokens are mode-independent