# Component: Slider

> **TL;DR:** A range input for selecting numeric values. 4 rendered parts: Slider (root), Track, Range, Thumb. Built on Radix Slider. Supports horizontal and vertical orientations, multiple thumbs, and disabled state. Track uses `bg-muted`, range fill uses `bg-primary`, thumb is a bordered circle with focus ring. Used for volume controls, price ranges, and numeric filters.

## Metadata

- **Component Name:** Slider
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-slider`

## Behavior

A range input that allows users to select one or more numeric values by dragging thumbs along a track.

- Root uses `touch-none select-none` to prevent scroll interference on touch devices
- Supports horizontal (default) and vertical orientations via `data-[orientation]`
- Vertical orientation uses `min-h-44 flex-col`
- Track is a rounded bar (`rounded-full bg-muted`): horizontal `h-1.5`, vertical `w-1.5`
- Range fills the track with `bg-primary`
- Thumb is `size-4 rounded-full border border-primary bg-white shadow-sm`
- Thumb shows `ring-ring/50` on hover and focus
- Multiple thumbs are supported — one thumb is rendered per value in the value array
- Disabled state applies `opacity-50` on the root via `data-[disabled]`
- Default `min` is `0`, default `max` is `100`

### Anti-Patterns

#### Do
- Use for adjusting values within a defined range
- For large ranges or precise values, include a companion input or select
- Ensure enough space between tick marks in stepped sliders
- Add reference values for intermediary points
- For ordinal sliders (None-Low-Medium-High), every step should have a reference value

#### Don't
- Don't use reference values for information imperative to selection (not always visible on small viewports)
- Don't use reference labels for small numeric sliders (1-5 with labels 2,3,4 clutters the interface)

Sources: shadCN, Radix UI

## API

### Parts

- `Slider` — Root container. Manages value state, orientation, and disabled state. Renders Track, Range, and Thumb(s).

Internal rendered parts (not separately exported):
- `Track` — Background bar the thumb slides along.
- `Range` — Filled portion of the track indicating the selected range.
- `Thumb` — Draggable handle for selecting a value. One per value in the array.

### Props

#### Slider

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| defaultValue | `number[]` | — | Initial uncontrolled value(s) |
| value | `number[]` | — | Controlled value(s) |
| min | `number` | `0` | Minimum value |
| max | `number` | `100` | Maximum value |
| step | `number` | `1` | Step increment |
| orientation | `"horizontal" \| "vertical"` | `"horizontal"` | Slider orientation |
| disabled | `boolean` | `false` | Whether the slider is disabled |
| className | `string` | — | Additional CSS classes |

Plus all additional Radix Slider Root props (`onValueChange`, `onValueCommit`, `name`, `inverted`, etc.).

### Variants

No named variants. Orientation is controlled via the `orientation` prop.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"slider"` | Slider (root) |
| `[data-slot]` | `"slider-track"` | Track |
| `[data-slot]` | `"slider-range"` | Range |
| `[data-slot]` | `"slider-thumb"` | Thumb |
| `[data-orientation]` | `"horizontal"`, `"vertical"` | Slider, Track, Range (Radix) |
| `[data-disabled]` | present when disabled | Slider (Radix) |

### CSS Variables (Radix)

Radix Slider does not expose CSS variables. Layout and fill are handled via Tailwind classes and data attributes.

## Composition Patterns

### Basic Slider

```tsx
<Slider defaultValue={[50]} max={100} step={1} />
```

### Range Slider (Two Thumbs)

```tsx
<Slider defaultValue={[25, 75]} max={100} step={1} />
```

### Vertical Slider

```tsx
<Slider
  defaultValue={[50]}
  max={100}
  orientation="vertical"
/>
```

### With Label

```tsx
<div className="flex flex-col gap-2">
  <label htmlFor="volume">Volume</label>
  <Slider id="volume" defaultValue={[70]} max={100} step={1} />
</div>
```

### Disabled

```tsx
<Slider defaultValue={[50]} max={100} disabled />
```

## Accessibility

- **WAI-ARIA Pattern:** Slider

### ARIA Roles

- Each Thumb renders with `role="slider"` (provided by Radix)
- `aria-valuemin`, `aria-valuemax`, `aria-valuenow` are set automatically by Radix
- `aria-orientation` reflects the current orientation
- `aria-disabled` is set when the slider is disabled

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `ArrowRight` / `ArrowUp` | Increase value by one step |
| `ArrowLeft` / `ArrowDown` | Decrease value by one step |
| `PageUp` | Increase value by a larger step |
| `PageDown` | Decrease value by a larger step |
| `Home` | Set value to minimum |
| `End` | Set value to maximum |

## HTML

### Standalone HTML Structure

```html
<span data-slot="slider" data-orientation="horizontal">
  <span data-slot="slider-track">
    <span data-slot="slider-range"></span>
  </span>
  <span data-slot="slider-thumb" role="slider"
        aria-valuemin="0" aria-valuemax="100" aria-valuenow="50"
        tabindex="0">
  </span>
</span>
```

## CSS

### Raw CSS

```css
/* Slider (root) */
.Slider {
  position: relative;
  display: flex;
  width: 100%;
  touch-action: none;
  user-select: none;
  align-items: center;
}

.Slider[data-orientation="vertical"] {
  min-height: 11rem;
  flex-direction: column;
}

.Slider[data-disabled] {
  opacity: 0.5;
}

/* Track */
.SliderTrack {
  position: relative;
  overflow: hidden;
  border-radius: 9999px;
  background-color: var(--muted);
  flex-grow: 1;
}

.SliderTrack[data-orientation="horizontal"] {
  width: 100%;
  height: 0.375rem;
}

.SliderTrack[data-orientation="vertical"] {
  width: 0.375rem;
  height: 100%;
}

/* Range */
.SliderRange {
  position: absolute;
  background-color: var(--primary);
}

.SliderRange[data-orientation="horizontal"] {
  height: 100%;
}

.SliderRange[data-orientation="vertical"] {
  width: 100%;
}

/* Thumb */
.SliderThumb {
  display: block;
  width: 1rem;
  height: 1rem;
  border-radius: 9999px;
  border: 1px solid var(--primary);
  background-color: white;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
}

.SliderThumb:hover,
.SliderThumb:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Slider (root) | `relative flex w-full touch-none select-none items-center data-[orientation=vertical]:min-h-44 data-[orientation=vertical]:flex-col data-[disabled]:opacity-50` | Root layout with orientation and disabled support |
| Track | `relative grow overflow-hidden rounded-full bg-muted data-[orientation=horizontal]:h-1.5 data-[orientation=horizontal]:w-full data-[orientation=vertical]:h-full data-[orientation=vertical]:w-1.5` | Track bar |
| Range | `absolute bg-primary data-[orientation=horizontal]:h-full data-[orientation=vertical]:w-full` | Filled range indicator |
| Thumb | `block size-4 rounded-full border border-primary bg-white shadow-sm hover:ring-ring/50 focus-visible:ring-ring/50` | Draggable handle |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | Track background color | `shadcn` |
| `--primary` | Range fill color and thumb border color | `shadcn` |
| `--ring` | Thumb focus/hover ring color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into range variants and a sub-component:

Slider:
| Variant Property | Values |
|-----------------|--------|
| Range | False, True |

.Slider Item (sub-component — thumb):
| Variant Property | Values |
|-----------------|--------|
| State | Enabled, Focus |

- Range=True adds a second .Slider Item thumb for the range endpoint

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--secondary` | `--secondary` | Track background |
| `--primary` | `--primary` | Range fill and thumb border |
| `--background` | `--background` | Thumb background |
| `h-1.5` (6px) | `height` | Track height |
| `size-4` (16px) | `width` / `height` | Thumb size |
| `rounded-full` | `border-radius` | Track and thumb rounding |
| `shadow-md` | `box-shadow` | Thumb shadow |

### Theme Behavior

- Track uses `--muted` — adapts to light/dark via theme
- Range and thumb border use `--primary` — adapts to light/dark via theme
- Thumb focus ring uses `--ring` — adapts to light/dark via theme
- Thumb background is fixed `bg-white` — consistent across themes
- Disabled opacity and all spacing tokens are mode-independent
