# Component: Button Group

> **TL;DR:** A container that groups related buttons together with consistent styling. 3 parts: ButtonGroup, ButtonGroupSeparator, ButtonGroupText. Supports horizontal/vertical orientation via CVA. Merges border radii and removes internal borders for seamless appearance. Nestable for complex layouts. Composable with Input, Select, Popover, and DropdownMenu. Uses `role="group"` for accessibility.

## Metadata

- **Component Name:** Button Group
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

Groups related buttons together with merged borders and consistent styling. Creates a visually connected set of actions.

- Merges border radii: first child keeps left radius, last child keeps right radius, middle children have no radius
- Removes internal borders between adjacent children
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

Sources: shadCN, Radix UI

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

#### ButtonGroupText

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element (e.g., `<Label>`) |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `orientation="horizontal"` | Buttons in a row, left/right borders merged | Default toolbar layout |
| `orientation="vertical"` | Buttons stacked, top/bottom borders merged | Vertical action groups |

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
| ButtonGroup | `aria-label` or `aria-labelledby` | Should provide accessible name |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Tab | Moves focus between buttons in the group |
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

## CSS

### Raw CSS

```css
.ButtonGroup {
  display: flex;
  width: fit-content;
  align-items: stretch;
}
.ButtonGroup[data-orientation="horizontal"] > *:not(:first-child) {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  border-left: 0;
}
.ButtonGroup[data-orientation="horizontal"] > *:not(:last-child) {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}
.ButtonGroup[data-orientation="vertical"] { flex-direction: column; }
.ButtonGroup > *:focus-visible { position: relative; z-index: 10; }
.ButtonGroup:has(> [data-slot="button-group"]) { gap: 0.5rem; }
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
}
.ButtonGroupSeparator { margin: 0; align-self: stretch; background-color: var(--input); }
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ButtonGroup (base) | `flex w-fit items-stretch` | Flex container |
| ButtonGroup (horizontal) | `[&>*:not(:first-child)]:rounded-l-none [&>*:not(:first-child)]:border-l-0 [&>*:not(:last-child)]:rounded-r-none` | Border merging |
| ButtonGroup (vertical) | `flex-col [&>*:not(:first-child)]:rounded-t-none [&>*:not(:first-child)]:border-t-0 [&>*:not(:last-child)]:rounded-b-none` | Vertical merging |
| ButtonGroupText | `flex items-center gap-2 rounded-md border bg-muted px-4 text-sm font-medium shadow-xs` | Text label |
| ButtonGroupSeparator | `relative m-0! self-stretch bg-input` | Separator |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | ButtonGroupText background | `shadcn` |
| `--border` | ButtonGroupText border | `shadcn` |
| `--input` | ButtonGroupSeparator background | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Outline, Secondary |
| Orientation | Horizontal, Vertical |

Total: 6 variant combinations (3 types × 2 orientations)

Default horizontal layout: flex row (gap 0) → Button items (rounded on first/last only) separated by ButtonGroupSeparator (1px wide, self-stretch). First button gets left radius, last button gets right radius, middle buttons have no radius.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--primary)` | `--primary` | Default type button background, separator color |
| `var(--primary-foreground)` | `--primary-foreground` | Default type button text |
| `--radius` (derived) | `--radius` | First/last button border radius |
| `px-4` (16px) | `px-4` | Button horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Button text weight |
| `text-sm` (14px) | `text-sm` | Button text size |

### Theme Behavior

- ButtonGroupText uses `--muted` background — adapts to light/dark via theme
- Separator uses `--input` background — adapts to light/dark via theme
- ButtonGroup itself is transparent — child buttons provide their own theme colors
- Border merging is purely structural (not theme-dependent)
- All spacing and layout tokens are mode-independent
