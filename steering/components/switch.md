# Component: Switch

> **TL;DR:** A toggle switch built on Radix Switch primitive. 1 exported part with an internal thumb. Supports `default` and `sm` sizes via `data-size`. Checked state uses primary color, unchecked uses input color. Thumb translates horizontally on toggle. Focus ring, disabled state, and dark mode overrides included. Commonly used for boolean settings, feature toggles, and preference controls.

## Metadata

- **Component Name:** Switch
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-switch`

## Behavior

A toggle control for switching between on and off states.

- Built on Radix Switch primitive — manages checked/unchecked state and accessibility
- Root renders as an inline-flex button with a sliding thumb inside
- Thumb translates horizontally: `translate-x-0` (unchecked) to `translate-x-[calc(100%-2px)]` (checked)
- Supports two sizes: `default` (h-[1.15rem] w-8, thumb size-4) and `sm` (h-3.5 w-6, thumb size-3)
- Checked: root background uses primary color
- Unchecked: root background uses input color (with reduced opacity in dark mode)
- Focus: border changes to ring color, 3px ring with 50% opacity
- Disabled: cursor-not-allowed with 50% opacity
- Dark mode: checked thumb uses primary-foreground, unchecked thumb uses foreground
- Includes shadow-xs on root and ring-0 on thumb

### Anti-Patterns

#### Do
- Use for options that take effect immediately (e.g., toggling versioning on a bucket)
- Use for turning groups of elements on/off (progressive disclosure)
- Use when the on-state description is sufficient to understand implications

#### Don't
- Don't change the label depending on switch state
- Don't use for options activated at form submission (EULA, Terms)
- Don't use for options requiring descriptions of both on/off states
- Don't label as optional
- Don't use to toggle groups containing other toggles
- Don't hide the label in read-only state

Sources: shadCN, Radix UI

## API

### Parts

- `Switch` — Root toggle element with internal thumb. Inline-flex button with rounded-full track and sliding circular thumb.

### Props

#### Switch

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| size | `"default" \| "sm"` | `"default"` | Switch size variant |
| checked | `boolean` | — | Controlled checked state |
| defaultChecked | `boolean` | — | Initial checked state (uncontrolled) |
| onCheckedChange | `(checked: boolean) => void` | — | Callback when checked state changes |
| disabled | `boolean` | `false` | Disables the switch |
| required | `boolean` | `false` | Marks as required for form validation |
| name | `string` | — | Form field name |
| value | `string` | `"on"` | Form field value when checked |
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `size="default"` | Standard size (`h-[1.15rem] w-8`, thumb `size-4`) | General toggle controls |
| `size="sm"` | Compact size (`h-3.5 w-6`, thumb `size-3`) | Dense layouts, table rows |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"switch"`, `"switch-thumb"` | Root, Thumb |
| `[data-size]` | `"default"`, `"sm"` | Root |
| `[data-state]` | `"checked"`, `"unchecked"` | Root, Thumb (from Radix) |

### CSS Variables (Radix)

Not applicable — Switch does not expose Radix CSS variables.

## Composition Patterns

### Basic Switch

```tsx
<Switch />
```

### With Label

```tsx
<div className="flex items-center gap-2">
  <Switch id="airplane-mode" />
  <label htmlFor="airplane-mode">Airplane Mode</label>
</div>
```

### Small Size

```tsx
<div className="flex items-center gap-2">
  <Switch size="sm" id="notifications" />
  <label htmlFor="notifications">Enable notifications</label>
</div>
```

### Controlled

```tsx
const [enabled, setEnabled] = useState(false)

<Switch checked={enabled} onCheckedChange={setEnabled} />
```

### Disabled

```tsx
<Switch disabled />
```

## Accessibility

- **WAI-ARIA Pattern:** [Switch](https://www.w3.org/WAI/ARIA/apg/patterns/switch/)

### ARIA Roles

- Root renders as a `button` with `role="switch"` (provided by Radix)
- `aria-checked` reflects the checked/unchecked state (managed by Radix)

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Space` | Toggle checked state |
| `Enter` | Toggle checked state |

## HTML

### Standalone HTML Structure

```html
<button data-slot="switch" data-size="default" data-state="unchecked" role="switch" aria-checked="false">
  <span data-slot="switch-thumb" data-state="unchecked"></span>
</button>
```

## CSS

### Raw CSS

```css
/* Switch root */
.Switch {
  display: inline-flex;
  flex-shrink: 0;
  align-items: center;
  border-radius: 9999px;
  border: 2px solid transparent;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: all 150ms;
}

/* Switch — default size */
.Switch[data-size="default"] {
  height: 1.15rem;
  width: 2rem;
}

/* Switch — sm size */
.Switch[data-size="sm"] {
  height: 0.875rem;
  width: 1.5rem;
}

/* Switch — checked */
.Switch[data-state="checked"] {
  background-color: var(--primary);
}

/* Switch — unchecked */
.Switch[data-state="unchecked"] {
  background-color: var(--input);
}

/* Switch — focus */
.Switch:focus-visible {
  border-color: var(--ring);
  outline: 3px solid color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Switch — disabled */
.Switch:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* Switch thumb */
.SwitchThumb {
  display: block;
  border-radius: 9999px;
  background-color: var(--background);
  box-shadow: none;
  pointer-events: none;
  transition: transform 150ms;
}

/* Thumb — default size */
.Switch[data-size="default"] .SwitchThumb {
  width: 1rem;
  height: 1rem;
}

/* Thumb — sm size */
.Switch[data-size="sm"] .SwitchThumb {
  width: 0.75rem;
  height: 0.75rem;
}

/* Thumb — checked */
.SwitchThumb[data-state="checked"] {
  transform: translateX(calc(100% - 2px));
}

/* Thumb — unchecked */
.SwitchThumb[data-state="unchecked"] {
  transform: translateX(0);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Switch (root) | `inline-flex shrink-0 items-center rounded-full border-2 border-transparent shadow-xs transition-all` | Track container |
| Switch (default size) | `h-[1.15rem] w-8` | Standard dimensions |
| Switch (sm size) | `h-3.5 w-6` | Compact dimensions |
| Switch (checked) | `data-[state=checked]:bg-primary` | Checked background |
| Switch (unchecked) | `data-[state=unchecked]:bg-input dark:data-[state=unchecked]:bg-input/80` | Unchecked background |
| Switch (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| Switch (disabled) | `disabled:cursor-not-allowed disabled:opacity-50` | Disabled state |
| Thumb | `pointer-events-none block rounded-full bg-background ring-0 transition-transform` | Sliding indicator |
| Thumb (default size) | `size-4` | Standard thumb |
| Thumb (sm size) | `size-3` | Compact thumb |
| Thumb (checked) | `data-[state=checked]:translate-x-[calc(100%-2px)] dark:data-[state=checked]:bg-primary-foreground` | Checked position + dark color |
| Thumb (unchecked) | `data-[state=unchecked]:translate-x-0 dark:data-[state=unchecked]:bg-foreground` | Unchecked position + dark color |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--primary` | Checked track background | `shadcn` |
| `--input` | Unchecked track background | `shadcn` |
| `--background` | Thumb background color | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--primary-foreground` | Dark mode checked thumb color | `shadcn` |
| `--foreground` | Dark mode unchecked thumb color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into two sub-components with variant matrices:

**Switch (composite)**

| Variant Property | Values |
|-----------------|--------|
| Type | Default, Card |
| Orientation | Left, Right |
| State | Enabled, Disabled |
| Toggled | True, False |

**Toggle (sub-component)**

| Variant Property | Values |
|-----------------|--------|
| Toggled | True, False |
| State | Default, Hover, Focus, Disabled |

- Switch → Label area (text) + Toggle sub-component
- Toggle → Track (`rounded-full`, `w-9` 36px, `h-5` 20px) + Thumb (circle, 16×16, `rounded-full`)
- Toggled=True: Track uses `bg-primary`, Thumb translated to right
- Toggled=False: Track uses `bg-input`, Thumb at left position
- Track has `shadow-2xs`, `p-0.5` (2px) internal padding for thumb inset
- Card type wraps the switch in a bordered card container


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--primary` | `--primary` | Checked track background |
| `--input` | `--input` | Unchecked track background |
| `--background` | `--background` | Thumb background |
| `--foreground` | `--foreground` | Label text color |
| `--muted-foreground` | `--muted-foreground` | Description text color (Card type) |
| `--border` | `--border` | Card type border |
| `w-9` (36px) | `width` | Track width |
| `h-5` (20px) | `height` | Track height |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Label font weight |
| `text-sm` (14px) | `font-size` | Label text size |

### Theme Behavior

- Checked track uses `--primary` — adapts to light/dark via theme
- Unchecked track uses `--input` (with `/80` opacity in dark mode) — adapts to light/dark via theme
- Thumb uses `--background` in light mode, `--primary-foreground` (checked) / `--foreground` (unchecked) in dark mode
- Focus ring uses `--ring` — adapts to light/dark via theme
- Shadow (`shadow-xs`) and border-transparent are mode-independent
