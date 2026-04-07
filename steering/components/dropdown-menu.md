# Component: Dropdown Menu

> **TL;DR:** A menu triggered by a button click. 15 parts built on Radix UI Dropdown Menu primitive. Supports items, checkbox items, radio items, sub-menus, groups, labels, separators, shortcuts, and inset alignment. Items support `default` and `destructive` variants. Renders via Portal with enter/exit animations. Nearly identical API to Context Menu but triggered by click instead of right-click.

## Metadata

- **Component Name:** Dropdown Menu
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A menu triggered by clicking a button, built on Radix UI Dropdown Menu primitive.

- DropdownMenuTrigger opens the menu on click; supports `asChild`
- DropdownMenuContent renders via Portal with configurable `sideOffset` (default 4px)
- DropdownMenuItem is a standard selectable action
- DropdownMenuCheckboxItem is a toggleable item with a check indicator
- DropdownMenuRadioItem is an exclusive-choice item with a circle indicator (inside RadioGroup)
- DropdownMenuSub + DropdownMenuSubTrigger + DropdownMenuSubContent create nested sub-menus
- DropdownMenuLabel provides non-interactive group headings
- DropdownMenuSeparator renders a horizontal divider
- DropdownMenuShortcut displays keyboard shortcut hints (right-aligned, muted)
- Items support `inset` prop for left-padding alignment
- Items support `variant="destructive"` for dangerous actions
- Disabled items have `pointer-events: none` and `opacity: 0.5`
- SVG icons inside items are auto-sized to 16px with muted foreground color
- Content max-height uses Radix CSS variable for available space
- Enter/exit animations: fade + zoom + directional slide
- Supports controlled mode via `open` and `onOpenChange` on root
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use for 5–15 contextual actions
- Sort actions logically with most relevant first, destructive last
- Use 6 or fewer items in expandable child menus
- Separate selectable items from non-selectable items
- Use selectable items only for immediate-effect actions

#### Don't
- Don't use expandable mode with primary button variant
- Don't change labels of selectable items based on selected state
- Don't overwhelm users with too many menu items

Sources: shadCN, Radix UI

## API

### Parts

- `DropdownMenu` — Root. Manages open/close state.
- `DropdownMenuTrigger` — Click target that opens the menu. Supports `asChild`.
- `DropdownMenuContent` — Menu panel. Rendered via Portal with animations.
- `DropdownMenuItem` — Standard selectable action.
- `DropdownMenuCheckboxItem` — Toggleable item with check indicator.
- `DropdownMenuRadioGroup` — Container for exclusive radio items.
- `DropdownMenuRadioItem` — Exclusive-choice item with circle indicator.
- `DropdownMenuSub` — Sub-menu container.
- `DropdownMenuSubTrigger` — Item that opens a sub-menu. Shows chevron-right icon.
- `DropdownMenuSubContent` — Sub-menu panel with animations.
- `DropdownMenuGroup` — Groups related items.
- `DropdownMenuLabel` — Non-interactive group heading.
- `DropdownMenuSeparator` — Horizontal divider.
- `DropdownMenuShortcut` — Keyboard shortcut hint text.
- `DropdownMenuPortal` — Portal wrapper for content rendering.

### Props

#### DropdownMenu

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | — | Default open state (uncontrolled) |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |
| modal | `boolean` | `true` | Whether to render as modal |
| dir | `"ltr" \| "rtl"` | — | Text direction |

#### DropdownMenuTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| sideOffset | `number` | `4` | Offset from trigger |
| side | `"top" \| "bottom" \| "left" \| "right"` | `"bottom"` | Preferred side |
| align | `"start" \| "center" \| "end"` | — | Alignment relative to trigger |
| loop | `boolean` | — | Loop keyboard navigation |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| variant | `"default" \| "destructive"` | `"default"` | Visual style variant |
| disabled | `boolean` | `false` | Disable this item |
| onSelect | `(event: Event) => void` | — | Called when item is selected |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuCheckboxItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| checked | `boolean` | — | Checked state |
| onCheckedChange | `(checked: boolean) => void` | — | Called when checked changes |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuRadioItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Radio item value |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuRadioGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Controlled selected value |
| onValueChange | `(value: string) => void` | — | Called when selection changes |

#### DropdownMenuSubTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

#### DropdownMenuLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

### Variants

#### DropdownMenuItem Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Standard item styling | Normal actions |
| `destructive` | Red text, red hover background | Delete, remove, dangerous actions |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"dropdown-menu"`, `"dropdown-menu-trigger"`, `"dropdown-menu-content"`, `"dropdown-menu-item"`, `"dropdown-menu-checkbox-item"`, `"dropdown-menu-radio-item"`, `"dropdown-menu-sub"`, `"dropdown-menu-sub-trigger"`, `"dropdown-menu-sub-content"`, `"dropdown-menu-group"`, `"dropdown-menu-radio-group"`, `"dropdown-menu-label"`, `"dropdown-menu-separator"`, `"dropdown-menu-shortcut"`, `"dropdown-menu-portal"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Content, SubContent, SubTrigger, CheckboxItem, RadioItem |
| `[data-variant]` | `"default"`, `"destructive"` | DropdownMenuItem |
| `[data-inset]` | present when `inset` is true | MenuItem, SubTrigger, Label |
| `[data-disabled]` | present when disabled | MenuItem, CheckboxItem, RadioItem |
| `[data-highlighted]` | present when focused | MenuItem, CheckboxItem, RadioItem |
| `[data-side]` | `"top"`, `"bottom"`, `"left"`, `"right"` | Content, SubContent |

### CSS Variables (Radix)

| Variable | Description | Applies To |
|----------|-------------|------------|
| `--radix-dropdown-menu-content-transform-origin` | Transform origin for animations | Content, SubContent |
| `--radix-dropdown-menu-content-available-height` | Available height for content | Content |

## Composition Patterns

### Basic with Groups

```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="outline">Open</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent className="w-56" align="start">
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuGroup>
      <DropdownMenuItem>
        Profile
        <DropdownMenuShortcut>⇧⌘P</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        Billing
        <DropdownMenuShortcut>⌘B</DropdownMenuShortcut>
      </DropdownMenuItem>
      <DropdownMenuItem>
        Settings
        <DropdownMenuShortcut>⌘S</DropdownMenuShortcut>
      </DropdownMenuItem>
    </DropdownMenuGroup>
    <DropdownMenuSeparator />
    <DropdownMenuItem>
      Log out
      <DropdownMenuShortcut>⇧⌘Q</DropdownMenuShortcut>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### With Sub-Menu

```tsx
<DropdownMenuContent>
  <DropdownMenuGroup>
    <DropdownMenuItem>Team</DropdownMenuItem>
    <DropdownMenuSub>
      <DropdownMenuSubTrigger>Invite users</DropdownMenuSubTrigger>
      <DropdownMenuPortal>
        <DropdownMenuSubContent>
          <DropdownMenuItem>Email</DropdownMenuItem>
          <DropdownMenuItem>Message</DropdownMenuItem>
          <DropdownMenuSeparator />
          <DropdownMenuItem>More...</DropdownMenuItem>
        </DropdownMenuSubContent>
      </DropdownMenuPortal>
    </DropdownMenuSub>
  </DropdownMenuGroup>
</DropdownMenuContent>
```

### With Checkbox Items

```tsx
<DropdownMenuContent>
  <DropdownMenuCheckboxItem checked>Show Toolbar</DropdownMenuCheckboxItem>
  <DropdownMenuCheckboxItem>Show Sidebar</DropdownMenuCheckboxItem>
</DropdownMenuContent>
```

### With Radio Items

```tsx
<DropdownMenuContent>
  <DropdownMenuRadioGroup value="bottom">
    <DropdownMenuLabel>Panel Position</DropdownMenuLabel>
    <DropdownMenuRadioItem value="top">Top</DropdownMenuRadioItem>
    <DropdownMenuRadioItem value="bottom">Bottom</DropdownMenuRadioItem>
    <DropdownMenuRadioItem value="right">Right</DropdownMenuRadioItem>
  </DropdownMenuRadioGroup>
</DropdownMenuContent>
```

### Destructive Item

```tsx
<DropdownMenuContent>
  <DropdownMenuItem>Edit</DropdownMenuItem>
  <DropdownMenuSeparator />
  <DropdownMenuItem variant="destructive">Delete</DropdownMenuItem>
</DropdownMenuContent>
```

## Accessibility

- **WAI-ARIA Pattern:** [Menu Button Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| DropdownMenuTrigger | `button` with `aria-haspopup="menu"` | Trigger button |
| DropdownMenuTrigger | `aria-expanded` | Set by Radix based on open state |
| DropdownMenuContent | `menu` (implicit via Radix) | Menu container |
| DropdownMenuItem | `menuitem` (implicit via Radix) | Standard menu item |
| DropdownMenuCheckboxItem | `menuitemcheckbox` (implicit via Radix) | Toggleable item |
| DropdownMenuRadioItem | `menuitemradio` (implicit via Radix) | Exclusive-choice item |
| DropdownMenuRadioGroup | `group` (implicit via Radix) | Radio group container |
| DropdownMenuLabel | label for group | Non-interactive heading |
| DropdownMenuSeparator | `separator` (implicit via Radix) | Visual divider |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Enter / Space | Open menu (on trigger); activate focused item |
| Arrow Down | Open menu (on trigger); move focus to next item |
| Arrow Up | Move focus to previous item |
| Arrow Right | Open sub-menu (on SubTrigger) |
| Arrow Left | Close sub-menu |
| Escape | Close the menu |
| Home | Move focus to first item |
| End | Move focus to last item |
| Type-ahead | Focus item starting with typed character |

## HTML

### Standalone HTML Structure

```html
<!-- Trigger -->
<button data-slot="dropdown-menu-trigger" aria-haspopup="menu" aria-expanded="false">
  Open
</button>
<!-- Menu (rendered in portal when open) -->
<div data-slot="dropdown-menu-content" role="menu">
  <div data-slot="dropdown-menu-label">My Account</div>
  <div data-slot="dropdown-menu-item" data-variant="default" role="menuitem">
    Profile
    <span data-slot="dropdown-menu-shortcut">⇧⌘P</span>
  </div>
  <div data-slot="dropdown-menu-separator" role="separator"></div>
  <div data-slot="dropdown-menu-item" data-variant="destructive" role="menuitem">
    Delete
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* DropdownMenuContent */
.DropdownMenuContent {
  z-index: 50;
  max-height: var(--radix-dropdown-menu-content-available-height);
  min-width: 8rem;
  transform-origin: var(--radix-dropdown-menu-content-transform-origin);
  overflow-x: hidden;
  overflow-y: auto;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* DropdownMenuSubContent */
.DropdownMenuSubContent {
  z-index: 50;
  min-width: 8rem;
  transform-origin: var(--radix-dropdown-menu-content-transform-origin);
  overflow: hidden;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

/* DropdownMenuItem */
.DropdownMenuItem {
  position: relative;
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.DropdownMenuItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.DropdownMenuItem[data-disabled] {
  pointer-events: none;
  opacity: 0.5;
}

.DropdownMenuItem[data-inset] {
  padding-left: 2rem;
}

/* Destructive variant */
.DropdownMenuItem[data-variant="destructive"] {
  color: var(--destructive);
}
.DropdownMenuItem[data-variant="destructive"]:focus {
  background-color: color-mix(in srgb, var(--destructive) 10%, transparent);
  color: var(--destructive);
}

/* Item icons */
.DropdownMenuItem svg {
  pointer-events: none;
  flex-shrink: 0;
}
.DropdownMenuItem svg:not([class*='size-']) {
  width: 1rem;
  height: 1rem;
}
.DropdownMenuItem svg:not([class*='text-']) {
  color: var(--muted-foreground);
}

/* CheckboxItem */
.DropdownMenuCheckboxItem {
  position: relative;
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem 0.375rem 2rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.DropdownMenuCheckboxItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* RadioItem */
.DropdownMenuRadioItem {
  position: relative;
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem 0.375rem 2rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.DropdownMenuRadioItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Indicator containers */
.DropdownMenuCheckboxItem .indicator,
.DropdownMenuRadioItem .indicator {
  pointer-events: none;
  position: absolute;
  left: 0.5rem;
  display: flex;
  width: 0.875rem;
  height: 0.875rem;
  align-items: center;
  justify-content: center;
}

/* SubTrigger */
.DropdownMenuSubTrigger {
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.DropdownMenuSubTrigger:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.DropdownMenuSubTrigger[data-state="open"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.DropdownMenuSubTrigger[data-inset] {
  padding-left: 2rem;
}

/* Label */
.DropdownMenuLabel {
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
}

.DropdownMenuLabel[data-inset] {
  padding-left: 2rem;
}

/* Separator */
.DropdownMenuSeparator {
  margin: 0.25rem -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* Shortcut */
.DropdownMenuShortcut {
  margin-left: auto;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Content | `z-50 max-h-(--radix-dropdown-menu-content-available-height) min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-x-hidden overflow-y-auto rounded-md border bg-popover p-1 text-popover-foreground shadow-md` | Menu panel |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Close animation |
| Content (slide) | `data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2` | Directional slide |
| SubContent | `z-50 min-w-[8rem] origin-(--radix-dropdown-menu-content-transform-origin) overflow-hidden rounded-md border bg-popover p-1 text-popover-foreground shadow-lg` | Sub-menu panel |
| Item | `relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none` | Item layout |
| Item (focus) | `focus:bg-accent focus:text-accent-foreground` | Focus state |
| Item (disabled) | `data-[disabled]:pointer-events-none data-[disabled]:opacity-50` | Disabled state |
| Item (inset) | `data-[inset]:pl-8` | Left padding alignment |
| Item (destructive) | `data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:bg-destructive/10 data-[variant=destructive]:focus:text-destructive dark:data-[variant=destructive]:focus:bg-destructive/20` | Destructive styling |
| Item icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='text-'])]:text-muted-foreground` | Icon sizing |
| Item (destructive icons) | `data-[variant=destructive]:*:[svg]:text-destructive!` | Destructive icon color |
| CheckboxItem | `relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Checkbox item |
| Checkbox indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Check icon position |
| RadioItem | `relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Radio item |
| Radio indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Circle icon position |
| SubTrigger | `flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground data-[inset]:pl-8 data-[state=open]:bg-accent data-[state=open]:text-accent-foreground` | Sub-menu trigger |
| SubTrigger icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='text-'])]:text-muted-foreground` | Sub-trigger icon sizing |
| SubTrigger chevron | `ml-auto size-4` | Right-aligned chevron |
| Label | `px-2 py-1.5 text-sm font-medium data-[inset]:pl-8` | Group heading |
| Separator | `-mx-1 my-1 h-px bg-border` | Divider |
| Shortcut | `ml-auto text-xs tracking-widest text-muted-foreground` | Shortcut hint |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Content/sub-content background | `shadcn` |
| `--popover-foreground` | Content/sub-content text | `shadcn` |
| `--border` | Content border, separator | `shadcn` |
| `--accent` | Focused item background, open sub-trigger background | `shadcn` |
| `--accent-foreground` | Focused item text | `shadcn` |
| `--muted-foreground` | Shortcut text, icon default color | `shadcn` |
| `--destructive` | Destructive item text, hover background, icon color | `shadcn` |

## Theme Support

### Component Structure

**Dropdown Menu Item** — 4 variant properties:

| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Hover, Disabled |
| Variant | Default, Checked, Radio, Icon, Item |
| SubTrigger | False, True |

Item layout: flex row (px 8px, py 6px, gap 10px, rounded) → Label text (text-sm) + optional Shortcut text (text-sm, muted-foreground)

**Dropdown Menu Trigger** — States: Enabled, Hover, Active

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--popover-foreground)` | `--popover-foreground` | Item label text color |
| `var(--muted-foreground)` | `--muted-foreground` | Shortcut text color |
| `rounded-sm` (derived) | `rounded-sm` | Item border radius |
| `px-2` (8px) | `px-2` | Item horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `text-sm` | Item text size |

### Theme Behavior

- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Focused items use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Destructive hover background differs: 10% opacity in light, 20% in dark
- Separator uses `--border` — adapts to light/dark via theme
- Shortcuts and icons use `--muted-foreground` — adapts to light/dark via theme
- All animations (fade, zoom, slide) are mode-independent
- Shadow intensity differs: `shadow-md` for content, `shadow-lg` for sub-content