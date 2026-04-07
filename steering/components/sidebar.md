# Component: Sidebar

> **TL;DR:** A collapsible navigation sidebar with 24 exported parts, a context provider, and a custom hook. Built on Radix Slot with CVA for button variants. Supports `left`/`right` sides, `sidebar`/`floating`/`inset` variants, and `offcanvas`/`icon`/`none` collapsible modes. Mobile renders as a Sheet overlay. Persists open/closed state via cookie. Keyboard shortcut Ctrl/Cmd+B toggles sidebar. Icon-collapsed mode shrinks to 3rem with tooltip support. Commonly used for app-level navigation, settings panels, and dashboard layouts.

## Metadata

- **Component Name:** Sidebar
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-slot` (Slot), `class-variance-authority`, `lucide-react` (PanelLeftIcon), internal: `useIsMobile`, `Button`, `Input`, `Separator`, `Sheet`, `Skeleton`, `Tooltip`

## Behavior

A collapsible application sidebar for primary navigation. Manages open/closed state, mobile responsiveness, and icon-collapsed mode through a context provider.

- SidebarProvider manages state via React context; persists to cookie (`sidebar_state`)
- Keyboard shortcut: Ctrl/Cmd+B toggles sidebar open/closed
- SidebarProvider wraps children in TooltipProvider (delayDuration=0)
- Sets `--sidebar-width` and `--sidebar-width-icon` CSS custom properties on the wrapper
- Desktop sidebar: fixed position, z-10, 200ms transition on left/right/width
- Mobile sidebar: renders inside a Sheet (slide-over overlay) with SheetContent
- Floating and inset variants add padding, rounded corners, border, and shadow
- `collapsible="none"` renders a static sidebar with no collapse behavior
- `collapsible="icon"` shrinks sidebar to 3rem width; labels, sub-menus, and group actions are hidden
- SidebarMenuButton supports tooltip prop for icon-collapsed mode
- SidebarRail provides a thin hover-target for toggling via click or double-click
- SidebarMenuSkeleton generates random-width skeleton placeholders (50-90%)
- SidebarInset is a `main` element that responds to sidebar variant via peer data attributes

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `SidebarProvider` — Context provider. Manages open/closed state, mobile detection, cookie persistence, and keyboard shortcut.
- `Sidebar` — Root sidebar container. Renders as Sheet on mobile, fixed element on desktop.
- `SidebarHeader` — Top section of the sidebar. Flex column with gap and padding.
- `SidebarContent` — Scrollable main content area. Overflow hidden when icon-collapsed.
- `SidebarFooter` — Bottom section of the sidebar. Flex column with gap and padding.
- `SidebarGroup` — Logical grouping container within SidebarContent.
- `SidebarGroupLabel` — Label for a SidebarGroup. Hidden when icon-collapsed.
- `SidebarGroupAction` — Action button positioned top-right of a SidebarGroup. Hidden when icon-collapsed.
- `SidebarGroupContent` — Content wrapper inside a SidebarGroup.
- `SidebarMenu` — Unordered list container for menu items.
- `SidebarMenuItem` — List item wrapper for a single menu entry.
- `SidebarMenuButton` — Interactive button/link for a menu item. Supports CVA variants, active state, and tooltip.
- `SidebarMenuAction` — Secondary action button positioned at the right of a menu item.
- `SidebarMenuBadge` — Badge element positioned at the right of a menu item.
- `SidebarMenuSkeleton` — Loading placeholder with optional icon skeleton.
- `SidebarMenuSub` — Nested sub-menu list. Border-left styled. Hidden when icon-collapsed.
- `SidebarMenuSubItem` — List item wrapper inside a sub-menu.
- `SidebarMenuSubButton` — Interactive button/link inside a sub-menu item.
- `SidebarTrigger` — Toggle button with PanelLeftIcon and sr-only label.
- `SidebarRail` — Thin vertical rail for hover/click toggling.
- `SidebarInset` — Main content area that adjusts layout based on sidebar variant.
- `SidebarInput` — Styled Input wrapper for search/filter inside the sidebar.
- `SidebarSeparator` — Styled Separator with sidebar-specific spacing.
- `useSidebar` — Hook returning sidebar context: state, open, setOpen, isMobile, openMobile, setOpenMobile, toggleSidebar.

### Props

#### SidebarProvider

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| defaultOpen | `boolean` | `true` | Initial open state |
| open | `boolean` | — | Controlled open state |
| onOpenChange | `(open: boolean) => void` | — | Callback when open state changes |
| className | `string` | — | Additional CSS classes |
| style | `React.CSSProperties` | — | Inline styles (merged with CSS variable definitions) |

#### Sidebar

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| side | `"left" \| "right"` | `"left"` | Which side the sidebar appears on |
| variant | `"sidebar" \| "floating" \| "inset"` | `"sidebar"` | Visual variant |
| collapsible | `"offcanvas" \| "icon" \| "none"` | `"offcanvas"` | Collapse behavior |
| className | `string` | — | Additional CSS classes |

#### SidebarMenuButton

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "outline"` | `"default"` | Button visual variant |
| size | `"default" \| "sm" \| "lg"` | `"default"` | Button size (`h-8`, `h-7`, `h-12`) |
| asChild | `boolean` | `false` | Render as child element via Slot |
| isActive | `boolean` | `false` | Active/selected state |
| tooltip | `string \| TooltipContentProps` | — | Tooltip content for icon-collapsed mode |
| className | `string` | — | Additional CSS classes |

#### SidebarMenuSubButton

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"sm" \| "md"` | `"md"` | Sub-button size |
| asChild | `boolean` | `false` | Render as child element via Slot |
| isActive | `boolean` | `false` | Active/selected state |
| className | `string` | — | Additional CSS classes |

#### SidebarMenuAction

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element via Slot |
| showOnHover | `boolean` | `false` | Only visible on parent hover |
| className | `string` | — | Additional CSS classes |

#### SidebarContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

All other simple wrapper parts (SidebarHeader, SidebarFooter, SidebarGroup, SidebarGroupLabel, SidebarGroupAction, SidebarGroupContent, SidebarMenu, SidebarMenuItem, SidebarMenuBadge, SidebarMenuSkeleton, SidebarMenuSub, SidebarMenuSubItem, SidebarInput, SidebarSeparator, SidebarInset, SidebarRail, SidebarTrigger) accept `className` and standard HTML attributes.

### Variants

#### SidebarMenuButton Variants (CVA)

| Variant | Value | Description |
|---------|-------|-------------|
| `variant` | `"default"` | Standard menu button styling |
| `variant` | `"outline"` | Outlined menu button styling |
| `size` | `"default"` | Standard height (`h-8`) |
| `size` | `"sm"` | Compact height (`h-7`) |
| `size` | `"lg"` | Large height (`h-12`) |

#### Sidebar Variants

| Prop | Value | Description |
|------|-------|-------------|
| `side` | `"left"` / `"right"` | Sidebar placement |
| `variant` | `"sidebar"` | Standard full-height sidebar |
| `variant` | `"floating"` | Padded with rounded corners, border, and shadow |
| `variant` | `"inset"` | Padded with rounded corners, border, and shadow (paired with SidebarInset) |
| `collapsible` | `"offcanvas"` | Slides off-screen when collapsed |
| `collapsible` | `"icon"` | Shrinks to icon-only width (3rem) |
| `collapsible` | `"none"` | Static, no collapse |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"sidebar-wrapper"`, `"sidebar"`, `"sidebar-trigger"`, `"sidebar-rail"`, `"sidebar-inset"`, `"sidebar-input"`, `"sidebar-header"`, `"sidebar-footer"`, `"sidebar-separator"`, `"sidebar-content"`, `"sidebar-group"`, `"sidebar-group-label"`, `"sidebar-group-action"`, `"sidebar-group-content"`, `"sidebar-menu"`, `"sidebar-menu-item"`, `"sidebar-menu-button"`, `"sidebar-menu-action"`, `"sidebar-menu-badge"`, `"sidebar-menu-skeleton"`, `"sidebar-menu-sub"`, `"sidebar-menu-sub-item"`, `"sidebar-menu-sub-button"`, `"sidebar-gap"`, `"sidebar-container"`, `"sidebar-inner"` | All parts |
| `[data-state]` | `"expanded"`, `"collapsed"` | Sidebar |
| `[data-collapsible]` | `"offcanvas"`, `"icon"`, `"none"`, `""` | Sidebar |
| `[data-variant]` | `"sidebar"`, `"floating"`, `"inset"` | Sidebar |
| `[data-side]` | `"left"`, `"right"` | Sidebar |

### CSS Variables (Radix)

Not directly applicable — Sidebar uses Radix Slot for composition but does not expose Radix CSS variables. Custom CSS properties are set on the wrapper:

| Variable | Default | Description |
|----------|---------|-------------|
| `--sidebar-width` | `16rem` | Desktop sidebar width |
| `--sidebar-width-icon` | `3rem` | Icon-collapsed sidebar width |
| `--sidebar-width-mobile` | `18rem` | Mobile sheet sidebar width (constant) |

## Composition Patterns

### Basic Sidebar Layout

```tsx
<SidebarProvider>
  <Sidebar>
    <SidebarHeader>
      <SidebarMenu>
        <SidebarMenuItem>
          <SidebarMenuButton size="lg">
            <AppIcon />
            <span>My App</span>
          </SidebarMenuButton>
        </SidebarMenuItem>
      </SidebarMenu>
    </SidebarHeader>
    <SidebarContent>
      <SidebarGroup>
        <SidebarGroupLabel>Navigation</SidebarGroupLabel>
        <SidebarGroupContent>
          <SidebarMenu>
            <SidebarMenuItem>
              <SidebarMenuButton asChild isActive>
                <a href="/dashboard">
                  <DashboardIcon />
                  <span>Dashboard</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
            <SidebarMenuItem>
              <SidebarMenuButton asChild>
                <a href="/settings">
                  <SettingsIcon />
                  <span>Settings</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarGroupContent>
      </SidebarGroup>
    </SidebarContent>
    <SidebarFooter>
      <SidebarMenu>
        <SidebarMenuItem>
          <SidebarMenuButton>
            <UserIcon />
            <span>Profile</span>
          </SidebarMenuButton>
        </SidebarMenuItem>
      </SidebarMenu>
    </SidebarFooter>
  </Sidebar>
  <SidebarInset>
    <header>
      <SidebarTrigger />
      <h1>Page Title</h1>
    </header>
    <main>Page content</main>
  </SidebarInset>
</SidebarProvider>
```

### With Sub-Menu

```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <SidebarMenuButton>
      <FolderIcon />
      <span>Projects</span>
    </SidebarMenuButton>
    <SidebarMenuSub>
      <SidebarMenuSubItem>
        <SidebarMenuSubButton asChild isActive>
          <a href="/projects/alpha">Alpha</a>
        </SidebarMenuSubButton>
      </SidebarMenuSubItem>
      <SidebarMenuSubItem>
        <SidebarMenuSubButton asChild>
          <a href="/projects/beta">Beta</a>
        </SidebarMenuSubButton>
      </SidebarMenuSubItem>
    </SidebarMenuSub>
  </SidebarMenuItem>
</SidebarMenu>
```

### With Menu Action and Badge

```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <SidebarMenuButton>
      <InboxIcon />
      <span>Inbox</span>
    </SidebarMenuButton>
    <SidebarMenuBadge>24</SidebarMenuBadge>
    <SidebarMenuAction showOnHover>
      <PlusIcon />
      <span className="sr-only">Add item</span>
    </SidebarMenuAction>
  </SidebarMenuItem>
</SidebarMenu>
```

### Icon-Collapsed with Tooltips

```tsx
<SidebarProvider>
  <Sidebar collapsible="icon">
    <SidebarContent>
      <SidebarGroup>
        <SidebarGroupContent>
          <SidebarMenu>
            <SidebarMenuItem>
              <SidebarMenuButton tooltip="Dashboard">
                <DashboardIcon />
                <span>Dashboard</span>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarGroupContent>
      </SidebarGroup>
    </SidebarContent>
    <SidebarRail />
  </Sidebar>
  <SidebarInset>
    <main>Content</main>
  </SidebarInset>
</SidebarProvider>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Sidebar is a composite navigation container; individual interactive elements (buttons, links) carry their own semantics

### ARIA Roles

- SidebarTrigger renders a Button with sr-only text "Toggle Sidebar"
- SidebarMenuButton renders as `button` (or child element via `asChild`) — standard button semantics
- SidebarMenuSubButton renders as `a` (or child element via `asChild`) — standard link semantics
- SidebarMenu renders as `ul`, SidebarMenuItem as `li` — standard list semantics
- Sidebar itself renders as a `div` with `data-*` attributes for state; consider adding `role="navigation"` and `aria-label` at the application level

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Ctrl+B` / `Cmd+B` | Toggle sidebar open/closed (global shortcut via SidebarProvider) |
| `Tab` | Navigate between interactive elements within the sidebar |
| `Enter` / `Space` | Activate focused SidebarMenuButton, SidebarTrigger, or SidebarRail |

## HTML

### Standalone HTML Structure

```html
<div data-slot="sidebar-wrapper" style="--sidebar-width: 16rem; --sidebar-width-icon: 3rem;">
  <div data-slot="sidebar" data-state="expanded" data-collapsible="offcanvas" data-variant="sidebar" data-side="left">
    <div data-slot="sidebar-container">
      <div data-slot="sidebar-inner">
        <div data-slot="sidebar-header">
          <ul data-slot="sidebar-menu">
            <li data-slot="sidebar-menu-item">
              <button data-slot="sidebar-menu-button" data-size="lg">
                <!-- icon --> App Name
              </button>
            </li>
          </ul>
        </div>
        <div data-slot="sidebar-content">
          <div data-slot="sidebar-group">
            <div data-slot="sidebar-group-label">Navigation</div>
            <div data-slot="sidebar-group-content">
              <ul data-slot="sidebar-menu">
                <li data-slot="sidebar-menu-item">
                  <button data-slot="sidebar-menu-button" data-active="true">
                    <!-- icon --> Dashboard
                  </button>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <div data-slot="sidebar-footer">
          <!-- footer content -->
        </div>
      </div>
    </div>
  </div>
  <main data-slot="sidebar-inset">
    <button data-slot="sidebar-trigger">
      <!-- PanelLeftIcon -->
      <span class="sr-only">Toggle Sidebar</span>
    </button>
    <!-- page content -->
  </main>
</div>
```

## CSS

### Raw CSS

```css
/* SidebarProvider wrapper */
.SidebarWrapper {
  display: flex;
  min-height: 100svh;
  width: 100%;
}

/* Sidebar — desktop */
.Sidebar {
  position: fixed;
  inset-y: 0;
  z-index: 10;
  display: flex;
  height: 100svh;
  width: var(--sidebar-width);
  flex-direction: column;
  background-color: var(--sidebar);
  color: var(--sidebar-foreground);
  transition: left 200ms, right 200ms, width 200ms;
}

.Sidebar[data-side="left"] {
  left: 0;
  border-right: 1px solid var(--sidebar-border);
}

.Sidebar[data-side="right"] {
  right: 0;
  border-left: 1px solid var(--sidebar-border);
}

/* Sidebar — collapsed offcanvas */
.Sidebar[data-state="collapsed"][data-collapsible="offcanvas"][data-side="left"] {
  left: calc(var(--sidebar-width) * -1);
}

.Sidebar[data-state="collapsed"][data-collapsible="offcanvas"][data-side="right"] {
  right: calc(var(--sidebar-width) * -1);
}

/* Sidebar — collapsed icon */
.Sidebar[data-state="collapsed"][data-collapsible="icon"] {
  width: var(--sidebar-width-icon);
}

/* Sidebar — floating/inset variants */
.Sidebar[data-variant="floating"],
.Sidebar[data-variant="inset"] {
  padding: 0.5rem;
}

.Sidebar[data-variant="floating"] .SidebarInner,
.Sidebar[data-variant="inset"] .SidebarInner {
  border-radius: 0.5rem;
  border: 1px solid var(--sidebar-border);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* SidebarHeader */
.SidebarHeader {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 0.5rem;
}

/* SidebarFooter */
.SidebarFooter {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 0.5rem;
}

/* SidebarContent */
.SidebarContent {
  display: flex;
  min-height: 0;
  flex: 1;
  flex-direction: column;
  gap: 0.5rem;
  overflow: auto;
}

.Sidebar[data-state="collapsed"][data-collapsible="icon"] .SidebarContent {
  overflow: hidden;
}

/* SidebarSeparator */
.SidebarSeparator {
  margin-left: 0.5rem;
  margin-right: 0.5rem;
  background-color: var(--sidebar-border);
}

/* SidebarGroup */
.SidebarGroup {
  display: flex;
  flex-direction: column;
  padding: 0.5rem;
}

/* SidebarGroupLabel */
.SidebarGroupLabel {
  display: flex;
  height: 2rem;
  align-items: center;
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--sidebar-foreground);
  opacity: 0.7;
}

.Sidebar[data-state="collapsed"][data-collapsible="icon"] .SidebarGroupLabel {
  display: none;
}

/* SidebarGroupAction */
.SidebarGroupAction {
  position: absolute;
  top: 0.875rem;
  right: 0.75rem;
}

.Sidebar[data-state="collapsed"][data-collapsible="icon"] .SidebarGroupAction {
  display: none;
}

/* SidebarMenu */
.SidebarMenu {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

/* SidebarMenuButton */
.SidebarMenuButton {
  display: flex;
  width: 100%;
  align-items: center;
  gap: 0.5rem;
  border-radius: 0.375rem;
  padding: 0.25rem 0.5rem;
  height: 2rem;
  font-size: 0.875rem;
  outline: none;
  color: var(--sidebar-foreground);
  transition: background-color 150ms, color 150ms;
}

.SidebarMenuButton:hover {
  background-color: var(--sidebar-accent);
  color: var(--sidebar-accent-foreground);
}

.SidebarMenuButton[data-active="true"] {
  background-color: var(--sidebar-accent);
  color: var(--sidebar-accent-foreground);
  font-weight: 500;
}

.SidebarMenuButton:focus-visible {
  outline: 2px solid var(--sidebar-ring);
}

/* SidebarMenuButton — icon-collapsed */
.Sidebar[data-state="collapsed"][data-collapsible="icon"] .SidebarMenuButton {
  width: 2rem;
  height: 2rem;
  padding: 0.5rem;
}

/* SidebarMenuButton sizes */
.SidebarMenuButton[data-size="sm"] {
  height: 1.75rem;
}

.SidebarMenuButton[data-size="lg"] {
  height: 3rem;
}

/* SidebarMenuAction */
.SidebarMenuAction {
  position: absolute;
  top: 0.25rem;
  right: 0.25rem;
}

/* SidebarMenuBadge */
.SidebarMenuBadge {
  position: absolute;
  right: 0.25rem;
  font-variant-numeric: tabular-nums;
}

/* SidebarMenuSub */
.SidebarMenuSub {
  border-left: 1px solid var(--sidebar-border);
  padding-left: 0.5rem;
  margin-left: 0.75rem;
}

.Sidebar[data-state="collapsed"][data-collapsible="icon"] .SidebarMenuSub {
  display: none;
}

/* SidebarTrigger */
.SidebarTrigger {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

/* SidebarRail */
.SidebarRail {
  position: absolute;
  inset-y: 0;
  z-index: 20;
  width: 4px;
}

.SidebarRail:hover::after {
  content: "";
  position: absolute;
  inset-y: 0;
  width: 2px;
  background-color: var(--sidebar-border);
}

/* SidebarInset */
.SidebarInset {
  display: flex;
  flex: 1;
  flex-direction: column;
}

/* SidebarInput */
.SidebarInput {
  height: 2rem;
  background-color: var(--background);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| SidebarProvider wrapper | `group/sidebar-wrapper flex min-h-svh w-full has-data-[variant=inset]:bg-sidebar` | Full-height flex container |
| Sidebar (desktop) | `fixed inset-y-0 z-10 hidden md:flex h-svh flex-col bg-sidebar text-sidebar-foreground transition-[left,right,width] duration-200 ease-linear` | Fixed sidebar shell |
| Sidebar (left) | `left-0 border-r` | Left placement with right border |
| Sidebar (right) | `right-0 border-l` | Right placement with left border |
| Sidebar (floating/inset inner) | `p-2 rounded-lg border border-sidebar-border shadow-sm` | Floating variant styling |
| SidebarHeader | `flex flex-col gap-2 p-2` | Header layout |
| SidebarFooter | `flex flex-col gap-2 p-2` | Footer layout |
| SidebarContent | `flex min-h-0 flex-1 flex-col gap-2 overflow-auto group-data-[collapsible=icon]:overflow-hidden` | Scrollable content |
| SidebarSeparator | `mx-2 bg-sidebar-border` | Horizontal separator |
| SidebarGroup | `flex flex-col p-2` | Group container |
| SidebarGroupLabel | `flex h-8 items-center text-xs font-medium text-sidebar-foreground/70 group-data-[collapsible=icon]:hidden` | Group label |
| SidebarGroupAction | `absolute top-3.5 right-3 group-data-[collapsible=icon]:hidden` | Group action |
| SidebarMenu | `flex flex-col gap-1` | Menu list |
| SidebarMenuItem | `group/menu-item relative` | Menu item wrapper |
| SidebarMenuButton (default) | `flex w-full items-center gap-2 rounded-md px-2 h-8 text-sm outline-hidden ring-sidebar-ring hover:bg-sidebar-accent hover:text-sidebar-accent-foreground active:bg-sidebar-accent active:font-medium focus-visible:ring-2` | Menu button |
| SidebarMenuButton (icon-collapsed) | `group-data-[collapsible=icon]:size-8 group-data-[collapsible=icon]:p-2` | Forced icon size |
| SidebarMenuAction | `absolute top-1 right-1` | Action positioning |
| SidebarMenuBadge | `absolute right-1 tabular-nums` | Badge positioning |
| SidebarMenuSub | `border-l border-sidebar-border pl-2 ml-3 group-data-[collapsible=icon]:hidden` | Sub-menu with left border |
| SidebarTrigger | Button `variant="ghost" size="icon"` | Toggle button |
| SidebarRail | `absolute inset-y-0 z-20 w-1 hover:after:bg-sidebar-border` | Rail toggle target |
| SidebarInset | `flex flex-1 flex-col` | Main content area |
| SidebarInput | `h-8 bg-background` | Search input |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--sidebar` | Sidebar background color | `shadcn` |
| `--sidebar-foreground` | Sidebar text color | `shadcn` |
| `--sidebar-border` | Sidebar border and separator color | `shadcn` |
| `--sidebar-accent` | Menu button hover/active background | `shadcn` |
| `--sidebar-accent-foreground` | Menu button hover/active text color | `shadcn` |
| `--sidebar-ring` | Focus ring color | `shadcn` |
| `--background` | SidebarInput background | `shadcn` |

## Theme Support

### Component Structure

Component is organized into numerous sub-components and assembled examples:

Sub-components:
- .Sidebar Menu Button: State (Enabled/Hover/Active/Focus) × Type (Simple/Large icon) × Collapsed (True/False)
- .Sidebar Media Asset: Type (Icon/Avatar)
- .Sidebar Menu Item: Sub menu (True/False)
- .Sidebar Menu Sub Item: State (Enabled/Hover/Active/Focus)
- .Sidebar Menu Sub: Type (Separator/Default/Inset)
- .Sidebar Group Action: State (Enabled/Hover/Focus)
- .Sidebar Group Label: State (Enabled/Hover) × Size (Small/X-Small)
- .Sidebar Group: Collapsed (True/False)
- .Sidebar Header Button: State (Enabled/Hover/Focus)
- .Sidebar Popover Trigger: State (Enabled/Hover)
- .Sidebar Popover Menu Item: State (Enabled/Hover/Focus) × Type (Default/Button)
- .Sidebar Popover Menu Label (no variants)
- .Sidebar Popover Menu (no variants)
- .Sidebar Popover (no variants)
- .Sidebar Header: State (Enabled/Hover)
- .Sidebar Footer: State (Enabled/Hover)
- .Sidebar Inbox Item: State (Enabled/Hover)

Assembled examples: Sidebar 1 through Sidebar 14 (various compositions), Sidebar 7 includes Collapsed=True/False variants.

- Menu items use gap 8px between icon and label

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--sidebar` | `--sidebar` | Sidebar background |
| `--sidebar-foreground` | `--sidebar-foreground` | Sidebar text color |
| `--sidebar-primary` | `--sidebar-primary` | Media asset icon background |
| `--input` | `--input` | Input border and background (at 30% opacity) |
| `--muted-foreground` | `--muted-foreground` | Input placeholder text |
| `--radius` (derived) | `--radius` | Item and input border radius |
| `p-2` (8px) | `padding` / `gap` | Section padding, item padding |
| `px-3` (12px) | `padding-inline` | Input horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-semibold` (600) | `font-weight` | Header title font weight |
| `text-xs` (12px) | `font-size` | Group label text size |
| `text-sm` (14px) | `font-size` | Menu item text size |
| `shadow-xs` | `box-shadow` | Input shadow |

### Theme Behavior

- Sidebar uses `--sidebar` / `--sidebar-foreground` — adapts to light/dark via theme
- Borders and separators use `--sidebar-border` — adapts to light/dark via theme
- Interactive states use `--sidebar-accent` / `--sidebar-accent-foreground` — adapts to light/dark via theme
- Focus ring uses `--sidebar-ring` — adapts to light/dark via theme
- SidebarInput uses `--background` — adapts to light/dark via theme
- Width constants (`16rem`, `18rem`, `3rem`) and transition timing (`200ms`) are mode-independent
