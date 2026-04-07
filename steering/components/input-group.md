# Component: Input Group

> **TL;DR:** A composite input container for combining inputs with addons, buttons, and text. 6 parts: InputGroup, InputGroupAddon, InputGroupButton, InputGroupText, InputGroupInput, InputGroupTextarea. Addon supports 4 alignment positions (`inline-start`, `inline-end`, `block-start`, `block-end`) via CVA. Button supports 4 sizes (`xs`, `sm`, `icon-xs`, `icon-sm`). Group manages shared border, focus ring, and error state. No external primitive — uses internal Input, Textarea, and Button.

## Metadata

- **Component Name:** Input Group
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A composite container that combines an input or textarea with addons, buttons, icons, and text elements.

- InputGroup renders as a flex row with shared border, shadow, and focus ring
- Height defaults to `h-9`; auto-adjusts when containing textarea or block-aligned addons
- Focus state: when any `[data-slot=input-group-control]` inside receives focus, the group shows `border-ring` and ring
- Error state: when any child has `aria-invalid="true"`, the group shows `border-destructive` and destructive ring
- InputGroupAddon supports 4 alignment positions via CVA: `inline-start` (left), `inline-end` (right), `block-start` (top), `block-end` (bottom)
- Inline addons use CSS order for positioning; block addons switch to `flex-col` layout
- Addon clicks focus the sibling input (unless clicking a button inside)
- InputGroupButton wraps Button with reduced sizes for inline use; defaults to `variant="ghost"` and `size="xs"`
- InputGroupInput strips its own border/shadow/ring and delegates to the group
- InputGroupTextarea strips its own border/shadow/ring; group auto-adjusts to `h-auto`
- Disabled state: `data-disabled="true"` on group triggers 50% opacity on addons
- Dark mode: input/textarea background uses `bg-input/30`
- Kbd elements inside addons get reduced border-radius

### Anti-Patterns

#### Do
- Use to visually group an input with related elements (icons, buttons, text). Keep the group visually cohesive with consistent height and alignment.

#### Don't
- Don't nest input groups. Don't place too many elements in a single group — keep it focused.

Sources: shadCN, Radix UI

## API

### Parts

- `InputGroup` — Root container. Shared border, focus ring, and error state.
- `InputGroupAddon` — Addon container. Supports 4 alignment positions.
- `InputGroupButton` — Button sized for inline use. Ghost variant by default.
- `InputGroupText` — Text/icon container inside addons.
- `InputGroupInput` — Input element. Strips own border, delegates to group.
- `InputGroupTextarea` — Textarea element. Strips own border, delegates to group.

### Props

#### InputGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### InputGroupAddon

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| align | `"inline-start" \| "inline-end" \| "block-start" \| "block-end"` | `"inline-start"` | Position relative to input |
| className | `string` | — | Additional CSS classes |

#### InputGroupButton

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"xs" \| "sm" \| "icon-xs" \| "icon-sm"` | `"xs"` | Button size |
| variant | Button variant | `"ghost"` | Button style variant |
| type | `string` | `"button"` | Button type |
| className | `string` | — | Additional CSS classes |

#### InputGroupText

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### InputGroupInput

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### InputGroupTextarea

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

#### InputGroupAddon Alignment

| Variant | Description | Use Case |
|---------|-------------|----------|
| `inline-start` | Left side of input | Icons, currency symbols, prefixes |
| `inline-end` | Right side of input | Action buttons, clear buttons, suffixes |
| `block-start` | Above input (full width) | Labels, tags above input |
| `block-end` | Below input (full width) | Helper content below input |

#### InputGroupButton Sizes

| Variant | Description | Use Case |
|---------|-------------|----------|
| `xs` | Height 24px, small padding | Default inline button |
| `sm` | Height 32px, medium padding | Larger inline button |
| `icon-xs` | 24px square, no padding | Small icon-only button |
| `icon-sm` | 32px square, no padding | Larger icon-only button |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"input-group"`, `"input-group-addon"`, `"input-group-control"` | All parts |
| `[data-align]` | `"inline-start"`, `"inline-end"`, `"block-start"`, `"block-end"` | InputGroupAddon |
| `[data-size]` | `"xs"`, `"sm"`, `"icon-xs"`, `"icon-sm"` | InputGroupButton |
| `[data-disabled]` | `"true"` | InputGroup (when disabled) |

### CSS Variables (Radix)

Not applicable — Input Group does not use Radix primitives.

## Composition Patterns

### With Icon Prefix

```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <InputGroupText>
      <SearchIcon />
    </InputGroupText>
  </InputGroupAddon>
  <InputGroupInput placeholder="Search..." />
</InputGroup>
```

### With Button Suffix

```tsx
<InputGroup>
  <InputGroupInput placeholder="Enter URL..." />
  <InputGroupAddon align="inline-end">
    <InputGroupButton>
      <CopyIcon />
      Copy
    </InputGroupButton>
  </InputGroupAddon>
</InputGroup>
```

### With Kbd Shortcut

```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <InputGroupText>
      <SearchIcon />
    </InputGroupText>
  </InputGroupAddon>
  <InputGroupInput placeholder="Search..." />
  <InputGroupAddon align="inline-end">
    <Kbd>⌘K</Kbd>
  </InputGroupAddon>
</InputGroup>
```

### Block Layout (Label Above)

```tsx
<InputGroup>
  <InputGroupAddon align="block-start">
    <InputGroupText>Email Address</InputGroupText>
  </InputGroupAddon>
  <InputGroupInput type="email" placeholder="you@example.com" />
</InputGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Input Group is a presentational container

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| InputGroup | `role="group"` | Groups input with addons |
| InputGroupInput | inherits `input` semantics | Standard input element |
| InputGroupButton | `type="button"` (default) | Prevents form submission |

### Keyboard Behavior

No additional keyboard interaction — Input Group is a layout container. The input and buttons inside handle their own keyboard behavior. Tab moves between focusable elements within the group.

## HTML

### Standalone HTML Structure

```html
<div data-slot="input-group" role="group">
  <div data-slot="input-group-addon" data-align="inline-start">
    <span>
      <svg><!-- icon --></svg>
    </span>
  </div>
  <input data-slot="input-group-control" placeholder="Search..." />
  <div data-slot="input-group-addon" data-align="inline-end">
    <button type="button">Copy</button>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* InputGroup */
.InputGroup {
  position: relative;
  display: flex;
  width: 100%;
  align-items: center;
  border-radius: 0.375rem;
  border: 1px solid var(--input);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: color, box-shadow;
  outline: none;
  height: 2.25rem;
  min-width: 0;
}

/* Focus state */
.InputGroup:has([data-slot="input-group-control"]:focus-visible) {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Error state */
.InputGroup:has([data-slot][aria-invalid="true"]) {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

/* InputGroupAddon — base */
.InputGroupAddon {
  display: flex;
  height: auto;
  cursor: text;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding-top: 0.375rem;
  padding-bottom: 0.375rem;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--muted-foreground);
  user-select: none;
}

/* InputGroupAddon — inline-start */
.InputGroupAddon[data-align="inline-start"] {
  order: -9999;
  padding-left: 0.75rem;
}

/* InputGroupAddon — inline-end */
.InputGroupAddon[data-align="inline-end"] {
  order: 9999;
  padding-right: 0.75rem;
}

/* InputGroupInput */
.InputGroupInput {
  flex: 1;
  border-radius: 0;
  border: 0;
  background-color: transparent;
  box-shadow: none;
}

.InputGroupInput:focus-visible {
  box-shadow: none;
}

/* InputGroupText */
.InputGroupText {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| InputGroup | `group/input-group relative flex w-full items-center rounded-md border border-input shadow-xs transition-[color,box-shadow] outline-none h-9 min-w-0 dark:bg-input/30` | Container |
| InputGroup (block addons) | `has-[>[data-align=block-start]]:h-auto has-[>[data-align=block-start]]:flex-col` | Block layout |
| InputGroup (focus) | `has-[[data-slot=input-group-control]:focus-visible]:border-ring has-[[data-slot=input-group-control]:focus-visible]:ring-[3px] has-[[data-slot=input-group-control]:focus-visible]:ring-ring/50` | Focus ring |
| InputGroup (error) | `has-[[data-slot][aria-invalid=true]]:border-destructive has-[[data-slot][aria-invalid=true]]:ring-destructive/20 dark:has-[[data-slot][aria-invalid=true]]:ring-destructive/40` | Error ring |
| Addon (base) | `flex h-auto cursor-text items-center justify-center gap-2 py-1.5 text-sm font-medium text-muted-foreground select-none` | Addon layout |
| Addon (inline-start) | `order-first pl-3` | Left position |
| Addon (inline-end) | `order-last pr-3` | Right position |
| Addon (block-start) | `order-first w-full justify-start px-3 pt-3` | Top position |
| Addon (block-end) | `order-last w-full justify-start px-3 pb-3` | Bottom position |
| InputGroupInput | `flex-1 rounded-none border-0 bg-transparent shadow-none focus-visible:ring-0 dark:bg-transparent` | Stripped input |
| InputGroupTextarea | `flex-1 resize-none rounded-none border-0 bg-transparent py-3 shadow-none focus-visible:ring-0 dark:bg-transparent` | Stripped textarea |
| InputGroupText | `flex items-center gap-2 text-sm text-muted-foreground [&_svg]:pointer-events-none [&_svg:not([class*='size-'])]:size-4` | Text/icon container |
| InputGroupButton (xs) | `h-6 gap-1 rounded-[calc(var(--radius)-5px)] px-2` | Extra-small button |
| InputGroupButton (sm) | `h-8 gap-1.5 rounded-md px-2.5` | Small button |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Group border color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--destructive` | Error border and ring color | `shadcn` |
| `--muted-foreground` | Addon text, icon color | `shadcn` |
| `--radius` | Button border-radius calculation | `shadcn` |

## Theme Support

### Component Structure

Input Group sub-components:

| Variant Property | Values |
|-----------------|--------|
| Type | Text, Textarea |
| State | Enabled, Focus, Filled, Disabled |
| Error | False, True |

Addon Inline types: Check circle, Spinner, Icon, Text, Button, Kbd
Addon Block positions: Start, End
Button types: Default, Secondary, Destructive, Outline, Ghost, Link

Layout: flex row (gap 8px, h 36px, px 12px, py 8px), rounded, shadow-xs, background → Addon Inline (start) + Input + Addon Inline (end)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Group border color, background (with opacity) |
| `--muted-foreground` | `--muted-foreground` | Addon text/icon color, placeholder |
| `--foreground` | `--foreground` | Check circle addon background |
| `--muted` | `--muted` | Kbd background |
| `--radius` (derived) | `--radius` | Group border radius |
| `h-9` (36px) | `height` | Group height |
| `px-3` (12px) | `padding-inline` | Horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-xs` | `box-shadow` | Shadow |

### Theme Behavior

- Group border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error ring differs: `destructive/20` in light, `destructive/40` in dark
- Dark mode adds `bg-input/30` background to group
- Addon text uses `--muted-foreground` — adapts to light/dark via theme
- All spacing and layout tokens are mode-independent
