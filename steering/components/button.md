# Component: Button

> **TL;DR:** A clickable element that triggers an action. Single part (`Button`) with 6 style variants (default, destructive, outline, secondary, ghost, link) and 8 size variants (default, xs, sm, lg, icon, icon-xs, icon-sm, icon-lg). Uses CVA for variant management. Supports `asChild` via Radix Slot for rendering as links or other elements. Accepts icons and spinners as children. Tailwind v4 uses `cursor: default` — add custom CSS for `cursor: pointer`.

## Metadata

- **Component Name:** Button
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui (Slot only, for `asChild`), class-variance-authority

## Behavior

A clickable element that triggers an action or event. The most fundamental interactive component in the design system.

- 6 style variants: `default`, `destructive`, `outline`, `secondary`, `ghost`, `link`
- 8 size variants: `default` (36px), `xs` (24px), `sm` (32px), `lg` (40px), `icon` (36px square), `icon-xs` (24px square), `icon-sm` (32px square), `icon-lg` (40px square)
- Renders as `button` by default; renders as child element when `asChild` is true
- Disabled state: `pointer-events: none` and `opacity: 0.5`
- Focus-visible ring for keyboard navigation
- Supports `aria-invalid` state for error indication
- SVG icons inside buttons are automatically sized (16px default, 12px for xs)
- Tailwind v4 changed default cursor to `cursor: default` — add custom CSS for `cursor: pointer` if desired
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Use buttons for actions, links for navigation
- Use primary button for the most recommended action
- Limit to one primary button per page

#### Don't
- Don't hide buttons — disable them when action is unavailable
- Don't disable submit buttons in multi-field forms — use validation on submit instead
- Don't use icons for decoration — only when text is ambiguous

Sources: shadCN, Radix UI

## API

### Parts

- `Button` — Single-part component. Renders a `button` (or child element via `asChild`). Uses CVA for variant and size styling.

### Props

#### Button

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "destructive" \| "outline" \| "secondary" \| "ghost" \| "link"` | `"default"` | Visual style variant |
| size | `"default" \| "xs" \| "sm" \| "lg" \| "icon" \| "icon-xs" \| "icon-sm" \| "icon-lg"` | `"default"` | Button size |
| asChild | `boolean` | `false` | Render as child element instead of `<button>` |
| className | `string` | — | Additional CSS classes |
| disabled | `boolean` | `false` | Disables the button |

### Variants

#### Style Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `default` | Primary background with primary foreground text | Primary actions, CTAs |
| `destructive` | Destructive background with white text | Delete, remove, dangerous actions |
| `outline` | Border with background, accent hover | Secondary actions, form controls |
| `secondary` | Secondary background with secondary foreground text | Alternative actions |
| `ghost` | Transparent, accent hover | Toolbar buttons, minimal UI |
| `link` | Primary text with underline on hover | Inline link-style buttons |

#### Size Variants

| Size | Height | Padding | Icon Size | Use Case |
|------|--------|---------|-----------|----------|
| `default` | 36px (`h-9`) | `px-4 py-2` | 16px | Standard buttons |
| `xs` | 24px (`h-6`) | `px-2` | 12px | Compact UI, inline actions |
| `sm` | 32px (`h-8`) | `px-3` | 16px | Smaller buttons |
| `lg` | 40px (`h-10`) | `px-6` | 16px | Prominent buttons |
| `icon` | 36px square | — | 16px | Icon-only buttons |
| `icon-xs` | 24px square | — | 12px | Compact icon buttons |
| `icon-sm` | 32px square | — | 16px | Small icon buttons |
| `icon-lg` | 40px square | — | 16px | Large icon buttons |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"button"` | Button |
| `[data-variant]` | Style variant value | Button |
| `[data-size]` | Size variant value | Button |

### CSS Variables (Radix)

Not applicable — Button does not use Radix primitives (only Radix Slot for `asChild`).

## Composition Patterns

### Basic Variants

```tsx
<Button>Default</Button>
<Button variant="outline">Outline</Button>
<Button variant="secondary">Secondary</Button>
<Button variant="ghost">Ghost</Button>
<Button variant="destructive">Destructive</Button>
<Button variant="link">Link</Button>
```

### Sizes

```tsx
<Button size="xs">Extra Small</Button>
<Button size="sm">Small</Button>
<Button size="default">Default</Button>
<Button size="lg">Large</Button>
```

### Icon Only

```tsx
<Button variant="outline" size="icon" aria-label="Submit">
  <ArrowUpIcon />
</Button>
```

### With Icon

```tsx
<Button>
  <GitBranchIcon />
  Create Branch
</Button>
```

### With Spinner (Loading)

```tsx
<Button disabled>
  <Spinner />
  Loading...
</Button>
```

### As Link (asChild)

```tsx
<Button asChild>
  <Link href="/docs">Documentation</Link>
</Button>
```

## Accessibility

- **WAI-ARIA Pattern:** [Button Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button)

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Button | `button` (implicit) | Native button semantics |
| Button (icon-only) | `button` + `aria-label` required | Must provide accessible name |
| Button (disabled) | `disabled` attribute | Native disabled state |
| Button (invalid) | `aria-invalid` | Triggers destructive ring styling |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Space | Activates the button |
| Enter | Activates the button |
| Tab | Moves focus to the next focusable element |
| Shift + Tab | Moves focus to the previous focusable element |

## HTML

### Standalone HTML Structure

```html
<button data-slot="button" data-variant="default" data-size="default">
  Button text
</button>
```

### Icon Only

```html
<button data-slot="button" data-variant="outline" data-size="icon" aria-label="Submit">
  <svg><!-- icon --></svg>
</button>
```

## CSS

### Raw CSS

```css
.Button {
  display: inline-flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  border-radius: var(--radius);
  font-size: 0.875rem;
  font-weight: 500;
  white-space: nowrap;
  transition: all;
  outline: none;
}
.Button svg { pointer-events: none; flex-shrink: 0; }
.Button svg:not([class*='size-']) { width: 1rem; height: 1rem; }
.Button:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}
.Button:disabled { pointer-events: none; opacity: 0.5; }
.Button[data-variant="default"] { background-color: var(--primary); color: var(--primary-foreground); }
.Button[data-variant="default"]:hover { background-color: color-mix(in srgb, var(--primary) 90%, transparent); }
.Button[data-variant="destructive"] { background-color: var(--destructive); color: white; }
.Button[data-variant="outline"] { border: 1px solid var(--border); background-color: var(--background); }
.Button[data-variant="outline"]:hover { background-color: var(--accent); color: var(--accent-foreground); }
.Button[data-variant="secondary"] { background-color: var(--secondary); color: var(--secondary-foreground); }
.Button[data-variant="ghost"]:hover { background-color: var(--accent); color: var(--accent-foreground); }
.Button[data-variant="link"] { color: var(--primary); text-underline-offset: 4px; }
.Button[data-variant="link"]:hover { text-decoration: underline; }
.Button[data-size="default"] { height: 2.25rem; padding: 0.5rem 1rem; }
.Button[data-size="xs"] { height: 1.5rem; gap: 0.25rem; padding: 0 0.5rem; font-size: 0.75rem; }
.Button[data-size="sm"] { height: 2rem; gap: 0.375rem; padding: 0 0.75rem; }
.Button[data-size="lg"] { height: 2.5rem; padding: 0 1.5rem; }
.Button[data-size="icon"] { width: 2.25rem; height: 2.25rem; }
.Button[data-size="icon-xs"] { width: 1.5rem; height: 1.5rem; }
.Button[data-size="icon-sm"] { width: 2rem; height: 2rem; }
.Button[data-size="icon-lg"] { width: 2.5rem; height: 2.5rem; }
```

### Tailwind Mapping

| Element / Variant | Tailwind Classes | Purpose |
|-------------------|-----------------|---------|
| Button (base) | `inline-flex shrink-0 items-center justify-center gap-2 rounded-md text-sm font-medium whitespace-nowrap transition-all outline-none` | Base layout |
| default | `bg-primary text-primary-foreground hover:bg-primary/90` | Primary colors |
| destructive | `bg-destructive text-white hover:bg-destructive/90` | Destructive colors |
| outline | `border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground` | Border with hover |
| secondary | `bg-secondary text-secondary-foreground hover:bg-secondary/80` | Secondary colors |
| ghost | `hover:bg-accent hover:text-accent-foreground` | Transparent with hover |
| link | `text-primary underline-offset-4 hover:underline` | Link style |
| size: default | `h-9 px-4 py-2 has-[>svg]:px-3` | 36px height |
| size: xs | `h-6 gap-1 rounded-md px-2 text-xs has-[>svg]:px-1.5` | 24px height |
| size: sm | `h-8 gap-1.5 rounded-md px-3 has-[>svg]:px-2.5` | 32px height |
| size: lg | `h-10 rounded-md px-6 has-[>svg]:px-4` | 40px height |
| size: icon | `size-9` | 36px square |
| size: icon-xs | `size-6 rounded-md` | 24px square |
| size: icon-sm | `size-8` | 32px square |
| size: icon-lg | `size-10` | 40px square |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--primary` | Default variant background, link variant text | `shadcn` |
| `--primary-foreground` | Default variant text | `shadcn` |
| `--destructive` | Destructive variant background, invalid state ring | `shadcn` |
| `--background` | Outline variant background | `shadcn` |
| `--border` | Outline variant border (light mode) | `shadcn` |
| `--input` | Outline variant border/background (dark mode) | `shadcn` |
| `--secondary` | Secondary variant background | `shadcn` |
| `--secondary-foreground` | Secondary variant text | `shadcn` |
| `--accent` | Outline/ghost hover background | `shadcn` |
| `--accent-foreground` | Outline/ghost hover text | `shadcn` |
| `--ring` | Focus-visible ring | `shadcn` |
| `--radius` | Border radius | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Secondary, Destructive, Outline, Ghost, Link |
| State | Enabled, Hover, Focus, Loading, Disabled, Active |
| Size | Default (36px), Small (32px), Large (40px), Icon (36×36), Icon Small (32×32), Icon Large (40×40) |

Default layout: LeftIcon (optional, 16×16) + Label (font-medium, text-sm) + Kbd (optional) + RightIcon (optional, 16×16)

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--primary` | `--primary` | Default variant background |
| `--primary-foreground` | `--primary-foreground` | Default variant text color |
| `--muted` | `--muted` | Kbd background |
| `--muted-foreground` | `--muted-foreground` | Kbd text color |
| `h-9` (36px) | `height` | Default size height |
| `--radius` (derived) | `--radius` | Border radius |
| `px-4` (16px) | `padding-x` | Horizontal padding (default) |
| `gap-2` (8px) | `gap` | Gap between icon/label/kbd |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Label font weight |
| `text-sm` (14px) | `font-size` | Label font size |
| `shadow-2xs` | `box-shadow` | Button shadow |

### Theme Behavior

- Default variant uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Destructive variant uses `--destructive` with `text-white`; in dark mode uses reduced opacity
- Outline variant differs between modes: light uses `--border` / `--background`, dark uses `--input`
- Secondary variant uses `--secondary` / `--secondary-foreground` — adapts to light/dark via theme
- Ghost variant is transparent; hover uses `--accent`
- Link variant uses `--primary` text — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- All sizing is fixed per variant (not theme-dependent)
- Cursor behavior: Tailwind v4 defaults to `cursor: default` — add custom CSS for `cursor: pointer` if desired
