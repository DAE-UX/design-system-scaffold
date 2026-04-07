# Component: Resizable

> **TL;DR:** A set of resizable panels with draggable handles. 3 parts: ResizablePanelGroup, ResizablePanel, ResizableHandle. Depends on `react-resizable-panels`. Group uses flex with `aria-orientation` for vertical layout. Handle is 1px `bg-border` with optional grip handle (`withHandle` prop). Focus ring on handle.

## Metadata

- **Component Name:** Resizable
- **Source:** shadCN
- **Dependencies:** `react-resizable-panels`

## Behavior

A set of resizable panels separated by draggable handles, built on `react-resizable-panels`.

- ResizablePanelGroup renders as a flex container filling available space (`h-full w-full`)
- Vertical orientation is controlled via `aria-[orientation=vertical]:flex-col`
- ResizablePanel has no custom styling — just a `data-slot` attribute
- ResizableHandle renders as a 1px line using `bg-border`
- Handle has an `::after` pseudo-element for an expanded hit area
- Handle supports `focus-visible:ring-1 focus-visible:ring-ring` for keyboard accessibility
- Horizontal variant of the handle rotates for vertical panel groups
- Optional `withHandle` prop renders a visible grip indicator: a small `h-4 w-3` div with `bg-border`, `rounded-xs`, and a GripVerticalIcon (`size-2.5`)
- The grip handle has `z-10` to sit above the panel content

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `ResizablePanelGroup` — Root flex container for panels.
- `ResizablePanel` — Individual resizable panel.
- `ResizableHandle` — Draggable divider between panels with optional grip indicator.

### Props

#### ResizablePanelGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| direction | `"horizontal" \| "vertical"` | `"horizontal"` | Panel layout direction |
| className | `string` | — | Additional CSS classes |

#### ResizablePanel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| defaultSize | `number` | — | Default panel size (percentage) |
| minSize | `number` | — | Minimum panel size (percentage) |
| maxSize | `number` | — | Maximum panel size (percentage) |
| className | `string` | — | Additional CSS classes |

#### ResizableHandle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| withHandle | `boolean` | `false` | Show visible grip indicator |
| className | `string` | — | Additional CSS classes |

### Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| Handle (default) | 1px border line only | Minimal divider |
| Handle (`withHandle`) | 1px line with grip icon indicator | Visible drag affordance |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"resizable-panel-group"`, `"resizable-panel"`, `"resizable-handle"` | All parts |

### CSS Variables (Radix)

Not applicable — Resizable does not use Radix primitives.

## Composition Patterns

### Basic Horizontal Panels

```tsx
<ResizablePanelGroup direction="horizontal">
  <ResizablePanel defaultSize={50}>
    <div className="p-4">Left Panel</div>
  </ResizablePanel>
  <ResizableHandle />
  <ResizablePanel defaultSize={50}>
    <div className="p-4">Right Panel</div>
  </ResizablePanel>
</ResizablePanelGroup>
```

### Vertical Panels

```tsx
<ResizablePanelGroup direction="vertical">
  <ResizablePanel defaultSize={70}>
    <div className="p-4">Top Panel</div>
  </ResizablePanel>
  <ResizableHandle withHandle />
  <ResizablePanel defaultSize={30}>
    <div className="p-4">Bottom Panel</div>
  </ResizablePanel>
</ResizablePanelGroup>
```

### With Grip Handle

```tsx
<ResizablePanelGroup direction="horizontal">
  <ResizablePanel defaultSize={33}>
    <div className="p-4">Panel 1</div>
  </ResizablePanel>
  <ResizableHandle withHandle />
  <ResizablePanel defaultSize={34}>
    <div className="p-4">Panel 2</div>
  </ResizablePanel>
  <ResizableHandle withHandle />
  <ResizablePanel defaultSize={33}>
    <div className="p-4">Panel 3</div>
  </ResizablePanel>
</ResizablePanelGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** Separator (implicit via react-resizable-panels)

### ARIA Roles

`react-resizable-panels` provides built-in `separator` role on handles with `aria-orientation` and `aria-valuenow`/`aria-valuemin`/`aria-valuemax` for the current panel size.

### Keyboard Behavior

- `Tab` — Moves focus to the resize handle
- `Arrow Left` / `Arrow Right` — Resizes horizontal panels
- `Arrow Up` / `Arrow Down` — Resizes vertical panels
- `Home` — Collapses panel to minimum size
- `End` — Expands panel to maximum size
- `Enter` — Toggles panel collapse (if collapsible)

## HTML

### Standalone HTML Structure

```html
<div data-slot="resizable-panel-group" style="display: flex;">
  <div data-slot="resizable-panel">
    <div>Panel 1 content</div>
  </div>
  <div data-slot="resizable-handle" role="separator" tabindex="0">
    <!-- Optional grip handle -->
    <div>
      <svg><!-- GripVerticalIcon --></svg>
    </div>
  </div>
  <div data-slot="resizable-panel">
    <div>Panel 2 content</div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* ResizablePanelGroup */
.ResizablePanelGroup {
  display: flex;
  height: 100%;
  width: 100%;
}

.ResizablePanelGroup[aria-orientation="vertical"] {
  flex-direction: column;
}

/* ResizableHandle */
.ResizableHandle {
  position: relative;
  display: flex;
  width: 1px;
  align-items: center;
  justify-content: center;
  background-color: var(--border);
}

.ResizableHandle:focus-visible {
  box-shadow: 0 0 0 1px var(--ring);
}

/* ResizableHandle ::after (hit area) */
.ResizableHandle::after {
  content: "";
  position: absolute;
  /* expanded hit area */
}

/* Grip handle */
.ResizableHandleGrip {
  z-index: 10;
  display: flex;
  height: 1rem;
  width: 0.75rem;
  align-items: center;
  justify-content: center;
  border-radius: 0.125rem;
  border: 1px solid var(--border);
  background-color: var(--border);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ResizablePanelGroup | `flex h-full w-full aria-[orientation=vertical]:flex-col` | Flex container with orientation |
| ResizablePanel | (no custom classes) | Panel wrapper |
| ResizableHandle | `relative flex w-px items-center justify-center bg-border` | 1px divider line |
| ResizableHandle (focus) | `focus-visible:ring-1 focus-visible:ring-ring` | Focus ring |
| ResizableHandle (::after) | Pseudo-element for expanded hit area | Drag target area |
| Grip handle div | `z-10 flex h-4 w-3 items-center justify-center rounded-xs border bg-border` | Visible grip indicator |
| GripVerticalIcon | `size-2.5` | Grip icon size |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Handle line color and grip background/border | `shadcn` |
| `--ring` | Handle focus ring color | `shadcn` |

## Theme Support

### Component Structure

Component is organized into assembled examples and a sub-component:

Assembled examples (no variant properties — fixed compositions):
- Resizable Default — 2 horizontal panels + 1 vertical split (3 panels total), with grip handles
- Resizable Vertical — 2 vertical panels with grip handle
- Resizable Sidebar — sidebar + main content layout

.Resizable Handle (sub-component):
| Variant Property | Values |
|-----------------|--------|
| Orientation | Horizontal, Vertical |

- Root → Panel 01 + Handle (horizontal) + Left container (Panel 02 + Handle (vertical) + Panel 03)
- Root uses border, rounded with flex layout
- Panels use flex-1 with overflow clip, p 24px, centered text (foreground, text-xl, font-medium)
- Thumb is absolutely centered on the handle with z-10

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--border` | `--border` | Handle line, thumb background, root border |
| `--foreground` | `--foreground` | Panel text color |
| `--radius` | `--radius` | Root border radius |
| `p-6` (24px) | `padding` | Panel padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Panel text weight |
| `text-xl` | `font-size` | Panel text size |

### Theme Behavior

- Handle uses `--border` — adapts to light/dark via theme
- Grip indicator uses `--border` for background and border — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- All spacing and layout tokens are mode-independent
