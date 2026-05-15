# Component: Avatar

> **TL;DR:** An image element with a fallback for representing the user. Built on Radix UI Avatar primitive via shadCN. 6 parts: Avatar, AvatarImage, AvatarFallback, AvatarBadge, AvatarGroup, AvatarGroupCount. Supports 3 sizes (sm, default, lg), badge indicators, and grouped/stacked layouts. Fallback renders when image fails to load or is absent. Optionally delay fallback to avoid content flashing.

## Metadata

- **Component Name:** Avatar
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** radix-ui

## Behavior

An image element with a fallback for representing the user. Displays a user photo when available, or initials/icon as fallback.

- Image renders only after it has loaded successfully (Radix manages loading state)
- Fallback renders when the image hasn't loaded (during loading or on error)
- Fallback can optionally delay rendering via `delayMs` to avoid content flashing on fast connections
- `onLoadingStatusChange` handler provides manual control over image loading state
- Supports 3 sizes: `sm` (24px), `default` (32px), `lg` (40px)
- Supports badge indicator via `AvatarBadge` (positioned bottom-right)
- Supports grouped/stacked layout via `AvatarGroup` with overlapping negative spacing
- `AvatarGroupCount` displays a count of additional avatars in a group
- Default shape is circular (`rounded-full`); can be overridden via `className` (e.g., `rounded-lg`)
- Supports RTL layout via the Direction component

### Anti-Patterns

#### Do
- Always provide the full name in a tooltip
- Ensure avatar size complements the icon size when using icons inside

#### Don't
- Don't use custom sizes that break alignment with adjacent text or components
- Don't omit fallback content (initials or placeholder) when image fails to load

## API

### Parts

- `Avatar` — Root container. Wraps `AvatarPrimitive.Root`. Sets size, shape, and overflow.
- `AvatarImage` — The user image. Renders only when loaded. Accepts standard `img` props.
- `AvatarFallback` — Renders when image hasn't loaded. Accepts any children (text, icon).
- `AvatarBadge` — Status indicator badge positioned at bottom-right of the avatar.
- `AvatarGroup` — Container for multiple avatars with overlapping layout.
- `AvatarGroupCount` — Count indicator for additional avatars in a group.

### Props

#### Avatar

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm" \| "lg"` | `"default"` | Avatar size: `sm` = 24px, `default` = 32px, `lg` = 40px |
| className | `string` | — | Additional CSS classes (e.g., `rounded-lg` for square shape) |
| asChild | `boolean` | `false` | Merge props onto child element |

#### AvatarImage

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| src | `string` | — | Image source URL |
| alt | `string` | — | Alt text for the image |
| className | `string` | — | Additional CSS classes |
| asChild | `boolean` | `false` | Merge props onto child element |
| onLoadingStatusChange | `(status: "idle" \| "loading" \| "loaded" \| "error") => void` | — | Callback for image loading state changes |

#### AvatarFallback

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |
| asChild | `boolean` | `false` | Merge props onto child element |
| delayMs | `number` | — | Delay in ms before rendering fallback (avoids flash on fast connections) |

#### AvatarBadge

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes (e.g., custom badge color) |

#### AvatarGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### AvatarGroupCount

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="sm"` | 24px avatar, 2px badge, xs fallback text | Compact lists, inline mentions |
| `size="default"` | 32px avatar, 2.5px badge, sm fallback text | Standard user display |
| `size="lg"` | 40px avatar, 3px badge, sm fallback text | Profile headers, prominent display |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"avatar"`, `"avatar-image"`, `"avatar-fallback"`, `"avatar-badge"`, `"avatar-group"`, `"avatar-group-count"` | All parts |
| `[data-size]` | `"default" \| "sm" \| "lg"` | Avatar (root) |

### CSS Variables (Radix)

No component-specific Radix CSS variables.

## Composition Patterns

### Basic Avatar (Image + Fallback)

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
</Avatar>
```

### Square Shape

```tsx
<Avatar className="rounded-lg">
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
</Avatar>
```

### With Badge

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
  <AvatarBadge />
</Avatar>
```

### Badge with Custom Color

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
  <AvatarBadge className="bg-green-600 dark:bg-green-800" />
</Avatar>
```

### Badge with Icon

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback>CN</AvatarFallback>
  <AvatarBadge>
    <CheckIcon />
  </AvatarBadge>
</Avatar>
```

### Avatar Group (Stacked)

```tsx
<AvatarGroup>
  <Avatar>
    <AvatarImage src="..." alt="User 1" />
    <AvatarFallback>U1</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarImage src="..." alt="User 2" />
    <AvatarFallback>U2</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarImage src="..." alt="User 3" />
    <AvatarFallback>U3</AvatarFallback>
  </Avatar>
</AvatarGroup>
```

### Avatar Group with Count

```tsx
<AvatarGroup>
  <Avatar>
    <AvatarImage src="..." alt="User 1" />
    <AvatarFallback>U1</AvatarFallback>
  </Avatar>
  <Avatar>
    <AvatarImage src="..." alt="User 2" />
    <AvatarFallback>U2</AvatarFallback>
  </Avatar>
  <AvatarGroupCount>+5</AvatarGroupCount>
</AvatarGroup>
```

### Sizes

```tsx
<Avatar size="sm">
  <AvatarImage src="..." alt="Small" />
  <AvatarFallback>SM</AvatarFallback>
</Avatar>

<Avatar size="default">
  <AvatarImage src="..." alt="Default" />
  <AvatarFallback>DF</AvatarFallback>
</Avatar>

<Avatar size="lg">
  <AvatarImage src="..." alt="Large" />
  <AvatarFallback>LG</AvatarFallback>
</Avatar>
```

### With Tooltip

```tsx
<Tooltip>
  <TooltipTrigger>
    <Avatar>
      <AvatarImage src="..." alt="@shadcn" />
      <AvatarFallback>CN</AvatarFallback>
    </Avatar>
  </TooltipTrigger>
  <TooltipContent>@shadcn</TooltipContent>
</Tooltip>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Avatar is a presentational component

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| AvatarImage | `img` (implicit) | Must have `alt` text describing the user |
| AvatarFallback | Presentational | Fallback text (initials) is visible but not a semantic role |

### Keyboard Behavior

No keyboard interaction — Avatar is a static display component. When used as a trigger (e.g., inside a DropdownMenu or Tooltip), the parent component handles keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<span data-size="default" data-slot="avatar">
  <img src="..." alt="User name" data-slot="avatar-image" />
  <span data-slot="avatar-fallback">CN</span>
</span>
```

### With Badge

```html
<span data-size="default" data-slot="avatar">
  <img src="..." alt="User name" data-slot="avatar-image" />
  <span data-slot="avatar-fallback">CN</span>
  <span data-slot="avatar-badge"></span>
</span>
```

### Avatar Group

```html
<div data-slot="avatar-group">
  <span data-size="default" data-slot="avatar">
    <img src="..." alt="User 1" />
    <span>U1</span>
  </span>
  <span data-size="default" data-slot="avatar">
    <img src="..." alt="User 2" />
    <span>U2</span>
  </span>
  <div data-slot="avatar-group-count">+3</div>
</div>
```

## CSS

### Raw CSS

```css
/* Avatar root */
.Avatar {
  position: relative;
  display: flex;
  width: 2rem;
  height: 2rem;
  flex-shrink: 0;
  overflow: hidden;
  border-radius: 9999px;
  user-select: none;
}

/* Size variants */
.Avatar[data-size="sm"] { width: 1.5rem; height: 1.5rem; }
.Avatar[data-size="lg"] { width: 2.5rem; height: 2.5rem; }

/* Image */
.AvatarImage {
  aspect-ratio: 1 / 1;
  width: 100%;
  height: 100%;
}

/* Fallback */
.AvatarFallback {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
  border-radius: 9999px;
  background-color: var(--muted);
  font-size: 0.875rem;
  color: var(--muted-foreground);
}

.Avatar[data-size="sm"] .AvatarFallback {
  font-size: 0.75rem;
}

/* Badge */
.AvatarBadge {
  position: absolute;
  right: 0;
  bottom: 0;
  z-index: 10;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 9999px;
  background-color: var(--primary);
  color: var(--primary-foreground);
  outline: 2px solid var(--background);
  user-select: none;
}

/* Badge sizes */
.Avatar[data-size="sm"] .AvatarBadge { width: 0.5rem; height: 0.5rem; }
.Avatar[data-size="default"] .AvatarBadge { width: 0.625rem; height: 0.625rem; }
.Avatar[data-size="lg"] .AvatarBadge { width: 0.75rem; height: 0.75rem; }

/* Group */
.AvatarGroup {
  display: flex;
  margin-left: -0.5rem;
}

.AvatarGroup > .Avatar {
  outline: 2px solid var(--background);
}

/* Group count */
.AvatarGroupCount {
  position: relative;
  display: flex;
  width: 2rem;
  height: 2rem;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  border-radius: 9999px;
  background-color: var(--muted);
  font-size: 0.875rem;
  color: var(--muted-foreground);
  outline: 2px solid var(--background);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Avatar | `relative flex size-8 shrink-0 overflow-hidden rounded-full select-none` | Base layout and shape |
| Avatar (sm) | `data-[size=sm]:size-6` | 24px size |
| Avatar (lg) | `data-[size=lg]:size-10` | 40px size |
| AvatarImage | `aspect-square size-full` | Fill container |
| AvatarFallback | `flex size-full items-center justify-center rounded-full bg-muted text-sm text-muted-foreground` | Centered fallback with muted styling |
| AvatarFallback (sm) | `group-data-[size=sm]/avatar:text-xs` | Smaller text for sm size |
| AvatarBadge | `absolute right-0 bottom-0 z-10 inline-flex items-center justify-center rounded-full bg-primary text-primary-foreground ring-2 ring-background select-none` | Positioned badge indicator |
| AvatarBadge (sm) | `group-data-[size=sm]/avatar:size-2 group-data-[size=sm]/avatar:[&>svg]:hidden` | 8px badge, hide icon |
| AvatarBadge (default) | `group-data-[size=default]/avatar:size-2.5 group-data-[size=default]/avatar:[&>svg]:size-2` | 10px badge, 8px icon |
| AvatarBadge (lg) | `group-data-[size=lg]/avatar:size-3 group-data-[size=lg]/avatar:[&>svg]:size-2` | 12px badge, 8px icon |
| AvatarGroup | `flex -space-x-2 *:data-[slot=avatar]:ring-2 *:data-[slot=avatar]:ring-background` | Overlapping layout with ring |
| AvatarGroupCount | `relative flex size-8 shrink-0 items-center justify-center rounded-full bg-muted text-sm text-muted-foreground ring-2 ring-background` | Count indicator matching avatar size |
| AvatarGroupCount (sm) | `group-has-data-[size=sm]/avatar-group:size-6` | Match sm avatar size |
| AvatarGroupCount (lg) | `group-has-data-[size=lg]/avatar-group:size-10` | Match lg avatar size |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Fallback background, group count background | `shadcn` |
| `--muted-foreground` | Fallback text color, group count text color | `shadcn` |
| `--primary` | Badge background color | `shadcn` |
| `--primary-foreground` | Badge icon/text color | `shadcn` |
| `--background` | Badge ring color, group avatar ring color | `shadcn` |

## Theme Support

### Component Structure

The component contains two component sets:

**Avatar** — 3 variant properties:

| Variant Property | Values |
|-----------------|--------|
| Type | Image, Fallback |
| Size | 2X Small (20px), X Small (24px), Small (32px), Medium (40px), Large (48px), X-Large (56px), 2X-Large (64px), 3X-Large (80px), 4X-Large (96px), 5X-Large (112px), 6X-Large (128px) |
| Shape | Circle, Rounded rectangle |

Total: 44 variant combinations (2 types × 11 sizes × 2 shapes)

**Avatar Group** — 1 variant property:

| Variant Property | Values |
|-----------------|--------|
| Size | 2X Small, X Small, Small, Medium, Large |

### CSS Variable Mapping



| Token | CSS Variable / Tailwind Utility | Purpose |
|-------------|--------------------------------|---------|
| `var(--muted)` | `--muted` / `bg-muted` | Fallback background |
| `var(--foreground)` | `--foreground` / `text-foreground` | Fallback initials text color |
| `var(--background)` | `--background` / `ring-background` | Avatar group ring color |
| `border-radius` | `rounded-full` | Avatar shape (circle) |
| `width` | `size-10` (Tailwind `40px`) | Medium avatar size |
| `border-width` | `ring-2` (approx `1.33px`) | Group avatar ring width |
| `--font-sans` | `--font-sans` / `font-sans` | Font family |
| `font-normal` | `font-normal` (Tailwind `400`) | Fallback text weight |
| `text-sm` | `text-sm` (Tailwind `14px`) | Fallback text size (Medium) |
| `leading-5` | `leading-5` (Tailwind `20px`) | Fallback text line height |

### Theme Behavior

- Fallback uses `--muted` / `--muted-foreground` — adapts to light/dark via theme
- Badge uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Badge and group rings use `--background` to create visual separation — adapts to light/dark via theme
- Image has no theme dependency — displays the source image as-is
- Custom badge colors (e.g., `bg-green-600 dark:bg-green-800`) are applied at the usage site
- All sizing is fixed per variant (not theme-dependent)
