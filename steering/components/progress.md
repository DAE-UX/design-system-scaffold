# Component: Progress

> **TL;DR:** A horizontal bar indicating completion progress. 2 parts: Progress (root), ProgressIndicator. Built on Radix UI primitive. Root is `h-2` rounded-full with `bg-primary/20`. Indicator translates X based on value. Simple component.

## Metadata

- **Component Name:** Progress
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `radix-ui`

## Behavior

A horizontal bar that visually indicates the completion progress of a task or process, built on Radix UI's Progress primitive.

- Root renders as a rounded full-width bar with `bg-primary/20` background (20% opacity of primary)
- Root height is `h-2` (0.5rem) with `overflow-hidden` and `rounded-full`
- ProgressIndicator fills the bar from left to right based on the `value` prop
- Indicator uses `translateX` to animate: `translateX(-${100 - (value || 0)}%)`
- Indicator has `transition-all` for smooth value changes
- Indicator uses `bg-primary` for the filled portion
- When `value` is `0` or `undefined`, the indicator is fully translated left (hidden)
- When `value` is `100`, the indicator fills the entire bar

### Anti-Patterns

#### Do
- Use for operations longer than 10 seconds with foreseeable completion
- Place contextually near the related element
- Display matching result state on completion
- Provide retry button for transient errors

#### Don't
- Don't use for indeterminate actions
- Don't use for operations with separate manual steps
- Don't use multiple progress bars for one operation
- Don't display success before reaching 100%

Sources: shadCN, Radix UI

## API

### Parts

- `Progress` — Root container. Rounded bar with primary background at 20% opacity.
- `ProgressIndicator` — Fill indicator. Translates horizontally based on value.

### Props

#### Progress

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `number` | `0` | Current progress value (0–100) |
| max | `number` | `100` | Maximum progress value |
| className | `string` | — | Additional CSS classes |

### Variants

No named variants — Progress has a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"progress"`, `"progress-indicator"` | All parts |
| `[data-state]` | `"loading"`, `"complete"`, `"indeterminate"` | Progress (via Radix) |

### CSS Variables (Radix)

Not applicable — Progress does not use custom Radix CSS variables.

## Composition Patterns

### Basic Progress

```tsx
<Progress value={33} />
```

### With Label

```tsx
<div className="flex flex-col gap-2">
  <div className="flex justify-between text-sm">
    <span>Uploading...</span>
    <span>33%</span>
  </div>
  <Progress value={33} />
</div>
```

### Complete State

```tsx
<Progress value={100} />
```

## Accessibility

- **WAI-ARIA Pattern:** Progressbar (implicit via Radix)

### ARIA Roles

Radix UI Progress provides built-in `progressbar` role with `aria-valuenow`, `aria-valuemin`, and `aria-valuemax` attributes.

### Keyboard Behavior

No keyboard interaction — Progress is a read-only indicator. It is not focusable or interactive.

## HTML

### Standalone HTML Structure

```html
<div data-slot="progress" role="progressbar" aria-valuenow="33" aria-valuemin="0" aria-valuemax="100">
  <div data-slot="progress-indicator" style="transform: translateX(-67%)"></div>
</div>
```

## CSS

### Raw CSS

```css
/* Progress */
.Progress {
  position: relative;
  height: 0.5rem;
  width: 100%;
  overflow: hidden;
  border-radius: 9999px;
  background-color: color-mix(in srgb, var(--primary) 20%, transparent);
}

/* ProgressIndicator */
.ProgressIndicator {
  height: 100%;
  width: 100%;
  flex: 1;
  background-color: var(--primary);
  transition: all;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Progress | `relative h-2 w-full overflow-hidden rounded-full bg-primary/20` | Track bar |
| ProgressIndicator | `h-full w-full flex-1 bg-primary transition-all` | Fill indicator |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--primary` | Indicator fill color and track background (at 20% opacity) | `shadcn` |

## Theme Support

### Component Structure

Component organizes Progress into percentage-based variant states:

| Variant Property | Values |
|-----------------|--------|
| Progress | 0%, 10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90%, 100% |

- Track uses secondary background with rounded-full border radius
- Indicator uses primary background with left-side rounded corners matching track

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--secondary` | `--secondary` | Track background |
| `--primary` | `--primary` | Indicator fill |
| `h-2` (8px) | `height` | Track height |
| `rounded-full` | `border-radius` | Track and indicator rounding |

### Theme Behavior

- Track uses `--primary` at 20% opacity — adapts to light/dark via theme
- Indicator uses `--primary` — adapts to light/dark via theme
- All spacing and border-radius tokens are mode-independent
