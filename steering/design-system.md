# Design System Library

> **TL;DR:** Parent record for the three-tier design system: 57 components (atomic building blocks), patterns (functional compositions), and templates (layout blueprints). Defines global conventions (theming, validation, variable tracking) that apply across all tiers. Individual files contain only tier-specific content. Use the indexes below to navigate.

## Purpose

System-level documentation and index for the design system library. This file defines all global rules, conventions, and workflows that apply across components, patterns, and templates. Individual files contain only item-specific information.

**Use for:** Understanding design system conventions, navigating to specific items, validating implementations, determining which tier a new item belongs to.

**Do not use for:** Item-specific behavior, props, or styling — see the individual file.

---

## Three-Tier Structure

```text
steering/
├── design-system.md       — This file (parent record + indexes)
├── components/            — Atomic UI elements (57 files)
├── patterns/              — Functional compositions
└── templates/             — Layout blueprints
```

### Folder Responsibilities

| Folder | Purpose | Definition | Examples |
|--------|---------|------------|---------|
| `components/` | Atomic UI elements | Lowest-level reusable building blocks. Single-purpose, independently usable. | Button, Input, Badge, Dialog, Table |
| `patterns/` | Functional compositions | Structured combinations of components arranged to deliver a functional outcome. | Data Table (Table + sorting + filtering + pagination), Date Picker (Calendar + Popover + Input) |
| `templates/` | Layout blueprints | Non-functional layout structures that organize components and patterns into page-level arrangements. Templates do NOT add functionality. | Dashboard layout, Login page, Sidebar navigation layout |

### Usage Rules

- Components are the base layer — they do not depend on patterns or templates
- Patterns compose components to deliver functional outcomes
- Templates organize components and patterns into layouts without adding functionality
- Templates MUST NOT add behavior, state management, or business logic

### Authoring Guidance

| Question | Answer |
|----------|--------|
| When to create a component | When a new atomic UI element is needed that doesn't exist in the library |
| When to create a pattern | When a recurring functional grouping of components emerges across multiple use cases |
| When to create a template | When a page-level layout structure is reused across multiple contexts |
| Promotion rules | A component that grows to compose multiple sub-components may be promoted to a pattern. A pattern that becomes a full page layout may be promoted to a template. |
| Naming | All files: lowercase, hyphen-separated `.md`. Components match shadCN names. Patterns and templates use descriptive names. |
| Consistency | All items follow the same global conventions (theming, variable tracking) defined in this file |

---

## Global Conventions

### Design Tool Integration

Components can be mapped to any Figma library that implements shadCN components. Recommended Figma libraries for shadCN (from the official shadCN documentation):

- Official shadCN Figma kit (see `https://ui.shadcn.com/docs/figma`)
- Any community Figma library that maps to shadCN component APIs

Individual component files include a Component Structure section describing variant organization and a CSS Variable Mapping section for token-to-variable relationships.

### Design Tool Workflow

1. Open your design tool library
2. Navigate to the component page
3. Use dev mode to inspect properties, spacing, and color tokens
4. Map design tokens to shadCN/Tailwind CSS variables
5. Document tokens not found in shadCN or Tailwind as special-case variables
6. Hardcoded color values from design tools go to the theme file — the component file references only the variable name

### WCAG and Accessibility Reference

All components must conform to WCAG 2.2: `https://www.w3.org/TR/WCAG22/`

Individual component files include the specific WAI-ARIA pattern link for that component (e.g., Accordion Pattern, Dialog Pattern). The global WCAG reference is not repeated in individual files.

### Theme Compatibility Rules

These rules apply to every component file:

- Every component MUST support both light and dark modes
- No hardcoded color values in component files — reference CSS variables only (e.g., `--foreground`, `--border`)
- All resolved color/style values live in theme files, not in component files
- All CSS variables tagged with source: `shadcn`, `tailwind`, or `special-case`
- Design tokens in component files map to CSS variables, not raw color values
- When design tool inspection reveals a hardcoded value, that value goes to the theme file; the component file references only the variable name
- Background behavior (transparent, inherited, or explicit) must be documented per component

See: `default-theme.md`

### File Structure Convention

- One file per item: `[item-name].md`
- Component file names match shadCN component names (lowercase, hyphen-separated)
- Components live in `components/`, patterns in `patterns/`, templates in `templates/`

### Component File Section Template

Every individual component file follows this structure:

| Section | Content | Required |
|---------|---------|----------|
| Metadata | Name, source, dependencies | Yes |
| Behavior | What the component does, interaction model | Yes |
| Anti-Patterns | Known misuse patterns | Yes |
| API (Parts, Props, Variants, Data Attributes, CSS Variables) | Component-specific API surface | Yes |
| Composition Patterns | Usage examples, nesting, layout | Yes |
| Accessibility (WAI-ARIA pattern, ARIA Roles, Keyboard) | Component-specific a11y behavior | Yes |
| HTML | Standalone HTML structure | Yes |
| CSS (Raw CSS, Tailwind Mapping) | Component-specific styling | Yes |
| CSS Variable Dependencies | Variables this component uses, with purpose and source | Yes |
| Theme Support (Component Structure, CSS Variable Mapping, Theme Behavior) | Structure, token → CSS variable mapping, light/dark behavior | Yes |

---

## Validate Implementations

Each component includes standalone HTML structure, raw CSS + Tailwind mapping, and CSS variable dependency list. To validate:

1. Compare rendered output against design reference
2. Verify all CSS variables resolve to values in the active theme
3. Confirm light/dark mode behavior matches documented differences
4. Cross-reference CSS Variable Dependencies table against theme files

---

## Track Variable Dependencies

Each component lists all CSS variables it uses with their purpose and source. Cross-reference against theme files to ensure:

- All referenced variables are defined in `default-theme.md`
- Variable sources are tagged correctly (`shadcn`, `tailwind`, `special-case`)
- No undeclared variables exist in component files

---

## Component Index

57 components sourced from the shadCN registry.

| Component | File | Dependencies | Source |
|-----------|------|-------------|--------|
| Accordion | `components/accordion.md` | @radix-ui/react-accordion | shadCN (Radix UI primitive) |
| Alert | `components/alert.md` | (none) | shadCN |
| Alert Dialog | `components/alert-dialog.md` | radix-ui | shadCN (Radix UI primitive) |
| Aspect Ratio | `components/aspect-ratio.md` | radix-ui | shadCN (Radix UI primitive) |
| Avatar | `components/avatar.md` | radix-ui | shadCN (Radix UI primitive) |
| Badge | `components/badge.md` | radix-ui | shadCN (Radix UI primitive) |
| Breadcrumb | `components/breadcrumb.md` | radix-ui | shadCN (Radix UI primitive) |
| Button | `components/button.md` | radix-ui | shadCN (Radix UI primitive) |
| Button Group | `components/button-group.md` | (none) | shadCN |
| Calendar | `components/calendar.md` | react-day-picker, date-fns | shadCN (react-day-picker) |
| Card | `components/card.md` | (none) | shadCN |
| Carousel | `components/carousel.md` | embla-carousel-react | shadCN (embla-carousel) |
| Chart | `components/chart.md` | recharts, lucide-react | shadCN (recharts) |
| Checkbox | `components/checkbox.md` | radix-ui | shadCN (Radix UI primitive) |
| Collapsible | `components/collapsible.md` | radix-ui | shadCN (Radix UI primitive) |
| Combobox | `components/combobox.md` | @base-ui/react | shadCN (Base UI) |
| Command | `components/command.md` | cmdk | shadCN (cmdk) |
| Context Menu | `components/context-menu.md` | radix-ui | shadCN (Radix UI primitive) |
| Dialog | `components/dialog.md` | radix-ui | shadCN (Radix UI primitive) |
| Direction | `components/direction.md` | (none) | shadCN |
| Drawer | `components/drawer.md` | vaul | shadCN (vaul) |
| Dropdown Menu | `components/dropdown-menu.md` | radix-ui | shadCN (Radix UI primitive) |
| Empty | `components/empty.md` | (none) | shadCN |
| Field | `components/field.md` | (none) | shadCN |
| Form | `components/form.md` | radix-ui, @hookform/resolvers, zod, react-hook-form | shadCN (Radix UI primitive) |
| Hover Card | `components/hover-card.md` | radix-ui | shadCN (Radix UI primitive) |
| Input | `components/input.md` | (none) | shadCN |
| Input Group | `components/input-group.md` | (none) | shadCN |
| Input OTP | `components/input-otp.md` | input-otp | shadCN (input-otp) |
| Item | `components/item.md` | radix-ui | shadCN (Radix UI primitive) |
| Kbd | `components/kbd.md` | (none) | shadCN |
| Label | `components/label.md` | radix-ui | shadCN (Radix UI primitive) |
| Menubar | `components/menubar.md` | radix-ui | shadCN (Radix UI primitive) |
| Native Select | `components/native-select.md` | (none) | shadCN |
| Navigation Menu | `components/navigation-menu.md` | radix-ui | shadCN (Radix UI primitive) |
| Pagination | `components/pagination.md` | (none) | shadCN |
| Popover | `components/popover.md` | radix-ui | shadCN (Radix UI primitive) |
| Progress | `components/progress.md` | radix-ui | shadCN (Radix UI primitive) |
| Radio Group | `components/radio-group.md` | radix-ui | shadCN (Radix UI primitive) |
| Resizable | `components/resizable.md` | react-resizable-panels | shadCN (react-resizable-panels) |
| Scroll Area | `components/scroll-area.md` | radix-ui | shadCN (Radix UI primitive) |
| Select | `components/select.md` | radix-ui | shadCN (Radix UI primitive) |
| Separator | `components/separator.md` | radix-ui | shadCN (Radix UI primitive) |
| Sheet | `components/sheet.md` | radix-ui | shadCN (Radix UI primitive) |
| Sidebar | `components/sidebar.md` | radix-ui, class-variance-authority, lucide-react | shadCN (Radix UI primitive) |
| Skeleton | `components/skeleton.md` | (none) | shadCN |
| Slider | `components/slider.md` | radix-ui | shadCN (Radix UI primitive) |
| Sonner | `components/sonner.md` | sonner, next-themes | shadCN (sonner) |
| Spinner | `components/spinner.md` | class-variance-authority | shadCN |
| Switch | `components/switch.md` | radix-ui | shadCN (Radix UI primitive) |
| Table | `components/table.md` | (none) | shadCN |
| Tabs | `components/tabs.md` | radix-ui | shadCN (Radix UI primitive) |
| Textarea | `components/textarea.md` | (none) | shadCN |
| Toast | `components/toast.md` | (none) | shadCN (general-purpose toast; see also Sonner) |
| Toggle | `components/toggle.md` | radix-ui | shadCN (Radix UI primitive) |
| Toggle Group | `components/toggle-group.md` | radix-ui | shadCN (Radix UI primitive) |
| Tooltip | `components/tooltip.md` | radix-ui | shadCN (Radix UI primitive) |

---

## Pattern Index

Patterns are functional compositions of components.

| Pattern | File | Composes | Source |
|---------|------|----------|--------|
| Data Table | `patterns/data-table.md` | Table, sorting, filtering, pagination | shadCN |
| Date Picker | `patterns/date-picker.md` | Calendar, Popover, Input | shadCN |

---

## Template Index

Templates are layout blueprints without functionality.

| Template | File | Uses | Source |
|----------|------|------|--------|
| Calendar Template | `templates/calendar-template.md` | Calendar, Sidebar, Card, page layout | shadCN |
| Dashboard Template | `templates/dashboard-template.md` | Sidebar, Card, Chart, Table, Tabs, page layout | shadCN |
| Login Template | `templates/login-template.md` | Card, Field, Input, Button, auth page layout | shadCN |
| Sidebar Template | `templates/sidebar-template.md` | Sidebar (component), Breadcrumb, Separator, Dropdown Menu, page layout | shadCN |
