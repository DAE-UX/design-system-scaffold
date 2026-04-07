# Component: Breadcrumb

> **TL;DR:** A navigation component showing the user's location in a site hierarchy. 7 parts: Breadcrumb, BreadcrumbList, BreadcrumbItem, BreadcrumbLink, BreadcrumbPage, BreadcrumbSeparator, BreadcrumbEllipsis. Uses semantic `nav` > `ol` > `li` structure with `aria-label="breadcrumb"`. Supports custom separators, collapsed state with ellipsis, dropdown menus for hidden items, and `asChild` for custom link components. No Radix primitive — pure shadCN with Radix Slot.

## Metadata

- **Component Name:** Breadcrumb
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui (Slot only, for `asChild`), lucide-react (ChevronRight, MoreHorizontal icons)

## Behavior

Displays a navigation trail showing the user's current location within a site hierarchy. Each item represents a level in the hierarchy.

- Renders as a semantic `nav` element with `aria-label="breadcrumb"`
- Uses ordered list (`ol` > `li`) for proper document structure
- Links are navigable; the current page is non-clickable (`aria-current="page"`, `aria-disabled="true"`)
- Default separator is a ChevronRight icon; custom separators supported via children
- Collapsed state via `BreadcrumbEllipsis` (MoreHorizontal icon with "More" screen reader text)
- Ellipsis can be composed with DropdownMenu for expandable hidden items
- `BreadcrumbLink` supports `asChild` for custom link components (e.g., Next.js `Link`, React Router `Link`)
- Supports RTL layout via the Direction component
- Responsive gap: `gap-1.5` on mobile, `gap-2.5` on sm+ screens

### Anti-Patterns

#### Do
- Start the first breadcrumb with the service/app home
- Use breadcrumbs to reflect information architecture, not browsing history

#### Don't
- Don't use breadcrumbs as a history trail
- Don't omit the home/root link
- Don't make the current page a clickable link in the breadcrumb

Sources: shadCN, Radix UI

## API

### Parts

- `Breadcrumb` — Root `nav` element with `aria-label="breadcrumb"`.
- `BreadcrumbList` — Ordered list (`ol`) containing all breadcrumb items.
- `BreadcrumbItem` — List item (`li`) wrapping a single breadcrumb entry.
- `BreadcrumbLink` — Clickable link for navigable breadcrumb items. Supports `asChild`.
- `BreadcrumbPage` — Non-clickable current page indicator with `aria-current="page"`.
- `BreadcrumbSeparator` — Visual separator between items. Defaults to ChevronRight icon.
- `BreadcrumbEllipsis` — Collapsed state indicator (MoreHorizontal icon).

### Props

#### Breadcrumb

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### BreadcrumbLink

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element (e.g., Next.js `Link`) instead of `<a>` |
| href | `string` | — | Link destination |

#### BreadcrumbSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| children | `ReactNode` | `<ChevronRight />` | Custom separator element |

### Variants

No variants — Breadcrumb is a single-style navigation component.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"breadcrumb"`, `"breadcrumb-list"`, `"breadcrumb-item"`, `"breadcrumb-link"`, `"breadcrumb-page"`, `"breadcrumb-separator"`, `"breadcrumb-ellipsis"` | All parts |

### CSS Variables (Radix)

Not applicable — Breadcrumb does not use Radix primitives.

## Composition Patterns

### Basic Breadcrumb

```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/components">Components</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Breadcrumb</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Custom Separator

```tsx
<BreadcrumbSeparator>
  <SlashIcon />
</BreadcrumbSeparator>
```

### With Ellipsis and Dropdown

```tsx
<BreadcrumbItem>
  <DropdownMenu>
    <DropdownMenuTrigger className="flex items-center gap-1">
      <BreadcrumbEllipsis className="size-4" />
      <span className="sr-only">Toggle menu</span>
    </DropdownMenuTrigger>
    <DropdownMenuContent align="start">
      <DropdownMenuItem>Documentation</DropdownMenuItem>
      <DropdownMenuItem>Themes</DropdownMenuItem>
    </DropdownMenuContent>
  </DropdownMenu>
</BreadcrumbItem>
```

### With Custom Link Component (Next.js)

```tsx
<BreadcrumbLink asChild>
  <Link href="/components">Components</Link>
</BreadcrumbLink>
```

## Accessibility

- **WAI-ARIA Pattern:** [Breadcrumb Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Breadcrumb | `nav` with `aria-label="breadcrumb"` | Landmark navigation |
| BreadcrumbList | `ol` (implicit `list` role) | Ordered list |
| BreadcrumbPage | `role="link"`, `aria-disabled="true"`, `aria-current="page"` | Current page (non-clickable) |
| BreadcrumbSeparator | `role="presentation"`, `aria-hidden="true"` | Decorative |
| BreadcrumbEllipsis | `role="presentation"`, `aria-hidden="true"` | Decorative with sr-only text |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Tab | Moves focus to the next breadcrumb link |
| Shift + Tab | Moves focus to the previous breadcrumb link |
| Enter | Activates the focused breadcrumb link |

## HTML

### Standalone HTML Structure

```html
<nav aria-label="breadcrumb" data-slot="breadcrumb">
  <ol data-slot="breadcrumb-list">
    <li data-slot="breadcrumb-item">
      <a href="/" data-slot="breadcrumb-link">Home</a>
    </li>
    <li role="presentation" aria-hidden="true" data-slot="breadcrumb-separator">
      <svg><!-- ChevronRight icon --></svg>
    </li>
    <li data-slot="breadcrumb-item">
      <span role="link" aria-disabled="true" aria-current="page" data-slot="breadcrumb-page">
        Breadcrumb
      </span>
    </li>
  </ol>
</nav>
```

## CSS

### Raw CSS

```css
.BreadcrumbList {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.375rem;
  font-size: 0.875rem;
  color: var(--muted-foreground);
}
@media (min-width: 640px) { .BreadcrumbList { gap: 0.625rem; } }
.BreadcrumbItem { display: inline-flex; align-items: center; gap: 0.375rem; }
.BreadcrumbLink { transition: color 150ms; }
.BreadcrumbLink:hover { color: var(--foreground); }
.BreadcrumbPage { font-weight: 400; color: var(--foreground); }
.BreadcrumbSeparator > svg { width: 0.875rem; height: 0.875rem; }
.BreadcrumbEllipsis { display: flex; width: 2.25rem; height: 2.25rem; align-items: center; justify-content: center; }
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| BreadcrumbList | `flex flex-wrap items-center gap-1.5 text-sm break-words text-muted-foreground sm:gap-2.5` | Responsive flex layout |
| BreadcrumbItem | `inline-flex items-center gap-1.5` | Inline item layout |
| BreadcrumbLink | `transition-colors hover:text-foreground` | Hover color transition |
| BreadcrumbPage | `font-normal text-foreground` | Current page styling |
| BreadcrumbSeparator | `[&>svg]:size-3.5` | Separator icon sizing |
| BreadcrumbEllipsis | `flex size-9 items-center justify-center` | Ellipsis container |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted-foreground` | List text color (non-active items) | `shadcn` |
| `--foreground` | Active page text, link hover color | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Hover, Dropdown, Ellipsis |

Default layout: flex row → Text (text-sm, font-normal, muted-foreground)
Hover uses foreground instead of muted-foreground. Dropdown adds a chevron icon. Ellipsis renders as a `...` icon.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--muted-foreground)` | `--muted-foreground` | Default item text color |
| `var(--foreground)` | `--foreground` | Hover/current item text color |
| `font-sans` | `--font-sans` | Font family |
| `font-normal` | `font-normal` | Item text weight |
| `text-sm` | `text-sm` | Item text size |

### Theme Behavior

- List text uses `--muted-foreground` — adapts to light/dark via theme
- Current page and link hover use `--foreground` — adapts to light/dark via theme
- Separator and ellipsis icons inherit `--muted-foreground` from the list
- Background is transparent — inherits from parent container
- All spacing and typography tokens are mode-independent
