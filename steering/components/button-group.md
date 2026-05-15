# Component: Button Group

> **TL;DR:** A container that groups related buttons together with consistent styling. 3 parts: ButtonGroup, ButtonGroupSeparator, ButtonGroupText. Supports horizontal/vertical orientation via CVA. Merges border radii and removes internal borders for seamless appearance. Nestable for complex layouts. Composable with Input, Select, Popover, and DropdownMenu. Uses `role="group"` for accessibility.

## Metadata

- **Component Name:** Button Group
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

Groups related buttons together with merged borders and consistent styling. Creates a visually connected set of actions.

- Merges border radii: first child keeps left radius, last child keeps right radius, middle children have no radius
- Removes internal borders between adjacent children (`border-l-0` horizontal, `border-t-0` vertical)
- Supports horizontal (default) and vertical orientation
- Nestable: inner `ButtonGroup` components get spacing (`gap-2`) instead of merged borders
- `ButtonGroupSeparator` visually divides buttons (recommended for non-outline variants)
- `ButtonGroupText` displays static text or labels within the group
- Composable with Button, Input, InputGroup, Select, Popover, DropdownMenu
- Focus management: focused children get `z-10` to ensure focus ring is visible above siblings
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Display frequently used actions as standalone items
- Group related actions together
- Use overflow menu for less common actions

#### Don't
- Don't add arbitrary content in action popovers — use them for success/error states only
- Don't place actions requiring feedback (like copy-to-clipboard) in the overflow menu — keep them visible

## API

### Parts

- `ButtonGroup` — Root container with `role="group"`. Manages border merging and orientation.
- `ButtonGroupSeparator` — Visual divider between buttons. Wraps the Separator component.
- `ButtonGroupText` — Static text or label within the group. Supports `asChild`.

### Props

#### ButtonGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"horizontal" \| "vertical"` | `"horizontal"` | Layout direction |
| className | `string` | — | Additional CSS classes |
| aria-label | `string` | — | Accessible label for the group |

#### ButtonGroupSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"horizontal" \| "vertical"` | `"vertical"` | Separator direction |
| className | `string` | — | Additional CSS classes |

#### ButtonGroupText

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element (e.g., `<Label>`) |
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `orientation="horizontal"` | Buttons arranged in a row, left/right borders merged | Default toolbar layout |
| `orientation="vertical"` | Buttons stacked vertically, top/bottom borders merged | Vertical action groups |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"button-group"`, `"button-group-separator"` | ButtonGroup, ButtonGroupSeparator |
| `[data-orientation]` | `"horizontal" \| "vertical"` | ButtonGroup |

### CSS Variables (Radix)

Not applicable — ButtonGroup does not use Radix primitives.

## Composition Patterns

### Basic Button Group

```tsx
<ButtonGroup aria-label="Actions">
  <Button variant="outline">Button 1</Button>
  <Button variant="outline">Button 2</Button>
  <Button variant="outline">Button 3</Button>
</ButtonGroup>
```

### Vertical Orientation

```tsx
<ButtonGroup orientation="vertical">
  <Button variant="outline">Top</Button>
  <Button variant="outline">Middle</Button>
  <Button variant="outline">Bottom</Button>
</ButtonGroup>
```

### Nested Groups (with Spacing)

```tsx
<ButtonGroup>
  <ButtonGroup>
    <Button variant="outline">Archive</Button>
    <Button variant="outline">Report</Button>
  </ButtonGroup>
  <ButtonGroup>
    <Button variant="outline">Snooze</Button>
    <Button variant="outline" size="icon" aria-label="More">
      <MoreHorizontalIcon />
    </Button>
  </ButtonGroup>
</ButtonGroup>
```

### With Separator

```tsx
<ButtonGroup>
  <Button>Save</Button>
  <ButtonGroupSeparator />
  <Button size="icon" aria-label="Options">
    <ChevronDownIcon />
  </Button>
</ButtonGroup>
```

### With Text Label

```tsx
<ButtonGroup>
  <ButtonGroupText>Label</ButtonGroupText>
  <Button variant="outline">Action</Button>
</ButtonGroup>
```

### With Input

```tsx
<ButtonGroup>
  <Input placeholder="Search..." />
  <Button variant="outline" size="icon" aria-label="Search">
    <SearchIcon />
  </Button>
</ButtonGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** Uses `role="group"` on the root element

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| ButtonGroup | `role="group"` | Groups related buttons semantically |
| ButtonGroup | `aria-label` or `aria-labelledby` | Should provide accessible name for the group |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Tab | Moves focus between buttons in the group (standard tab order) |
| Space / Enter | Activates the focused button |

## HTML

### Standalone HTML Structure

```html
<div role="group" aria-label="Actions" data-slot="button-group" data-orientation="horizontal">
  <button data-slot="button">Button 1</button>
  <button data-slot="button">Button 2</button>
  <button data-slot="button">Button 3</button>
</div>
```

### With Separator

```html
<div role="group" data-slot="button-group">
  <button data-slot="button">Save</button>
  <div role="separator" data-slot="button-group-separator"></div>
  <button data-slot="button" aria-label="Options">
    <svg><!-- icon --></svg>
  </button>
</div>
```

## CSS

### Raw CSS

```css
/* ButtonGroup base */
.ButtonGroup {
  display: flex;
  width: fit-content;
  align-items: stretch;
}

/* Horizontal: merge borders */
.ButtonGroup[data-orientation="horizontal"] > *:not(:first-child) {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  border-left: 0;
}
.ButtonGroup[data-orientation="horizontal"] > *:not(:last-child) {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

/* Vertical: merge borders */
.ButtonGroup[data-orientation="vertical"] {
  flex-direction: column;
}
.ButtonGroup[data-orientation="vertical"] > *:not(:first-child) {
  border-top-left-radius: 0;
  border-top-right-radius: 0;
  border-top: 0;
}
.ButtonGroup[data-orientation="vertical"] > *:not(:last-child) {
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}

/* Focus z-index */
.ButtonGroup > *:focus-visible {
  position: relative;
  z-index: 10;
}

/* Nested groups get spacing */
.ButtonGroup:has(> [data-slot="button-group"]) {
  gap: 0.5rem;
}

/* ButtonGroupText */
.ButtonGroupText {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  background-color: var(--muted);
  padding: 0 1rem;
  font-size: 0.875rem;
  font-weight: 500;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* ButtonGroupSeparator */
.ButtonGroupSeparator {
  margin: 0;
  align-self: stretch;
  background-color: var(--input);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ButtonGroup (base) | `flex w-fit items-stretch` | Flex container |
| ButtonGroup (focus) | `[&>*]:focus-visible:relative [&>*]:focus-visible:z-10` | Focus ring visibility |
| ButtonGroup (nested) | `has-[>[data-slot=button-group]]:gap-2` | Spacing for nested groups |
| ButtonGroup (horizontal) | `[&>*:not(:first-child)]:rounded-l-none [&>*:not(:first-child)]:border-l-0 [&>*:not(:last-child)]:rounded-r-none` | Border merging |
| ButtonGroup (vertical) | `flex-col [&>*:not(:first-child)]:rounded-t-none [&>*:not(:first-child)]:border-t-0 [&>*:not(:last-child)]:rounded-b-none` | Vertical border merging |
| ButtonGroup (input) | `[&>input]:flex-1` | Input fills available space |
| ButtonGroup (select) | `has-[select[aria-hidden=true]:last-child]:[&>[data-slot=select-trigger]:last-of-type]:rounded-r-md [&>[data-slot=select-trigger]:not([class*='w-'])]:w-fit` | Select integration |
| ButtonGroupText | `flex items-center gap-2 rounded-md border bg-muted px-4 text-sm font-medium shadow-xs [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4` | Text label styling |
| ButtonGroupSeparator | `relative m-0! self-stretch bg-input data-[orientation=vertical]:h-auto` | Separator styling |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | ButtonGroupText background | `shadcn` |
| `--border` | ButtonGroupText border (implicit from `border` class) | `shadcn` |
| `--input` | ButtonGroupSeparator background | `shadcn` |

## Theme Support

### Component Structure

The component contains:

**Button Group** — 2 variant properties:

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Outline, Secondary |
| Orientation | Horizontal, Vertical |

Total: 6 variant combinations (3 types × 2 orientations)

**Button Group & Input** — 1 variant property:

| Variant Property | Values |
|-----------------|--------|
| Property 1 | Right, Left & right, Left |

Positions input field relative to button group (left, right, or both sides).

**.Button Group Popover Example Content** — example composition showing dropdown menu integration.

### CSS Variable Mapping



| Token | CSS Variable | Purpose |
|-------------|-------------|---------|
| `var(--primary)` | `--primary` | Default type button background, separator color |
| `var(--primary-foreground)` | `--primary-foreground` | Default type button text |
| `--radius` (derived) | `--radius` (derived) | First/last button border radius |
| `padding` (4 units) | `px-4` | Button horizontal padding |
| `padding` (2 units) | `py-2` / `gap-2` | Button vertical padding, icon gap |
| `--font-sans` | `--font-sans` | Font family |
| `font-medium` | `font-medium` | Button text weight |
| `text-sm` | `text-sm` | Button text size |
| `leading-5` | `leading-5` | Button text line height |
| `shadow-2xs` | `shadow-2xs` | Button box shadow |

### Theme Behavior

- ButtonGroupText uses `--muted` background — adapts to light/dark via theme
- Separator uses `--input` background — adapts to light/dark via theme
- ButtonGroup itself is transparent — child buttons provide their own theme colors
- Border merging is purely structural (not theme-dependent)
- All spacing and layout tokens are mode-independent
