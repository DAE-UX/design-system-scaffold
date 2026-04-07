# Component: Chart

> **TL;DR:** A charting system built on Recharts. 6 exported parts: ChartContainer, ChartTooltip, ChartTooltipContent, ChartLegend, ChartLegendContent, ChartStyle. Uses a `ChartConfig` object to define labels, icons, and color tokens per data series. Supports CSS variable theming with automatic light/dark mode via `ChartStyle` injected `<style>` tag. Composable with all Recharts chart types (Bar, Line, Area, Pie, Radar, Radial). Requires `min-h-[VALUE]` on ChartContainer for responsive sizing. Supports `accessibilityLayer` prop on Recharts charts for keyboard/screen reader access.

## Metadata

- **Component Name:** Chart
- **Source:** shadCN (recharts)
- **Dependencies:** recharts@2.15.4, lucide-react

## Behavior

A charting system that wraps Recharts with shadCN theming and custom tooltip/legend components. Does not abstract Recharts — you compose charts using standard Recharts components and add shadCN tooltip/legend where needed.

- `ChartContainer` wraps `ResponsiveContainer` from Recharts and provides chart context
- `ChartConfig` defines labels, icons, and colors per data series — decoupled from chart data
- Colors in config can be CSS variables (recommended: `var(--chart-1)`), or theme objects with light/dark keys. Hex, hsl, and oklch formats are also accepted by Recharts but CSS variables are preferred for theme compatibility.
- `ChartStyle` injects a `<style>` tag that generates `--color-[key]` CSS variables from the config, scoped to `[data-chart=ID]`
- Light/dark theme support: `ChartStyle` generates rules for both `:root` and `.dark` selectors
- `ChartTooltip` is a re-export of Recharts `Tooltip` — use with `ChartTooltipContent` for styled tooltips
- `ChartTooltipContent` supports 3 indicator styles: `dot`, `line`, `dashed`
- `ChartTooltipContent` supports `hideLabel`, `hideIndicator`, `labelKey`, `nameKey` props
- `ChartLegend` is a re-export of Recharts `Legend` — use with `ChartLegendContent` for styled legends
- `ChartLegendContent` supports `hideIcon`, `nameKey`, `verticalAlign` props
- Requires `min-h-[VALUE]` on ChartContainer for responsive chart sizing
- Recharts components reference colors via `var(--color-[key])` format (e.g., `fill="var(--color-desktop)"`)
- `accessibilityLayer` prop on Recharts chart components adds keyboard and screen reader support
- ChartContainer applies global Recharts element styling via descendant selectors (axis ticks, grid lines, cursors, etc.)

### Anti-Patterns

#### Do
- Maintain consistent color order across chart segments
- Limit pie/donut charts to 5 segments max
- Limit metric breakdowns to 7 items
- Skip axis labels for regular time intervals to reduce clutter

#### Don't
- Don't use pie/donut charts for time series — use bar or line charts
- Don't use pie charts for a single metric
- Don't truncate names if unique identifiers are lost
- Don't add links to both metric key and value — pick one
- Don't show time units on Y axis
- Don't use a filter when only one metric exists
- Don't use a legend when only one metric is shown and the title explains it
- Don't use area charts for single series (use line), negative data (use line/bar), or non-stacked data (use line)

Sources: shadCN, Radix UI

## API

### Parts

- `ChartContainer` — Root wrapper. Provides ChartConfig context, generates unique chart ID, injects ChartStyle, wraps ResponsiveContainer.
- `ChartStyle` — Injects `<style>` tag with `--color-[key]` CSS variables for light/dark themes. Not used directly.
- `ChartTooltip` — Re-export of Recharts `Tooltip`. Use with `content={<ChartTooltipContent />}`.
- `ChartTooltipContent` — Custom tooltip renderer with indicator, label, and value display.
- `ChartLegend` — Re-export of Recharts `Legend`. Use with `content={<ChartLegendContent />}`.
- `ChartLegendContent` — Custom legend renderer with color indicators and config-driven labels.

### Props

#### ChartContainer

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| config | `ChartConfig` | (required) | Chart configuration with labels, icons, and colors |
| id | `string` | auto-generated | Chart identifier for CSS variable scoping |
| className | `string` | — | Additional CSS classes (use `min-h-[VALUE]` for sizing) |
| children | `ReactElement` | (required) | Recharts chart component |

#### ChartConfig (type)

```typescript
type ChartConfig = {
  [key: string]: {
    label?: React.ReactNode
    icon?: React.ComponentType
  } & (
    | { color?: string; theme?: never }
    | { color?: never; theme: Record<"light" | "dark", string> }
  )
}
```

#### ChartTooltipContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| indicator | `"dot" \| "line" \| "dashed"` | `"dot"` | Indicator style |
| hideLabel | `boolean` | `false` | Hide the tooltip label |
| hideIndicator | `boolean` | `false` | Hide the color indicator |
| labelKey | `string` | — | Custom key for tooltip label lookup |
| nameKey | `string` | — | Custom key for tooltip name lookup |
| labelFormatter | `function` | — | Custom label formatter |
| formatter | `function` | — | Custom value formatter |
| labelClassName | `string` | — | Additional CSS classes for label |

#### ChartLegendContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| hideIcon | `boolean` | `false` | Hide the color icon |
| nameKey | `string` | — | Custom key for legend name lookup |
| verticalAlign | `"top" \| "bottom"` | `"bottom"` | Legend position |

### Variants

No CVA variants. Chart appearance is controlled by Recharts component props and ChartConfig colors.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"chart"` | ChartContainer root |
| `[data-chart]` | `"chart-{id}"` | ChartContainer root (CSS variable scope) |

### CSS Variables (Radix)

Not applicable — Chart uses Recharts, not Radix primitives.

Generated CSS variables (from ChartConfig):

| Variable Pattern | Description |
|-----------------|-------------|
| `--color-[key]` | Color for each data series, generated by ChartStyle from ChartConfig |

## Composition Patterns

### Basic Bar Chart

```tsx
const chartConfig = {
  desktop: { label: "Desktop", color: "var(--chart-1)" },
  mobile: { label: "Mobile", color: "var(--chart-2)" },
} satisfies ChartConfig

<ChartContainer config={chartConfig} className="min-h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

### With Grid and Axis

```tsx
<ChartContainer config={chartConfig} className="min-h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis
      dataKey="month"
      tickLine={false}
      tickMargin={10}
      axisLine={false}
      tickFormatter={(value) => value.slice(0, 3)}
    />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

### With Tooltip

```tsx
<ChartContainer config={chartConfig} className="min-h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis dataKey="month" tickLine={false} tickMargin={10} axisLine={false} />
    <ChartTooltip content={<ChartTooltipContent />} />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

### With Legend

```tsx
<ChartContainer config={chartConfig} className="min-h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis dataKey="month" tickLine={false} tickMargin={10} axisLine={false} />
    <ChartTooltip content={<ChartTooltipContent />} />
    <ChartLegend content={<ChartLegendContent />} />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

### Theme Object Colors (Light/Dark)

```tsx
const chartConfig = {
  desktop: {
    label: "Desktop",
    theme: {
      light: "var(--chart-1)",
      dark: "var(--chart-1)",
    },
  },
} satisfies ChartConfig
```

### Custom Tooltip Keys

```tsx
<ChartTooltip
  content={<ChartTooltipContent labelKey="visitors" nameKey="browser" />}
/>
```

### With Icon in Config

```tsx
import { Monitor } from "lucide-react"

const chartConfig = {
  desktop: {
    label: "Desktop",
    icon: Monitor,
    color: "var(--chart-1)",
  },
} satisfies ChartConfig
```

## Accessibility

- **WAI-ARIA Pattern:** Use `accessibilityLayer` prop on Recharts chart components for keyboard and screen reader support

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| ChartContainer | `data-slot="chart"` | Chart container |
| Recharts chart | `accessibilityLayer` prop | Adds keyboard access and screen reader support when enabled |

### Keyboard Behavior

When `accessibilityLayer` is enabled on the Recharts chart component:

| Key | Behavior |
|-----|----------|
| Tab | Focus the chart area |
| Arrow Left / Right | Navigate between data points |
| Arrow Up / Down | Navigate between data series |

## HTML

### Standalone HTML Structure

```html
<div data-slot="chart" data-chart="chart-abc123" class="chart-container">
  <style>
    /* Generated by ChartStyle from ChartConfig — resolves color/theme values to --color-[key] variables */
    [data-chart=chart-abc123] { --color-desktop: var(--chart-1); --color-mobile: var(--chart-2); }
    .dark [data-chart=chart-abc123] { --color-desktop: var(--chart-1); --color-mobile: var(--chart-2); }
  </style>
  <div class="recharts-responsive-container">
    <!-- Recharts SVG output -->
    <svg>
      <!-- Chart elements (bars, lines, axes, etc.) -->
    </svg>
  </div>
</div>
```

### Tooltip Structure

```html
<div class="tooltip">
  <div class="font-medium">January</div>
  <div class="tooltip-item">
    <div class="indicator dot" style="--color-bg: var(--color-desktop);"></div>
    <div class="tooltip-content">
      <span class="text-muted-foreground">Desktop</span>
      <span class="font-mono font-medium tabular-nums">186</span>
    </div>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* ChartContainer */
.ChartContainer {
  display: flex;
  aspect-ratio: 16 / 9;
  justify-content: center;
  font-size: 0.75rem;
}

/* Recharts axis tick text */
.ChartContainer .recharts-cartesian-axis-tick text {
  fill: var(--muted-foreground);
}

/* Recharts grid lines */
.ChartContainer .recharts-cartesian-grid line[stroke='#ccc'] {
  stroke: color-mix(in srgb, var(--border) 50%, transparent);
}

/* Recharts tooltip cursor (line) */
.ChartContainer .recharts-curve.recharts-tooltip-cursor {
  stroke: var(--border);
}

/* Recharts tooltip cursor (rect) */
.ChartContainer .recharts-rectangle.recharts-tooltip-cursor {
  fill: var(--muted);
}

/* Recharts dot stroke */
.ChartContainer .recharts-dot[stroke='#fff'] {
  stroke: transparent;
}

/* Recharts sector stroke */
.ChartContainer .recharts-sector[stroke='#fff'] {
  stroke: transparent;
}

/* Recharts polar grid */
.ChartContainer .recharts-polar-grid [stroke='#ccc'] {
  stroke: var(--border);
}

/* Recharts radial bar background */
.ChartContainer .recharts-radial-bar-background-sector {
  fill: var(--muted);
}

/* Recharts focus outlines */
.ChartContainer .recharts-layer,
.ChartContainer .recharts-sector,
.ChartContainer .recharts-surface {
  outline: none;
}

/* ChartTooltipContent */
.ChartTooltipContent {
  display: grid;
  min-width: 8rem;
  align-items: start;
  gap: 0.375rem;
  border-radius: 0.5rem;
  border: 1px solid color-mix(in srgb, var(--border) 50%, transparent);
  background-color: var(--background);
  padding: 0.375rem 0.625rem;
  font-size: 0.75rem;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
}

/* Tooltip indicator: dot */
.ChartTooltipContent .indicator-dot {
  width: 0.625rem;
  height: 0.625rem;
  flex-shrink: 0;
  border-radius: 2px;
}

/* Tooltip indicator: line */
.ChartTooltipContent .indicator-line {
  width: 0.25rem;
  flex-shrink: 0;
  border-radius: 2px;
}

/* Tooltip indicator: dashed */
.ChartTooltipContent .indicator-dashed {
  width: 0;
  border-width: 1.5px;
  border-style: dashed;
  background: transparent;
  flex-shrink: 0;
}

/* Tooltip value */
.ChartTooltipContent .value {
  font-family: monospace;
  font-weight: 500;
  font-variant-numeric: tabular-nums;
  color: var(--foreground);
}

/* ChartLegendContent */
.ChartLegendContent {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.ChartLegendContent[data-align="top"] {
  padding-bottom: 0.75rem;
}

.ChartLegendContent[data-align="bottom"] {
  padding-top: 0.75rem;
}

/* Legend color indicator */
.ChartLegendContent .color-indicator {
  width: 0.5rem;
  height: 0.5rem;
  flex-shrink: 0;
  border-radius: 2px;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| ChartContainer | `flex aspect-video justify-center text-xs` | Container layout |
| ChartContainer (axis ticks) | `[&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground` | Axis text color |
| ChartContainer (grid lines) | `[&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50` | Grid line color |
| ChartContainer (tooltip cursor line) | `[&_.recharts-curve.recharts-tooltip-cursor]:stroke-border` | Cursor stroke |
| ChartContainer (tooltip cursor rect) | `[&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted` | Cursor fill |
| ChartContainer (dot stroke) | `[&_.recharts-dot[stroke='#fff']]:stroke-transparent` | Remove white dot stroke |
| ChartContainer (sector stroke) | `[&_.recharts-sector[stroke='#fff']]:stroke-transparent` | Remove white sector stroke |
| ChartContainer (polar grid) | `[&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border` | Polar grid color |
| ChartContainer (radial bg) | `[&_.recharts-radial-bar-background-sector]:fill-muted` | Radial bar background |
| ChartContainer (outlines) | `[&_.recharts-layer]:outline-hidden [&_.recharts-sector]:outline-hidden [&_.recharts-surface]:outline-hidden` | Remove focus outlines |
| ChartTooltipContent | `grid min-w-[8rem] items-start gap-1.5 rounded-lg border border-border/50 bg-background px-2.5 py-1.5 text-xs shadow-xl` | Tooltip container |
| Tooltip indicator (dot) | `h-2.5 w-2.5 shrink-0 rounded-[2px]` | Dot indicator |
| Tooltip indicator (line) | `w-1 shrink-0 rounded-[2px]` | Line indicator |
| Tooltip indicator (dashed) | `w-0 border-[1.5px] border-dashed bg-transparent shrink-0` | Dashed indicator |
| Tooltip value | `font-mono font-medium text-foreground tabular-nums` | Value typography |
| Tooltip name | `text-muted-foreground` | Name color |
| ChartLegendContent | `flex items-center justify-center gap-4` | Legend layout |
| ChartLegendContent (top) | `pb-3` | Top-aligned padding |
| ChartLegendContent (bottom) | `pt-3` | Bottom-aligned padding |
| Legend color indicator | `h-2 w-2 shrink-0 rounded-[2px]` | Color swatch |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--muted-foreground` | Axis tick text, tooltip name text, legend icon color | `shadcn` |
| `--border` | Grid lines, tooltip cursor stroke, polar grid, tooltip border (at 50% opacity) | `shadcn` |
| `--muted` | Tooltip cursor fill (rect), radial bar background | `shadcn` |
| `--background` | Tooltip background | `shadcn` |
| `--foreground` | Tooltip value text | `shadcn` |
| `--chart-1` through `--chart-5` | Default chart color palette (defined in theme CSS) | `shadcn` |

## Theme Support

### Component Structure

Chart contains 6 chart categories and subcomponents:

**Area Charts** — 8 variants: Interactive, Curved, Linear, Step, Stacked, Stacked Expanded, Gradient, Axes

**Bar Charts** — 10 variants: Interactive, Default, Horizontal, Multiple, Stacked, Label, Custom Label, Mixed, Active, Negative

**Pie Charts** — 8 variants: Default, Label, Label List, Donut, Donut Active, Donut with Text, Stacked, Interactive

**Radar Charts** — 11 variants: Default, Dots, Lines Only, Custom Label, Grid Custom, Grid None, Grid Circle, Grid Circle - No lines, Grid Circle Filled, Grid Filled, Multiple

**Radial Charts** — 6 variants: Default, Label, Grid, Text, Shape, Stacked

**Chart Subcomponents:**

| Sub-component | Variants |
|--------------|----------|
| Tooltip | Composed tooltip with title, items, footer |
| Tooltip Item | Type: Default, Icon, Line, Total |
| Legend Item | Type: Legend Item, Icon |
| Legend | Composed legend bar |
| Chart Dot | Single 8×8px dot indicator |


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--background)` | `--background` | Tooltip background |
| `var(--border)` | `--border` | Tooltip border |
| `var(--foreground)` | `--foreground` | Tooltip title, item value text |
| `var(--muted-foreground)` | `--muted-foreground` | Item label, unit text |
| `var(--chart-1)` | `--chart-1` | Chart color 1 |
| `var(--chart-3)` | `--chart-3` | Chart color 3 |
| `var(--chart-5)` | `--chart-5` | Chart color 5 |
| `--radius` (derived) | `--radius` | Tooltip border radius |
| `px-2.5` (10px) | `px-2.5` | Tooltip horizontal padding |
| `py-1.5` (6px) | `py-1.5` | Tooltip vertical padding |
| `font-sans` | `--font-sans` | Font family |
| `text-xs` | `text-xs` | All tooltip text size |
| `shadow-lg` | `shadow-lg` | Tooltip box shadow |

### Theme Behavior

- ChartContainer has no background — inherits from parent
- Axis ticks use `--muted-foreground` — adapts to light/dark via theme
- Grid lines use `--border` at 50% opacity — adapts to light/dark via theme
- Tooltip uses `--background` bg and `--border` border — adapts to light/dark via theme
- Tooltip values use `--foreground` — adapts to light/dark via theme
- ChartStyle generates separate CSS rules for `:root` (light) and `.dark` (dark) selectors
- When using `theme` object in ChartConfig, different colors are applied per mode automatically
- When using `color` string in ChartConfig, the same color is used in both modes
- `--chart-1` through `--chart-5` are defined in the global theme CSS with different light/dark values
- Recharts default white strokes (`#fff`) are overridden to `transparent` for theme compatibility
- Recharts default gray strokes (`#ccc`) are overridden to use `--border` for theme compatibility
