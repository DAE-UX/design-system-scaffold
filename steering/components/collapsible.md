# Component: Collapsible

> **TL;DR:** An interactive component that expands and collapses a panel. 3 parts: Collapsible, CollapsibleTrigger, CollapsibleContent. Built on Radix UI Collapsible primitive. No built-in styling — provides only the show/hide behavior with data attributes for animation. Supports controlled and uncontrolled open state. Commonly used for settings panels, file trees, and expandable sections.

## Metadata

- **Component Name:** Collapsible
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

An interactive component that expands and collapses a panel of content.

- Wraps Radix UI Collapsible primitive with `data-slot` attributes
- No built-in styling — shadCN exports the Radix primitives directly with slot markers
- CollapsibleTrigger toggles the open/closed state
- CollapsibleContent is the panel that shows/hides
- Controlled mode: use `open` and `onOpenChange` props on Collapsible
- Uncontrolled mode: use `defaultOpen` prop
- Radix provides `data-state="open"` / `data-state="closed"` for CSS animations
- CollapsibleContent supports CSS animation via `--radix-collapsible-content-height` and `--radix-collapsible-content-width` variables
- CollapsibleTrigger can use `asChild` to render as any element (commonly a Button)
- Can be nested for tree structures (file trees, nested menus)
- Supports disabled state on the root to prevent toggling
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use for toggling visibility of a single content section.
- Provide a clear trigger that indicates expandable content.

#### Don't
- Don't use for multiple collapsible sections — use accordion instead.
- Don't hide critical information inside a collapsible.

Sources: shadCN, Radix UI

## API

### Parts

- `Collapsible` — Root container. Manages open/closed state. Renders a `div`.
- `CollapsibleTrigger` — Toggle button. Clicking toggles the content visibility. Renders a `button`.
- `CollapsibleContent` — Collapsible panel. Shows/hides based on open state. Renders a `div`.

### Props

#### Collapsible

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| open | `boolean` | — | Controlled open state |
| defaultOpen | `boolean` | `false` | Default open state (uncontrolled) |
| onOpenChange | `(open: boolean) => void` | — | Called when open state changes |
| disabled | `boolean` | `false` | Prevents toggling |
| className | `string` | — | Additional CSS classes |

#### CollapsibleTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| asChild | `boolean` | `false` | Render as child element instead of `<button>` |
| className | `string` | — | Additional CSS classes |

#### CollapsibleContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| forceMount | `boolean` | — | Force mount content (useful for animations) |
| className | `string` | — | Additional CSS classes |

### Variants

No style variants — Collapsible is an unstyled behavioral primitive. All visual styling is applied via className.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"collapsible"`, `"collapsible-trigger"`, `"collapsible-content"` | All parts |
| `[data-state]` | `"open"`, `"closed"` | Collapsible, CollapsibleTrigger, CollapsibleContent |
| `[data-disabled]` | present when disabled | Collapsible, CollapsibleTrigger |

### CSS Variables (Radix)

| Variable | Description | Applies To |
|----------|-------------|------------|
| `--radix-collapsible-content-height` | Height of the content when open | CollapsibleContent |
| `--radix-collapsible-content-width` | Width of the content when open | CollapsibleContent |

## Composition Patterns

### Basic

```tsx
<Collapsible>
  <CollapsibleTrigger>Toggle</CollapsibleTrigger>
  <CollapsibleContent>
    Content that can be shown or hidden.
  </CollapsibleContent>
</Collapsible>
```

### With Button Trigger

```tsx
<Collapsible open={isOpen} onOpenChange={setIsOpen} className="flex w-[350px] flex-col gap-2">
  <div className="flex items-center justify-between gap-4 px-4">
    <h4 className="text-sm font-semibold">@peduarte starred 3 repositories</h4>
    <CollapsibleTrigger asChild>
      <Button variant="ghost" size="icon" className="size-8">
        <ChevronsUpDown />
        <span className="sr-only">Toggle</span>
      </Button>
    </CollapsibleTrigger>
  </div>
  <div className="rounded-md border px-4 py-2 font-mono text-sm">
    @radix-ui/primitives
  </div>
  <CollapsibleContent className="flex flex-col gap-2">
    <div className="rounded-md border px-4 py-2 font-mono text-sm">
      @radix-ui/colors
    </div>
    <div className="rounded-md border px-4 py-2 font-mono text-sm">
      @stitches/react
    </div>
  </CollapsibleContent>
</Collapsible>
```

### Controlled

```tsx
const [open, setOpen] = React.useState(false)

<Collapsible open={open} onOpenChange={setOpen}>
  <CollapsibleTrigger>Toggle</CollapsibleTrigger>
  <CollapsibleContent>Content</CollapsibleContent>
</Collapsible>
```

### With Animation

```tsx
<CollapsibleContent className="overflow-hidden data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
  Content with slide animation
</CollapsibleContent>
```

### Nested (File Tree)

```tsx
<Collapsible>
  <CollapsibleTrigger>src/</CollapsibleTrigger>
  <CollapsibleContent className="pl-4">
    <Collapsible>
      <CollapsibleTrigger>components/</CollapsibleTrigger>
      <CollapsibleContent className="pl-4">
        <div>button.tsx</div>
        <div>card.tsx</div>
      </CollapsibleContent>
    </Collapsible>
  </CollapsibleContent>
</Collapsible>
```

## Accessibility

- **WAI-ARIA Pattern:** [Disclosure Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| CollapsibleTrigger | `button` (implicit) | Native button semantics |
| CollapsibleTrigger | `aria-expanded` | Automatically set by Radix based on open state |
| CollapsibleTrigger | `aria-controls` | Automatically set by Radix, references content ID |
| CollapsibleContent | `role="region"` | Implicit via Radix when content is visible |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Space | Toggles the collapsible open/closed |
| Enter | Toggles the collapsible open/closed |
| Tab | Moves focus to the next focusable element |
| Shift + Tab | Moves focus to the previous focusable element |

## HTML

### Standalone HTML Structure

```html
<div data-slot="collapsible" data-state="closed">
  <button data-slot="collapsible-trigger" data-state="closed" aria-expanded="false">
    Toggle
  </button>
  <div data-slot="collapsible-content" data-state="closed" hidden>
    Content that can be shown or hidden.
  </div>
</div>
```

### Open State

```html
<div data-slot="collapsible" data-state="open">
  <button data-slot="collapsible-trigger" data-state="open" aria-expanded="true">
    Toggle
  </button>
  <div data-slot="collapsible-content" data-state="open">
    Content that can be shown or hidden.
  </div>
</div>
```

## CSS

### Raw CSS

Collapsible has no built-in CSS from shadCN — it is an unstyled behavioral primitive. All styling is applied via className props. Common animation patterns:

```css
/* Collapsible animation (expand/collapse) */
@keyframes collapsible-down {
  from { height: 0; }
  to { height: var(--radix-collapsible-content-height); }
}

@keyframes collapsible-up {
  from { height: var(--radix-collapsible-content-height); }
  to { height: 0; }
}

.CollapsibleContent[data-state="open"] {
  animation: collapsible-down 200ms ease-out;
}

.CollapsibleContent[data-state="closed"] {
  animation: collapsible-up 200ms ease-out;
}
```

### Tailwind Mapping

Collapsible has no built-in Tailwind classes from shadCN. Common patterns used in composition:

| Pattern | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Animation (open) | `data-[state=open]:animate-collapsible-down` | Expand animation |
| Animation (closed) | `data-[state=closed]:animate-collapsible-up` | Collapse animation |
| Overflow | `overflow-hidden` | Required for height animation |

## CSS Variable Dependencies

Collapsible itself has no CSS variable dependencies. It is an unstyled primitive. Styling is applied via className and depends on the context where it is used.

| Variable | Purpose | Source |
|----------|---------|--------|
| `--radix-collapsible-content-height` | Content height for animations (set by Radix) | `radix` |
| `--radix-collapsible-content-width` | Content width for animations (set by Radix) | `radix` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Open | False, True |

Closed layout: flex column (gap 8px, width 350px) → Trigger row + CollapsibleItem(s)
Trigger: flex row (gap 16px, px 16px) → Title text (text-sm, font-semibold) + Toggle button (36×36px, chevrons-up-down icon)
Item: flex row (border, rounded, px 16px, py 12px) → Label text (text-sm)
Open state shows trigger + multiple CollapsibleItem children.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--foreground)` | `--foreground` | Title and item text color |
| `var(--input)` | `--input` | Item border color |
| `--radius` (derived) | `--radius` | Item and button border radius |
| `px-4` (16px) | `px-4` | Trigger padding, item horizontal padding |
| `py-3` (12px) | `py-3` | Item vertical padding |
| `font-sans` | `--font-sans` | Title font family |
| `font-semibold` (600) | `font-semibold` | Title font weight |
| `text-sm` (14px) | `text-sm` | Text size |

### Theme Behavior

- Collapsible is an unstyled behavioral primitive — no theme-dependent styling
- Visual appearance is entirely determined by className props applied in composition
- Radix CSS variables (`--radix-collapsible-content-height/width`) are layout values, not theme values
- Light/dark mode behavior depends on the styled content inside, not the Collapsible itself
