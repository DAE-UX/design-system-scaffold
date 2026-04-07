# Component: Command

> **TL;DR:** A command menu for search and quick actions. 9 parts built on the cmdk library. Provides a searchable, keyboard-navigable list of commands organized in groups. Includes a dialog variant (CommandDialog) that wraps Command in a Dialog for ⌘K-style palettes. Supports groups, separators, shortcuts, disabled items, and empty state. Icons and SVGs inside items are auto-sized.

## Metadata

- **Component Name:** Command
- **Source:** shadCN (cmdk)
- **Dependencies:** cmdk

## Behavior

A command menu for search and quick actions, built on the cmdk library by Dip.

- Command is the root container — full-width flex column with overflow hidden
- CommandInput renders a search input with a SearchIcon, wrapped in a border-bottom container
- CommandList is a scrollable container (max 300px height) for command items
- CommandGroup organizes items under a heading label
- CommandItem is an individual selectable command — highlights on selection via cmdk
- CommandEmpty displays when no items match the search query
- CommandSeparator renders a horizontal divider between groups
- CommandShortcut displays keyboard shortcut hints (right-aligned, muted)
- CommandDialog wraps Command inside a Dialog for modal command palette usage (⌘K pattern)
- cmdk handles filtering, keyboard navigation, and selection internally
- Items can be disabled via the `disabled` prop
- SVG icons inside items are auto-sized to 16px with `pointer-events: none`
- Icon colors default to `--muted-foreground` unless explicitly classed
- CommandDialog includes sr-only DialogHeader with configurable title and description
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use for command palette / search interfaces.
- Group related commands with headings.
- Provide keyboard shortcuts where applicable.

#### Don't
- Don't use as a replacement for standard navigation.
- Don't include too many items without grouping.
- Don't omit the empty state message.

Sources: shadCN, Radix UI

## API

### Parts

- `Command` — Root container. Wraps cmdk Command primitive. Flex column layout.
- `CommandDialog` — Modal variant. Wraps Command in a Dialog with sr-only header.
- `CommandInput` — Search input with SearchIcon. Wrapped in a border-bottom container.
- `CommandList` — Scrollable list container. Max height 300px.
- `CommandEmpty` — Empty state message. Shown when no results match.
- `CommandGroup` — Group container with heading. Organizes related items.
- `CommandItem` — Individual selectable command. Highlights on selection.
- `CommandSeparator` — Horizontal divider between groups.
- `CommandShortcut` — Keyboard shortcut hint. Right-aligned muted text.

### Props

#### Command

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |
| filter | `(value: string, search: string) => number` | — | Custom filter function |
| shouldFilter | `boolean` | `true` | Enable/disable built-in filtering |
| loop | `boolean` | `false` | Loop keyboard navigation |
| value | `string` | — | Controlled selected value |
| onValueChange | `(value: string) => void` | — | Called when selection changes |

#### CommandDialog

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| title | `string` | `"Command Palette"` | Dialog title (sr-only) |
| description | `string` | `"Search for a command to run..."` | Dialog description (sr-only) |
| showCloseButton | `boolean` | `true` | Show close button on dialog |
| className | `string` | — | Additional CSS classes |
| open | `boolean` | — | Controlled open state (from Dialog) |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |

#### CommandInput

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| placeholder | `string` | — | Placeholder text |
| className | `string` | — | Additional CSS classes |

#### CommandGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| heading | `string` | — | Group heading text |
| className | `string` | — | Additional CSS classes |

#### CommandItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Item value for filtering and selection |
| disabled | `boolean` | `false` | Disable this item |
| onSelect | `(value: string) => void` | — | Called when item is selected |
| className | `string` | — | Additional CSS classes |

### Variants

No style variants — Command has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"command"`, `"command-input-wrapper"`, `"command-input"`, `"command-list"`, `"command-empty"`, `"command-group"`, `"command-item"`, `"command-separator"`, `"command-shortcut"` | All parts |
| `[data-selected]` | `"true"` when item is selected/highlighted | CommandItem |
| `[data-disabled]` | `"true"` when disabled | CommandItem |
| `[cmdk-group-heading]` | present on group heading | CommandGroup heading |

### CSS Variables (Radix)

Not applicable — Command uses cmdk, not Radix primitives.

## Composition Patterns

### Basic with Groups

```tsx
<Command className="rounded-lg border shadow-md md:min-w-[450px]">
  <CommandInput placeholder="Type a command or search..." />
  <CommandList>
    <CommandEmpty>No results found.</CommandEmpty>
    <CommandGroup heading="Suggestions">
      <CommandItem>
        <Calendar />
        <span>Calendar</span>
      </CommandItem>
      <CommandItem>
        <Smile />
        <span>Search Emoji</span>
      </CommandItem>
      <CommandItem disabled>
        <Calculator />
        <span>Calculator</span>
      </CommandItem>
    </CommandGroup>
    <CommandSeparator />
    <CommandGroup heading="Settings">
      <CommandItem>
        <User />
        <span>Profile</span>
        <CommandShortcut>⌘P</CommandShortcut>
      </CommandItem>
      <CommandItem>
        <CreditCard />
        <span>Billing</span>
        <CommandShortcut>⌘B</CommandShortcut>
      </CommandItem>
      <CommandItem>
        <Settings />
        <span>Settings</span>
        <CommandShortcut>⌘S</CommandShortcut>
      </CommandItem>
    </CommandGroup>
  </CommandList>
</Command>
```

### Dialog (⌘K Palette)

```tsx
const [open, setOpen] = React.useState(false)

React.useEffect(() => {
  const down = (e: KeyboardEvent) => {
    if (e.key === "k" && (e.metaKey || e.ctrlKey)) {
      e.preventDefault()
      setOpen((open) => !open)
    }
  }
  document.addEventListener("keydown", down)
  return () => document.removeEventListener("keydown", down)
}, [])

<CommandDialog open={open} onOpenChange={setOpen}>
  <CommandInput placeholder="Type a command or search..." />
  <CommandList>
    <CommandEmpty>No results found.</CommandEmpty>
    <CommandGroup heading="Suggestions">
      <CommandItem>Calendar</CommandItem>
      <CommandItem>Search Emoji</CommandItem>
    </CommandGroup>
  </CommandList>
</CommandDialog>
```

### Simple List

```tsx
<Command className="max-w-sm rounded-lg border">
  <CommandInput placeholder="Type a command or search..." />
  <CommandList>
    <CommandEmpty>No results found.</CommandEmpty>
    <CommandGroup heading="Suggestions">
      <CommandItem>Calendar</CommandItem>
      <CommandItem>Search Emoji</CommandItem>
      <CommandItem>Calculator</CommandItem>
    </CommandGroup>
  </CommandList>
</Command>
```

## Accessibility

- **WAI-ARIA Pattern:** [Listbox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/listbox) (cmdk implements a custom accessible pattern)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Command | `role` managed by cmdk | Root container |
| CommandInput | `combobox` (implicit via cmdk) | Search input |
| CommandInput | `aria-expanded`, `aria-controls` | Managed by cmdk |
| CommandList | `listbox` (implicit via cmdk) | List of commands |
| CommandItem | `option` (implicit via cmdk) | Selectable command |
| CommandItem | `aria-selected` | Set by cmdk when highlighted |
| CommandItem | `aria-disabled` | Set when disabled |
| CommandDialog | Dialog semantics | sr-only title and description |
| CommandGroup | `group` with heading | Heading via `[cmdk-group-heading]` |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Down | Move selection to next item |
| Arrow Up | Move selection to previous item |
| Enter | Execute the selected command |
| Escape | Close dialog (if CommandDialog) |
| ⌘K / Ctrl+K | Open command palette (when wired up) |
| Tab | Move focus out of command menu |
| Home | Move to first item (when loop is false) |
| End | Move to last item (when loop is false) |

## HTML

### Standalone HTML Structure

```html
<div data-slot="command">
  <div data-slot="command-input-wrapper">
    <svg><!-- search icon --></svg>
    <input data-slot="command-input" placeholder="Type a command or search..." />
  </div>
  <div data-slot="command-list">
    <div data-slot="command-empty" style="display: none;">No results found.</div>
    <div data-slot="command-group">
      <div cmdk-group-heading>Suggestions</div>
      <div data-slot="command-item" data-selected="true">
        <svg><!-- icon --></svg>
        <span>Calendar</span>
      </div>
      <div data-slot="command-item">
        <svg><!-- icon --></svg>
        <span>Search Emoji</span>
      </div>
    </div>
    <div data-slot="command-separator"></div>
    <div data-slot="command-group">
      <div cmdk-group-heading>Settings</div>
      <div data-slot="command-item">
        <svg><!-- icon --></svg>
        <span>Profile</span>
        <span data-slot="command-shortcut">⌘P</span>
      </div>
    </div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Command */
.Command {
  display: flex;
  height: 100%;
  width: 100%;
  flex-direction: column;
  overflow: hidden;
  border-radius: 0.375rem;
  background-color: var(--popover);
  color: var(--popover-foreground);
}

/* CommandInput wrapper */
.CommandInputWrapper {
  display: flex;
  height: 2.25rem;
  align-items: center;
  gap: 0.5rem;
  border-bottom: 1px solid var(--border);
  padding: 0 0.75rem;
}

/* CommandInput wrapper icon */
.CommandInputWrapper svg {
  width: 1rem;
  height: 1rem;
  flex-shrink: 0;
  opacity: 0.5;
}

/* CommandInput */
.CommandInput {
  display: flex;
  height: 2.5rem;
  width: 100%;
  border-radius: 0.375rem;
  background-color: transparent;
  padding: 0.75rem 0;
  font-size: 0.875rem;
  outline: none;
}

.CommandInput::placeholder {
  color: var(--muted-foreground);
}

.CommandInput:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* CommandList */
.CommandList {
  max-height: 300px;
  scroll-padding: 0.25rem;
  overflow-x: hidden;
  overflow-y: auto;
}

/* CommandEmpty */
.CommandEmpty {
  padding: 1.5rem 0;
  text-align: center;
  font-size: 0.875rem;
}

/* CommandGroup */
.CommandGroup {
  overflow: hidden;
  padding: 0.25rem;
  color: var(--foreground);
}

/* CommandGroup heading */
.CommandGroup [cmdk-group-heading] {
  padding: 0.375rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--muted-foreground);
}

/* CommandItem */
.CommandItem {
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

.CommandItem[data-selected="true"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.CommandItem[data-disabled="true"] {
  pointer-events: none;
  opacity: 0.5;
}

/* CommandItem icons */
.CommandItem svg {
  pointer-events: none;
  flex-shrink: 0;
}
.CommandItem svg:not([class*='size-']) {
  width: 1rem;
  height: 1rem;
}
.CommandItem svg:not([class*='text-']) {
  color: var(--muted-foreground);
}

/* CommandSeparator */
.CommandSeparator {
  margin: 0 -0.25rem;
  height: 1px;
  background-color: var(--border);
}

/* CommandShortcut */
.CommandShortcut {
  margin-left: auto;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Command | `flex h-full w-full flex-col overflow-hidden rounded-md bg-popover text-popover-foreground` | Root layout |
| Input wrapper | `flex h-9 items-center gap-2 border-b px-3` | Input container with border |
| Input wrapper icon | `size-4 shrink-0 opacity-50` | Search icon |
| Input | `flex h-10 w-full rounded-md bg-transparent py-3 text-sm outline-hidden placeholder:text-muted-foreground disabled:cursor-not-allowed disabled:opacity-50` | Search input |
| List | `max-h-[300px] scroll-py-1 overflow-x-hidden overflow-y-auto` | Scrollable list |
| Empty | `py-6 text-center text-sm` | Empty state |
| Group | `overflow-hidden p-1 text-foreground` | Group container |
| Group heading | `px-2 py-1.5 text-xs font-medium text-muted-foreground` | Group label |
| Item | `relative flex cursor-default items-center gap-2 rounded-sm px-2 py-1.5 text-sm outline-hidden select-none` | Item layout |
| Item (selected) | `data-[selected=true]:bg-accent data-[selected=true]:text-accent-foreground` | Selected state |
| Item (disabled) | `data-[disabled=true]:pointer-events-none data-[disabled=true]:opacity-50` | Disabled state |
| Item icons | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4 [&_svg:not([class*='text-'])]:text-muted-foreground` | Icon sizing and color |
| Separator | `-mx-1 h-px bg-border` | Divider |
| Shortcut | `ml-auto text-xs tracking-widest text-muted-foreground` | Shortcut hint |
| Dialog Command | `**:data-[slot=command-input-wrapper]:h-12 [&_[cmdk-item]]:px-2 [&_[cmdk-item]]:py-3 [&_[cmdk-item]_svg]:h-5 [&_[cmdk-item]_svg]:w-5` | Dialog overrides (larger items) |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--popover` | Command background | `shadcn` |
| `--popover-foreground` | Command text color | `shadcn` |
| `--border` | Input wrapper border, separator | `shadcn` |
| `--foreground` | Group text color | `shadcn` |
| `--muted-foreground` | Input placeholder, group heading, shortcut, icon color | `shadcn` |
| `--accent` | Selected item background | `shadcn` |
| `--accent-foreground` | Selected item text | `shadcn` |

## Theme Support

### Component Structure

**Command** (composed): Full command palette with search field, grouped item lists, and optional "No results" state

**Command Search** — States: Default, Focus-empty, Focus-filled, Disabled

Command layout: flex column (border, rounded, shadow-md, overflow clip) → Search field + Item groups (separated by border-top)
Search field: flex row (popover bg, border-bottom, px 12px, py 8px) → Search icon (16×16px) + Placeholder text (text-sm, muted-foreground)
Item group: flex column (popover bg, p 4px, max-h 300px) → Group label (text-xs, muted-foreground) + Menu items
Menu item: flex row (px 8px, py 6px, rounded) → Item text (text-sm)
Active item: accent bg, accent-foreground text

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--popover)` | `--popover` | Search field and item list background |
| `var(--popover-foreground)` | `--popover-foreground` | Item text color |
| `var(--border)` | `--border` | Container border, group separator |
| `var(--muted-foreground)` | `--muted-foreground` | Search placeholder, group label text |
| `var(--accent)` | `--accent` | Active/hover item background |
| `var(--accent-foreground)` | `--accent-foreground` | Active/hover item text |
| `--radius` | `--radius` | Container border radius |
| `px-3` (12px) | `px-3` | Search field horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `text-sm` | Item and search text size |
| `shadow-md` | `shadow-md` | Container box shadow |

### Theme Behavior

- Command uses `--popover` / `--popover-foreground` — adapts to light/dark via theme
- Selected items use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Group headings, shortcuts, and icons use `--muted-foreground` — adapts to light/dark via theme
- Separator uses `--border` — adapts to light/dark via theme
- Input wrapper border uses `--border` (implicit from `border-b`) — adapts to light/dark via theme
- All spacing, typography, and layout tokens are mode-independent
