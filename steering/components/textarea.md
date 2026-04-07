# Component: Textarea

> **TL;DR:** A styled native textarea with auto-height via `field-sizing-content`. 1 part: Textarea. No external primitive — pure shadCN wrapper around the native `textarea` element. Includes focus ring, disabled state, error state via `aria-invalid`, responsive font sizing, and dark mode background override. Commonly used for multi-line text input in forms, comments, and descriptions.

## Metadata

- **Component Name:** Textarea
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A styled multi-line text input that auto-sizes to its content.

- Uses `field-sizing-content` CSS property for automatic height adjustment based on content
- Minimum height of `min-h-16` (4rem)
- Border with input color, transparent background (light mode), rounded-md corners
- Responsive font sizing: `text-base` on mobile, `md:text-sm` on medium breakpoints
- Includes `shadow-xs` for subtle depth
- Placeholder text uses muted foreground color
- Focus: border changes to ring color, 3px ring with 50% opacity
- Disabled: cursor-not-allowed with 50% opacity
- Error state via `aria-invalid`: border changes to destructive color, ring uses destructive at 20% opacity (40% in dark mode)
- Dark mode: background uses input color at 30% opacity (`bg-input/30`)

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Textarea` — Styled native `textarea` element with auto-height, focus ring, error state, and responsive font sizing.

### Props

#### Textarea

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |
| placeholder | `string` | — | Placeholder text |
| disabled | `boolean` | `false` | Disables the textarea |
| rows | `number` | — | Number of visible text rows (overrides auto-height) |
| aria-invalid | `boolean` | — | Marks the textarea as invalid (triggers error styling) |

All standard HTML `textarea` attributes are also accepted.

### Variants

No CVA variants — Textarea has a single visual style with state-based modifications (focus, disabled, error).

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"textarea"` | Textarea |

### CSS Variables (Radix)

Not applicable — Textarea does not use Radix primitives.

## Composition Patterns

### Basic Textarea

```tsx
<Textarea placeholder="Type your message here." />
```

### With Label

```tsx
<div className="grid gap-1.5">
  <label htmlFor="message">Your message</label>
  <Textarea id="message" placeholder="Type your message here." />
</div>
```

### Disabled

```tsx
<Textarea disabled placeholder="This textarea is disabled." />
```

### With Error State

```tsx
<Textarea aria-invalid placeholder="This field has an error." />
```

### In a Form

```tsx
<form>
  <div className="grid gap-1.5">
    <label htmlFor="bio">Bio</label>
    <Textarea id="bio" placeholder="Tell us about yourself." />
    <p className="text-sm text-muted-foreground">
      You can write up to 500 characters.
    </p>
  </div>
</form>
```

## Accessibility

- **WAI-ARIA Pattern:** None — native textarea element

### ARIA Roles

- Renders as a native `textarea` element — no additional ARIA roles required
- Associate with a `label` element via `id`/`htmlFor` for accessible naming
- Use `aria-invalid="true"` to communicate error state to assistive technologies
- Use `aria-describedby` to link to error messages or helper text

### Keyboard Behavior

No custom keyboard interaction — uses native textarea keyboard behavior. Text entry, selection, and navigation are handled by the browser.

## HTML

### Standalone HTML Structure

```html
<textarea data-slot="textarea" placeholder="Type your message here."></textarea>
```

## CSS

### Raw CSS

```css
/* Textarea */
.Textarea {
  field-sizing: content;
  min-height: 4rem;
  width: 100%;
  border-radius: 0.375rem;
  border: 1px solid var(--input);
  background-color: transparent;
  padding: 0.5rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  outline: none;
  transition: border-color 150ms, box-shadow 150ms;
}

@media (min-width: 768px) {
  .Textarea {
    font-size: 0.875rem;
  }
}

/* Textarea — placeholder */
.Textarea::placeholder {
  color: var(--muted-foreground);
}

/* Textarea — focus */
.Textarea:focus-visible {
  border-color: var(--ring);
  outline: 3px solid color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Textarea — disabled */
.Textarea:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* Textarea — error (aria-invalid) */
.Textarea[aria-invalid="true"] {
  border-color: var(--destructive);
  outline-color: color-mix(in srgb, var(--destructive) 20%, transparent);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Textarea | `field-sizing-content min-h-16 w-full rounded-md border border-input bg-transparent px-3 py-2 text-base shadow-xs outline-none md:text-sm` | Base textarea styling |
| Textarea (placeholder) | `placeholder:text-muted-foreground` | Placeholder color |
| Textarea (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| Textarea (disabled) | `disabled:cursor-not-allowed disabled:opacity-50` | Disabled state |
| Textarea (error) | `aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40` | Error state |
| Textarea (dark) | `dark:bg-input/30` | Dark mode background |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Border color, dark mode background (at 30% opacity) | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--muted-foreground` | Placeholder text color | `shadcn` |
| `--destructive` | Error state border and ring color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into state variants:

| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Filled, Focussed, Disabled, Error, Error Focus |

- Container: background, px 12px, py 10px, min-w 128px, max-w 512px, h 80px
- Placeholder text: muted-foreground, text-sm, font-normal
- Resize thumb: resize icon at bottom-right, opacity 0.6, 20px
- Focussed state adds focus ring
- Error state changes border to destructive
- Disabled state reduces opacity


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Border color and background (at 30% opacity) |
| `--muted-foreground` | `--muted-foreground` | Placeholder text color |
| `--foreground` | `--foreground` | Filled text color |
| `px-3` (12px) | `padding-inline` | Horizontal padding |
| `py-2.5` (10px) | `padding-block` | Vertical padding |
| `--radius` (derived) | `border-radius` | Border radius |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Placeholder uses `--muted-foreground` — adapts to light/dark via theme
- Error state uses `--destructive` with different ring opacities per mode (20% light, 40% dark)
- Dark mode background uses `--input` at 30% opacity (`bg-input/30`)
- Shadow (`shadow-xs`), border-radius, padding, and font sizing are mode-independent
