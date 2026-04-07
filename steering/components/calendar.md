# Component: Calendar

> **TL;DR:** A date picker calendar built on react-day-picker (DayPicker). Supports single, range, and multiple date selection modes. Customizable cell size via `--cell-size` CSS variable. Includes dropdown caption layout for month/year selection. Supports timezone-aware selection, week numbers, Persian/Hijri calendars, RTL, and Embla-style plugins. 2 custom sub-components: Calendar (wrapper) and CalendarDayButton (day cell). Navigation via ChevronLeft/Right icons with ghost buttons.

## Metadata

- **Component Name:** Calendar
- **Source:** shadCN (react-day-picker)
- **Dependencies:** react-day-picker@latest, date-fns

## Behavior

A date selection calendar built on top of react-day-picker's DayPicker component. Provides styled day cells, navigation, and caption layouts.

- Wraps `DayPicker` from react-day-picker with shadCN styling
- Supports `mode="single"`, `mode="range"`, and `mode="multiple"` selection
- Shows outside days by default (`showOutsideDays={true}`)
- Default caption layout is `"label"` — can switch to `"dropdown"` for month/year selectors
- Cell size controlled via `--cell-size` CSS variable (default: `--spacing(8)` / 2rem)
- Cell size is responsive: use breakpoint-specific values (e.g., `[--cell-size:--spacing(11)] md:[--cell-size:--spacing(12)]`)
- Navigation buttons use the Button component with configurable `buttonVariant` (default: `"ghost"`)
- CalendarDayButton renders each day as a Button with `variant="ghost"` and `size="icon"`
- Selected single dates get `--primary` / `--primary-foreground` styling
- Range start/end get `--primary` background with rounded corners; range middle gets `--accent` background
- Today's date highlighted with `--accent` / `--accent-foreground`
- Outside days and disabled days use `--muted-foreground` with reduced opacity
- Supports timezone-aware selection via `timeZone` prop (detect client-side to avoid hydration mismatch)
- Supports week numbers via `showWeekNumber` prop
- Supports Persian/Hijri/Jalali calendar by swapping import to `react-day-picker/persian`
- Supports RTL via `locale` and `dir` props
- Transparent background when nested inside `[data-slot=card-content]` or `[data-slot=popover-content]`
- Composable with Popover for date picker patterns

### Anti-Patterns

#### Do
- Use for date selection within a defined range.
- Disable dates outside valid ranges.
- Show the current date as a reference point.

#### Don't
- Don't use for time selection — pair with a separate time picker.
- Don't allow selection of disabled dates.
- Don't hide navigation controls.

Sources: shadCN, Radix UI

## API

### Parts

- `Calendar` — Wrapper around DayPicker. Applies shadCN class names, navigation icons, and day button styling.
- `CalendarDayButton` — Custom day cell component. Renders as a Button with selection state data attributes.

### Props

#### Calendar

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| mode | `"single" \| "range" \| "multiple"` | — | Selection mode |
| selected | `Date \| DateRange \| Date[]` | — | Currently selected date(s) |
| onSelect | `function` | — | Selection change handler |
| showOutsideDays | `boolean` | `true` | Show days from adjacent months |
| captionLayout | `"label" \| "dropdown"` | `"label"` | Caption display mode |
| buttonVariant | `ButtonVariant` | `"ghost"` | Variant for navigation buttons |
| showWeekNumber | `boolean` | `false` | Show week numbers column |
| timeZone | `string` | — | IANA timezone for date selection |
| locale | `Locale` | — | Locale object from react-day-picker/locale |
| dir | `"ltr" \| "rtl"` | — | Text direction |
| formatters | `object` | — | Custom date formatters |
| components | `object` | — | Custom sub-component overrides |
| classNames | `object` | — | Custom class name overrides per element |
| className | `string` | — | Additional CSS classes |

#### CalendarDayButton

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| day | `CalendarDay` | — | Day object from react-day-picker |
| modifiers | `Modifiers` | — | Day state modifiers (selected, range_start, range_end, etc.) |
| className | `string` | — | Additional CSS classes |

### Variants

No CVA variants. Visual states are controlled by react-day-picker modifiers and data attributes on CalendarDayButton.

| State | Description | Visual Treatment |
|-------|-------------|-----------------|
| Selected (single) | Single selected date | `--primary` bg, `--primary-foreground` text |
| Range start | First date in range | `--primary` bg, rounded start |
| Range end | Last date in range | `--primary` bg, rounded end |
| Range middle | Dates between start and end | `--accent` bg, no rounding |
| Today | Current date | `--accent` bg, `--accent-foreground` text |
| Outside | Days from adjacent months | `--muted-foreground` text |
| Disabled | Non-selectable days | `--muted-foreground` text, 50% opacity |
| Focused | Keyboard-focused day | Focus ring with `--ring` |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"calendar"` | Root element |
| `[data-day]` | Localized date string | CalendarDayButton |
| `[data-selected-single]` | `"true" \| "false"` | CalendarDayButton (single selection, not range) |
| `[data-range-start]` | `"true" \| "false"` | CalendarDayButton |
| `[data-range-end]` | `"true" \| "false"` | CalendarDayButton |
| `[data-range-middle]` | `"true" \| "false"` | CalendarDayButton |
| `[data-selected]` | `"true"` | Day cell (from react-day-picker) |
| `[data-focused]` | `"true"` | Day cell (from react-day-picker) |

### CSS Variables (Radix)

Not applicable — Calendar uses react-day-picker, not Radix primitives.

| Variable | Default | Description |
|----------|---------|-------------|
| `--cell-size` | `--spacing(8)` (2rem) | Width and height of each day cell |

## Composition Patterns

### Basic Single Selection

```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-lg border"
/>
```

### Range Selection

```tsx
<Calendar
  mode="range"
  selected={dateRange}
  onSelect={setDateRange}
  className="rounded-lg border"
/>
```

### Month/Year Dropdown

```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  captionLayout="dropdown"
  className="rounded-lg border"
/>
```

### With Timezone

```tsx
const [timeZone, setTimeZone] = React.useState<string | undefined>(undefined)

React.useEffect(() => {
  setTimeZone(Intl.DateTimeFormat().resolvedOptions().timeZone)
}, [])

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  timeZone={timeZone}
/>
```

### Custom Cell Size

```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-lg border [--cell-size:--spacing(11)] md:[--cell-size:--spacing(12)]"
/>
```

### With Week Numbers

```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  showWeekNumber
  className="rounded-lg border"
/>
```

### RTL with Locale

```tsx
import { arSA } from "react-day-picker/locale"

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  locale={arSA}
  dir="rtl"
/>
```

### Date Picker (with Popover)

```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">{format(date, "PPP")}</Button>
  </PopoverTrigger>
  <PopoverContent>
    <Calendar mode="single" selected={date} onSelect={setDate} />
  </PopoverContent>
</Popover>
```

## Accessibility

- **WAI-ARIA Pattern:** Grid pattern — react-day-picker renders a `table` with `role` attributes for grid navigation

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Root | `data-slot="calendar"` | Calendar container |
| Navigation buttons | `button` with `aria-disabled` | Previous/next month navigation |
| Day cell | `gridcell` | Each day in the calendar grid (from react-day-picker) |
| Day button | `button` | Interactive day selection |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Up | Move focus to same day in previous week |
| Arrow Down | Move focus to same day in next week |
| Arrow Left | Move focus to previous day |
| Arrow Right | Move focus to next day |
| Home | Move focus to start of week |
| End | Move focus to end of week |
| Page Up | Move focus to same day in previous month |
| Page Down | Move focus to same day in next month |
| Space / Enter | Select the focused day |

## HTML

### Standalone HTML Structure

```html
<div data-slot="calendar" class="calendar">
  <!-- Navigation -->
  <div class="nav">
    <button aria-disabled="false">
      <svg><!-- ChevronLeft --></svg>
    </button>
    <div class="caption">March 2026</div>
    <button aria-disabled="false">
      <svg><!-- ChevronRight --></svg>
    </button>
  </div>
  <!-- Month grid -->
  <table>
    <thead>
      <tr>
        <th>Su</th><th>Mo</th><th>Tu</th><th>We</th><th>Th</th><th>Fr</th><th>Sa</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><button data-day="3/1/2026">1</button></td>
        <td><button data-day="3/2/2026">2</button></td>
        <!-- ... -->
      </tr>
      <!-- ... -->
    </tbody>
  </table>
</div>
```

## CSS

### Raw CSS

```css
/* Calendar root */
.Calendar {
  background-color: var(--background);
  padding: 0.75rem;
}

/* Cell size variable */
.Calendar {
  --cell-size: 2rem;
}

/* Months container */
.Calendar .months {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

/* Navigation */
.Calendar .nav {
  position: absolute;
  inset-inline: 0;
  top: 0;
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  gap: 0.25rem;
}

/* Navigation buttons */
.Calendar .button_previous,
.Calendar .button_next {
  width: var(--cell-size);
  height: var(--cell-size);
  padding: 0;
  user-select: none;
}
.Calendar .button_previous[aria-disabled="true"],
.Calendar .button_next[aria-disabled="true"] {
  opacity: 0.5;
}

/* Month caption */
.Calendar .month_caption {
  display: flex;
  height: var(--cell-size);
  width: 100%;
  align-items: center;
  justify-content: center;
}

/* Weekday headers */
.Calendar .weekday {
  flex: 1;
  border-radius: var(--radius);
  font-size: 0.8rem;
  font-weight: 400;
  color: var(--muted-foreground);
  user-select: none;
}

/* Week number */
.Calendar .week_number {
  font-size: 0.8rem;
  color: var(--muted-foreground);
  user-select: none;
}

/* Day cell */
.Calendar .day {
  position: relative;
  aspect-ratio: 1;
  height: 100%;
  width: 100%;
  padding: 0;
  text-align: center;
  user-select: none;
}

/* Today */
.Calendar .today {
  border-radius: var(--radius);
  background-color: var(--accent);
  color: var(--accent-foreground);
}

/* Outside days */
.Calendar .outside {
  color: var(--muted-foreground);
}

/* Disabled days */
.Calendar .disabled {
  color: var(--muted-foreground);
  opacity: 0.5;
}

/* Range start */
.Calendar .range_start {
  border-start-start-radius: var(--radius);
  border-end-start-radius: var(--radius);
  background-color: var(--accent);
}

/* Range end */
.Calendar .range_end {
  border-start-end-radius: var(--radius);
  border-end-end-radius: var(--radius);
  background-color: var(--accent);
}

/* CalendarDayButton */
.CalendarDayButton {
  display: flex;
  aspect-ratio: 1;
  width: 100%;
  min-width: var(--cell-size);
  flex-direction: column;
  gap: 0.25rem;
  line-height: 1;
  font-weight: 400;
}

/* Selected single */
.CalendarDayButton[data-selected-single="true"] {
  background-color: var(--primary);
  color: var(--primary-foreground);
}

/* Range start/end */
.CalendarDayButton[data-range-start="true"],
.CalendarDayButton[data-range-end="true"] {
  background-color: var(--primary);
  color: var(--primary-foreground);
  border-radius: var(--radius);
}

/* Range middle */
.CalendarDayButton[data-range-middle="true"] {
  background-color: var(--accent);
  color: var(--accent-foreground);
  border-radius: 0;
}

/* Focused day */
.CalendarDayButton:focus-visible {
  position: relative;
  z-index: 10;
  border-color: var(--ring);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--ring) 50%, transparent);
}

/* Dropdown (caption layout) */
.Calendar .dropdown_root {
  position: relative;
  border-radius: var(--radius);
  border: 1px solid var(--input);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}

.Calendar .dropdown {
  position: absolute;
  inset: 0;
  background-color: var(--popover);
  opacity: 0;
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Calendar (root) | `group/calendar bg-background p-3 [--cell-size:--spacing(8)]` | Container with cell size variable |
| Calendar (in card/popover) | `[[data-slot=card-content]_&]:bg-transparent [[data-slot=popover-content]_&]:bg-transparent` | Transparent when nested |
| months | `relative flex flex-col gap-4 md:flex-row` | Month grid layout |
| nav | `absolute inset-x-0 top-0 flex w-full items-center justify-between gap-1` | Navigation positioning |
| button_previous / button_next | `size-(--cell-size) p-0 select-none aria-disabled:opacity-50` | Nav button sizing |
| month_caption | `flex h-(--cell-size) w-full items-center justify-center px-(--cell-size)` | Caption centering |
| weekday | `flex-1 rounded-md text-[0.8rem] font-normal text-muted-foreground select-none` | Weekday header |
| week | `mt-2 flex w-full` | Week row |
| day | `group/day relative aspect-square h-full w-full p-0 text-center select-none` | Day cell |
| today | `rounded-md bg-accent text-accent-foreground data-[selected=true]:rounded-none` | Today highlight |
| outside | `text-muted-foreground aria-selected:text-muted-foreground` | Outside day styling |
| disabled | `text-muted-foreground opacity-50` | Disabled day styling |
| range_start | `rounded-s-md bg-accent` | Range start (logical property for RTL) |
| range_end | `rounded-e-md bg-accent` | Range end (logical property for RTL) |
| CalendarDayButton | `flex aspect-square size-auto w-full min-w-(--cell-size) flex-col gap-1 leading-none font-normal` | Day button layout |
| CalendarDayButton (selected) | `data-[selected-single=true]:bg-primary data-[selected-single=true]:text-primary-foreground` | Selected state |
| CalendarDayButton (range) | `data-[range-start=true]:bg-primary data-[range-start=true]:text-primary-foreground data-[range-end=true]:bg-primary data-[range-end=true]:text-primary-foreground data-[range-middle=true]:bg-accent data-[range-middle=true]:text-accent-foreground` | Range states |
| CalendarDayButton (focus) | `group-data-[focused=true]/day:border-ring group-data-[focused=true]/day:ring-[3px] group-data-[focused=true]/day:ring-ring/50` | Focus ring |
| dropdown_root | `relative rounded-md border border-input shadow-xs has-focus:border-ring has-focus:ring-[3px] has-focus:ring-ring/50` | Dropdown container |
| dropdown | `absolute inset-0 bg-popover opacity-0` | Hidden native select |
| caption_label (dropdown) | `flex h-8 items-center gap-1 rounded-md pr-1 pl-2 text-sm` | Dropdown caption |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--background` | Calendar root background | `shadcn` |
| `--primary` | Selected day background | `shadcn` |
| `--primary-foreground` | Selected day text | `shadcn` |
| `--accent` | Today / range middle background | `shadcn` |
| `--accent-foreground` | Today / range middle text | `shadcn` |
| `--muted-foreground` | Weekday headers, outside days, disabled days, week numbers | `shadcn` |
| `--muted` | Radial bar background sector (recharts integration) | `shadcn` |
| `--border` | Grid lines, tooltip cursor stroke | `shadcn` |
| `--ring` | Focus ring color | `shadcn` |
| `--input` | Dropdown border | `shadcn` |
| `--popover` | Dropdown background | `shadcn` |
| `--radius` | Border radius for cells and dropdowns | `shadcn` |

## Theme Support

### Component Structure

**.Calendar Day Button** — 4 variant properties:

| Variant Property | Values |
|-----------------|--------|
| Selected | False, True |
| State | Enabled, Disabled, Hover, Focus |
| Alignment | Central |
| Size | Default (32px), Large (48px), Custom days (52px) |

Default day button layout: flex column (32×32px, rounded from `--radius`, padding 6px, overflow clip) → Day number text (text-sm, font-normal, text-foreground, text-center)

**.Calendar Arrow Button** — States: Enabled, Hover, Focus, Disabled

**.Calendar Header** — Types: Default, Date dropdown, Month dropdown, Year dropdown

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--foreground)` | `--foreground` | Day number text color |
| `--radius` (derived) | `--radius` | Day button border radius |
| `size-8` (32px) | `size-8` | Default day button size |
| `p-1.5` (6px) | `p-1.5` | Day button padding |
| `font-sans` | `--font-sans` | Font family |
| `font-normal` | `font-normal` | Day number font weight |
| `text-sm` | `text-sm` | Day number font size |

### Theme Behavior

- Calendar root uses `--background` — adapts to light/dark via theme
- Selected days use `--primary` / `--primary-foreground` — adapts to light/dark via theme
- Today and range middle use `--accent` / `--accent-foreground` — adapts to light/dark via theme
- Outside and disabled days use `--muted-foreground` — adapts to light/dark via theme
- Dropdown elements use `--input` border and `--popover` background — adapts to light/dark via theme
- Focus ring uses `--ring` with 50% opacity — adapts to light/dark via theme
- `--cell-size` is a layout token, not theme-dependent
- Dark mode hover on day buttons uses `dark:hover:text-accent-foreground`
- Transparent background when nested in card-content or popover-content (inherits parent background)
