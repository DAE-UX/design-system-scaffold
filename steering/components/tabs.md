# Component: Tabs

> **TL;DR:** A tabbed interface built on Radix Tabs primitive with CVA for list variants. 4 parts + CVA export: Tabs, TabsList, TabsTrigger, TabsContent, tabsListVariants. Supports `horizontal` and `vertical` orientation. TabsList has `default` (filled background) and `line` (underline indicator) variants. Active trigger styling differs per variant — shadow/border for default, pseudo-element underline for line. Commonly used for content sections, settings pages, and form organization.

## Metadata

- **Component Name:** Tabs
- **Source:** shadCN (Radix UI primitive)
- **Dependencies:** `@radix-ui/react-tabs`, `class-variance-authority`

## Behavior

A tabbed interface for organizing content into selectable panels.

- Built on Radix Tabs primitive — manages active tab state, keyboard navigation, and accessibility
- Tabs root supports `horizontal` (default) and `vertical` orientation
- Horizontal orientation: flex-col layout (list above content)
- Vertical orientation: flex (row) layout (list beside content)
- TabsList has two CVA variants: `default` (muted background, rounded-lg) and `line` (transparent background, gap-1)
- TabsList adapts to orientation: horizontal uses h-9, vertical uses h-fit with flex-col
- TabsTrigger inactive state: text-foreground/60 (light), text-muted-foreground (dark)
- TabsTrigger active (default variant): bg-background, shadow-sm; dark mode adds border-input and bg-input/30
- TabsTrigger active (line variant): bg-transparent with an `::after` pseudo-element underline
- Line variant `::after`: bg-foreground, h-0.5 (horizontal) or w-0.5 (vertical), opacity-0 to opacity-100 on active
- Focus: border-ring, ring-[3px] ring-ring/50
- Disabled: pointer-events-none opacity-50
- Icons inside triggers: pointer-events-none shrink-0 size-4
- TabsContent: flex-1 outline-none
- Uses `group/tabs` on root for descendant styling via `data-orientation`

### Anti-Patterns

#### Do
- Use for organizing discrete blocks of information
- Use 2-7 tabs
- Disable icon buttons in action slot if tab is disabled
- Provide clear empty state when all dismissible tabs are closed

#### Don't
- Don't nest tabs
- Don't use tabs as steps, navigation, or in-page anchors
- Don't use tabs as navigation in edit/create flows — use multi-page flow
- Don't use icons in tab labels as decoration
- Don't use normal/primary/icon buttons for actions in tabs
- Don't include info links as actions — add within tab content

Sources: shadCN, Radix UI

## API

### Parts

- `Tabs` — Root container. Manages orientation and active tab state. Applies flex direction based on orientation.
- `TabsList` — Container for tab triggers. CVA variants control visual style (default vs line).
- `TabsTrigger` — Individual tab button. Complex styling with active/inactive states per variant.
- `TabsContent` — Content panel associated with a tab. Renders when its tab is active.
- `tabsListVariants` — Exported CVA variant function for TabsList styling.

### Props

#### Tabs

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"horizontal" \| "vertical"` | `"horizontal"` | Layout direction |
| defaultValue | `string` | — | Default active tab value (uncontrolled) |
| value | `string` | — | Controlled active tab value |
| onValueChange | `(value: string) => void` | — | Callback when active tab changes |
| className | `string` | — | Additional CSS classes |

#### TabsList

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"default" \| "line"` | `"default"` | Visual style variant |
| className | `string` | — | Additional CSS classes |

#### TabsTrigger

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Tab value (required, matches TabsContent) |
| disabled | `boolean` | `false` | Disables the trigger |
| className | `string` | — | Additional CSS classes |

#### TabsContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| value | `string` | — | Tab value (required, matches TabsTrigger) |
| className | `string` | — | Additional CSS classes |

### Variants

#### TabsList Variants (CVA)

| Variant | Description | Use Case |
|---------|-------------|----------|
| `variant="default"` | Muted background (`bg-muted`), rounded-lg, p-[3px] | Standard tabbed interface |
| `variant="line"` | Transparent background, gap-1, rounded-none | Underline-style tabs |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"tabs"`, `"tabs-list"`, `"tabs-trigger"`, `"tabs-content"` | All parts |
| `[data-orientation]` | `"horizontal"`, `"vertical"` | Tabs |
| `[data-variant]` | `"default"`, `"line"` | TabsList |
| `[data-state]` | `"active"`, `"inactive"` | TabsTrigger (from Radix) |

### CSS Variables (Radix)

Not applicable — Tabs does not expose Radix CSS variables.

## Composition Patterns

### Basic Tabs (Default Variant)

```tsx
<Tabs defaultValue="account">
  <TabsList>
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabsContent value="account">
    <p>Account settings content.</p>
  </TabsContent>
  <TabsContent value="password">
    <p>Password settings content.</p>
  </TabsContent>
</Tabs>
```

### Line Variant

```tsx
<Tabs defaultValue="overview">
  <TabsList variant="line">
    <TabsTrigger value="overview">Overview</TabsTrigger>
    <TabsTrigger value="analytics">Analytics</TabsTrigger>
    <TabsTrigger value="reports">Reports</TabsTrigger>
  </TabsList>
  <TabsContent value="overview">Overview content</TabsContent>
  <TabsContent value="analytics">Analytics content</TabsContent>
  <TabsContent value="reports">Reports content</TabsContent>
</Tabs>
```

### Vertical Orientation

```tsx
<Tabs defaultValue="general" orientation="vertical">
  <TabsList>
    <TabsTrigger value="general">General</TabsTrigger>
    <TabsTrigger value="security">Security</TabsTrigger>
    <TabsTrigger value="notifications">Notifications</TabsTrigger>
  </TabsList>
  <TabsContent value="general">General settings</TabsContent>
  <TabsContent value="security">Security settings</TabsContent>
  <TabsContent value="notifications">Notification preferences</TabsContent>
</Tabs>
```

### With Icons

```tsx
<Tabs defaultValue="code">
  <TabsList>
    <TabsTrigger value="code">
      <CodeIcon />
      Code
    </TabsTrigger>
    <TabsTrigger value="preview">
      <EyeIcon />
      Preview
    </TabsTrigger>
  </TabsList>
  <TabsContent value="code">Code editor</TabsContent>
  <TabsContent value="preview">Live preview</TabsContent>
</Tabs>
```

## Accessibility

- **WAI-ARIA Pattern:** [Tabs](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)

### ARIA Roles

- Tabs root: `tablist` role is applied to TabsList (provided by Radix)
- TabsTrigger: `tab` role with `aria-selected` and `aria-controls` (provided by Radix)
- TabsContent: `tabpanel` role with `aria-labelledby` (provided by Radix)
- Orientation is communicated via `aria-orientation` on the tablist

### Keyboard Behavior

| Key | Action |
|-----|--------|
| `ArrowRight` | Move focus to next tab (horizontal orientation) |
| `ArrowLeft` | Move focus to previous tab (horizontal orientation) |
| `ArrowDown` | Move focus to next tab (vertical orientation) |
| `ArrowUp` | Move focus to previous tab (vertical orientation) |
| `Home` | Move focus to first tab |
| `End` | Move focus to last tab |
| `Tab` | Move focus into the active tab panel |
| `Space` / `Enter` | Activate focused tab |

## HTML

### Standalone HTML Structure

```html
<div data-slot="tabs" data-orientation="horizontal">
  <div data-slot="tabs-list" data-variant="default" role="tablist" aria-orientation="horizontal">
    <button data-slot="tabs-trigger" role="tab" data-state="active" aria-selected="true">
      Account
    </button>
    <button data-slot="tabs-trigger" role="tab" data-state="inactive" aria-selected="false">
      Password
    </button>
  </div>
  <div data-slot="tabs-content" role="tabpanel" data-state="active">
    <p>Account settings content.</p>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Tabs root */
.Tabs[data-orientation="horizontal"] {
  display: flex;
  flex-direction: column;
}

.Tabs[data-orientation="vertical"] {
  display: flex;
  flex-direction: row;
}

/* TabsList — base */
.TabsList {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 3px;
  color: var(--muted-foreground);
}

/* TabsList — horizontal */
.Tabs[data-orientation="horizontal"] .TabsList {
  height: 2.25rem;
  width: 100%;
}

/* TabsList — vertical */
.Tabs[data-orientation="vertical"] .TabsList {
  height: fit-content;
  flex-direction: column;
}

/* TabsList — default variant */
.TabsList[data-variant="default"] {
  background-color: var(--muted);
  border-radius: 0.5rem;
}

/* TabsList — line variant */
.TabsList[data-variant="line"] {
  background-color: transparent;
  gap: 0.25rem;
  border-radius: 0;
}

/* TabsTrigger — base */
.TabsTrigger {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.375rem;
  white-space: nowrap;
  border-radius: 0.375rem;
  padding: 0.25rem 0.75rem;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 150ms;
  position: relative;
  outline: none;
}

/* TabsTrigger — inactive */
.TabsTrigger[data-state="inactive"] {
  color: color-mix(in srgb, var(--foreground) 60%, transparent);
}

/* TabsTrigger — active (default variant) */
.TabsList[data-variant="default"] .TabsTrigger[data-state="active"] {
  background-color: var(--background);
  color: var(--foreground);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

/* TabsTrigger — active (line variant) */
.TabsList[data-variant="line"] .TabsTrigger[data-state="active"] {
  background-color: transparent;
  color: var(--foreground);
}

/* TabsTrigger — line variant ::after (horizontal) */
.TabsList[data-variant="line"] .TabsTrigger::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 0.125rem;
  background-color: var(--foreground);
  opacity: 0;
  border-radius: 9999px;
}

.TabsList[data-variant="line"] .TabsTrigger[data-state="active"]::after {
  opacity: 1;
}

/* TabsTrigger — line variant ::after (vertical) */
.Tabs[data-orientation="vertical"] .TabsList[data-variant="line"] .TabsTrigger::after {
  top: 0;
  bottom: 0;
  left: 0;
  right: auto;
  width: 0.125rem;
  height: auto;
}

/* TabsTrigger — focus */
.TabsTrigger:focus-visible {
  border-color: var(--ring);
  outline: 3px solid color-mix(in srgb, var(--ring) 50%, transparent);
}

/* TabsTrigger — disabled */
.TabsTrigger:disabled {
  pointer-events: none;
  opacity: 0.5;
}

/* TabsTrigger icons */
.TabsTrigger svg {
  pointer-events: none;
  flex-shrink: 0;
  width: 1rem;
  height: 1rem;
}

/* TabsContent */
.TabsContent {
  flex: 1;
  outline: none;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Tabs (horizontal) | `group/tabs flex flex-col` | Vertical stack layout |
| Tabs (vertical) | `group/tabs flex` | Horizontal row layout |
| TabsList (base) | `inline-flex items-center justify-center p-[3px] text-muted-foreground` | List container |
| TabsList (horizontal) | `h-9 w-full` | Horizontal dimensions |
| TabsList (vertical) | `h-fit flex-col` | Vertical dimensions |
| TabsList (default) | `bg-muted rounded-lg` | Filled background |
| TabsList (line) | `bg-transparent gap-1 rounded-none` | Transparent with gap |
| TabsTrigger (base) | `inline-flex items-center justify-center gap-1.5 whitespace-nowrap rounded-md px-3 py-1 text-sm font-medium transition-all relative` | Trigger base |
| TabsTrigger (inactive) | `text-foreground/60 dark:text-muted-foreground` | Inactive text color |
| TabsTrigger (active, default) | `data-[state=active]:bg-background data-[state=active]:text-foreground data-[state=active]:shadow-sm dark:data-[state=active]:border-input dark:data-[state=active]:bg-input/30` | Active default styling |
| TabsTrigger (active, line) | `data-[state=active]:bg-transparent data-[state=active]:after:opacity-100` | Active line styling |
| TabsTrigger (::after, line, horizontal) | `after:absolute after:inset-x-0 after:bottom-0 after:h-0.5 after:bg-foreground after:opacity-0 after:rounded-full` | Horizontal underline |
| TabsTrigger (::after, line, vertical) | `after:inset-y-0 after:left-0 after:right-auto after:w-0.5 after:h-auto` | Vertical side indicator |
| TabsTrigger (focus) | `focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50` | Focus ring |
| TabsTrigger (disabled) | `disabled:pointer-events-none disabled:opacity-50` | Disabled state |
| TabsTrigger (icons) | `[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg]:size-4` | Icon sizing |
| TabsContent | `flex-1 outline-none` | Content panel |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted` | TabsList default variant background | `shadcn` |
| `--muted-foreground` | TabsList text color, dark mode inactive trigger color | `shadcn` |
| `--foreground` | Active trigger text, line variant underline, inactive trigger (at 60% opacity) | `shadcn` |
| `--background` | Active trigger background (default variant) | `shadcn` |
| `--ring` | Focus border and ring color | `shadcn` |
| `--input` | Dark mode active trigger border and background (default variant) | `shadcn` |

## Theme Support

### Component Structure

Component organizes Tabs with a variant matrix on the tab item sub-component:

**Tab Item**

| Variant Property | Values |
|-----------------|--------|
| Active | True, False |
| Type | Default |
| State | Default, Focus, Disabled |

- Tabs → TabsList + TabsContent
- TabsList: secondary bg, rounded, p 4px, inline-flex container
- Active tab item: background, border (dark), shadow-sm, rounded
- Inactive tab item: transparent against list background
- Tab item text: foreground, text-sm, font-medium
- Tab items arranged horizontally with consistent padding


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--secondary` | `--muted` | TabsList background |
| `--foreground` | `--foreground` | Tab item text color |
| `--background` | `--background` | Active tab item background |
| `--input` | `--input` | Active tab item border (dark mode) |
| `--ring` | `--ring` | Focus ring color |
| `--radius` | `border-radius` | TabsList border radius |
| `p-1` (4px) | `padding` | TabsList internal padding |
| `px-3` (12px) | `padding` | Tab item horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Tab item font weight |
| `text-sm` (14px) | `font-size` | Tab item text size |

### Theme Behavior

- TabsList default variant uses `--muted` — adapts to light/dark via theme
- Active trigger (default variant) uses `--background` in light mode, `--input/30` in dark mode
- Inactive triggers use `--foreground` at 60% opacity (light) and `--muted-foreground` (dark)
- Line variant underline uses `--foreground` — adapts to light/dark via theme
- Focus ring uses `--ring` — adapts to light/dark via theme
- Shadow (`shadow-sm`) on active default trigger is mode-independent
- All spacing, typography, and layout tokens are mode-independent
