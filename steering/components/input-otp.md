# Component: Input OTP

> **TL;DR:** A one-time password input with individual character slots. 4 parts: InputOTP, InputOTPGroup, InputOTPSlot, InputOTPSeparator. Built on the `input-otp` library. Each slot displays one character with active state highlighting and a fake caret animation. Supports grouping slots with separators. Error state via `aria-invalid`. Dark mode uses `bg-input/30` on slots.

## Metadata

- **Component Name:** Input OTP
- **Source:** shadCN (input-otp library)
- **Dependencies:** input-otp

## Behavior

A one-time password input that renders individual character slots, built on the `input-otp` library.

- InputOTP wraps `OTPInput` from `input-otp`; renders a hidden native input with visual slot overlays
- Container uses flex with `gap-2` and `has-disabled:opacity-50`
- InputOTPGroup groups adjacent slots visually (no gap between slots in a group)
- InputOTPSlot renders a single character cell; reads slot state from `OTPInputContext`
- Active slot shows `border-ring` with 3px ring via `data-active="true"`
- Each slot has `border-y` and `border-r`; first slot gets `border-l` and left rounding; last slot gets right rounding
- Fake caret: when slot is active and empty, renders an animated blinking cursor (`animate-caret-blink`, 1s duration)
- Error state: `aria-invalid` on slots triggers `border-destructive` and destructive ring
- InputOTPSeparator renders a minus icon between groups with `role="separator"`
- Supports `maxLength`, `pattern`, `onComplete` callback from `input-otp` library
- Dark mode: slots use `bg-input/30` background

### Anti-Patterns

#### Do
- Use for one-time password or verification code entry. Auto-focus the first slot. Auto-advance focus on input. Support paste of full code.

#### Don't
- Don't use for general text input. Don't make individual slots too small for touch targets. Don't omit the expected code length indicator.

Sources: shadCN, Radix UI

## API

### Parts

- `InputOTP` — Root. Wraps `OTPInput` with container styling.
- `InputOTPGroup` — Groups adjacent slots visually.
- `InputOTPSlot` — Individual character cell. Reads state from context.
- `InputOTPSeparator` — Visual separator between groups (minus icon).

### Props

#### InputOTP

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| maxLength | `number` | — | Total number of OTP characters |
| pattern | `string` | — | Regex pattern for allowed characters |
| onComplete | `(value: string) => void` | — | Called when all slots are filled |
| containerClassName | `string` | — | CSS classes for the container div |
| className | `string` | — | CSS classes for the hidden input |

#### InputOTPGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### InputOTPSlot

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| index | `number` | — | Slot index (0-based) |
| className | `string` | — | Additional CSS classes |

#### InputOTPSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

### Variants

No variants — Input OTP has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"input-otp"`, `"input-otp-group"`, `"input-otp-slot"`, `"input-otp-separator"` | All parts |
| `[data-active]` | `"true"` | InputOTPSlot (when focused) |

### CSS Variables (Radix)

Not applicable — Input OTP does not use Radix primitives.

## Composition Patterns

### Basic 6-Digit OTP

```tsx
<InputOTP maxLength={6}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
  </InputOTPGroup>
  <InputOTPSeparator />
  <InputOTPGroup>
    <InputOTPSlot index={3} />
    <InputOTPSlot index={4} />
    <InputOTPSlot index={5} />
  </InputOTPGroup>
</InputOTP>
```

### 4-Digit PIN

```tsx
<InputOTP maxLength={4}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
    <InputOTPSlot index={3} />
  </InputOTPGroup>
</InputOTP>
```

### With Pattern (Numbers Only)

```tsx
<InputOTP maxLength={6} pattern="^[0-9]+$">
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
    <InputOTPSlot index={3} />
    <InputOTPSlot index={4} />
    <InputOTPSlot index={5} />
  </InputOTPGroup>
</InputOTP>
```

## Accessibility

- **WAI-ARIA Pattern:** None — uses native input with visual overlay

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| InputOTP | hidden `input` element | Native input handles all ARIA; visual slots are presentational |
| InputOTPSlot | `aria-invalid` | Inherited from parent input when in error state |
| InputOTPSeparator | `role="separator"` | Visual divider between groups |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| 0-9 / a-z | Enter character in current slot, advance to next |
| Backspace | Delete character in current slot, move to previous |
| Arrow Left | Move to previous slot |
| Arrow Right | Move to next slot |
| Paste | Fill slots from clipboard content |

## HTML

### Standalone HTML Structure

```html
<div data-slot="input-otp">
  <!-- Hidden native input -->
  <input type="text" maxlength="6" autocomplete="one-time-code" />
  <!-- Visual slots -->
  <div data-slot="input-otp-group">
    <div data-slot="input-otp-slot" data-active="true">
      1
      <!-- Fake caret when empty and active -->
      <div class="caret"><div class="caret-line"></div></div>
    </div>
    <div data-slot="input-otp-slot">2</div>
    <div data-slot="input-otp-slot">3</div>
  </div>
  <div data-slot="input-otp-separator" role="separator">
    <svg><!-- minus icon --></svg>
  </div>
  <div data-slot="input-otp-group">
    <div data-slot="input-otp-slot"></div>
    <div data-slot="input-otp-slot"></div>
    <div data-slot="input-otp-slot"></div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* InputOTP container */
.InputOTP {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.InputOTP:has(:disabled) {
  opacity: 0.5;
}

/* InputOTPGroup */
.InputOTPGroup {
  display: flex;
  align-items: center;
}

/* InputOTPSlot */
.InputOTPSlot {
  position: relative;
  display: flex;
  height: 2.25rem;
  width: 2.25rem;
  align-items: center;
  justify-content: center;
  border-top: 1px solid var(--input);
  border-bottom: 1px solid var(--input);
  border-right: 1px solid var(--input);
  font-size: 0.875rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: all;
  outline: none;
}

.InputOTPSlot:first-child {
  border-left: 1px solid var(--input);
  border-top-left-radius: 0.375rem;
  border-bottom-left-radius: 0.375rem;
}

.InputOTPSlot:last-child {
  border-top-right-radius: 0.375rem;
  border-bottom-right-radius: 0.375rem;
}

/* Active slot */
.InputOTPSlot[data-active="true"] {
  z-index: 10;
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Error state */
.InputOTPSlot[aria-invalid="true"] {
  border-color: var(--destructive);
}

.InputOTPSlot[data-active="true"][aria-invalid="true"] {
  border-color: var(--destructive);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--destructive) 20%, transparent);
}

/* Fake caret */
.InputOTPSlot .caret-line {
  height: 1rem;
  width: 1px;
  background-color: var(--foreground);
  animation: caret-blink 1s infinite;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Container | `flex items-center gap-2 has-disabled:opacity-50` | OTP container |
| Group | `flex items-center` | Slot grouping |
| Slot | `relative flex h-9 w-9 items-center justify-center border-y border-r border-input text-sm shadow-xs transition-all outline-none first:rounded-l-md first:border-l last:rounded-r-md dark:bg-input/30` | Individual slot |
| Slot (active) | `data-[active=true]:z-10 data-[active=true]:border-ring data-[active=true]:ring-[3px] data-[active=true]:ring-ring/50` | Active highlight |
| Slot (error) | `aria-invalid:border-destructive data-[active=true]:aria-invalid:border-destructive data-[active=true]:aria-invalid:ring-destructive/20 dark:data-[active=true]:aria-invalid:ring-destructive/40` | Error state |
| Fake caret | `pointer-events-none absolute inset-0 flex items-center justify-center` | Caret container |
| Caret line | `h-4 w-px animate-caret-blink bg-foreground duration-1000` | Blinking cursor |
| Separator | `role="separator"` | Minus icon divider |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--input` | Slot border color | `shadcn` |
| `--ring` | Active slot border and ring | `shadcn` |
| `--destructive` | Error slot border and ring | `shadcn` |
| `--foreground` | Fake caret color | `shadcn` |

## Theme Support

### Component Structure

Input OTP sub-components:

| Variant Property | Values |
|-----------------|--------|
| Segments | One, Two, Three |
| State | Enabled, Destructive, Disabled |
| Alignment | Left, Center |

Individual slot types: Left, Center, Right — States: Default, Focus, Focus-Filled

Layout: optional Label + Input row (6× slots, no gap, shared borders) + optional Help text
Each slot: flex column, centered, 36×36px. Left slot gets left radius, right slot gets right radius.

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--input` | `--input` | Slot border color, background (with opacity) |
| `--foreground` | `--foreground` | Filled text color, label text |
| `--muted-foreground` | `--muted-foreground` | Help text color |
| `--radius` (derived) | `--radius` | Slot corner radius |
| `size-9` (36px) | `width` / `height` | Slot size |
| `font-sans` | `--font-sans` | Font family |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-xs` | `box-shadow` | Shadow |

### Theme Behavior

- Slot borders use `--input` — adapts to light/dark via theme
- Active ring uses `--ring` — adapts to light/dark via theme
- Error ring differs: `destructive/20` in light, `destructive/40` in dark
- Dark mode adds `bg-input/30` background to slots
- Fake caret uses `--foreground` — adapts to light/dark via theme
- All sizing and spacing tokens are mode-independent
