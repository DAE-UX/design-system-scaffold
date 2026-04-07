# Component: Toast

> **TL;DR:** A general-purpose toast notification component. Less opinionated than Sonner — provides the visual structure (container, title, description, action button, close button) without prescribing a specific toast library. 2 variants: Default and Error (destructive). Supports optional title, description, action button, and close button (visible on hover). Use for transient feedback messages where Sonner's opinionated API is not desired.

## Metadata

- **Component Name:** Toast
- **Dependencies:** None (visual component only; pair with a toast management library of choice)

## Behavior

A transient notification that appears temporarily to communicate a succinct message. The Toast is the general-purpose visual structure; Sonner (`components/sonner.md`) is the opinionated React implementation that wraps the Sonner library with theme integration.

- Appears at a configurable viewport position (typically bottom-right)
- Auto-dismisses after a configurable duration
- Close button appears on hover (positioned top-right corner)
- Optional action button allows the user to respond (e.g., "Undo")
- Error variant uses destructive styling for critical messages
- Multiple toasts stack vertically

### Relationship to Sonner

| Aspect | Toast (this component) | Sonner (`components/sonner.md`) |
|--------|----------------------|--------------------------------|
| Purpose | General visual structure | Opinionated React toast system |
| Library | None (bring your own) | `sonner` library |
| Theme integration | CSS variables directly | `next-themes` via `useTheme()` |
| Icon overrides | None (no icons by default) | Custom Lucide icons per type |
| Toast types | Default, Error | Success, Info, Warning, Error, Loading |
| API | Render-based (JSX) | Function-based (`toast()`, `toast.success()`) |

Use Toast when you need a library-agnostic toast visual or are building a custom toast system. Use Sonner when you want a batteries-included React toast solution.

### Anti-Patterns

| Anti-Pattern | Why It Fails | Correct Approach |
|-------------|-------------|-----------------|
| Using Toast for persistent messages | Toasts are transient — users may miss them | Use Alert or Dialog for persistent messages |
| Stacking more than 3-5 toasts simultaneously | Overwhelms the user, obscures content | Limit visible toasts; queue excess |
| Using Error variant for non-critical messages | Dilutes the urgency signal | Reserve destructive styling for actual errors |
| Omitting the close button | Users cannot dismiss unwanted toasts | Always include close button (visible on hover) |
| Placing action buttons for complex workflows | Toast disappears before user can decide | Use Dialog for multi-step decisions |

## API

### Parts

- `Toast` — Root container. Flex row layout with content area and optional action button.
- `ToastTitle` — Title text. Semibold weight.
- `ToastDescription` — Description text. Normal weight, 90% opacity.
- `ToastAction` — Action button (e.g., "Undo"). Outline style for default, border style for error.
- `ToastClose` — Close button. Positioned absolute top-right, visible on hover.

### Props

#### Toast

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "destructive"` | `"default"` | Visual variant |
| className | `string` | — | Additional CSS classes |

#### ToastAction

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| altText | `string` | — | Accessible description of the action for screen readers |

### Variants

| Variant | Background | Border | Text Color | Action Button |
|---------|-----------|--------|------------|---------------|
| `default` | `--background` | `--border` (1px solid) | `--foreground` | Outline style (`--input` border, `--foreground` text) |
| `destructive` | `--destructive` | `--destructive` (1px solid) | `--destructive-foreground` | `--destructive` bg, `--muted` border, `--destructive-foreground` text |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `data-state` | `"open"`, `"closed"` | Toast (animation state) |
| `data-swipe` | `"start"`, `"move"`, `"cancel"`, `"end"` | Toast (swipe-to-dismiss) |

### CSS Variables (Radix)

Not applicable — Toast does not use Radix primitives directly.

## Composition Patterns

### Basic Toast Structure

```tsx
<Toast>
  <div className="flex flex-1 flex-col gap-1">
    <ToastTitle>Scheduled: Catch up</ToastTitle>
    <ToastDescription>
      Friday, February 10, 2023 at 5:57 PM
    </ToastDescription>
  </div>
  <ToastAction altText="Undo scheduling">Undo</ToastAction>
  <ToastClose />
</Toast>
```

### Error Toast

```tsx
<Toast variant="destructive">
  <div className="flex flex-1 flex-col gap-1">
    <ToastTitle>Error</ToastTitle>
    <ToastDescription>
      Something went wrong. Please try again.
    </ToastDescription>
  </div>
  <ToastAction altText="Try again">Try again</ToastAction>
  <ToastClose />
</Toast>
```

### Title Only (No Description)

```tsx
<Toast>
  <div className="flex flex-1 flex-col gap-1">
    <ToastTitle>Changes saved</ToastTitle>
  </div>
</Toast>
```

### With Toast Provider (Library Integration)

When paired with a toast management library (e.g., Radix Toast, custom context):

```tsx
// Provider wraps the app
<ToastProvider>
  {children}
  <ToastViewport />
</ToastProvider>

// Trigger from anywhere
function SaveButton() {
  const { toast } = useToast()
  return (
    <Button onClick={() => toast({
      title: "Saved",
      description: "Your changes have been saved.",
      action: <ToastAction altText="Undo">Undo</ToastAction>,
    })}>
      Save
    </Button>
  )
}
```

## Accessibility

- **WAI-ARIA Pattern:** Alert (status messages) — `https://www.w3.org/WAI/ARIA/apg/patterns/alert/`

### ARIA Roles

| Element | Role/Attribute | Purpose |
|---------|---------------|---------|
| Toast container | `role="status"`, `aria-live="polite"` | Announces toast to screen readers (non-urgent) |
| Error toast | `role="alert"`, `aria-live="assertive"` | Immediately announces error toasts |
| ToastAction | `altText` prop → `aria-label` | Describes the action for screen readers |
| ToastClose | `aria-label="Close"` | Labels the close button |
| Toast viewport | `role="region"`, `aria-label="Notifications"` | Groups all toasts |

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `Tab` | Moves focus to the next interactive element within the toast (action button, close button) |
| `Escape` | Dismisses the focused toast |
| `Enter` / `Space` | Activates the focused action or close button |

## HTML

### Standalone HTML Structure

```html
<!-- Toast viewport -->
<ol role="region" aria-label="Notifications" tabindex="-1">
  <!-- Default toast -->
  <li role="status" aria-live="polite" class="toast">
    <div class="toast-content">
      <p class="toast-title">Scheduled: Catch up</p>
      <p class="toast-description">Friday, February 10, 2023 at 5:57 PM</p>
    </div>
    <button class="toast-action">Undo</button>
    <button class="toast-close" aria-label="Close">
      <svg><!-- X icon --></svg>
    </button>
  </li>

  <!-- Error toast -->
  <li role="alert" aria-live="assertive" class="toast toast-destructive">
    <div class="toast-content">
      <p class="toast-title">Error</p>
      <p class="toast-description">Something went wrong.</p>
    </div>
    <button class="toast-action">Try again</button>
    <button class="toast-close" aria-label="Close">
      <svg><!-- X icon --></svg>
    </button>
  </li>
</ol>
```

## CSS

### Raw CSS

```css
/* Toast container — default */
.toast {
  display: flex;
  gap: var(--spacing-4, 16px);
  align-items: center;
  overflow: hidden;
  padding: var(--spacing-6, 24px);
  padding-right: var(--spacing-8, 32px);
  border-radius: calc(var(--radius) - 2px);
  box-shadow:
    0px 4px 6px -4px rgba(26, 26, 26, 0.05),
    0px 10px 15px -3px rgba(26, 26, 26, 0.05);
  width: 388px;
  position: relative;
  background-color: var(--background);
  border: 1px solid var(--border);
  color: var(--foreground);
}

/* Toast container — destructive */
.toast-destructive {
  background-color: var(--destructive);
  border-color: var(--destructive);
  color: var(--destructive-foreground);
}

/* Toast content area */
.toast-content {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: var(--spacing-1, 4px);
  min-height: 1px;
  min-width: 1px;
}

/* Toast title */
.toast-title {
  font-family: var(--font-sans);
  font-weight: 600;
  font-size: var(--text-sm, 14px);
  line-height: var(--leading-5, 20px);
}

/* Toast description */
.toast-description {
  font-family: var(--font-sans);
  font-weight: 400;
  font-size: var(--text-sm, 14px);
  line-height: var(--leading-5, 20px);
  opacity: 0.9;
}

/* Toast action button — default */
.toast-action {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-2, 8px);
  height: 36px;
  padding: var(--spacing-2, 8px) var(--spacing-4, 16px);
  border-radius: calc(var(--radius) - 2px);
  font-family: var(--font-sans);
  font-weight: 500;
  font-size: var(--text-sm, 14px);
  line-height: var(--leading-5, 20px);
  white-space: nowrap;
  background-color: var(--background);
  border: 1px solid var(--input);
  color: var(--foreground);
  box-shadow: 0px 1px 2px 0px rgba(26, 26, 26, 0.05);
  cursor: pointer;
}

/* Toast action button — destructive */
.toast-destructive .toast-action {
  background-color: var(--destructive);
  border-color: var(--muted);
  color: var(--destructive-foreground);
}

/* Toast close button */
.toast-close {
  position: absolute;
  top: 3px;
  right: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-1, 4px);
  border-radius: calc(var(--radius) - 4px);
  opacity: 0;
  cursor: pointer;
}

.toast:hover .toast-close {
  opacity: 0.5;
}

.toast-close:hover {
  opacity: 1;
}

.toast-close:focus-visible {
  opacity: 1;
  box-shadow: 0px 0px 0px 2px var(--background), 0px 0px 0px 4px var(--ring);
}

.toast-destructive .toast-close {
  background-color: var(--destructive);
}

.toast-destructive .toast-close:focus-visible {
  box-shadow: 0px 0px 0px 3px rgba(220, 38, 38, 0.4);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Toast (default) | `flex gap-4 items-center overflow-hidden pl-6 pr-8 py-6 rounded-[calc(var(--radius)-2px)] shadow-lg relative bg-background border border-border text-foreground w-[388px]` | Container |
| Toast (destructive) | `bg-destructive border-destructive text-destructive-foreground` | Error variant |
| Content | `flex flex-1 flex-col gap-1 min-h-px min-w-px` | Content area |
| Title | `font-semibold text-sm leading-5` | Title text |
| Description | `font-normal text-sm leading-5 opacity-90` | Description text |
| Action (default) | `flex items-center justify-center gap-2 h-9 px-4 py-2 rounded-[calc(var(--radius)-2px)] font-medium text-sm leading-5 whitespace-nowrap bg-background border border-input text-foreground shadow-2xs` | Action button |
| Action (destructive) | `bg-destructive border-muted text-destructive-foreground` | Error action button |
| Close | `absolute top-[3px] right-[3px] flex items-center justify-center p-1 rounded-[calc(var(--radius)-4px)] opacity-0 group-hover:opacity-50 hover:opacity-100 focus-visible:opacity-100` | Close button |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Default toast background, action button background | `shadcn` |
| `--foreground` | Default toast text, action button text | `shadcn` |
| `--border` | Default toast border | `shadcn` |
| `--input` | Default action button border | `shadcn` |
| `--destructive` | Error toast background and border, error close button background | `shadcn` |
| `--destructive-foreground` | Error toast text, error action button text | `shadcn` |
| `--muted` | Error action button border | `shadcn` |
| `--ring` | Default close button focus ring | `shadcn` |
| `--radius` | Border radius (derived: `calc(var(--radius)-2px)` for toast, `calc(var(--radius)-4px)` for close) | `shadcn` |
| `--font-sans` | Font family | `shadcn` |

## Theme Support

### Component Structure

Source: shadCN

The Toast component page contains the Toast variants, close button states, and light/dark examples.

#### Toast Variants

| Variant Property | Values |
|-----------------|--------|
| Error | `False` (default), `True` (destructive) |

Both variants share the same layout structure:
- Container: flex row, `gap: 16px`, `pl: 24px`, `pr: 32px`, `py: 24px`, `rounded: calc(var(--radius)-2px)`, `shadow-lg`, `w: 388px`
- Content area: flex column, `gap: 4px`, flex-1
- Title: `font-semibold`, `text-sm`, `leading-5`
- Description: `font-normal`, `text-sm`, `leading-5`, `opacity: 0.9`
- Action button: `h: 36px`, `px: 16px`, `py: 8px`, `rounded: calc(var(--radius)-2px)`, `font-medium`, `text-sm`, `shadow-2xs`

#### Close Button States

| Error | State | Behavior |
|-------|-------|----------|
| False | Default | Transparent background, icon at 50% opacity |
| False | Hover | Transparent background, icon at full opacity |
| False | Focus | `--background` bg, double ring (`--background` + `--ring`) |
| True | Default | `--destructive` background, icon at 50% opacity |
| True | Hover | `--destructive` background, icon at full opacity |
| True | Focus | `--destructive` bg, `--destructive-60` ring (3px spread) |

Close button: `p: 4px`, `rounded: calc(var(--radius)-4px)`, positioned absolute `top: 3px`, `right: 3px`. Icon is 16×16 X (cross) icon.

#### Examples

Light and dark mode examples showing a default toast with title ("Scheduled: Catch up"), description ("Friday, February 10, 2023 at 5:57 PM"), and action button ("Undo").

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--background` | `--background` | Default toast background |
| `--foreground` | `--foreground` | Default toast text |
| `--border` | `--border` | Default toast border |
| `--input` | `--input` | Default action button border |
| `--destructive` | `--destructive` | Error toast background, border, close button bg |
| `--destructive-foreground` | `--destructive-foreground` | Error toast text |
| `--muted` | `--muted` | Error action button border |
| `--ring` | `--ring` | Default close button focus ring |
| `--radius` (derived) | `border-radius` | Toast and button border radius |
| `font-sans` | `--font-sans` | Font family |
| `font-semibold` (600) | `font-weight` | Title weight |
| `text-sm` (14px) | `font-size` | Text size |
| `shadow-lg` | `shadow-lg` | Toast container shadow |

### Theme Behavior

- Default toast uses `--background` / `--foreground` / `--border` — adapts to light/dark via theme
- Error toast uses `--destructive` / `--destructive-foreground` — adapts to light/dark via theme
- Action button uses `--input` border (default) or `--muted` border (error) — adapts to light/dark
- Close button focus ring uses `--ring` (default) or destructive-60 opacity ring (error)
- Shadow values change between light (`rgba(26,26,26,0.05)`) and dark (`rgba(0,0,0,0.07)`) modes
- All values resolve from theme CSS variables — no hardcoded colors in component
