# Component: Card

> **TL;DR:** A container for grouping related content and actions. 7 parts: Card, CardHeader, CardTitle, CardDescription, CardAction, CardContent, CardFooter. Supports `default` and `sm` sizes. CardHeader uses CSS grid with auto-placement for title, description, and action. No external primitive — pure shadCN. Commonly used for forms, settings panels, and content previews.

## Metadata

- **Component Name:** Card
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A container for grouping related content and actions. Provides visual separation with border, background, and shadow.

- Renders as a flex column with consistent gap between sections
- CardHeader uses CSS grid: title and description in first column, CardAction auto-placed top-right
- CardHeader has `@container/card-header` for container queries
- Supports `default` and `sm` sizes (sm uses smaller spacing)
- CardFooter aligns items horizontally with flex
- When CardHeader or CardFooter has a border (`.border-b` / `.border-t`), extra padding is added
- Supports RTL layout via the Direction component
- Can include images before CardHeader for media cards

### Anti-Patterns

#### Do
- Use cards as containers for related content that forms a distinct unit
- Use consistent card sizing within a grid

#### Don't
- Don't nest cards within cards
- Don't use cards when a simple section or list would suffice — if removing border, shadow, background, or radius does not hurt interaction or understanding, it should not be a card
- Don't use cards in hero sections
- Don't default to cards for layout — prefer sections, columns, dividers, lists, and media blocks

Sources: shadCN, Radix UI, UI Guidelines

## API

### Parts

- `Card` — Root container. Flex column with border, background, shadow, and rounded corners.
- `CardHeader` — Grid container for title, description, and optional action.
- `CardTitle` — Card heading text.
- `CardDescription` — Subheading or helper text below the title.
- `CardAction` — Action element (e.g., button, badge) positioned top-right of the header.
- `CardContent` — Main body content area.
- `CardFooter` — Bottom section for actions and secondary content.

### Props

#### Card

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm"` | `"default"` | Card spacing size |
| className | `string` | — | Additional CSS classes |

#### CardHeader

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### CardTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### CardDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### CardAction

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### CardContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### CardFooter

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="default"` | Standard spacing (`gap-6`, `py-6`, `px-6`) | General cards, forms |
| `size="sm"` | Compact spacing | Dense layouts, sidebar cards |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"card"`, `"card-header"`, `"card-title"`, `"card-description"`, `"card-action"`, `"card-content"`, `"card-footer"` | All parts |

### CSS Variables (Radix)

Not applicable — Card does not use Radix primitives.

## Composition Patterns

### Basic Card

```tsx
<Card>
  <CardHeader>
    <CardTitle>Card Title</CardTitle>
    <CardDescription>Card Description</CardDescription>
  </CardHeader>
  <CardContent>
    <p>Card Content</p>
  </CardContent>
  <CardFooter>
    <p>Card Footer</p>
  </CardFooter>
</Card>
```

### With Action

```tsx
<Card>
  <CardHeader>
    <CardTitle>Login to your account</CardTitle>
    <CardDescription>Enter your email below</CardDescription>
    <CardAction>
      <Button variant="link">Sign Up</Button>
    </CardAction>
  </CardHeader>
  <CardContent>
    <form>...</form>
  </CardContent>
  <CardFooter>
    <Button className="w-full">Login</Button>
  </CardFooter>
</Card>
```

### With Image

```tsx
<Card>
  <img
    src="..."
    alt="Featured image"
    className="aspect-video w-full rounded-t-xl object-cover"
  />
  <CardHeader>
    <CardTitle>Design systems meetup</CardTitle>
    <CardDescription>A practical talk on component APIs</CardDescription>
  </CardHeader>
  <CardFooter>
    <Button>Register</Button>
  </CardFooter>
</Card>
```

### Small Size

```tsx
<Card size="sm">
  <CardHeader>
    <CardTitle>Small Card</CardTitle>
    <CardDescription>Compact spacing variant</CardDescription>
  </CardHeader>
  <CardContent>
    <p>Content with smaller gaps</p>
  </CardContent>
</Card>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Card is a presentational container

### ARIA Roles

No implicit ARIA roles. Card renders as a `div`. If the card represents a distinct section, consider adding `role="region"` with `aria-label`.

### Keyboard Behavior

No keyboard interaction — Card is a static container. Interactive elements inside (buttons, links, inputs) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<div data-slot="card">
  <div data-slot="card-header">
    <div data-slot="card-title">Card Title</div>
    <div data-slot="card-description">Card Description</div>
    <div data-slot="card-action">
      <button>Action</button>
    </div>
  </div>
  <div data-slot="card-content">
    <p>Card content here.</p>
  </div>
  <div data-slot="card-footer">
    <button>Submit</button>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Card */
.Card {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  border-radius: 0.75rem;
  border: 1px solid var(--border);
  background-color: var(--card);
  padding-top: 1.5rem;
  padding-bottom: 1.5rem;
  color: var(--card-foreground);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* CardHeader */
.CardHeader {
  container-type: inline-size;
  container-name: card-header;
  display: grid;
  grid-template-rows: auto auto;
  grid-auto-rows: min-content;
  align-items: start;
  gap: 0.5rem;
  padding-left: 1.5rem;
  padding-right: 1.5rem;
}

/* CardHeader with action */
.CardHeader:has([data-slot="card-action"]) {
  grid-template-columns: 1fr auto;
}

/* CardHeader with bottom border */
.border-b .CardHeader {
  padding-bottom: 1.5rem;
}

/* CardTitle */
.CardTitle {
  line-height: 1;
  font-weight: 600;
}

/* CardDescription */
.CardDescription {
  font-size: 0.875rem;
  color: var(--muted-foreground);
}

/* CardAction */
.CardAction {
  grid-column-start: 2;
  grid-row: 1 / span 2;
  align-self: start;
  justify-self: end;
}

/* CardContent */
.CardContent {
  padding-left: 1.5rem;
  padding-right: 1.5rem;
}

/* CardFooter */
.CardFooter {
  display: flex;
  align-items: center;
  padding-left: 1.5rem;
  padding-right: 1.5rem;
}

/* CardFooter with top border */
.border-t .CardFooter {
  padding-top: 1.5rem;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Card | `flex flex-col gap-6 rounded-xl border bg-card py-6 text-card-foreground shadow-sm` | Container layout |
| CardHeader | `@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-2 px-6` | Grid header layout |
| CardHeader (with action) | `has-data-[slot=card-action]:grid-cols-[1fr_auto]` | Two-column grid when action present |
| CardHeader (border-b) | `[.border-b]:pb-6` | Extra padding with bottom border |
| CardTitle | `leading-none font-semibold` | Title typography |
| CardDescription | `text-sm text-muted-foreground` | Description typography |
| CardAction | `col-start-2 row-span-2 row-start-1 self-start justify-self-end` | Top-right grid placement |
| CardContent | `px-6` | Horizontal padding |
| CardFooter | `flex items-center px-6 [.border-t]:pt-6` | Footer layout with optional top border padding |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--card` | Card background color | `shadcn` |
| `--card-foreground` | Card text color | `shadcn` |
| `--border` | Card border color (implicit from `border` class) | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |

## Theme Support

### Component Structure

Card is organized into a base component, sub-components, and example compositions:

Base component: Card → Card Header (optional) + Content area + Card Footer (optional)

| Sub-Component | Variant Property | Values |
|--------------|-----------------|--------|
| Card Header | Alignment | Left, Center |
| Card Header | Size | Large, Medium, Small |
| Card Footer | Type | Button left, Button right, Two buttons, Full width button, Calendar, Pagination |
| Card Footer | Stacked | True, False |

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--card` | `--card` | Card background |
| `--card-foreground` | `--card-foreground` | Card title text color |
| `--border` | `--border` | Card border |
| `--muted-foreground` | `--muted-foreground` | Card description text color |
| `--radius` | `--radius` | Card border radius |
| `p-6` | `padding` | Header/footer/content padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Title font weight (medium size) |
| `font-semibold` (600) | `font-weight` | Title font weight (large size) |
| `text-sm` | `font-size` | Description font size |
| `shadow-sm` | `box-shadow` | Card shadow |

### Theme Behavior

- Card uses `--card` / `--card-foreground` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Border uses `--border` (implicit) — adapts to light/dark via theme
- Shadow is fixed (`shadow-sm`) — same in both modes
- All spacing and typography tokens are mode-independent
