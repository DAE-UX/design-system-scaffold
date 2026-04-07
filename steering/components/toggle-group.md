# Component: Toggle Group

> **TL;DR:** A group of toggle buttons where one or multiple can be active. 2 parts + context: ToggleGroup, ToggleGroupItem. Built on Radix ToggleGroup. Reuses `toggleVariants` from Toggle. Supports `single` and `multiple` selection types. `spacing` prop controls gap between items — `0` (default) joins items with shared borders, non-zero adds gap. Context propagates variant and size to all items. Commonly used for text formatting toolbars, view mode selectors, and filter groups.

## Metadata

- **Component Name:** Toggle Group
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-toggle-group`, `class-variance-authority` (via Toggle's `toggleVariants`)

## Behavior

A group of toggle buttons that manages single or multiple selection state.

- Built on Radix ToggleGroup primitive — manages selection state across items
- Reuses `toggleVariants` CVA from Toggle component for consistent styling
- ToggleGroup provides context (variant, size, spacing) to all ToggleGroupItem children
- `spacing` prop (default `0`) controls gap between items:
  - `spacing=0`: items are joined — rounded corners only on first/last, no individual shadows, outline variant removes internal left borders
  - Non-zero spacing: items have individual rounded corners and spacing gap
- `type="single"` allows one active item; `type="multiple"` allows multiple
- ToggleGroupItem inherits variant and size from context but can override individually
- Items use `w-auto min-w-0 shrink-0 px-3` for flexible width
- Focus: z-10 elevation to prevent ring clipping by adjacent items
- Outline variant with `spacing=0` applies shared `shadow-xs` on the group root

### Anti-Patterns

#### Do
- Use for mutually exclusive formatting options (bold, italic, underline)
- Group related toggle actions together
- Use consistent icon styles across the group

#### Don't
- Don't mix toggle types (single vs multiple) in the same group without clear visual distinction
- Don't use for navigation
- Don't use without icons

Sources: shadCN, Radix UI

## API

### Parts

- `ToggleGroup` — Root container. Manages selection state and provides variant/size/spacing context to items.
- `ToggleGroupItem` — Individual toggle button within the group. Inherits styling from context.

### Props

#### ToggleGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| type | `"single" \| "multiple"` | — | Selection mode (required) |
| value | `string \| string[]` | — | Controlled selected value(s) |
| defaultValue | `string \| string[]` | — | Initial selected value(s) |
| onValueChange | `(value: string \| string[]) => void` | — | Callback when selection changes |
| variant | `"default" \| "outline"` | `"default"` | Visual style variant (propagated to items) |
| size | `"default" \| "sm" \| "lg"` | `"default"` | Button size (propagated to items) |
| spacing | `number` | `0` | Gap between items (0 = joined) |
| disabled | `boolean` | `false` | Disables all items |
| className | `string` | — | Additional CSS classes |

#### ToggleGroupItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Item value (required) |
| variant | `"default" \| "outline"` | — | Override group variant |
| size | `"default" \| "sm" \| "lg"` | — | Override group size |
| disabled | `boolean` | `false` | Disables this item |
| className | `string` | — | Additional CSS classes |

### Variants

Inherits all variants from Toggle's `toggleVariants` (see Toggle component). Additionally:

| Prop | Value | Description |
|------|-------|-------------|
| `spacing` | `0` | Joined items — shared border, no individual rounding |
| `spacing` | `> 0` | Spaced items — individual rounding and gap |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"toggle-group"`, `"toggle-group-item"` | ToggleGroup, ToggleGroupItem |
| `[data-variant]` | `"default"`, `"outline"` | ToggleGroup, ToggleGroupItem |
| `[data-size]` | `"default"`, `"sm"`, `"lg"` | ToggleGroup, ToggleGroupItem |
| `[data-spacing]` | `0`, `number` | ToggleGroup, ToggleGroupItem |
| `[data-state]` | `"on"`, `"off"` | ToggleGroupItem (from Radix) |

### CSS Variables (Radix)

Not applicable — ToggleGroup does not expose Radix CSS variables. Uses a custom `--gap` CSS property for spacing.

## Composition Patterns

### Single Selection

```tsx
<ToggleGroup type="single" defaultValue="center">
  <ToggleGroupItem value="left" aria-label="Align left">
    <AlignLeftIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="center" aria-label="Align center">
    <AlignCenterIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="right" aria-label="Align right">
    <AlignRightIcon />
  </ToggleGroupItem>
</ToggleGroup>
```

### Multiple Selection

```tsx
<ToggleGroup type="multiple" defaultValue={["bold", "italic"]}>
  <ToggleGroupItem value="bold" aria-label="Toggle bold">
    <BoldIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="italic" aria-label="Toggle italic">
    <ItalicIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="underline" aria-label="Toggle underline">
    <UnderlineIcon />
  </ToggleGroupItem>
</ToggleGroup>
```

### Outline Variant

```tsx
<ToggleGroup type="single" variant="outline" defaultValue="list">
  <ToggleGroupItem value="grid" aria-label="Grid view">
    <GridIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="list" aria-label="List view">
    <ListIcon />
  </ToggleGroupItem>
</ToggleGroup>
```

### With Spacing

```tsx
<ToggleGroup type="single" variant="outline" spacing={1} defaultValue="a">
  <ToggleGroupItem value="a">Option A</ToggleGroupItem>
  <ToggleGroupItem value="b">Option B</ToggleGroupItem>
  <ToggleGroupItem value="c">Option C</ToggleGroupItem>
</ToggleGroup>
```

### Small Size

```tsx
<ToggleGroup type="single" size="sm" defaultValue="left">
  <ToggleGroupItem value="left" aria-label="Align left">
    <AlignLeftIcon />
  </ToggleGroupItem>
  <ToggleGroupItem value="center" aria-label="Align center">
    <AlignCenterIcon />
  </ToggleGroupItem>
</ToggleGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** [Toolbar](https://www.w3.org/WAI/ARIA/apg/patterns/toolbar/) (group of toggle buttons)

### ARIA Roles

- ToggleGroup renders with `role="group"` (provided by Radix)
- Each ToggleGroupItem renders as a `button` with `aria-pressed` (provided by Radix)
- Always provide `aria-label` on items that contain only icons

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Tab` | Move focus into/out of the group |
| `ArrowRight` | Move focus to next item |
| `ArrowLeft` | Move focus to previous item |
| `Space` / `Enter` | Toggle the focused item |
| `Home` | Move focus to first item |
| `End` | Move focus to last item |

## HTML

### Standalone HTML Structure

```html
<div data-slot="toggle-group" data-variant="default" data-size="default" data-spacing="0" role="group">
  <button data-slot="toggle-group-item" data-state="on" aria-pressed="true">
    <svg><!-- icon --></svg>
  </button>
  <button data-slot="toggle-group-item" data-state="off" aria-pressed="false">
    <svg><!-- icon --></svg>
  </button>
  <button data-slot="toggle-group-item" data-state="off" aria-pressed="false">
    <svg><!-- icon --></svg>
  </button>
</div>
```

## CSS

### Raw CSS

```css
/* ToggleGroup */
.ToggleGroup {
  display: flex;
  width: fit-content;
  align-items: center;
  border-radius: 0.375rem;
}

/* ToggleGroup — spacing=0, outline variant */
.ToggleGroup[data-spacing="0"][data-variant="outline"] {
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* ToggleGroupItem — spacing=0 */
.ToggleGroup[data-spacing="0"] .ToggleGroupItem {
  border-radius: 0;
  box-shadow: none;
}

.ToggleGroup[data-spacing="0"] .ToggleGroupItem:first-child {
  border-top-left-radius: 0.375rem;
  border-bottom-left-radius: 0.375rem;
}

.ToggleGroup[data-spacing="0"] .ToggleGroupItem:last-child {
  border-top-right-radius: 0.375rem;
  border-bottom-right-radius: 0.375rem;
}

/* ToggleGroupItem — spacing=0, outline: remove internal left borders */
.ToggleGroup[data-spacing="0"][data-variant="outline"] .ToggleGroupItem {
  border-left: 0;
}

.ToggleGroup[data-spacing="0"][data-variant="outline"] .ToggleGroupItem:first-child {
  border-left: 1px solid var(--input);
}

/* ToggleGroupItem — base overrides */
.ToggleGroupItem {
  width: auto;
  min-width: 0;
  flex-shrink: 0;
  padding-left: 0.75rem;
  padding-right: 0.75rem;
}

.ToggleGroupItem:focus {
  z-index: 10;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ToggleGroup | `group/toggle-group flex w-fit items-center rounded-md gap-[--spacing(var(--gap))]` | Group container with dynamic gap |
| ToggleGroup (spacing=0, outline) | `data-[spacing=default]:data-[variant=outline]:shadow-xs` | Shared shadow for joined outline |
| ToggleGroupItem (base) | `w-auto min-w-0 shrink-0 px-3 focus:z-10 focus-visible:z-10` | Item overrides |
| ToggleGroupItem (spacing=0) | `data-[spacing=0]:rounded-none data-[spacing=0]:shadow-none data-[spacing=0]:first:rounded-l-md data-[spacing=0]:last:rounded-r-md` | Joined item rounding |
| ToggleGroupItem (spacing=0, outline) | `data-[spacing=0]:data-[variant=outline]:border-l-0 data-[spacing=0]:data-[variant=outline]:first:border-l` | Shared border for joined outline |

## CSS Variable Dependencies

Inherits all CSS variable dependencies from Toggle's `toggleVariants`:

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Hover background | `shadcn` |
| `--muted-foreground` | Hover text color | `shadcn` |
| `--accent` | Pressed background, outline hover background | `shadcn` |
| `--accent-foreground` | Pressed text color, outline hover text color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--input` | Outline variant border color | `shadcn` |
| `--destructive` | Error state border and ring color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into variant states:

| Variant Property | Values |
|-----------------|--------|
| Outlined | True, False |

- Toggle Group → Row of Toggle items (3 items: bold, italic, underline icons)
- Group container: flex, gap 0, shadow-xs, items joined with shared borders
- Each Toggle item: background, border, h 36px, px 8px, py 10px
- First item: rounded left; Last item: rounded right; Middle items: no rounding
- Outlined=False: no borders, no shadow, transparent background
- Icons: Lucide icons (bold, italic, underline), 16×16


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Toggle item background (outlined) |
| `--input` | `--input` | Item border color (outlined) |
| `--foreground` | `--foreground` | Icon color |
| `--accent` | `--accent` | Pressed item background |
| `--accent-foreground` | `--accent-foreground` | Pressed item icon color |
| `--radius` (derived) | `border-radius` | First/last item border radius |
| `p-2` (8px) | `padding` | Item horizontal padding |

### Theme Behavior

- All styling inherits from Toggle's `toggleVariants` — same theme behavior applies
- Pressed state uses `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Hover uses `--muted` / `--muted-foreground` — adapts to light/dark via theme
- Outline border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Spacing gap and joined-item border logic are mode-independent
