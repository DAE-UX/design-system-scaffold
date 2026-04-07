# Component: Menubar

> **TL;DR:** A horizontal menu bar with dropdown menus. 16 parts built on Radix UI Menubar primitive. Supports items, checkbox items, radio items, sub-menus, groups, labels, separators, shortcuts, and inset alignment. Items support `default` and `destructive` variants. Renders via Portal with enter/exit animations. Similar API to Dropdown Menu but with a persistent horizontal bar containing multiple menu triggers.

## Metadata

- **Component Name:** Menubar
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A horizontal menu bar containing multiple dropdown menus, built on Radix UI Menubar primitive.

- Menubar renders as a horizontal flex container with `h-9`, border, background, and shadow
- MenubarMenu wraps each individual menu (trigger + content pair)
- MenubarTrigger opens its menu on click; shows accent background when focused or open
- MenubarContent renders via Portal with configurable `align` (default `"start"`), `alignOffset` (default `-4`), and `sideOffset` (default `8`)
- MenubarItem is a standard selectable action; supports `inset` and `variant="destructive"`
- MenubarCheckboxItem is a toggleable item with a check indicator
- MenubarRadioItem is an exclusive-choice item with a circle indicator (inside RadioGroup)
- MenubarSub + MenubarSubTrigger + MenubarSubContent create nested sub-menus
- MenubarLabel provides non-interactive group headings
- MenubarSeparator renders a horizontal divider
- MenubarShortcut displays keyboard shortcut hints (right-aligned, muted)
- Items support `inset` prop for left-padding alignment
- Disabled items have `pointer-events: none` and `opacity: 0.5`
- SVG icons inside items are auto-sized to 16px with muted foreground color
- Content uses `--radix-menubar-content-transform-origin` for animation origin
- Enter/exit animations: fade + zoom + directional slide
- Keyboard navigation moves between menu triggers with arrow keys

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Menubar` — Root horizontal bar container.
- `MenubarMenu` — Individual menu wrapper (trigger + content).
- `MenubarTrigger` — Click target that opens a menu.
- `MenubarContent` — Menu panel. Rendered via Portal with animations.
- `MenubarItem` — Standard selectable action.
- `MenubarCheckboxItem` — Toggleable item with check indicator.
- `MenubarRadioGroup` — Container for exclusive radio items.
- `MenubarRadioItem` — Exclusive-choice item with circle indicator.
- `MenubarSub` — Sub-menu container.
- `MenubarSubTrigger` — Item that opens a sub-menu. Shows chevron-right icon.
- `MenubarSubContent` — Sub-menu panel with animations.
- `MenubarGroup` — Groups related items.
- `MenubarLabel` — Non-interactive group heading.
- `MenubarSeparator` — Horizontal divider.
- `MenubarShortcut` — Keyboard shortcut hint text.
- `MenubarPortal` — Portal wrapper for content rendering.

### Props

#### Menubar

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### MenubarMenu

No additional props beyond Radix MenubarMenu.

#### MenubarTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### MenubarContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| align | `"start" \| "center" \| "end"` | `"start"` | Alignment relative to trigger |
| alignOffset | `number` | `-4` | Alignment offset |
| sideOffset | `number` | `8` | Offset from trigger |
| className | `string` | — | Additional CSS classes |

#### MenubarItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| variant | `"default" \| "destructive"` | `"default"` | Visual style variant |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### MenubarCheckboxItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| checked | `boolean` | — | Checked state |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### MenubarRadioItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Radio item value |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### MenubarRadioGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Controlled selected value |
| onValueChange | `(value: string) => void` | — | Called when selection changes |

#### MenubarSubTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

#### MenubarLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

### Variants

#### MenubarItem Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Standard item styling | Normal actions |
| `destructive` | Red text, red hover background | Delete, remove, dangerous actions |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"menubar"`, `"menubar-menu"`, `"menubar-trigger"`, `"menubar-content"`, `"menubar-item"`, `"menubar-checkbox-item"`, `"menubar-radio-item"`, `"menubar-sub"`, `"menubar-sub-trigger"`, `"menubar-sub-content"`, `"menubar-group"`, `"menubar-radio-group"`, `"menubar-label"`, `"menubar-separator"`, `"menubar-shortcut"`, `"menubar-portal"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Content, SubContent, SubTrigger, Trigger, CheckboxItem, RadioItem |
| `[data-variant]` | `"default"`, `"destructive"` | MenubarItem |
| `[data-inset]` | present when `inset` is true | MenuItem, SubTrigger, Label |
| `[data-disabled]` | present when disabled | MenuItem, CheckboxItem, RadioItem |

### CSS Variables (Radix)

| Variable | Description | Applies To |
|----------|-------------|------------|
| `--radix-menubar-content-transform-origin` | Transform origin for animations | Content, SubContent |

## Composition Patterns

### Basic Menubar

```tsx
<Menubar>
  <MenubarMenu>
    <MenubarTrigger>File</MenubarTrigger>
    <MenubarContent>
      <MenubarItem>
        New Tab <MenubarShortcut>⌘T</MenubarShortcut>
      </MenubarItem>
      <MenubarItem>
        New Window <MenubarShortcut>⌘N</MenubarShortcut>
      </MenubarItem>
      <MenubarSeparator />
      <MenubarItem>Print <MenubarShortcut>⌘P</MenubarShortcut></MenubarItem>
    </MenubarContent>
  </MenubarMenu>
  <MenubarMenu>
    <MenubarTrigger>Edit</MenubarTrigger>
    <MenubarContent>
      <MenubarItem>
        Undo <MenubarShortcut>⌘Z</MenubarShortcut>
      </MenubarItem>
      <MenubarItem>
        Redo <MenubarShortcut>⇧⌘Z</MenubarShortcut>
      </MenubarItem>
    </MenubarContent>
  </MenubarMenu>
</Menubar>
```

### With Sub-Menu

```tsx
<MenubarContent>
  <MenubarItem>New File</MenubarItem>
  <MenubarSub>
    <MenubarSubTrigger>Share</MenubarSubTrigger>
    <MenubarSubContent>
      <MenubarItem>Email</MenubarItem>
      <MenubarItem>Link</MenubarItem>
    </MenubarSubContent>
  </MenubarSub>
</MenubarContent>
```

### With Checkbox and Radio Items

```tsx
<MenubarContent>
  <MenubarCheckboxItem checked>Show Toolbar</MenubarCheckboxItem>
  <MenubarCheckboxItem>Show Sidebar</MenubarCheckboxItem>
  <MenubarSeparator />
  <MenubarRadioGroup value="bottom">
    <MenubarLabel>Panel Position</MenubarLabel>
    <MenubarRadioItem value="top">Top</MenubarRadioItem>
    <MenubarRadioItem value="bottom">Bottom</MenubarRadioItem>
    <MenubarRadioItem value="right">Right</MenubarRadioItem>
  </MenubarRadioGroup>
</MenubarContent>
```

## Accessibility

- **WAI-ARIA Pattern:** [Menu Bar Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menubar)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Menubar | `menubar` (implicit via Radix) | Horizontal menu bar |
| MenubarTrigger | `menuitem` (implicit via Radix) | Menu bar item that opens a menu |
| MenubarContent | `menu` (implicit via Radix) | Dropdown menu |
| MenubarItem | `menuitem` (implicit via Radix) | Standard menu item |
| MenubarCheckboxItem | `menuitemcheckbox` (implicit via Radix) | Toggleable item |
| MenubarRadioItem | `menuitemradio` (implicit via Radix) | Exclusive-choice item |
| MenubarRadioGroup | `group` (implicit via Radix) | Radio group container |
| MenubarLabel | label for group | Non-interactive heading |
| MenubarSeparator | `separator` (implicit via Radix) | Visual divider |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Right | Move to next menu trigger in the bar |
| Arrow Left | Move to previous menu trigger in the bar |
| Arrow Down | Open menu (on trigger); move focus to next item |
| Arrow Up | Move focus to previous item |
| Enter / Space | Open menu (on trigger); activate focused item |
| Arrow Right (on SubTrigger) | Open sub-menu |
| Arrow Left (in sub-menu) | Close sub-menu |
| Escape | Close the current menu |
| Home | Move focus to first item |
| End | Move focus to last item |
| Type-ahead | Focus item starting with typed character |

## HTML

### Standalone HTML Structure

```html
<div data-slot="menubar" role="menubar">
  <div data-slot="menubar-menu">
    <button data-slot="menubar-trigger" role="menuitem" data-state="closed">
      File
    </button>
    <!-- Menu (rendered in portal when open) -->
    <div data-slot="menubar-content" role="menu">
      <div data-slot="menubar-item" data-variant="default" role="menuitem">
        New Tab
        <span data-slot="menubar-shortcut">⌘T</span>
      </div>
      <div data-slot="menubar-separator" role="separator"></div>
      <div data-slot="menubar-item" data-variant="default" role="menuitem">
        Print
      </div>
    </div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Menubar */
.Menubar {
  display: flex;
  height: 2.25rem;
  align-items: center;
  gap: 0.25rem;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--background);
  padding: 0.25rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* MenubarTrigger */
.MenubarTrigger {
  display: flex;
  align-items: center;
  border-radius: 0.125rem;
  padding: 0.25rem 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  outline: none;
  user-select: none;
}

.MenubarTrigger:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.MenubarTrigger[data-state="open"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* MenubarContent */
.MenubarContent {
  z-index: 50;
  min-width: 12rem;
  transform-origin: var(--radix-menubar-content-transform-origin);
  overflow: hidden;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* MenubarItem */
.MenubarItem {
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

.MenubarItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.MenubarItem[data-disabled] {
  pointer-events: none;
  opacity: 0.5;
}

.MenubarItem[data-inset] {
  padding-left: 2rem;
}

/* Destructive variant */
.MenubarItem[data-variant="destructive"] {
  color: var(--destructive);
}

.MenubarItem[data-variant="destructive"]:focus {
  background-color: color-mix(in srgb, var(--destructive) 10%, transparent);
  color: var(--destructive);
}

/* Item icons */
.MenubarItem svg {
  pointer-events: none;
  flex-shrink: 0;
}

.MenubarItem svg:not([class*='size-']) {
  width: 1rem;
  height: 1rem;
}

.MenubarItem svg:not([class*='text-']) {
  color: var(--muted-foreground);
}

/* CheckboxItem */
.MenubarCheckboxItem {
  position: relative;
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.0625rem;
  padding: 0.375rem 0.5rem 0.375rem 2rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.MenubarCheckboxItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* RadioItem */
.MenubarRadioItem {
  position: relative;
  display: flex;
  cursor: default;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.0625rem;
  padding: 0.375rem 0.5rem 0.375rem 2rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.MenubarRadioItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Indicator containers */
.MenubarCheckboxItem .indicator,
.MenubarRadioItem .indicator {
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
.MenubarSubTrigger {
  display: flex;
  cursor: default;
  align-items: center;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.MenubarSubTrigger:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.MenubarSubTrigger[data-state="open"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.MenubarSubTrigger[data-inset] {
  padding-left: 2rem;
}

/* SubContent */
.MenubarSubContent {
  z-index: 50;
  min-width: 8rem;
  transform-origin: var(--radix-menubar-content-transform-origin);
  overflow: hidden;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

/* Label */
.MenubarLabel {
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
}

.MenubarLabel[data-inset] {
  padding-left: 2rem;
}

/* Separator */
.MenubarSeparator {
  margin: 0.25rem -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* Shortcut */
.MenubarShortcut {
  margin-left: auto;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Menubar | `flex h-9 items-center gap-1 rounded-md border bg-background p-1 shadow-xs` | Horizontal bar |
| Trigger | `flex items-center rounded-sm px-2 py-1 text-sm font-medium outline-hidden select-none focus:bg-accent focus:text-accent-foreground data-[state=open]:bg-accent data-[state=open]:text-accent-foreground` | Menu trigger |
| Content | `z-50 min-w-[12rem] origin-(--radix-menubar-content-transform-origin) overflow-hidden rounded-md border bg-popover p-1 text-popover-foreground shadow-md` | Menu panel |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Close animation |
| Content (slide) | `data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2` | Directional slide |
| Item | `relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50 data-[inset]:pl-8` | Item layout |
| Item (destructive) | `data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:bg-destructive/10 data-[variant=destructive]:focus:text-destructive dark:data-[variant=destructive]:focus:bg-destructive/20` | Destructive styling |
| Item icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='text-'])]:text-muted-foreground` | Icon sizing |
| Item (destructive icons) | `data-[variant=destructive]:*:[svg]:text-destructive!` | Destructive icon color |
| CheckboxItem | `relative flex cursor-default items-center gap-2 rounded-xs py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Checkbox item |
| Checkbox indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Check icon position |
| RadioItem | `relative flex cursor-default items-center gap-2 rounded-xs py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Radio item |
| Radio indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Circle icon position |
| SubTrigger | `flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm outline-none select-none focus:bg-accent focus:text-accent-foreground data-[inset]:pl-8 data-[state=open]:bg-accent data-[state=open]:text-accent-foreground` | Sub-menu trigger |
| SubTrigger chevron | `ml-auto h-4 w-4` | Right-aligned chevron |
| SubContent | `z-50 min-w-[8rem] origin-(--radix-menubar-content-transform-origin) overflow-hidden rounded-md border bg-popover p-1 text-popover-foreground shadow-lg` | Sub-menu panel |
| Label | `px-2 py-1.5 text-sm font-medium data-[inset]:pl-8` | Group heading |
| Separator | `-mx-1 my-1 h-px bg-border` | Divider |
| Shortcut | `ml-auto text-xs tracking-widest text-muted-foreground` | Shortcut hint |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Menubar bar background | `shadcn` |
| `--border` | Bar border, content border, separator | `shadcn` |
| `--popover` | Content/sub-content background | `shadcn` |
| `--popover-foreground` | Content/sub-content text | `shadcn` |
| `--accent` | Focused/open trigger and item background | `shadcn` |
| `--accent-foreground` | Focused/open trigger and item text | `shadcn` |
| `--muted-foreground` | Shortcut text, icon default color | `shadcn` |
| `--destructive` | Destructive item text, hover background, icon color | `shadcn` |

## Theme Support

### Component Structure

Menubar Trigger states: Selected (False/True)

Menu Item variants:

| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Hover, Disabled |
| Variant | Default, Checked, Radio, Icon |
| SubTrigger | False, True |

Bar layout: flex row, background, border, h 36px, p 4px, gap 4px, rounded, shadow-xs
Trigger (selected): accent bg, accent-foreground text, text-sm, font-medium

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Menubar background |
| `--border` | `--border` | Menubar border |
| `--foreground` | `--foreground` | Trigger text (unselected) |
| `--accent` | `--accent` | Trigger background (selected/hover) |
| `--accent-foreground` | `--accent-foreground` | Trigger text (selected/hover) |
| `h-9` (36px) | `height` | Menubar height |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Trigger text size |
| `shadow-xs` | `box-shadow` | Bar shadow |

### Theme Behavior

- Bar uses `--background` / `--border` — adapts to light/dark via theme
- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Focused items use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Destructive hover background differs: 10% opacity in light, 20% in dark
- Separator uses `--border` — adapts to light/dark via theme
- Shortcuts and icons use `--muted-foreground` — adapts to light/dark via theme
- All animations (fade, zoom, slide) are mode-independent
