# Component: Item

> **TL;DR:** A flexible list item layout component. 10 parts: Item, ItemGroup, ItemSeparator, ItemMedia, ItemContent, ItemTitle, ItemDescription, ItemActions, ItemHeader, ItemFooter. Item supports 3 style variants (`default`, `outline`, `muted`) and 2 sizes (`default`, `sm`) via CVA. ItemMedia supports 3 variants (`default`, `icon`, `image`). Supports `asChild` via Radix Slot. Uses internal Separator. `role="list"` on ItemGroup.

## Metadata

- **Component Name:** Item
- **Source:** shadCN
- **Dependencies:** radix-ui

## Behavior

A flexible list item layout component for building structured lists, settings panels, and data displays.

- ItemGroup renders as a flex column with `role="list"`
- Item renders as a flex row with wrap, rounded corners, and transparent border (for consistent sizing across variants)
- Item supports `asChild` via Radix Slot for rendering as links or other elements
- Item supports 3 style variants: `default` (transparent), `outline` (visible border), `muted` (muted background)
- Item supports 2 sizes: `default` (gap-4, p-4) and `sm` (gap-2.5, px-4, py-3)
- ItemMedia supports 3 variants: `default` (transparent), `icon` (32px square with border and muted bg), `image` (40px square with overflow hidden)
- ItemMedia auto-aligns to top when sibling ItemDescription exists (via `translate-y-0.5` and `self-start`)
- ItemContent uses `flex-1`; second ItemContent in same Item gets `flex-none`
- ItemTitle renders as `text-sm font-medium` with inline gap for badges/icons
- ItemDescription renders with `line-clamp-2`, `text-balance`, and auto-styled links
- ItemActions renders as a flex row for action buttons
- ItemHeader and ItemFooter span full width (`basis-full`) with space-between layout
- ItemSeparator renders a horizontal separator with `my-0` (no vertical margin)
- Focus-visible: ring with `border-ring` and 3px ring; links inside get `hover:bg-accent/50`

### Anti-Patterns

#### Do
- Use as a generic list item within menus, sidebars, or selection lists. Maintain consistent height and padding across items in the same list.

#### Don't
- Don't use outside of a list context. Don't mix item sizes within the same list.

Sources: shadCN, Radix UI

## API

### Parts

- `Item` — Root layout container. Flex row with variants for style and size.
- `ItemGroup` — List container with `role="list"`.
- `ItemSeparator` — Horizontal divider between items.
- `ItemMedia` — Media container. Supports default, icon, and image variants.
- `ItemContent` — Main content area. Flex column with title/description stacking.
- `ItemTitle` — Item heading text.
- `ItemDescription` — Secondary text with line clamping and link styling.
- `ItemActions` — Action button container.
- `ItemHeader` — Full-width header row with space-between.
- `ItemFooter` — Full-width footer row with space-between.

### Props

#### Item

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "outline" \| "muted"` | `"default"` | Visual style variant |
| size | `"default" \| "sm"` | `"default"` | Spacing size |
| asChild | `boolean` | `false` | Render as child element via Radix Slot |
| className | `string` | — | Additional CSS classes |

#### ItemMedia

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "icon" \| "image"` | `"default"` | Media container variant |
| className | `string` | — | Additional CSS classes |

#### ItemGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### ItemSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### ItemContent, ItemTitle, ItemDescription, ItemActions, ItemHeader, ItemFooter

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

#### Item Style Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Transparent background, transparent border | Standard list items |
| `outline` | Visible border | Bordered list items, cards |
| `muted` | Muted background (`bg-muted/50`) | Highlighted or selected items |

#### Item Size Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | `gap-4 p-4` | Standard spacing |
| `sm` | `gap-2.5 px-4 py-3` | Compact lists |

#### ItemMedia Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Transparent, no container | Custom media content |
| `icon` | 32px square, border, muted bg, 16px icon | Icon indicators |
| `image` | 40px square, overflow hidden, object-cover | Thumbnails, avatars |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"item"`, `"item-group"`, `"item-separator"`, `"item-media"`, `"item-content"`, `"item-title"`, `"item-description"`, `"item-actions"`, `"item-header"`, `"item-footer"` | All parts |
| `[data-variant]` | `"default"`, `"outline"`, `"muted"` | Item |
| `[data-variant]` | `"default"`, `"icon"`, `"image"` | ItemMedia |
| `[data-size]` | `"default"`, `"sm"` | Item |

### CSS Variables (Radix)

Not applicable — Item does not use Radix visual primitives (only Slot for `asChild`).

## Composition Patterns

### Basic Item List

```tsx
<ItemGroup>
  <Item>
    <ItemMedia variant="icon">
      <FileIcon />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Document.pdf</ItemTitle>
      <ItemDescription>Uploaded 2 days ago</ItemDescription>
    </ItemContent>
    <ItemActions>
      <Button variant="ghost" size="icon-sm">
        <MoreHorizontalIcon />
      </Button>
    </ItemActions>
  </Item>
  <ItemSeparator />
  <Item>
    <ItemMedia variant="icon">
      <ImageIcon />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Photo.jpg</ItemTitle>
      <ItemDescription>Uploaded yesterday</ItemDescription>
    </ItemContent>
  </Item>
</ItemGroup>
```

### With Image

```tsx
<Item variant="outline">
  <ItemMedia variant="image">
    <img src="..." alt="User avatar" />
  </ItemMedia>
  <ItemContent>
    <ItemTitle>Jane Doe</ItemTitle>
    <ItemDescription>Software Engineer</ItemDescription>
  </ItemContent>
</Item>
```

### As Link

```tsx
<Item asChild>
  <a href="/settings/profile">
    <ItemContent>
      <ItemTitle>Profile Settings</ItemTitle>
      <ItemDescription>Manage your profile information</ItemDescription>
    </ItemContent>
  </a>
</Item>
```

### With Header and Footer

```tsx
<Item variant="outline">
  <ItemHeader>
    <ItemTitle>Order #1234</ItemTitle>
    <Badge>Shipped</Badge>
  </ItemHeader>
  <ItemContent>
    <ItemDescription>3 items · Estimated delivery: March 25</ItemDescription>
  </ItemContent>
  <ItemFooter>
    <span className="text-sm text-muted-foreground">$49.99</span>
    <Button size="sm">Track</Button>
  </ItemFooter>
</Item>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Item uses semantic list structure

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| ItemGroup | `role="list"` | Semantic list container |
| Item | implicit `listitem` when inside ItemGroup | List item |
| ItemSeparator | `separator` (from Separator component) | Visual divider |

### Keyboard Behavior

No keyboard interaction — Item is a layout container. When rendered as a link via `asChild`, standard link keyboard behavior applies. Interactive elements inside (buttons) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<div data-slot="item-group" role="list">
  <div data-slot="item" data-variant="default" data-size="default">
    <div data-slot="item-media" data-variant="icon">
      <svg><!-- icon --></svg>
    </div>
    <div data-slot="item-content">
      <div data-slot="item-title">Document.pdf</div>
      <p data-slot="item-description">Uploaded 2 days ago</p>
    </div>
    <div data-slot="item-actions">
      <button>...</button>
    </div>
  </div>
  <div data-slot="item-separator" role="separator"></div>
</div>
```

## CSS

### Raw CSS

```css
/* ItemGroup */
.ItemGroup {
  display: flex;
  flex-direction: column;
}

/* Item — base */
.Item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  border-radius: 0.375rem;
  border: 1px solid transparent;
  font-size: 0.875rem;
  transition: color 100ms;
  outline: none;
}

.Item:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Item — default variant */
.Item[data-variant="default"] {
  background-color: transparent;
}

/* Item — outline variant */
.Item[data-variant="outline"] {
  border-color: var(--border);
}

/* Item — muted variant */
.Item[data-variant="muted"] {
  background-color: color-mix(in srgb, var(--muted) 50%, transparent);
}

/* Item — default size */
.Item[data-size="default"] {
  gap: 1rem;
  padding: 1rem;
}

/* Item — sm size */
.Item[data-size="sm"] {
  gap: 0.625rem;
  padding: 0.75rem 1rem;
}

/* ItemMedia — base */
.ItemMedia {
  display: flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

/* ItemMedia — icon variant */
.ItemMedia[data-variant="icon"] {
  width: 2rem;
  height: 2rem;
  border-radius: 0.125rem;
  border: 1px solid var(--border);
  background-color: var(--muted);
}

/* ItemMedia — image variant */
.ItemMedia[data-variant="image"] {
  width: 2.5rem;
  height: 2.5rem;
  overflow: hidden;
  border-radius: 0.125rem;
}

/* ItemContent */
.ItemContent {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: 0.25rem;
}

/* ItemTitle */
.ItemTitle {
  display: flex;
  width: fit-content;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  line-height: 1.375;
  font-weight: 500;
}

/* ItemDescription */
.ItemDescription {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  font-size: 0.875rem;
  line-height: 1.5;
  font-weight: 400;
  text-wrap: balance;
  color: var(--muted-foreground);
}

.ItemDescription a {
  text-decoration: underline;
  text-underline-offset: 4px;
}

.ItemDescription a:hover {
  color: var(--primary);
}

/* ItemActions */
.ItemActions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* ItemHeader / ItemFooter */
.ItemHeader,
.ItemFooter {
  display: flex;
  flex-basis: 100%;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
}

/* ItemSeparator */
.ItemSeparator {
  margin-top: 0;
  margin-bottom: 0;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ItemGroup | `group/item-group flex flex-col` | List container |
| Item (base) | `group/item flex flex-wrap items-center rounded-md border border-transparent text-sm transition-colors duration-100 outline-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Item layout |
| Item (default) | `bg-transparent` | Default variant |
| Item (outline) | `border-border` | Bordered variant |
| Item (muted) | `bg-muted/50` | Muted variant |
| Item (default size) | `gap-4 p-4` | Standard spacing |
| Item (sm size) | `gap-2.5 px-4 py-3` | Compact spacing |
| Item links | `[a]:transition-colors [a]:hover:bg-accent/50` | Link hover |
| ItemMedia (base) | `flex shrink-0 items-center justify-center gap-2 [&_svg]:pointer-events-none` | Media base |
| ItemMedia (icon) | `size-8 rounded-sm border bg-muted [&_svg:not([class*='size-'])]:size-4` | Icon container |
| ItemMedia (image) | `size-10 overflow-hidden rounded-sm [&_img]:size-full [&_img]:object-cover` | Image container |
| ItemContent | `flex flex-1 flex-col gap-1 [&+[data-slot=item-content]]:flex-none` | Content area |
| ItemTitle | `flex w-fit items-center gap-2 text-sm leading-snug font-medium` | Title text |
| ItemDescription | `line-clamp-2 text-sm leading-normal font-normal text-balance text-muted-foreground [&>a]:underline [&>a]:underline-offset-4 [&>a:hover]:text-primary` | Description text |
| ItemActions | `flex items-center gap-2` | Action buttons |
| ItemHeader / ItemFooter | `flex basis-full items-center justify-between gap-2` | Full-width rows |
| ItemSeparator | `my-0` | No-margin separator |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Outline variant border, icon media border | `shadcn` |
| `--muted` | Muted variant background, icon media background | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |
| `--primary` | Description link hover color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--accent` | Link hover background inside items | `shadcn` |

## Theme Support

### Component Structure

Item sub-components:

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Outline |
| Size | Default, Small |
| State | Enabled, Hover |

Media types: Icon, Spinner, Feature icon, Avatar, Avatar Group, Image
Actions: Button1, Button2, Icon, Text (each true/false)

Layout: optional Header + Container (Media + Content + Actions) + optional Footer
Container: flex row (gap 16px). Content: flex column (gap 4px, title + description)
Item padding: 16px, border radius from `--radius`

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Title text color |
| `--muted-foreground` | `--muted-foreground` | Description text color |
| `--muted` | `--muted` | Media icon background |
| `--border` | `--border` | Media icon border, outline variant border |
| `--input` | `--input` | Action button border |
| `--radius` | `--radius` | Item border radius |
| `p-4` (16px) | `padding` | Item padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Outline variant uses `--border` — adapts to light/dark via theme
- Muted variant uses `--muted/50` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Link hover uses `--accent/50` — adapts to light/dark via theme
- All spacing and typography tokens are mode-independent
