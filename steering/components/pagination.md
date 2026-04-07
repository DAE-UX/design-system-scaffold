# Component: Pagination

> **TL;DR:** A navigation component for paginated content. 7 parts: Pagination, PaginationContent, PaginationItem, PaginationLink, PaginationPrevious, PaginationNext, PaginationEllipsis. Uses Button variants (outline when active, ghost when not). Previous/Next show text on `sm+` breakpoint. Ellipsis uses MoreHorizontalIcon with sr-only "More pages". No external primitive — uses shadCN Button variants.

## Metadata

- **Component Name:** Pagination
- **Source:** shadCN
- **Dependencies:** (none — no external primitive, uses Button variants)

## Behavior

A navigation component for moving between pages of paginated content, built using Button variants for consistent styling.

- Pagination root is a `nav` element with `role="navigation"` and `aria-label="pagination"`
- PaginationContent renders as a `ul` with flex row layout and `gap-1`
- PaginationItem renders as a `li` wrapper
- PaginationLink renders as an `a` element using `buttonVariants` — `outline` variant when active, `ghost` when not
- Active link receives `aria-current="page"` and `data-active` attribute
- PaginationPrevious includes ChevronLeftIcon and "Previous" text (text hidden below `sm` breakpoint)
- PaginationNext includes ChevronRightIcon and "Next" text (text hidden below `sm` breakpoint)
- PaginationEllipsis renders MoreHorizontalIcon with `aria-hidden` and sr-only "More pages" text

### Anti-Patterns

#### Do
- Use for horizontal navigation between pages of a collection (tables, cards, lists)
- Use default pagination for collections that won't change size

#### Don't
- Don't use if the collection has fewer than 5 elements
- Don't use open-end pagination if you can determine the full collection size

Sources: shadCN, Radix UI

## API

### Parts

- `Pagination` — Root `nav` element with navigation role and aria-label.
- `PaginationContent` — Unordered list container for pagination items.
- `PaginationItem` — List item wrapper for each pagination element.
- `PaginationLink` — Anchor element styled with Button variants for page numbers.
- `PaginationPrevious` — Previous page link with chevron icon and responsive text.
- `PaginationNext` — Next page link with chevron icon and responsive text.
- `PaginationEllipsis` — Visual indicator for skipped page ranges.

### Props

#### Pagination

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PaginationContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PaginationItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PaginationLink

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| isActive | `boolean` | `false` | Whether this is the current page |
| className | `string` | — | Additional CSS classes |

#### PaginationPrevious

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PaginationNext

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### PaginationEllipsis

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| PaginationLink (active) | `outline` button variant, `aria-current="page"` | Current page indicator |
| PaginationLink (inactive) | `ghost` button variant | Non-current page links |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"pagination"`, `"pagination-content"`, `"pagination-item"`, `"pagination-link"`, `"pagination-previous"`, `"pagination-next"`, `"pagination-ellipsis"` | All parts |
| `[data-active]` | `"true"` | PaginationLink (when active) |

### CSS Variables (Radix)

Not applicable — Pagination does not use Radix primitives.

## Composition Patterns

### Basic Pagination

```tsx
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#" isActive>2</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">3</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

### With Ellipsis

```tsx
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationEllipsis />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#" isActive>5</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationEllipsis />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">10</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

## Accessibility

- **WAI-ARIA Pattern:** Navigation

### ARIA Roles

Pagination root uses `role="navigation"` with `aria-label="pagination"`. Active page link uses `aria-current="page"`. Ellipsis uses `aria-hidden="true"` with sr-only text "More pages".

### Keyboard Behavior

- `Tab` — Moves focus between pagination links
- `Enter` — Activates the focused link
- Standard anchor element keyboard behavior applies

## HTML

### Standalone HTML Structure

```html
<nav data-slot="pagination" role="navigation" aria-label="pagination">
  <ul data-slot="pagination-content">
    <li data-slot="pagination-item">
      <a data-slot="pagination-previous" href="#">
        <svg><!-- ChevronLeftIcon --></svg>
        <span>Previous</span>
      </a>
    </li>
    <li data-slot="pagination-item">
      <a data-slot="pagination-link" href="#">1</a>
    </li>
    <li data-slot="pagination-item">
      <a data-slot="pagination-link" href="#" aria-current="page" data-active="true">2</a>
    </li>
    <li data-slot="pagination-item">
      <a data-slot="pagination-link" href="#">3</a>
    </li>
    <li data-slot="pagination-item">
      <span data-slot="pagination-ellipsis" aria-hidden="true">
        <svg><!-- MoreHorizontalIcon --></svg>
        <span class="sr-only">More pages</span>
      </span>
    </li>
    <li data-slot="pagination-item">
      <a data-slot="pagination-next" href="#">
        <span>Next</span>
        <svg><!-- ChevronRightIcon --></svg>
      </a>
    </li>
  </ul>
</nav>
```

## CSS

### Raw CSS

```css
/* Pagination */
.Pagination {
  margin-left: auto;
  margin-right: auto;
  display: flex;
  width: 100%;
  justify-content: center;
}

/* PaginationContent */
.PaginationContent {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.25rem;
  list-style: none;
}

/* PaginationItem */
.PaginationItem {
  /* li wrapper — no custom styles */
}

/* PaginationLink — inherits from Button variants */
/* Active: outline variant */
/* Inactive: ghost variant */

/* PaginationPrevious */
.PaginationPrevious {
  gap: 0.25rem;
  padding-left: 0.625rem;
  padding-right: 0.625rem;
}

.PaginationPrevious span {
  display: none;
}

@media (min-width: 640px) {
  .PaginationPrevious span {
    display: block;
  }
  .PaginationPrevious {
    padding-left: 0.625rem;
  }
}

/* PaginationNext */
.PaginationNext {
  gap: 0.25rem;
  padding-left: 0.625rem;
  padding-right: 0.625rem;
}

.PaginationNext span {
  display: none;
}

@media (min-width: 640px) {
  .PaginationNext span {
    display: block;
  }
  .PaginationNext {
    padding-right: 0.625rem;
  }
}

/* PaginationEllipsis */
.PaginationEllipsis {
  display: flex;
  width: 2.25rem;
  height: 2.25rem;
  align-items: center;
  justify-content: center;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Pagination | `mx-auto flex w-full justify-center` | Centered nav container |
| PaginationContent | `flex flex-row items-center gap-1` | Horizontal item list |
| PaginationItem | (li wrapper) | List item |
| PaginationLink (active) | `buttonVariants({ variant: "outline" })` + `aria-current="page"` | Active page button |
| PaginationLink (inactive) | `buttonVariants({ variant: "ghost" })` | Inactive page button |
| PaginationPrevious | `gap-1 px-2.5 sm:pl-2.5` + `"Previous"` text `hidden sm:block` | Previous with responsive text |
| PaginationNext | `gap-1 px-2.5 sm:pr-2.5` + `"Next"` text `hidden sm:block` | Next with responsive text |
| PaginationEllipsis | `flex size-9 items-center justify-center` | Ellipsis container |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| Inherits from Button variants | All button styling variables | `shadcn` (Button) |

## Theme Support

### Component Structure

Component organizes Pagination into two sub-components:

Pagination (composite):
- Single variant showing Previous + page numbers + ellipsis + Next in flex row

.Pagination Item:

| Variant Property | Values |
|-----------------|--------|
| Type | Page, Previous, Next, Ellipsis |
| Selected | False, True |
| State | Enabled, Hover, Focus, Disabled, Default (Ellipsis), Previous (Next) |

- Pagination → Previous item + Page items + Ellipsis + Next item
- Container: flex with gap 4px, centered
- Page items: 36×36px, rounded
- Selected page: background, border, shadow
- Previous/Next: flex with gap 4px, px 10px, py 8px, icon + text


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Selected page background |
| `--input` | `--input` | Selected page border |
| `--primary` | `--primary` | Page number text color |
| `--radius` (derived) | `--radius` | Item border radius |
| `h-9` (36px) | `height` | Item height |
| `gap-1` (4px) | `gap` | Gap between items |
| `px-2.5` (10px) | `padding-inline` | Previous/Next horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Text font weight |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-xs` | `box-shadow` | Selected page shadow |

### Theme Behavior

- PaginationLink inherits theme behavior from Button variants — adapts to light/dark via theme
- Active state uses `outline` Button variant — adapts to light/dark via theme
- Inactive state uses `ghost` Button variant — adapts to light/dark via theme
- All spacing and typography tokens are mode-independent
