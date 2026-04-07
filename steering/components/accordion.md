# Component: Accordion

> **TL;DR:** Vertically stacked interactive headings that reveal content sections. Built on Radix UI primitive via shadCN. Supports single/multiple expand, controlled/uncontrolled, keyboard navigation, and WAI-ARIA accordion pattern. All colors reference CSS variables — resolved values live in theme files. See API for props, Composition Patterns for usage, and Theme Support for CSS variable mapping.

## Metadata

- **Component Name:** Accordion
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** @radix-ui/react-accordion

## Behavior

A vertically stacked set of interactive headings that each reveal an associated section of content. Built on Radix UI Accordion primitive.

- Supports single or multiple expanded items via `type` prop
- Can be controlled or uncontrolled
- Supports horizontal/vertical orientation
- Supports RTL direction
- Collapsible mode allows all items to be closed (single type only)

### Anti-Patterns

#### Do
- Use for organizing content into collapsible sections.
- Use descriptive trigger text that summarizes the hidden content.
- Allow multiple items open simultaneously when content comparison is needed (`type="multiple"`).

#### Don't
- Don't nest accordions inside accordions.
- Don't use for critical content that users must see — it should be visible by default.
- Don't use as a replacement for tabs when content sections are unrelated.

Sources: shadCN, Radix UI

## API

### Parts

- `Accordion` (Root) — contains all accordion parts
- `AccordionItem` — contains all parts of a collapsible section
- `AccordionTrigger` — toggles the collapsed state of its associated item (wrapped in Header)
- `AccordionContent` — contains the collapsible content for an item

### Props

#### Root

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| type* | `"single" \| "multiple"` | — | Determines whether one or multiple items can be opened at the same time |
| value | `string` (single) / `string[]` (multiple) | — | Controlled value of the expanded item(s) |
| defaultValue | `string` (single) / `string[]` (multiple) | — | Default value of the expanded item(s) |
| onValueChange | `function` | — | Callback when the expanded state changes |
| collapsible | `boolean` | `false` | When type is "single", allows closing content by clicking the open trigger |
| disabled | `boolean` | `false` | Prevents user interaction with all items |
| dir | `"ltr" \| "rtl"` | `"ltr"` | Reading direction |
| orientation | `"vertical" \| "horizontal"` | `"vertical"` | Orientation of the accordion |

#### Item

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value* | `string` | — | Unique value for the item |
| disabled | `boolean` | `false` | Prevents user interaction with this item |

#### Content

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| forceMount | `boolean` | — | Forces mounting when used with animation libraries |

### Variants

EMPTY: None recorded.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-state]` | `"open" \| "closed"` | Item, Header, Trigger, Content |
| `[data-disabled]` | Present when disabled | Item, Header, Trigger, Content |
| `[data-orientation]` | `"vertical" \| "horizontal"` | Root, Item, Header, Trigger, Content |

### CSS Variables (Radix)

| Variable | Description |
|----------|-------------|
| `--radix-accordion-content-width` | Width of the content when it opens/closes |
| `--radix-accordion-content-height` | Height of the content when it opens/closes |

## Composition Patterns

### Basic Usage (shadCN)

```tsx
<Accordion type="single" collapsible defaultValue="item-1">
  <AccordionItem value="item-1">
    <AccordionTrigger>Section heading</AccordionTrigger>
    <AccordionContent>
      Section content here.
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

### Multiple Items Open

```tsx
<Accordion type="multiple">
  <AccordionItem value="item-1">…</AccordionItem>
  <AccordionItem value="item-2">…</AccordionItem>
</Accordion>
```

### With Card Wrapper

Accordion can be wrapped in a Card component for bordered container styling.

### Nesting / Layout

- AccordionItem contains AccordionTrigger + AccordionContent
- AccordionTrigger is internally wrapped in an AccordionHeader
- Content can contain any valid React children

## Accessibility

- **WAI-ARIA Pattern:** [Accordion Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/accordion)
- Adheres to the WAI-ARIA Accordion design pattern

### ARIA Roles

- Trigger uses `button` role within a heading element
- Content region is associated with its trigger
- `aria-expanded` state is managed automatically by Radix
- Decorative icons should use `aria-hidden`

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Space | Expands/collapses the focused section |
| Enter | Expands/collapses the focused section |
| Tab | Moves focus to the next focusable element |
| Shift + Tab | Moves focus to the previous focusable element |
| ArrowDown | Moves focus to the next trigger (vertical orientation) |
| ArrowUp | Moves focus to the previous trigger (vertical orientation) |
| ArrowRight | Moves focus to the next trigger (horizontal orientation) |
| ArrowLeft | Moves focus to the previous trigger (horizontal orientation) |
| Home | Moves focus to the first trigger |
| End | Moves focus to the last trigger |

## HTML

### Standalone HTML Structure

```html
<div data-orientation="vertical">
  <!-- Item -->
  <div data-state="open" data-orientation="vertical">
    <!-- Header -->
    <h3 data-orientation="vertical" data-state="open">
      <button
        type="button"
        aria-expanded="true"
        data-state="open"
        data-orientation="vertical"
      >
        Section heading
        <svg aria-hidden="true"><!-- chevron icon --></svg>
      </button>
    </h3>
    <!-- Content -->
    <div
      role="region"
      data-state="open"
      data-orientation="vertical"
    >
      <div>Section content here.</div>
    </div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Accordion content animation */
.AccordionContent {
  overflow: hidden;
}
.AccordionContent[data-state="open"] {
  animation: slideDown 300ms ease-out;
}
.AccordionContent[data-state="closed"] {
  animation: slideUp 300ms ease-out;
}

@keyframes slideDown {
  from { height: 0; }
  to { height: var(--radix-accordion-content-height); }
}

@keyframes slideUp {
  from { height: var(--radix-accordion-content-height); }
  to { height: 0; }
}

/* Chevron rotation */
.AccordionChevron {
  transition: transform 300ms;
}
.AccordionTrigger[data-state="open"] > .AccordionChevron {
  transform: rotate(180deg);
}
```

### Tailwind Mapping

shadCN accordion uses these Tailwind utilities:
- `border-b` on AccordionItem for separator lines
- `flex`, `flex-1`, `items-center`, `justify-between` on trigger layout
- `py-4`, `font-medium` on trigger text
- `text-sm` on trigger
- `transition-all` for chevron rotation
- `overflow-hidden` on content
- `pb-4`, `pt-0` on content inner padding
- `text-sm` on content text

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | AccordionItem bottom border color | `shadcn` |
| `--foreground` | Trigger text color | `shadcn` |
| `--muted-foreground` | Content text color | `shadcn` |
| `--radix-accordion-content-height` | Content height for open/close animation | Radix internal |
| `--radix-accordion-content-width` | Content width for open/close animation | Radix internal |

## Theme Support

### Component Structure

- Accordion Item variants: `Open=False, Hover=False` | `Open=False, Hover=True` | `Open=True, Hover=False` | `Open=True, Hover=True`
- Composed Accordion example (3 items, all collapsed)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable / Tailwind Utility | Purpose |
|-------|--------------------------------|---------|
| `var(--foreground)` | `--foreground` / `text-foreground` | Trigger text color |
| `var(--border)` | `--border` / `border-border` | Item separator border |
| `font-sans` | `--font-sans` / `font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Trigger font weight |
| `leading-6` (24px) | `leading-6` | Trigger line height |
| `text-base` (16px) | `text-base` | Trigger font size |
| `py-4` (16px) | `py-4` | Trigger vertical padding |
| `h-4` (16px) | `h-4` / `w-4` | Chevron icon size |

### Theme Behavior

- `--foreground` and `--border` change between light and dark modes — values are set in the theme file
- All spacing, typography, and layout tokens are mode-independent and consistent across themes
- The chevron icon (Lucide `chevron-down`) inherits `--foreground`, so it adapts automatically
- Background is transparent — inherits from parent container
