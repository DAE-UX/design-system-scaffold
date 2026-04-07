# Storybook Configuration — Design System Skill

> **TL;DR:** Instructions for generating a Storybook that showcases all 57 components, 2 patterns, and 4 templates from the design system with theme switching and light/dark mode support. Uses Storybook 9+ with React/Vite. Theme and mode are controlled via global toolbar decorators. Each component gets a story file with variants, composition patterns, and interactive controls. Patterns and templates get dedicated story files showing composed usage.

## Purpose

Configuration and generation instructions for building a complete Storybook from the design system skill files.

**Use for:** Generating a Storybook in a target project, configuring theme/mode switching, creating story files for components/patterns/templates.

**Do not use for:** Component behavior details (see `design-system/components/`), theme values (see `themes/`), shadCN setup (see `shadcn.md`).

---

## Prerequisites

- A React project with shadCN components installed (see `shadcn.md`)
- Node.js 20+
- All design system components added to the project via `npx shadcn@latest add`

Source: `https://storybook.js.org/docs`

---

## Install Storybook

```bash
npm create storybook@latest
```

When prompted:
- Select "Recommended" configuration (includes docs, testing, accessibility)
- Storybook will detect your framework (Next.js, Vite, etc.) automatically

After installation:

```bash
npm run storybook
```

---

## Project Structure

```
.storybook/
├── main.ts              — Storybook configuration
├── preview.ts           — Global decorators, parameters, globals
├── manager.ts           — UI theme configuration
└── DSTheme.ts          — Custom Storybook UI theme

src/
├── stories/
│   ├── components/      — One story file per design system component
│   ├── patterns/        — Story files for composed patterns
│   └── templates/       — Story files for layout templates
```

---

## Configure Storybook

### main.ts

```typescript
import type { StorybookConfig } from "@storybook/react-vite";

const config: StorybookConfig = {
  stories: ["../src/**/*.stories.@(js|jsx|mjs|ts|tsx)"],
  addons: [
    "@storybook/addon-docs",
    "@storybook/addon-essentials",
    "@storybook/addon-a11y",
  ],
  framework: {
    name: "@storybook/react-vite",
    options: {},
  },
};

export default config;
```

### preview.ts — Global Decorators and Theme/Mode Switching

```typescript
import type { Preview } from "@storybook/react";
import React from "react";
import "../src/app/globals.css"; // Your CSS file with theme variables

const preview: Preview = {
  globalTypes: {
    theme: {
      description: "Design system theme",
      toolbar: {
        title: "Theme",
        icon: "paintbrush",
        items: [
          { value: "default", title: "Default (New York)" },
        ],
        dynamicTitle: true,
      },
    },
    mode: {
      description: "Color mode",
      toolbar: {
        title: "Mode",
        icon: "mirror",
        items: [
          { value: "light", title: "Light" },
          { value: "dark", title: "Dark" },
        ],
        dynamicTitle: true,
      },
    },
  },
  initialGlobals: {
    theme: "default",
    mode: "light",
  },
  decorators: [
    (Story, context) => {
      const theme = context.globals.theme || "default";
      const mode = context.globals.mode || "light";

      React.useEffect(() => {
        const root = document.documentElement;
        // Remove all theme classes, then apply selected
        root.classList.remove("theme-default");
        root.classList.add(`theme-${theme}`);
        // Toggle dark mode
        root.classList.toggle("dark", mode === "dark");
      }, [theme, mode]);

      return <Story />;
    },
  ],
  parameters: {
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/i,
      },
    },
    a11y: {
      // Enable accessibility checks for all stories
    },
  },
};

export default preview;
```

### Theme CSS Setup

Each theme defines its CSS variables. The theme switcher works by applying a theme class to the root element, which overrides the default variables:

```css
/* globals.css */
@import "tailwindcss";

/* Default theme (light) */
:root, .theme-default {
  --background: /* Default light background */;
  --foreground: /* Default light foreground */;
  /* ... all Default light variables from themes/default.md */
}

.dark, .theme-default.dark {
  --background: /* Default dark background */;
  --foreground: /* Default dark foreground */;
  /* ... all Default dark variables from themes/default.md */
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  /* ... register all variables as Tailwind utilities */
}
```

Variable values come from the theme file: `themes/default.md`.

---

## Story File Conventions

### Component Story Format (CSF)

Every story file uses CSF with the following structure:

```typescript
import type { Meta, StoryObj } from "@storybook/react";
import { ComponentName } from "@/components/ui/component-name";

const meta: Meta<typeof ComponentName> = {
  title: "Components/ComponentName",
  component: ComponentName,
  tags: ["autodocs"],
  argTypes: {
    // Map component props to controls
    variant: {
      control: "select",
      options: ["default", "destructive", "outline", "secondary", "ghost", "link"],
    },
    size: {
      control: "select",
      options: ["default", "sm", "lg", "icon"],
    },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

export const Default: Story = {
  args: {
    children: "Button",
  },
};

export const Destructive: Story = {
  args: {
    variant: "destructive",
    children: "Delete",
  },
};
```

### Naming Conventions

| Item Type | Story Title Pattern | File Location |
|-----------|-------------------|---------------|
| Component | `Components/[Name]` | `src/stories/components/[name].stories.tsx` |
| Pattern | `Patterns/[Name]` | `src/stories/patterns/[name].stories.tsx` |
| Template | `Templates/[Name]` | `src/stories/templates/[name].stories.tsx` |

### Story Requirements Per Component

Each component story file must include:

1. A `Default` story showing the component in its default state
2. One story per variant (if the component has variants)
3. Key composition patterns from the component spec (see `design-system/components/[name].md` → Composition Patterns)
4. Interactive controls for all configurable props
5. `tags: ["autodocs"]` for automatic documentation generation

### Pattern Stories

Pattern stories demonstrate composed functionality:

```typescript
// data-table.stories.tsx
import type { Meta, StoryObj } from "@storybook/react";

const meta: Meta = {
  title: "Patterns/Data Table",
  tags: ["autodocs"],
};

export default meta;

export const BasicWithSorting: StoryObj = {
  render: () => (
    // Compose Table + sorting + filtering using @tanstack/react-table
    // See design-system/patterns/data-table.md for composition details
  ),
};

export const WithRowSelection: StoryObj = { /* ... */ };
export const WithColumnVisibility: StoryObj = { /* ... */ };
export const WithRowActions: StoryObj = { /* ... */ };
```

### Template Stories

Template stories show full page layouts:

```typescript
// dashboard.stories.tsx
import type { Meta, StoryObj } from "@storybook/react";

const meta: Meta = {
  title: "Templates/Dashboard",
  tags: ["autodocs"],
  parameters: {
    layout: "fullscreen",
  },
};

export default meta;

export const Default: StoryObj = {
  render: () => (
    // Compose the dashboard layout from templates/dashboard-template.md
    // Sidebar + header + KPI cards + chart + data table
  ),
};
```

---

## Component Coverage

Generate story files for all design system items:

### Components (57)

One story file per component. Source: `design-system/components/[name].md`

Each component spec contains: variants, props, composition patterns, and accessibility requirements. Use these to generate comprehensive stories with controls.

### Patterns (2)

| Pattern | Story File | Key Stories |
|---------|-----------|-------------|
| Data Table | `data-table.stories.tsx` | Basic with sorting, row selection, column visibility, row actions |
| Date Picker | `date-picker.stories.tsx` | Single date, date range, with presets |

### Templates (4)

| Template | Story File | Key Stories |
|----------|-----------|-------------|
| Calendar | `calendar.stories.tsx` | Month view, week view, day view, agenda view |
| Dashboard | `dashboard.stories.tsx` | Full layout with sidebar, KPI cards, chart, data table |
| Login | `login.stories.tsx` | Centered card, split layout, full-width, with remember-me |
| Sidebar | `sidebar.stories.tsx` | Base template, inset variant, floating variant, collapsible |

---

## Accessibility Testing

Storybook's `@storybook/addon-a11y` runs automated accessibility checks on every story. This complements (but does not replace) the WCAG 2.2 requirements documented in each component spec.

The addon is included in the recommended Storybook configuration and runs automatically.

---

## Storybook UI Theme (Optional)

Customize the Storybook UI itself to match the design system branding:

```typescript
// .storybook/DSTheme.ts
import { create } from "storybook/theming";

export default create({
  base: "light",
  brandTitle: "Design System",
  brandUrl: "https://github.com/YOUR-ORG/YOUR-REPO",
  // Customize colors, fonts, etc.
});
```

```typescript
// .storybook/manager.ts
import { addons } from "storybook/manager-api";
import DSTheme from "./DSTheme";

addons.setConfig({
  theme: DSTheme,
});
```

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Theme CSS must be loaded globally | Cannot scope theme variables per story | Use global decorator to switch themes |
| Storybook 9+ requires Node.js 20+ | Older Node versions not supported | Upgrade Node.js |
| `@storybook/addon-a11y` is automated only | Does not replace manual WCAG testing | Use as complement to manual testing |
| Template stories need `layout: "fullscreen"` | Default padding clips full-page layouts | Set parameter per template story |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| Storybook Docs | `https://storybook.js.org/docs` | Installation, configuration, story format |
| Storybook Theming | `https://storybook.js.org/docs/configure/theming` | UI theme customization |
| Storybook Controls | `https://storybook.js.org/docs/essentials/controls` | Interactive arg controls |
| Storybook Globals | `https://storybook.js.org/docs/essentials/toolbars-and-globals` | Theme/mode toolbar switching |
| Storybook a11y | `https://storybook.js.org/docs/writing-tests/accessibility-testing` | Accessibility addon |
