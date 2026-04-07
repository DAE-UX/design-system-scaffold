# Component: Navigation Menu

> **TL;DR:** A collection of navigation links with animated viewport and indicator. 8 parts + exported CVA style: NavigationMenu, NavigationMenuList, NavigationMenuItem, NavigationMenuTrigger, NavigationMenuContent, NavigationMenuLink, NavigationMenuIndicator, NavigationMenuViewport, plus `navigationMenuTriggerStyle`. Built on Radix UI primitive. Root has `viewport` prop (boolean, default true). Viewport renders animated container below menu. Content uses motion-based animations. Supports `viewport=false` mode with inline content. Link has active state. Indicator shows arrow pointing to active item.

## Metadata

- **Component Name:** NavigationMenu
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A collection of navigation links with animated viewport and indicator, built on Radix UI's NavigationMenu primitive.

- Root component wraps a list of navigation items with optional viewport
- `viewport` prop (boolean, default `true`) controls whether content renders in a shared viewport below the menu or inline
- NavigationMenuList renders items in a horizontal flex row with `gap-1`
- NavigationMenuTrigger uses `navigationMenuTriggerStyle` CVA and includes a ChevronDownIcon that rotates 180° on open
- NavigationMenuContent uses motion-based animations (`from-start`/`from-end`/`to-start`/`to-end` for slide transitions)
- When `viewport=false`, content renders inline with border, `bg-popover`, and shadow
- NavigationMenuViewport renders an animated container below the menu, sized via Radix CSS variables (`--radix-navigation-menu-viewport-height`, `--radix-navigation-menu-viewport-width`)
- NavigationMenuLink supports active state via `data-[active=true]` with accent background
- NavigationMenuIndicator shows an arrow (rotated 45° div) pointing to the active item, positioned at the top of the viewport
- `navigationMenuTriggerStyle` is exported as a CVA style for reuse on custom trigger elements

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `NavigationMenu` — Root container. Flex row with relative positioning and group class.
- `NavigationMenuList` — Horizontal list of navigation items.
- `NavigationMenuItem` — Individual navigation item wrapper.
- `NavigationMenuTrigger` — Trigger button for dropdown content with chevron icon.
- `NavigationMenuContent` — Dropdown content panel with motion animations.
- `NavigationMenuLink` — Navigation link with active state styling.
- `NavigationMenuIndicator` — Visual indicator arrow pointing to active item.
- `NavigationMenuViewport` — Shared animated viewport container for content panels.

### Props

#### NavigationMenu

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| viewport | `boolean` | `true` | Whether to render content in a shared viewport below the menu |
| className | `string` | — | Additional CSS classes |

#### NavigationMenuList

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuLink

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuIndicator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### NavigationMenuViewport

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Exported Styles

#### navigationMenuTriggerStyle (CVA)

A reusable CVA style for trigger-like elements within the navigation menu.

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `viewport={true}` | Content renders in shared viewport below menu | Standard navigation menus with animated transitions |
| `viewport={false}` | Content renders inline with border/shadow | Simpler menus without shared viewport |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"navigation-menu"`, `"navigation-menu-list"`, `"navigation-menu-item"`, `"navigation-menu-trigger"`, `"navigation-menu-content"`, `"navigation-menu-link"`, `"navigation-menu-indicator"`, `"navigation-menu-viewport"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | NavigationMenuTrigger, NavigationMenuContent |
| `[data-active]` | `"true"` | NavigationMenuLink |

### CSS Variables (Radix)

| Variable | Purpose |
|----------|---------|
| `--radix-navigation-menu-viewport-height` | Viewport container height |
| `--radix-navigation-menu-viewport-width` | Viewport container width |

## Composition Patterns

### Basic Navigation Menu

```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Getting Started</NavigationMenuTrigger>
      <NavigationMenuContent>
        <NavigationMenuLink href="/docs">
          Documentation
        </NavigationMenuLink>
      </NavigationMenuContent>
    </NavigationMenuItem>
    <NavigationMenuItem>
      <NavigationMenuLink href="/about">About</NavigationMenuLink>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Without Viewport

```tsx
<NavigationMenu viewport={false}>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Menu</NavigationMenuTrigger>
      <NavigationMenuContent>
        <NavigationMenuLink href="/settings">Settings</NavigationMenuLink>
        <NavigationMenuLink href="/profile">Profile</NavigationMenuLink>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### With Indicator

```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Products</NavigationMenuTrigger>
      <NavigationMenuContent>
        <NavigationMenuLink href="/products/a">Product A</NavigationMenuLink>
        <NavigationMenuLink href="/products/b">Product B</NavigationMenuLink>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
  <NavigationMenuIndicator />
</NavigationMenu>
```

### Using Trigger Style on Custom Link

```tsx
<NavigationMenuItem>
  <a href="/docs" className={navigationMenuTriggerStyle()}>
    Documentation
  </a>
</NavigationMenuItem>
```

## Accessibility

- **WAI-ARIA Pattern:** Navigation

### ARIA Roles

Radix UI NavigationMenu provides built-in `navigation` role on the root element. Menu items use appropriate roles for disclosure patterns.

### Keyboard Behavior

- `Tab` — Moves focus between top-level navigation items
- `Enter` / `Space` — Opens/closes trigger content
- `Arrow Left` / `Arrow Right` — Moves focus between sibling items
- `Escape` — Closes open content panel
- `Home` / `End` — Moves focus to first/last item

## HTML

### Standalone HTML Structure

```html
<nav data-slot="navigation-menu">
  <ul data-slot="navigation-menu-list">
    <li data-slot="navigation-menu-item">
      <button data-slot="navigation-menu-trigger" data-state="closed">
        Getting Started
        <svg><!-- ChevronDownIcon --></svg>
      </button>
      <div data-slot="navigation-menu-content" data-state="closed">
        <a data-slot="navigation-menu-link" href="/docs">Documentation</a>
      </div>
    </li>
  </ul>
  <div data-slot="navigation-menu-indicator">
    <div><!-- arrow --></div>
  </div>
  <div data-slot="navigation-menu-viewport"></div>
</nav>
```

## CSS

### Raw CSS

```css
/* NavigationMenu */
.NavigationMenu {
  position: relative;
  display: flex;
  max-width: max-content;
  flex: 1;
  align-items: center;
  justify-content: center;
}

/* NavigationMenuList */
.NavigationMenuList {
  display: flex;
  flex: 1;
  list-style: none;
  align-items: center;
  justify-content: center;
  gap: 0.25rem;
}

/* NavigationMenuItem */
.NavigationMenuItem {
  position: relative;
}

/* navigationMenuTriggerStyle */
.NavigationMenuTriggerStyle {
  display: inline-flex;
  height: 2.25rem;
  width: max-content;
  align-items: center;
  justify-content: center;
  border-radius: 0.375rem;
  background-color: var(--background);
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
  font-weight: 500;
  transition: color, box-shadow;
  outline: none;
}

.NavigationMenuTriggerStyle:hover {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.NavigationMenuTriggerStyle:focus {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.NavigationMenuTriggerStyle:focus-visible {
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

.NavigationMenuTriggerStyle:disabled {
  pointer-events: none;
  opacity: 0.5;
}

.NavigationMenuTriggerStyle[data-state="open"] {
  background-color: color-mix(in srgb, var(--accent) 50%, transparent);
  color: var(--accent-foreground);
}

/* NavigationMenuContent (viewport mode) */
.NavigationMenuContent {
  /* Motion-based slide animations: from-start, from-end, to-start, to-end */
}

/* NavigationMenuContent (inline mode, viewport=false) */
.NavigationMenuContentInline {
  border: 1px solid var(--border);
  background-color: var(--popover);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* NavigationMenuViewport */
.NavigationMenuViewport {
  transform-origin: top center;
  margin-top: 0.375rem;
  height: var(--radix-navigation-menu-viewport-height);
  width: var(--radix-navigation-menu-viewport-width);
  border-radius: 0.375rem;
  border: 1px solid var(--border);
  background-color: var(--popover);
  color: var(--popover-foreground);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* NavigationMenuLink */
.NavigationMenuLink {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  border-radius: 0.125rem;
  padding: 0.5rem;
  font-size: 0.875rem;
}

.NavigationMenuLink:hover {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

.NavigationMenuLink:focus {
  background-color: var(--accent);
}

.NavigationMenuLink[data-active="true"] {
  background-color: color-mix(in srgb, var(--accent) 50%, transparent);
}

/* NavigationMenuIndicator */
.NavigationMenuIndicator {
  top: 100%;
  z-index: 1;
  height: 0.375rem;
}

.NavigationMenuIndicatorArrow {
  background-color: var(--border);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  transform: rotate(45deg);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| NavigationMenu | `group/navigation-menu relative flex max-w-max flex-1 items-center justify-center` | Root flex container |
| NavigationMenuList | `group flex flex-1 list-none items-center justify-center gap-1` | Horizontal item list |
| NavigationMenuItem | `relative` | Item wrapper |
| navigationMenuTriggerStyle | `group inline-flex h-9 w-max items-center justify-center rounded-md bg-background px-4 py-2 text-sm font-medium transition-[color,box-shadow] outline-none hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:opacity-50 data-[state=open]:bg-accent/50 data-[state=open]:text-accent-foreground` | Reusable trigger style |
| NavigationMenuViewport | `origin-top-center mt-1.5 h-[var(--radix-navigation-menu-viewport-height)] w-[var(--radix-navigation-menu-viewport-width)] rounded-md border bg-popover text-popover-foreground shadow` | Animated viewport container |
| NavigationMenuLink | `flex flex-col gap-1 rounded-sm p-2 text-sm hover:bg-accent hover:text-accent-foreground focus:bg-accent data-[active=true]:bg-accent/50` | Link with active state |
| NavigationMenuIndicator | `top-full z-[1] h-1.5` | Indicator positioning |
| NavigationMenuIndicator arrow | `bg-border shadow-md rotate-45` | Arrow indicator |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Trigger default background | `shadcn` |
| `--accent` | Hover/focus/active background | `shadcn` |
| `--accent-foreground` | Hover/focus/active text color | `shadcn` |
| `--ring` | Focus ring color | `shadcn` |
| `--popover` | Viewport and inline content background | `shadcn` |
| `--popover-foreground` | Viewport text color | `shadcn` |
| `--border` | Viewport border and indicator arrow color | `shadcn` |
| `--muted-foreground` | Chevron icon color (in trigger) | `shadcn` |

## Theme Support

### Component Structure

Component is organized into multiple sub-components:

Navigation Menu (composite):
- Single variant showing a horizontal row of 5 navigation items

.Navigation Menu / Item:

| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Hover, Focus, Disabled |

Navigation Menu / Navigation Menu Popover:

| Variant Property | Values |
|-----------------|--------|
| Layout | Two column, One Column |

.Navigation Menu / Menu Link:

| Variant Property | Values |
|-----------------|--------|
| State | Default, Hover, Disabled |
| Type | Description, Simple |

.Navigation Menu Base / Poster — single variant (featured content card, 188×250)

- Container: flex with gap 4px
- Each item: h 36px, px 16px, py 8px, rounded
- Text: foreground, text-sm, font-medium


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Item text color |
| `--radius` (derived) | `--radius` | Item border radius |
| `px-4` (16px) | `padding-inline` | Item horizontal padding |
| `py-2` (8px) | `padding-block` | Item vertical padding |
| `gap-1` (4px) | `gap` | Gap between items |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Item font weight |
| `text-sm` (14px) | `font-size` | Item text size |

### Theme Behavior

- Trigger uses `--background` / `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Viewport uses `--popover` / `--popover-foreground` / `--border` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Link active state uses `--accent` at 50% opacity — adapts to light/dark via theme
- Indicator arrow uses `--border` — adapts to light/dark via theme
- All spacing, typography, and animation tokens are mode-independent
