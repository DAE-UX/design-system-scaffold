# Component: Empty

> **TL;DR:** A placeholder container for empty states. 6 parts: Empty, EmptyHeader, EmptyMedia, EmptyTitle, EmptyDescription, EmptyContent. EmptyMedia supports `default` and `icon` variants via CVA. No external primitive — pure shadCN. Dashed border, centered flex column, text-balance. Used for zero-data states with optional icon, title, description, and action buttons.

## Metadata

- **Component Name:** Empty
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A container for displaying empty states when no data or content is available.

- Renders as a centered flex column with dashed border and balanced text
- Empty root uses `flex-1` to fill available space
- EmptyHeader constrains content to `max-w-sm` and centers text
- EmptyMedia supports two variants: `default` (transparent background) and `icon` (muted background with rounded-lg, 40px square)
- EmptyMedia auto-sizes SVG icons: `pointer-events-none`, `shrink-0`, and 24px default size for `icon` variant
- EmptyTitle uses `text-lg` with `font-medium` and `tracking-tight`
- EmptyDescription uses `text-sm/relaxed` with muted foreground color; links inside get underline styling and primary color on hover
- EmptyContent constrains actions to `max-w-sm` with centered flex column
- Responsive padding: `p-6` on mobile, `p-12` on `md` breakpoint
- All parts accept `className` for customization

### Anti-Patterns

#### Do
- Use to communicate when a view has no content.
- Provide a clear message explaining why it's empty.
- Include a primary action to help users populate the view.

#### Don't
- Don't leave empty states blank — always provide guidance.
- Don't use generic messages ("No data") — be specific about what's missing and what to do.

Sources: shadCN, Radix UI

## API

### Parts

- `Empty` — Root container. Centered flex column with dashed border and balanced text.
- `EmptyHeader` — Header section. Constrains title/description to `max-w-sm`, centered.
- `EmptyMedia` — Media/icon container. Supports `default` and `icon` variants via CVA.
- `EmptyTitle` — Heading text for the empty state.
- `EmptyDescription` — Descriptive text below the title. Links auto-styled.
- `EmptyContent` — Action area for buttons or other interactive content.

### Props

#### Empty

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### EmptyHeader

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### EmptyMedia

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "icon"` | `"default"` | Visual style variant |
| className | `string` | — | Additional CSS classes |

#### EmptyTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### EmptyDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### EmptyContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

#### EmptyMedia Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Transparent background, no container styling | Custom media (images, illustrations) |
| `icon` | 40px square, muted background, rounded-lg, 24px icon | Standard icon empty state |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"empty"`, `"empty-header"`, `"empty-icon"`, `"empty-title"`, `"empty-description"`, `"empty-content"` | All parts |
| `[data-variant]` | `"default"`, `"icon"` | EmptyMedia |

### CSS Variables (Radix)

Not applicable — Empty does not use Radix primitives.

## Composition Patterns

### Basic Empty State

```tsx
<Empty>
  <EmptyHeader>
    <EmptyMedia variant="icon">
      <FolderIcon />
    </EmptyMedia>
    <EmptyTitle>No Projects Yet</EmptyTitle>
    <EmptyDescription>
      You haven't created any projects yet. Get started by creating
      your first project.
    </EmptyDescription>
  </EmptyHeader>
  <EmptyContent>
    <div className="flex gap-2">
      <Button>Create Project</Button>
      <Button variant="outline">Import Project</Button>
    </div>
  </EmptyContent>
</Empty>
```

### With Link

```tsx
<Empty>
  <EmptyHeader>
    <EmptyMedia variant="icon">
      <SearchIcon />
    </EmptyMedia>
    <EmptyTitle>No Results Found</EmptyTitle>
    <EmptyDescription>
      Try adjusting your search or <a href="#">browse all items</a>.
    </EmptyDescription>
  </EmptyHeader>
</Empty>
```

### Minimal (No Icon)

```tsx
<Empty>
  <EmptyHeader>
    <EmptyTitle>Nothing here</EmptyTitle>
    <EmptyDescription>
      This section is empty.
    </EmptyDescription>
  </EmptyHeader>
</Empty>
```

### With Custom Media

```tsx
<Empty>
  <EmptyHeader>
    <EmptyMedia>
      <img src="..." alt="Empty illustration" className="size-24" />
    </EmptyMedia>
    <EmptyTitle>No Notifications</EmptyTitle>
    <EmptyDescription>
      You're all caught up.
    </EmptyDescription>
  </EmptyHeader>
</Empty>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Empty is a presentational container

### ARIA Roles

No implicit ARIA roles. Empty renders as a `div`. If the empty state represents a distinct status region, consider adding `role="status"` with `aria-live="polite"` for dynamic empty states that appear after content is removed.

### Keyboard Behavior

No keyboard interaction — Empty is a static container. Interactive elements inside (buttons, links) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<div data-slot="empty">
  <div data-slot="empty-header">
    <div data-slot="empty-icon" data-variant="icon">
      <svg><!-- icon --></svg>
    </div>
    <div data-slot="empty-title">No Projects Yet</div>
    <div data-slot="empty-description">
      You haven't created any projects yet.
    </div>
  </div>
  <div data-slot="empty-content">
    <div>
      <button>Create Project</button>
      <button>Import Project</button>
    </div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Empty */
.Empty {
  display: flex;
  min-width: 0;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  border-radius: 0.5rem;
  border-style: dashed;
  padding: 1.5rem;
  text-align: center;
  text-wrap: balance;
}

@media (min-width: 768px) {
  .Empty {
    padding: 3rem;
  }
}

/* EmptyHeader */
.EmptyHeader {
  display: flex;
  max-width: 24rem;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  text-align: center;
}

/* EmptyMedia — base */
.EmptyMedia {
  margin-bottom: 0.5rem;
  display: flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
}

.EmptyMedia svg {
  pointer-events: none;
  flex-shrink: 0;
}

/* EmptyMedia — default variant */
.EmptyMedia[data-variant="default"] {
  background-color: transparent;
}

/* EmptyMedia — icon variant */
.EmptyMedia[data-variant="icon"] {
  display: flex;
  width: 2.5rem;
  height: 2.5rem;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  border-radius: 0.5rem;
  background-color: var(--muted);
  color: var(--foreground);
}

.EmptyMedia[data-variant="icon"] svg:not([class*='size-']) {
  width: 1.5rem;
  height: 1.5rem;
}

/* EmptyTitle */
.EmptyTitle {
  font-size: 1.125rem;
  font-weight: 500;
  letter-spacing: -0.025em;
}

/* EmptyDescription */
.EmptyDescription {
  font-size: 0.875rem;
  line-height: 1.625;
  color: var(--muted-foreground);
}

.EmptyDescription a {
  text-decoration: underline;
  text-underline-offset: 4px;
}

.EmptyDescription a:hover {
  color: var(--primary);
}

/* EmptyContent */
.EmptyContent {
  display: flex;
  width: 100%;
  max-width: 24rem;
  min-width: 0;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  font-size: 0.875rem;
  text-wrap: balance;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Empty | `flex min-w-0 flex-1 flex-col items-center justify-center gap-6 rounded-lg border-dashed p-6 text-center text-balance md:p-12` | Centered container with dashed border |
| EmptyHeader | `flex max-w-sm flex-col items-center gap-2 text-center` | Constrained header layout |
| EmptyMedia (base) | `mb-2 flex shrink-0 items-center justify-center [&_svg]:pointer-events-none [&_svg]:shrink-0` | Media container base |
| EmptyMedia (default) | `bg-transparent` | Transparent background |
| EmptyMedia (icon) | `flex size-10 shrink-0 items-center justify-center rounded-lg bg-muted text-foreground [&_svg:not([class*='size-'])]:size-6` | Icon container with muted background |
| EmptyTitle | `text-lg font-medium tracking-tight` | Title typography |
| EmptyDescription | `text-sm/relaxed text-muted-foreground [&>a]:underline [&>a]:underline-offset-4 [&>a:hover]:text-primary` | Description with link styling |
| EmptyContent | `flex w-full max-w-sm min-w-0 flex-col items-center gap-4 text-sm text-balance` | Action area layout |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | EmptyMedia icon variant background | `shadcn` |
| `--foreground` | EmptyMedia icon variant text/icon color | `shadcn` |
| `--muted-foreground` | EmptyDescription text color | `shadcn` |
| `--primary` | EmptyDescription link hover color | `shadcn` |

## Theme Support

### Component Structure

**Empty Media** — Types: Icon, Avatar, Avatar Group

**Empty Content** — Types: Single Button, 2 Buttons - Vert, 2 Buttons - Horiz, 3 Buttons, Input & Description

Layout: flex column (gap 12px, width 300px, center) → Empty Header + Empty Content
Header: flex column (gap 8px, max-w 384px, center) → Media + Copy block (Title + Description)
Media (Icon type): muted bg, padding 8px, rounded → icon (24×24px)
Content: flex column (gap 8px, max-w 384px, center) → Button row(s) + optional link

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--foreground)` | `--foreground` | Title text color |
| `var(--muted-foreground)` | `--muted-foreground` | Description text |
| `var(--muted)` | `--muted` | Icon container background |
| `var(--primary)` | `--primary` | Primary button background |
| `--radius` (derived) | `--radius` | Icon container and button border radius |
| `gap-3` (12px) | `gap-3` | Root vertical gap |
| `font-sans` | `--font-sans` | Font family |
| `text-lg` (18px) | `text-lg` | Title text size |
| `text-sm` (14px) | `text-sm` | Description and button text size |

### Theme Behavior

- EmptyMedia icon variant uses `--muted` / `--foreground` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Link hover uses `--primary` — adapts to light/dark via theme
- Dashed border inherits from `--border` (implicit via `border-dashed` class) — adapts to light/dark via theme
- All spacing, typography, and layout tokens are mode-independent
