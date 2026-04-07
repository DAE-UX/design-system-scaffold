# Component: Context Menu

> **TL;DR:** A menu triggered by right-click (or long-press on touch). 15 parts built on Radix UI Context Menu primitive. Supports items, checkbox items, radio items, sub-menus, groups, labels, separators, shortcuts, and inset alignment. Items support `default` and `destructive` variants. Renders via Portal with enter/exit animations.

## Metadata

- **Component Name:** Context Menu
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

A menu triggered by right-click (desktop) or long-press (touch) on a trigger area.

- ContextMenuTrigger defines the right-click/long-press target area
- ContextMenuContent renders the menu via Portal with animations
- ContextMenuItem is a standard selectable action
- ContextMenuCheckboxItem is a toggleable item with a check indicator
- ContextMenuRadioItem is an exclusive-choice item with a circle indicator (used inside RadioGroup)
- ContextMenuSub + ContextMenuSubTrigger + ContextMenuSubContent create nested sub-menus
- ContextMenuLabel provides non-interactive group headings
- ContextMenuSeparator renders a horizontal divider
- ContextMenuShortcut displays keyboard shortcut hints (right-aligned, muted)
- Items support `inset` prop for left-padding alignment (when some items have icons and others don't)
- Items support `variant="destructive"` for dangerous actions (red text, red hover background)
- Disabled items have `pointer-events: none` and `opacity: 0.5`
- SVG icons inside items are auto-sized to 16px with muted foreground color
- Content max-height uses Radix CSS variable for available space
- Enter/exit animations: fade + zoom + directional slide
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use for contextual actions triggered by right-click.
- Keep the menu focused on actions relevant to the clicked element.
- Place destructive actions at the bottom with a separator.

#### Don't
- Don't use as the only way to access actions — provide alternative paths.
- Don't include more than 10-12 items without sub-menus.
- Don't use for primary navigation.

Sources: shadCN, Radix UI

## API

### Parts

- `ContextMenu` — Root. Manages open/close state.
- `ContextMenuTrigger` — Right-click/long-press target area.
- `ContextMenuContent` — Menu panel. Rendered via Portal with animations.
- `ContextMenuItem` — Standard selectable action.
- `ContextMenuCheckboxItem` — Toggleable item with check indicator.
- `ContextMenuRadioGroup` — Container for exclusive radio items.
- `ContextMenuRadioItem` — Exclusive-choice item with circle indicator.
- `ContextMenuSub` — Sub-menu container.
- `ContextMenuSubTrigger` — Item that opens a sub-menu. Shows chevron-right icon.
- `ContextMenuSubContent` — Sub-menu panel with animations.
- `ContextMenuGroup` — Groups related items.
- `ContextMenuLabel` — Non-interactive group heading.
- `ContextMenuSeparator` — Horizontal divider.
- `ContextMenuShortcut` — Keyboard shortcut hint text.
- `ContextMenuPortal` — Portal wrapper for content rendering.

### Props

#### ContextMenu

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |

#### ContextMenuTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| disabled | `boolean` | `false` | Disable the trigger |
| className | `string` | — | Additional CSS classes |

#### ContextMenuContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |
| alignOffset | `number` | — | Alignment offset |
| loop | `boolean` | — | Loop keyboard navigation |

#### ContextMenuItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| variant | `"default" \| "destructive"` | `"default"` | Visual style variant |
| disabled | `boolean` | `false` | Disable this item |
| onSelect | `(event: Event) => void` | — | Called when item is selected |
| className | `string` | — | Additional CSS classes |

#### ContextMenuCheckboxItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| checked | `boolean` | — | Checked state |
| onCheckedChange | `(checked: boolean) => void` | — | Called when checked changes |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### ContextMenuRadioItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Radio item value |
| disabled | `boolean` | `false` | Disable this item |
| className | `string` | — | Additional CSS classes |

#### ContextMenuRadioGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Controlled selected value |
| onValueChange | `(value: string) => void` | — | Called when selection changes |

#### ContextMenuSubTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

#### ContextMenuLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| inset | `boolean` | — | Add left padding for icon alignment |
| className | `string` | — | Additional CSS classes |

### Variants

#### ContextMenuItem Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Standard item styling | Normal actions |
| `destructive` | Red text, red hover background | Delete, remove, dangerous actions |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"context-menu"`, `"context-menu-trigger"`, `"context-menu-content"`, `"context-menu-item"`, `"context-menu-checkbox-item"`, `"context-menu-radio-item"`, `"context-menu-sub"`, `"context-menu-sub-trigger"`, `"context-menu-sub-content"`, `"context-menu-group"`, `"context-menu-radio-group"`, `"context-menu-label"`, `"context-menu-separator"`, `"context-menu-shortcut"`, `"context-menu-portal"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Content, SubContent, SubTrigger, CheckboxItem, RadioItem |
| `[data-variant]` | `"default"`, `"destructive"` | ContextMenuItem |
| `[data-inset]` | present when `inset` is true | MenuItem, SubTrigger, Label |
| `[data-disabled]` | present when disabled | MenuItem, CheckboxItem, RadioItem |
| `[data-highlighted]` | present when focused | MenuItem, CheckboxItem, RadioItem |
| `[data-side]` | `"top"`, `"bottom"`, `"left"`, `"right"` | Content, SubContent |

### CSS Variables (Radix)

| Variable | Description | Applies To |
|----------|-------------|------------|
| `--radix-context-menu-content-transform-origin` | Transform origin for animations | Content, SubContent |
| `--radix-context-menu-content-available-height` | Available height for content | Content |

## Composition Patterns

### Basic

```tsx
<ContextMenu>
  <ContextMenuTrigger className="flex h-[150px] w-[300px] items-center justify-center rounded-md border border-dashed text-sm">
    Right click here
  </ContextMenuTrigger>
  <ContextMenuContent className="w-52">
    <ContextMenuItem>Back</ContextMenuItem>
    <ContextMenuItem>Forward</ContextMenuItem>
    <ContextMenuItem>Reload</ContextMenuItem>
  </ContextMenuContent>
</ContextMenu>
```

### With Shortcuts

```tsx
<ContextMenuContent className="w-52">
  <ContextMenuItem inset>
    Back
    <ContextMenuShortcut>⌘[</ContextMenuShortcut>
  </ContextMenuItem>
  <ContextMenuItem inset disabled>
    Forward
    <ContextMenuShortcut>⌘]</ContextMenuShortcut>
  </ContextMenuItem>
  <ContextMenuItem inset>
    Reload
    <ContextMenuShortcut>⌘R</ContextMenuShortcut>
  </ContextMenuItem>
</ContextMenuContent>
```

### With Sub-Menu

```tsx
<ContextMenuContent className="w-52">
  <ContextMenuItem inset>Back</ContextMenuItem>
  <ContextMenuSub>
    <ContextMenuSubTrigger inset>More Tools</ContextMenuSubTrigger>
    <ContextMenuSubContent className="w-44">
      <ContextMenuItem>Save Page...</ContextMenuItem>
      <ContextMenuItem>Create Shortcut...</ContextMenuItem>
      <ContextMenuSeparator />
      <ContextMenuItem>Developer Tools</ContextMenuItem>
    </ContextMenuSubContent>
  </ContextMenuSub>
</ContextMenuContent>
```

### With Checkbox Items

```tsx
<ContextMenuContent>
  <ContextMenuCheckboxItem checked>Show Bookmarks</ContextMenuCheckboxItem>
  <ContextMenuCheckboxItem>Show Full URLs</ContextMenuCheckboxItem>
</ContextMenuContent>
```

### With Radio Items

```tsx
<ContextMenuContent>
  <ContextMenuRadioGroup value="pedro">
    <ContextMenuLabel inset>People</ContextMenuLabel>
    <ContextMenuRadioItem value="pedro">Pedro Duarte</ContextMenuRadioItem>
    <ContextMenuRadioItem value="colm">Colm Tuite</ContextMenuRadioItem>
  </ContextMenuRadioGroup>
</ContextMenuContent>
```

### Destructive Item

```tsx
<ContextMenuContent>
  <ContextMenuItem>Edit</ContextMenuItem>
  <ContextMenuSeparator />
  <ContextMenuItem variant="destructive">Delete</ContextMenuItem>
</ContextMenuContent>
```

## Accessibility

- **WAI-ARIA Pattern:** [Menu Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| ContextMenuContent | `menu` (implicit via Radix) | Menu container |
| ContextMenuItem | `menuitem` (implicit via Radix) | Standard menu item |
| ContextMenuCheckboxItem | `menuitemcheckbox` (implicit via Radix) | Toggleable item |
| ContextMenuRadioItem | `menuitemradio` (implicit via Radix) | Exclusive-choice item |
| ContextMenuRadioGroup | `group` (implicit via Radix) | Radio group container |
| ContextMenuLabel | label for group | Non-interactive heading |
| ContextMenuSeparator | `separator` (implicit via Radix) | Visual divider |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Down | Move focus to next item |
| Arrow Up | Move focus to previous item |
| Arrow Right | Open sub-menu (on SubTrigger) |
| Arrow Left | Close sub-menu |
| Enter | Activate the focused item |
| Space | Activate the focused item; toggle checkbox/radio items |
| Escape | Close the context menu |
| Home | Move focus to first item |
| End | Move focus to last item |

## HTML

### Standalone HTML Structure

```html
<!-- Trigger area -->
<div data-slot="context-menu-trigger">
  Right click here
</div>
<!-- Menu (rendered in portal when open) -->
<div data-slot="context-menu-content" role="menu">
  <div data-slot="context-menu-item" data-variant="default" role="menuitem">
    Back
    <span data-slot="context-menu-shortcut">⌘[</span>
  </div>
  <div data-slot="context-menu-separator" role="separator"></div>
  <div data-slot="context-menu-checkbox-item" role="menuitemcheckbox" aria-checked="true">
    <span><!-- check icon --></span>
    Show Bookmarks
  </div>
  <div data-slot="context-menu-label">People</div>
  <div data-slot="context-menu-radio-item" role="menuitemradio" aria-checked="true">
    <span><!-- circle icon --></span>
    Pedro Duarte
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* ContextMenuContent */
.ContextMenuContent {
  z-index: 50;
  max-height: var(--radix-context-menu-content-available-height);
  min-width: 8rem;
  transform-origin: var(--radix-context-menu-content-transform-origin);
  overflow-x: hidden;
  overflow-y: auto;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* ContextMenuSubContent */
.ContextMenuSubContent {
  z-index: 50;
  min-width: 8rem;
  transform-origin: var(--radix-context-menu-content-transform-origin);
  overflow: hidden;
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  padding: 0.25rem;
  color: var(--popover-foreground);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

/* ContextMenuItem */
.ContextMenuItem {
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

.ContextMenuItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.ContextMenuItem[data-disabled] {
  pointer-events: none;
  opacity: 0.5;
}

.ContextMenuItem[data-inset] {
  padding-left: 2rem;
}

/* Destructive variant */
.ContextMenuItem[data-variant="destructive"] {
  color: var(--destructive);
}
.ContextMenuItem[data-variant="destructive"]:focus {
  background-color: color-mix(in srgb, var(--destructive) 10%, transparent);
  color: var(--destructive);
}

/* ContextMenuItem icons */
.ContextMenuItem svg {
  pointer-events: none;
  flex-shrink: 0;
}
.ContextMenuItem svg:not([class*='size-']) {
  width: 1rem;
  height: 1rem;
}
.ContextMenuItem svg:not([class*='text-']) {
  color: var(--muted-foreground);
}

/* ContextMenuCheckboxItem */
.ContextMenuCheckboxItem {
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

.ContextMenuCheckboxItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Checkbox indicator */
.ContextMenuCheckboxItem .indicator {
  pointer-events: none;
  position: absolute;
  left: 0.5rem;
  display: flex;
  width: 0.875rem;
  height: 0.875rem;
  align-items: center;
  justify-content: center;
}

/* ContextMenuRadioItem */
.ContextMenuRadioItem {
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

.ContextMenuRadioItem:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Radio indicator */
.ContextMenuRadioItem .indicator {
  pointer-events: none;
  position: absolute;
  left: 0.5rem;
  display: flex;
  width: 0.875rem;
  height: 0.875rem;
  align-items: center;
  justify-content: center;
}

/* ContextMenuSubTrigger */
.ContextMenuSubTrigger {
  display: flex;
  cursor: default;
  align-items: center;
  border-radius: 0.125rem;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  outline: none;
  user-select: none;
}

.ContextMenuSubTrigger:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.ContextMenuSubTrigger[data-state="open"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.ContextMenuSubTrigger[data-inset] {
  padding-left: 2rem;
}

/* ContextMenuLabel */
.ContextMenuLabel {
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--foreground);
}

.ContextMenuLabel[data-inset] {
  padding-left: 2rem;
}

/* ContextMenuSeparator */
.ContextMenuSeparator {
  margin: 0.25rem -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* ContextMenuShortcut */
.ContextMenuShortcut {
  margin-left: auto;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Content | `z-50 max-h-(--radix-context-menu-content-available-height) min-w-[8rem] origin-(--radix-context-menu-content-transform-origin) overflow-x-hidden overflow-y-auto rounded-md border bg-popover p-1 text-popover-foreground shadow-md` | Menu panel |
| Content (open) | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` | Open animation |
| Content (closed) | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` | Close animation |
| Content (slide) | `data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2` | Directional slide |
| SubContent | `z-50 min-w-[8rem] origin-(--radix-context-menu-content-transform-origin) overflow-hidden rounded-md border bg-popover p-1 text-popover-foreground shadow-lg` | Sub-menu panel |
| Item | `relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none` | Item layout |
| Item (focus) | `focus:bg-accent focus:text-accent-foreground` | Focus state |
| Item (disabled) | `data-[disabled]:pointer-events-none data-[disabled]:opacity-50` | Disabled state |
| Item (inset) | `data-[inset]:pl-8` | Left padding alignment |
| Item (destructive) | `data-[variant=destructive]:text-destructive data-[variant=destructive]:focus:bg-destructive/10 data-[variant=destructive]:focus:text-destructive dark:data-[variant=destructive]:focus:bg-destructive/20` | Destructive styling |
| Item icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='text-'])]:text-muted-foreground` | Icon sizing |
| Item (destructive icons) | `data-[variant=destructive]:*:[svg]:text-destructive!` | Destructive icon color |
| CheckboxItem | `relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Checkbox item layout |
| Checkbox indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Check icon position |
| RadioItem | `relative flex cursor-default items-center gap-2 rounded-sm py-1.5 pr-2 pl-8 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground` | Radio item layout |
| Radio indicator | `pointer-events-none absolute left-2 flex size-3.5 items-center justify-center` | Circle icon position |
| SubTrigger | `flex cursor-default items-center rounded-sm px-2 py-1.5 text-sm outline-hidden select-none focus:bg-accent focus:text-accent-foreground data-[state=open]:bg-accent data-[state=open]:text-accent-foreground` | Sub-menu trigger |
| SubTrigger chevron | `ml-auto` | Right-aligned chevron |
| Label | `px-2 py-1.5 text-sm font-medium text-foreground data-[inset]:pl-8` | Group heading |
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
| `--foreground` | Label text color | `shadcn` |
| `--muted-foreground` | Shortcut text, icon default color | `shadcn` |
| `--destructive` | Destructive item text, hover background, icon color | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Level | 1, 2 |

Level 1 is the primary menu; Level 2 is a sub-menu.

**Context Menu Item** — 4 variant properties:

| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Hover, Disabled |
| Level | 1, 2 |
| Variant | Default, Checked, Radio |
| SubTrigger | False, True |

Item layout: flex row (pl 32px, pr 8px, py 6px, gap 10px, rounded) → Label text (text-sm) + optional Shortcut text (text-sm, muted-foreground)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--popover-foreground)` | `--popover-foreground` | Item label text color |
| `var(--muted-foreground)` | `--muted-foreground` | Shortcut text, group label text |
| `rounded-sm` (derived) | `rounded-sm` | Item border radius |
| `pl-8` (32px) | `pl-8` | Item left padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `text-sm` | Item text size |

### Theme Behavior

- Content uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Focused items use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Destructive hover background differs: 10% opacity in light, 20% in dark
- Separator uses `--border` — adapts to light/dark via theme
- Label uses `--foreground` — adapts to light/dark via theme
- Shortcuts and icons use `--muted-foreground` — adapts to light/dark via theme
- All animations (fade, zoom, slide) are mode-independent
- Shadow intensity differs: `shadow-md` for content, `shadow-lg` for sub-content