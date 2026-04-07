# Component: Toggle

> **TL;DR:** A two-state button for toggling between on and off. 1 part + CVA export: Toggle, toggleVariants. Built on Radix Toggle. Supports `default` and `outline` style variants, 3 sizes (`default`, `sm`, `lg`). Pressed state uses accent colors. Focus ring, disabled state, and icon auto-sizing included. Commonly used for bold/italic formatting, view mode switches, and feature toggles.

## Metadata

- **Component Name:** Toggle
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-toggle`, `class-variance-authority`

## Behavior

A two-state button that can be toggled between on (pressed) and off (not pressed).

- Built on Radix Toggle primitive — manages pressed/not-pressed state
- Renders as an inline-flex button with centered content
- Pressed state (`data-[state=on]`): uses accent background and accent-foreground text
- Unpressed hover: muted background and muted-foreground text
- Two style variants via CVA: `default` (transparent background) and `outline` (bordered with shadow)
- Three sizes: `default` (h-9 min-w-9), `sm` (h-8 min-w-8), `lg` (h-10 min-w-10)
- Focus: border-ring, ring-[3px] ring-ring/50
- Disabled: pointer-events-none opacity-50
- Error state via `aria-invalid`: border-destructive, ring-destructive/20 (dark: /40)
- Icons auto-sized: pointer-events-none, shrink-0, size-4 default
- `toggleVariants` is exported for reuse by ToggleGroup

### Anti-Patterns

#### Do
- Use to immediately trigger binary, mutually exclusive actions like saving or favoriting
- Always use an icon — outline for not-pressed, filled for pressed
- Maintain consistent label across all states

#### Don't
- Don't use for system-wide settings (light/dark mode) — use switch
- Don't use to change content formatting on a page
- Don't use for options requiring descriptions of both states
- Don't use in forms

Sources: shadCN, Radix UI

## API

### Parts

- `Toggle` — Two-state toggle button. Inline-flex with CVA variants for style and size.
- `toggleVariants` — Exported CVA variant function, reused by ToggleGroup.

### Props

#### Toggle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "outline"` | `"default"` | Visual style variant |
| size | `"default" \| "sm" \| "lg"` | `"default"` | Button size |
| pressed | `boolean` | — | Controlled pressed state |
| defaultPressed | `boolean` | — | Initial pressed state (uncontrolled) |
| onPressedChange | `(pressed: boolean) => void` | — | Callback when pressed state changes |
| disabled | `boolean` | `false` | Disables the toggle |
| className | `string` | — | Additional CSS classes |

### Variants

#### Style Variants (CVA)

| Variant | Description | Use Case |
|---------|-------------|----------|
| `variant="default"` | Transparent background, no border | Toolbar buttons, icon toggles |
| `variant="outline"` | Border with input color, shadow-xs, accent hover | Outlined toggle buttons |

#### Size Variants (CVA)

| Size | Dimensions | Use Case |
|------|-----------|----------|
| `default` | `h-9 min-w-9 px-2` | Standard toggle |
| `sm` | `h-8 min-w-8 px-1.5` | Compact toolbars |
| `lg` | `h-10 min-w-10 px-2.5` | Prominent toggles |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"toggle"` | Toggle |
| `[data-state]` | `"on"`, `"off"` | Toggle (from Radix) |

### CSS Variables (Radix)

Not applicable — Toggle does not expose Radix CSS variables.

## Composition Patterns

### Basic Toggle

```tsx
<Toggle aria-label="Toggle bold">
  <BoldIcon />
</Toggle>
```

### With Text

```tsx
<Toggle aria-label="Toggle italic">
  <ItalicIcon />
  Italic
</Toggle>
```

### Outline Variant

```tsx
<Toggle variant="outline" aria-label="Toggle underline">
  <UnderlineIcon />
</Toggle>
```

### Small Size

```tsx
<Toggle size="sm" aria-label="Toggle strikethrough">
  <StrikethroughIcon />
</Toggle>
```

### Controlled

```tsx
const [bold, setBold] = useState(false)

<Toggle pressed={bold} onPressedChange={setBold} aria-label="Toggle bold">
  <BoldIcon />
</Toggle>
```

### Disabled

```tsx
<Toggle disabled aria-label="Toggle bold">
  <BoldIcon />
</Toggle>
```

## Accessibility

- **WAI-ARIA Pattern:** [Toggle Button](https://www.w3.org/WAI/ARIA/apg/patterns/button/)

### ARIA Roles

- Renders as a `button` with `aria-pressed` reflecting the on/off state (provided by Radix)
- Always provide `aria-label` when the toggle contains only an icon

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Space` | Toggle pressed state |
| `Enter` | Toggle pressed state |

## HTML

### Standalone HTML Structure

```html
<button data-slot="toggle" data-state="off" aria-pressed="false">
  <svg><!-- icon --></svg>
</button>
```

## CSS

### Raw CSS

```css
/* Toggle — base */
.Toggle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  border-radius: 0.375rem;
  font-size: 0.875rem;
  font-weight: 500;
  white-space: nowrap;
  outline: none;
  transition: color 150ms, box-shadow 150ms;
}

.Toggle:hover {
  background-color: var(--muted);
  color: var(--muted-foreground);
}

/* Toggle — pressed */
.Toggle[data-state="on"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Toggle — focus */
.Toggle:focus-visible {
  border-color: var(--ring);
  outline: 3px solid color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Toggle — disabled */
.Toggle:disabled {
  pointer-events: none;
  opacity: 0.5;
}

/* Toggle — default variant */
.Toggle[data-variant="default"] {
  background-color: transparent;
}

/* Toggle — outline variant */
.Toggle[data-variant="outline"] {
  border: 1px solid var(--input);
  background-color: transparent;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.Toggle[data-variant="outline"]:hover {
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Toggle — default size */
.Toggle[data-size="default"] {
  height: 2.25rem;
  min-width: 2.25rem;
  padding-left: 0.5rem;
  padding-right: 0.5rem;
}

/* Toggle — sm size */
.Toggle[data-size="sm"] {
  height: 2rem;
  min-width: 2rem;
  padding-left: 0.375rem;
  padding-right: 0.375rem;
}

/* Toggle — lg size */
.Toggle[data-size="lg"] {
  height: 2.5rem;
  min-width: 2.5rem;
  padding-left: 0.625rem;
  padding-right: 0.625rem;
}

/* Toggle icons */
.Toggle svg {
  pointer-events: none;
  flex-shrink: 0;
  width: 1rem;
  height: 1rem;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Toggle (base) | `inline-flex items-center justify-center gap-2 rounded-md text-sm font-medium whitespace-nowrap transition-[color,box-shadow] outline-none` | Base layout |
| Toggle (hover) | `hover:bg-muted hover:text-muted-foreground` | Hover state |
| Toggle (pressed) | `data-[state=on]:bg-accent data-[state=on]:text-accent-foreground` | Pressed state |
| Toggle (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| Toggle (disabled) | `disabled:pointer-events-none disabled:opacity-50` | Disabled state |
| Toggle (error) | `aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40` | Error state |
| Toggle (default variant) | `bg-transparent` | Transparent background |
| Toggle (outline variant) | `border border-input bg-transparent shadow-xs hover:bg-accent hover:text-accent-foreground` | Bordered with shadow |
| Toggle (default size) | `h-9 min-w-9 px-2` | Standard dimensions |
| Toggle (sm size) | `h-8 min-w-8 px-1.5` | Compact dimensions |
| Toggle (lg size) | `h-10 min-w-10 px-2.5` | Large dimensions |
| Toggle (icons) | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4` | Icon sizing |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Hover background | `shadcn` |
| `--muted-foreground` | Hover text color | `shadcn` |
| `--accent` | Pressed background, outline hover background | `shadcn` |
| `--accent-foreground` | Pressed text color, outline hover text color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--input` | Outline variant border color | `shadcn` |
| `--destructive` | Error state border and ring color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into a variant matrix:

| Variant Property | Values |
|-----------------|--------|
| Outlined | False, True |
| State | Enabled, Hover, Focus, Disabled |
| Pressed | False, True |
| Size | Default, Small, Large |

- Toggle → Icon (Lucide icon, 16×16) + Label text (optional)
- Container: h 36px, rounded, px 8px, py 10px, gap 8px, inline-flex centered
- Label text: foreground, text-sm, font-medium
- Outlined=True adds border, shadow-xs
- Pressed=True uses accent background color
- Size=Small: h 28px, Size=Large: h 40px


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Label text color |
| `--muted` | `--muted` | Hover background |
| `--muted-foreground` | `--muted-foreground` | Hover text color |
| `--accent` | `--accent` | Pressed background |
| `--accent-foreground` | `--accent-foreground` | Pressed text color |
| `--input` | `--input` | Outlined variant border |
| `h-9` (36px) | `height` | Default size height |
| `--radius` (derived) | `border-radius` | Border radius |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Label font weight |
| `text-sm` (14px) | `font-size` | Label text size |

### Theme Behavior

- Hover uses `--muted` / `--muted-foreground` — adapts to light/dark via theme
- Pressed state uses `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Outline variant border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Shadow (`shadow-xs`) on outline variant is mode-independent
- All spacing, typography, and layout tokens are mode-independent
