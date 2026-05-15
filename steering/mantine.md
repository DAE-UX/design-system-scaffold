# Mantine Translation Layer — Design System Skill

> **TL;DR:** Bidirectional translation layer between Mantine UI and the shadCN/Tailwind system. Maps variable roles (not values) so translations work with any theme. Primary direction: Mantine → shadCN (importing Mantine values). Secondary: shadCN → Mantine (generating a Mantine theme). Covers CSS variable mapping, shade-to-role resolution, spacing, radius, shadow, breakpoint, and typography translations. Component name mapping for all 57 design system components.

## Purpose

Translation reference for agents working with Mantine-based UIs alongside this design system.

**Use for:** Importing Mantine theme values into shadCN themes, translating Mantine component code to shadCN equivalents, generating Mantine themes from shadCN theme files.

**Do not use for:** Mantine installation or setup (see Mantine docs), component behavior (see `design-system/components/`), shadCN theming details (see `technical-guidelines.md`).

**Stop conditions:**
- You have identified the Mantine-to-shadCN variable mapping for your use case
- You have enough detail to translate the component or theme
- The translation preserves the functional role, not the exact value

---

## Variable Role Mapping

The core translation. Maps Mantine CSS variables to shadCN CSS variables by functional role. Theme-agnostic — works with any theme on either side.

### Core UI Variables

| Mantine Variable | Role | shadCN Variable |
|-----------------|------|----------------|
| `--mantine-color-body` | Page background | `--background` |
| `--mantine-color-text` | Default text color | `--foreground` |
| `--mantine-color-bright` | High-contrast text | `--foreground` |
| `--mantine-primary-color-filled` | Primary action background | `--primary` |
| `--mantine-primary-color-contrast` | Text on primary background | `--primary-foreground` |
| `--mantine-color-default` | Default surface background | `--card` |
| `--mantine-color-default-color` | Default surface text | `--card-foreground` |
| `--mantine-color-default-border` | Default border | `--border` |
| `--mantine-color-default-hover` | Default hover background | `--accent` |
| `--mantine-color-dimmed` | Muted/secondary text | `--muted-foreground` |
| `--mantine-color-placeholder` | Placeholder text | `--muted-foreground` |
| `--mantine-color-anchor` | Link color | `--primary` |
| `--mantine-color-error` | Error/destructive color | `--destructive` |
| `--mantine-color-disabled` | Disabled background | `--muted` |
| `--mantine-color-disabled-color` | Disabled text | `--muted-foreground` |
| `--mantine-color-disabled-border` | Disabled border | `--border` |

### Input and Form Variables

| Mantine Variable | Role | shadCN Variable |
|-----------------|------|----------------|
| `--mantine-color-default-border` | Input border | `--input` |
| `--mantine-color-placeholder` | Input placeholder | `--muted-foreground` |
| `--mantine-color-error` | Input error state | `--destructive` |

### Popover and Overlay Variables

| Mantine Variable | Role | shadCN Variable |
|-----------------|------|----------------|
| `--mantine-color-default` | Popover background | `--popover` |
| `--mantine-color-default-color` | Popover text | `--popover-foreground` |

### Focus and Ring Variables

| Mantine Variable | Role | shadCN Variable |
|-----------------|------|----------------|
| `--mantine-primary-color-filled` | Focus ring color | `--ring` |

### No Direct Mapping

These Mantine variables have no direct shadCN equivalent. Document the recommended approach.

| Mantine Variable | Role | Recommended Approach |
|-----------------|------|---------------------|
| `--mantine-color-{color}-filled` | Per-color filled variant | Use shadCN's single-role variables. Map the primary color's filled variant to `--primary`. Other colors map to custom theme variables if needed. |
| `--mantine-color-{color}-light` | Per-color light variant | No equivalent. Use opacity modifiers on the base color (e.g., `bg-primary/10`). |
| `--mantine-color-{color}-outline` | Per-color outline variant | No equivalent. Use `border-primary` with transparent background. |
| `--mantine-color-{color}-text` | Per-color text variant | No equivalent. Use the filled variant as text color. |
| `--mantine-color-{color}-filled-hover` | Per-color hover | No equivalent. Use CSS `:hover` with opacity or shade shift. |
| `--mantine-color-{color}-light-hover` | Per-color light hover | No equivalent. Use CSS `:hover` with opacity shift. |
| `--mantine-color-{color}-outline-hover` | Per-color outline hover | No equivalent. Use CSS `:hover` with background opacity. |

---

## Shade-to-Role Resolution

Mantine uses a 10-shade color scale (0–9) per color. shadCN uses single values per role. These rules define which shade maps to which role.

| shadCN Role | Mantine Shade Selection (Light) | Mantine Shade Selection (Dark) |
|------------|-------------------------------|-------------------------------|
| `--primary` | `primaryColor[6]` (filled) | `primaryColor[8]` (filled) |
| `--primary-foreground` | `white` or `primaryColor[0]` (by contrast) | `white` or `primaryColor[0]` (by contrast) |
| `--secondary` | `gray[1]` | `dark[6]` |
| `--secondary-foreground` | `dark[9]` or `black` | `gray[0]` or `white` |
| `--muted` | `gray[1]` | `dark[6]` |
| `--muted-foreground` | `gray[6]` | `dark[2]` |
| `--accent` | `gray[1]` | `dark[5]` |
| `--accent-foreground` | `dark[9]` | `gray[0]` |
| `--destructive` | `red[6]` | `red[8]` |
| `--destructive-foreground` | `white` | `white` |
| `--border` | `gray[3]` | `dark[4]` |
| `--input` | `gray[3]` | `dark[4]` |
| `--ring` | `primaryColor[6]` | `primaryColor[8]` |
| `--card` | `white` | `dark[7]` |
| `--card-foreground` | `black` | `gray[0]` |
| `--popover` | `white` | `dark[7]` |
| `--popover-foreground` | `black` | `gray[0]` |

These are rules, not values. When importing a Mantine theme, apply these rules to extract the right shade for each shadCN role.

---

## Scale Translations

### Spacing

Maps Mantine's named spacing scale to Tailwind's numeric scale by position.

| Mantine | Tailwind | Default Mantine | Default Tailwind |
|---------|----------|----------------|-----------------|
| `xs` | `2.5` (10px) | 10px | 10px |
| `sm` | `3` (12px) | 12px | 12px |
| `md` | `4` (16px) | 16px | 16px |
| `lg` | `5` (20px) | 20px | 20px |
| `xl` | `8` (32px) | 32px | 32px |

Default values shown for reference. The mapping is by position — if a Mantine theme overrides `spacing.md` to 20px, map to the Tailwind position that best fits.

### Border Radius

| Mantine | Tailwind | Default Mantine | Default Tailwind |
|---------|----------|----------------|-----------------|
| `xs` | `rounded-sm` | 2px | 2px |
| `sm` | `rounded` | 4px | 4px |
| `md` | `rounded-md` | 8px | 6px |
| `lg` | `rounded-lg` | 16px | 12px |
| `xl` | `rounded-xl` | 32px | 16px |

Note value differences at `md`, `lg`, `xl`. The translation maps the position. Adjust if the Mantine theme has custom radius values.

### Shadow

| Mantine | Tailwind |
|---------|----------|
| `xs` | `shadow-xs` |
| `sm` | `shadow-sm` |
| `md` | `shadow-md` |
| `lg` | `shadow-lg` |
| `xl` | `shadow-xl` |

Shadow values are theme-dependent in both systems. The translation maps the scale position.

### Breakpoints

| Mantine | Mantine Default | Our Breakpoint | Our Default | Difference |
|---------|----------------|---------------|-------------|------------|
| `xs` | 36em (576px) | `sm` | 544px | 32px |
| `sm` | 48em (768px) | `md` | 768px | 0px |
| `md` | 62em (992px) | `lg` | 1012px | 20px |
| `lg` | 75em (1200px) | `xl` | 1280px | 80px |
| `xl` | 88em (1408px) | `2xl` | 1400px | 8px |

Breakpoints are defined in `technical-guidelines.md`. Document differences for layout-sensitive work.

### Typography

| Mantine Variable | shadCN/Tailwind Equivalent |
|-----------------|--------------------------|
| `--mantine-font-family` | `--font-sans` (via `@theme`) |
| `--mantine-font-family-monospace` | `--font-mono` (via `@theme`) |
| `--mantine-font-family-headings` | `--font-sans` (same as body unless overridden) |
| `--mantine-font-size-xs` | `text-xs` |
| `--mantine-font-size-sm` | `text-sm` |
| `--mantine-font-size-md` | `text-base` |
| `--mantine-font-size-lg` | `text-lg` |
| `--mantine-font-size-xl` | `text-xl` |
| `--mantine-line-height` | `leading-normal` |
| `--mantine-heading-font-weight` | `font-bold` |

Font family values come from the active theme file, not this translation layer.

---

## Dark Mode Translation

| Aspect | Mantine | shadCN |
|--------|---------|--------|
| Selector | `[data-mantine-color-scheme="dark"]` | `.dark` |
| Toggle | `colorScheme` prop on MantineProvider | Class toggle on root element |
| Variable scoping | Re-declares all variables under the dark selector | Declares separate values in `.dark {}` |
| Shade shifting | Colors shift shade index (shade 6 → shade 8) | Separate light/dark values per variable |

When importing a Mantine theme, translate the shade-shifting approach into explicit light/dark value pairs for the shadCN theme file.

---

## Component Name Mapping

Maps each of the 57 design system components to its Mantine equivalent.

### Direct Equivalents

| shadCN Component | Mantine Equivalent | Notes |
|-----------------|-------------------|-------|
| Accordion | Accordion | Same compound pattern |
| Alert | Alert | Same structure |
| Alert Dialog | Modal (with confirm pattern) | Mantine uses Modal for all dialogs |
| Avatar | Avatar | Same structure |
| Badge | Badge | Same structure |
| Breadcrumb | Breadcrumbs | Plural naming |
| Button | Button | Same structure, different variant names |
| Calendar | Calendar (`@mantine/dates`) | Extension package |
| Card | Card | Same structure |
| Checkbox | Checkbox | Same structure |
| Collapsible | Collapse | Different name |
| Combobox | Combobox | Same concept |
| Command | Spotlight (`@mantine/spotlight`) | Extension package, different API |
| Context Menu | Menu (with context trigger) | Mantine uses Menu for all menu types |
| Dialog | Modal | Mantine uses Modal for dialogs |
| Drawer | Drawer | Same structure |
| Dropdown Menu | Menu | Different name |
| Form | Form (with `@mantine/form`) | Extension package |
| Hover Card | HoverCard | Same structure |
| Input | Input / TextInput | Mantine separates base Input from TextInput |
| Input OTP | PinInput | Different name |
| Label | Input.Label | Part of Input compound component |
| Menubar | No equivalent | See "No Equivalent" section |
| Navigation Menu | NavLink | Different structure |
| Pagination | Pagination | Same structure |
| Popover | Popover | Same structure |
| Progress | Progress | Same structure |
| Radio Group | Radio.Group | Compound component |
| Resizable | No equivalent | See "No Equivalent" section |
| Scroll Area | ScrollArea | Same structure |
| Select | Select | Same structure |
| Separator | Divider | Different name |
| Sheet | Drawer (with side positioning) | Mantine Drawer supports all sides |
| Sidebar | AppShell.Navbar | Part of AppShell layout |
| Skeleton | Skeleton | Same structure |
| Slider | Slider | Same structure |
| Sonner | Notifications (`@mantine/notifications`) | Extension package |
| Spinner | Loader | Different name |
| Switch | Switch | Same structure |
| Table | Table | Same structure |
| Tabs | Tabs | Same structure |
| Textarea | Textarea | Same structure |
| Toast | Notifications (`@mantine/notifications`) | Extension package |
| Toggle | ActionIcon (with toggle state) | Different pattern |
| Toggle Group | SegmentedControl or Chip.Group | Depends on use case |
| Tooltip | Tooltip | Same structure |

### Structural Differences

| shadCN Pattern | Mantine Pattern |
|---------------|----------------|
| `className` prop for styling | `styles` API with named targets (root, label, input, etc.) |
| CVA variants (`variant`, `size`) | Built-in `variant` and `size` props with different value names |
| Compound components (`Dialog.Root`, `Dialog.Content`) | Compound components (`Modal.Root`, `Modal.Body`) with different part names |
| Radix data attributes (`data-state`) | Mantine data attributes (`data-active`, `data-disabled`) |
| CSS variable theming | `createTheme()` object + CSS variables |

### No Equivalent in Mantine

| shadCN Component | Description | Recommended Approach |
|-----------------|-------------|---------------------|
| Aspect Ratio | Maintains aspect ratio | Use CSS `aspect-ratio` property directly |
| Button Group | Groups buttons | Use Mantine `Group` with `gap={0}` |
| Chart | Data visualization | Use `@mantine/charts` (extension package) |
| Carousel | Content carousel | Use Embla Carousel directly (same underlying library) |
| Direction | RTL/LTR provider | Use Mantine's `DirectionProvider` |
| Empty | Empty state | No built-in equivalent — compose from primitives |
| Field | Form field wrapper | Use Mantine's `Input.Wrapper` |
| Input Group | Input with addons | Use Mantine's `Input` with `leftSection`/`rightSection` |
| Item | Generic list item | No equivalent — compose from primitives |
| Kbd | Keyboard shortcut display | Use Mantine's `Kbd` |
| Native Select | Native HTML select | Use Mantine's `NativeSelect` |

### No Equivalent in shadCN

Mantine components that don't map to any of our 57 components.

| Mantine Component | Description |
|------------------|-------------|
| Stepper | Multi-step progress indicator |
| Timeline | Vertical timeline |
| Spotlight | Command palette (closest: Command) |
| TransferList | Dual-list transfer |
| ColorPicker | Color selection |
| ColorInput | Color input field |
| RingProgress | Circular progress |
| NumberInput | Numeric input with controls |
| PasswordInput | Password field with visibility toggle |
| Autocomplete | Text input with suggestions |
| MultiSelect | Multi-value select |
| TagsInput | Tag input field |
| Rating | Star rating |
| Indicator | Badge-like notification dot |
| Affix | Fixed position element |
| Overlay | Full-screen overlay |
| LoadingOverlay | Loading state overlay |
| BackgroundImage | Image as background |
| Blockquote | Styled blockquote |
| Code | Inline code display |
| Highlight | Text highlighting |
| Mark | Text marking |
| Title | Heading component |
| TypographyStylesProvider | Prose styling |

---

## Reverse Direction: Generate Mantine Theme

Given a shadCN theme file (e.g., `themes/default.md`), generate a Mantine `createTheme()` object:

1. Read the theme file's CSS variable values (light and dark)
2. Apply the reverse variable role mapping (shadCN → Mantine)
3. Construct the Mantine 10-shade color scale from shadCN single values:
   - Place the `--primary` value at shade 6 (light) / shade 8 (dark)
   - Interpolate remaining shades by adjacency — find the closest Mantine default shade for each position
   - Do not modify shadCN theme values to fit the Mantine scale
4. Output a valid `createTheme()` object

Shade interpolation is inherently lossy. The shadCN values are the source of truth; Mantine shades are derived by proximity.

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Mantine uses 10-shade color scales; shadCN uses single values per role | Importing loses shade granularity; exporting requires interpolation | Use shade-to-role rules for import; adjacency-based interpolation for export |
| Mantine's `styles` API targets named parts; shadCN uses `className` | Direct CSS translation doesn't carry over | Map part names to shadCN's compound component structure |
| Mantine breakpoints are in `em`; our breakpoints are in `px` | Minor layout differences at breakpoint boundaries | Document differences; test layout-sensitive work at boundaries |
| Mantine extension packages (`@mantine/dates`, `@mantine/charts`) have different APIs | Component translation requires API-level mapping, not just naming | Use the component mapping table for structural guidance |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| Mantine CSS Variables | `https://mantine.dev/styles/css-variables-list/` | Complete variable inventory |
| Mantine Theming | `https://mantine.dev/theming/mantine-provider/` | Theme configuration |
| Mantine Colors | `https://mantine.dev/theming/colors/` | Color scale system |
| Mantine Responsive | `https://mantine.dev/styles/responsive/` | Breakpoint system |
| shadCN Variable Reference | `technical-guidelines.md` → Variable Reference | shadCN variable contract |
