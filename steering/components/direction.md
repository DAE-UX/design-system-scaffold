# Component: Direction

> **TL;DR:** A provider component that sets the text direction (LTR/RTL) for the application. 2 exports: `DirectionProvider` (component) and `useDirection` (hook). Built on Radix UI Direction primitive. No visual output — provides context for all Radix-based components to respect text direction. Essential for supporting right-to-left languages (Arabic, Hebrew, Persian).

## Metadata

- **Component Name:** Direction
- **Source:** shadCN
- **Dependencies:** (none — uses Radix Direction utility, bundled with radix-ui)

## Behavior

A provider component that sets the text direction for the application, enabling RTL support.

- DirectionProvider wraps the application (or a subtree) and sets the `dir` context
- Accepts `dir` or `direction` prop (both map to Radix's `dir` prop; `direction` is a convenience alias)
- All Radix-based components within the provider automatically respect the direction
- `useDirection` hook returns the current direction value from context
- No visual output — purely a context provider
- Should be paired with `dir="rtl"` on the `<html>` element for full RTL support
- Can be nested to create mixed-direction layouts (e.g., RTL page with LTR code blocks)

### Anti-Patterns

#### Do
- Use to set text direction (LTR/RTL) for internationalization.
- Apply at the highest appropriate level in the component tree.

#### Don't
- Don't mix directions within the same content block without clear visual separation.

Sources: shadCN, Radix UI

## API

### Parts

- `DirectionProvider` — Context provider. Sets text direction for all descendant Radix components.

### Props

#### DirectionProvider

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| dir | `"ltr" \| "rtl"` | — | Text direction (Radix native prop) |
| direction | `"ltr" \| "rtl"` | — | Text direction (convenience alias for `dir`) |
| children | `React.ReactNode` | — | Child components |

### Hooks

#### useDirection

```tsx
const direction = useDirection()
// Returns: "ltr" | "rtl"
```

Returns the current text direction from the nearest DirectionProvider context.

### Variants

Not applicable — Direction is a context provider with no visual variants.

### Data Attributes

Not applicable — Direction does not render DOM elements.

### CSS Variables (Radix)

Not applicable — Direction does not use CSS variables.

## Composition Patterns

### Application-Level RTL

```tsx
<html dir="rtl">
  <body>
    <DirectionProvider direction="rtl">
      {/* All components respect RTL */}
    </DirectionProvider>
  </body>
</html>
```

### Using the Hook

```tsx
function MyComponent() {
  const direction = useDirection()
  return <div>Current direction: {direction}</div>
}
```

### Mixed Direction

```tsx
<DirectionProvider direction="rtl">
  {/* RTL content */}
  <DirectionProvider direction="ltr">
    {/* LTR island within RTL page */}
    <code>const x = 1</code>
  </DirectionProvider>
</DirectionProvider>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Direction is a utility provider, not an interactive component

### ARIA Roles

Not applicable — Direction does not render DOM elements. RTL support is achieved via the `dir` attribute on the `<html>` element and Radix's internal direction context.

### Keyboard Behavior

Not applicable — Direction is not interactive.

## HTML

### Standalone HTML Structure

Direction does not render any HTML. The RTL behavior is achieved via:

```html
<html dir="rtl">
  <body>
    <!-- Content automatically flows RTL -->
  </body>
</html>
```

## CSS

### Raw CSS

Not applicable — Direction is a context provider with no CSS output.

### Tailwind Mapping

Not applicable — Direction has no Tailwind classes. RTL-aware Tailwind utilities (e.g., `rtl:`, `ltr:`) can be used in conjunction with the Direction provider.

## CSS Variable Dependencies

Not applicable — Direction does not use or depend on any CSS variables.

## Theme Support

### Component Structure

EMPTY: None recorded.

### CSS Variable Mapping

EMPTY: None recorded.

### Theme Behavior

- Direction is theme-independent — it controls text flow, not visual styling
- All theme variables work identically in LTR and RTL modes
- Components using Direction automatically mirror their layout (padding, margins, icons) based on the direction context
