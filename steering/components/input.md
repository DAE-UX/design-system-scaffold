# Component: Input

> **TL;DR:** A single-line text input. 1 part: Input. No external primitive — renders a native `input` element. Supports all native input types. Focus ring with `border-ring` and 3px ring. Error state via `aria-invalid`. File input has inline-flex styling. Selection uses primary colors. Dark mode uses `bg-input/30`. Responsive text: `text-base` on mobile, `text-sm` on `md`.

## Metadata

- **Component Name:** Input
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A single-line text input that renders a native HTML `input` element with consistent styling.

- Renders as a native `input` with `h-9` height and full width
- Supports all native input types (`text`, `email`, `password`, `number`, `file`, etc.)
- Focus state: `border-ring` with 3px ring using `ring-ring/50`
- Error state: `aria-invalid` triggers `border-destructive` with destructive ring
- File input: file button renders as `inline-flex` with `h-7`, transparent background, `text-sm`, `font-medium`
- Text selection uses `--primary` background and `--primary-foreground` text
- Placeholder uses `--muted-foreground` color
- Disabled state: `pointer-events-none`, `cursor-not-allowed`, `opacity-50`
- Responsive text size: `text-base` on mobile, `text-sm` on `md` breakpoint
- Dark mode: background uses `bg-input/30`
- Minimum width: `min-w-0` to prevent overflow in flex containers

### Anti-Patterns

#### Do
- Use for single-line text entry. Pair with a visible label. Use appropriate `type` attribute (email, password, number, etc.) for built-in validation and mobile keyboard optimization.

#### Don't
- Don't use for multi-line text — use textarea. Don't omit labels (placeholder is not a label). Don't use `type="number"` for non-numeric data like phone numbers or zip codes.

Sources: shadCN, Radix UI

## API

### Parts

- `Input` — Single input element. Renders a native `input`.

### Props

#### Input

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| type | `string` | — | HTML input type |
| className | `string` | — | Additional CSS classes |

All native `input` props are supported (e.g., `placeholder`, `disabled`, `value`, `onChange`, `aria-invalid`, `name`, `required`, `pattern`, `min`, `max`, `step`).

### Variants

No variants — Input has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"input"` | Input |

### CSS Variables (Radix)

Not applicable — Input does not use Radix primitives.

## Composition Patterns

### Basic Input

```tsx
<Input type="text" placeholder="Enter your name" />
```

### With Label

```tsx
<div className="grid gap-2">
  <Label htmlFor="email">Email</Label>
  <Input id="email" type="email" placeholder="you@example.com" />
</div>
```

### File Input

```tsx
<Input type="file" />
```

### Disabled

```tsx
<Input disabled placeholder="Disabled input" />
```

### With Error

```tsx
<Input aria-invalid="true" placeholder="Invalid input" />
```

## Accessibility

- **WAI-ARIA Pattern:** None — uses native `input` element semantics

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Input | native `input` | All native input ARIA attributes supported |
| Input | `aria-invalid` | Triggers error styling when `true` |

### Keyboard Behavior

Standard native input keyboard behavior. No custom keyboard handling.

## HTML

### Standalone HTML Structure

```html
<input data-slot="input" type="text" placeholder="Enter your name" />
```

## CSS

### Raw CSS

```css
/* Input */
.Input {
  height: 2.25rem;
  width: 100%;
  min-width: 0;
  border-radius: 0.375rem;
  border: 1px solid var(--input);
  background-color: transparent;
  padding: 0.25rem 0.75rem;
  font-size: 1rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: color, box-shadow;
  outline: none;
}

@media (min-width: 768px) {
  .Input {
    font-size: 0.875rem;
  }
}

/* Selection */
.Input::selection {
  background-color: var(--primary);
  color: var(--primary-foreground);
}

/* Placeholder */
.Input::placeholder {
  color: var(--muted-foreground);
}

/* Focus */
.Input:focus-visible {
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Error */
.Input[aria-invalid="true"] {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

/* Disabled */
.Input:disabled {
  pointer-events: none;
  cursor: not-allowed;
  opacity: 0.5;
}

/* File input */
.Input::file-selector-button {
  display: inline-flex;
  height: 1.75rem;
  border: 0;
  background-color: transparent;
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Input | `h-9 w-full min-w-0 rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-xs transition-[color,box-shadow] outline-none md:text-sm dark:bg-input/30` | Base input |
| Input (selection) | `selection:bg-primary selection:text-primary-foreground` | Text selection |
| Input (placeholder) | `placeholder:text-muted-foreground` | Placeholder text |
| Input (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| Input (error) | `aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40` | Error state |
| Input (disabled) | `disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50` | Disabled state |
| Input (file) | `file:inline-flex file:h-7 file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground` | File button |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Border color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--destructive` | Error border and ring color | `shadcn` |
| `--primary` | Text selection background | `shadcn` |
| `--primary-foreground` | Text selection text color | `shadcn` |
| `--muted-foreground` | Placeholder text color | `shadcn` |
| `--foreground` | File button text color | `shadcn` |

## Theme Support

### Component Structure

| Variant Property | Values |
|-----------------|--------|
| Type | Text, File, Password |
| State | Enabled, Focus, Filled, Disabled |
| Error | False, True |

Input layout: flex column (gap, px 12px, py 8px), rounded, shadow-xs, background
Placeholder text: text-sm, muted-foreground

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Border color, background (with opacity) |
| `--muted-foreground` | `--muted-foreground` | Placeholder text color |
| `--foreground` | `--foreground` | Filled text color |
| `--radius` (derived) | `--radius` | Border radius |
| `px-3` (12px) | `padding-inline` | Horizontal padding |
| `py-2` (8px) | `padding-block` | Vertical padding |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-xs` | `box-shadow` | Input shadow |

### Theme Behavior

- Border uses `--input` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Error ring differs: `destructive/20` in light, `destructive/40` in dark
- Dark mode adds `bg-input/30` background
- Selection uses `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Placeholder uses `--muted-foreground` — adapts to light/dark via theme
- Responsive text size (`text-base` → `md:text-sm`) is mode-independent
