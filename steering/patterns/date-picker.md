# Date Picker

## Metadata

- **Name:** Date Picker
- **Source:** shadcn (`https://ui.shadcn.com/docs/components/date-picker`)
- **Category:** Pattern (functional composition)
- **Composes:** Calendar, Popover, Button, Select
- **Dependencies:** `react-day-picker`, `date-fns`

## Behavior

Date selection input that composes Calendar, Popover, and Button components. User clicks the trigger button to open a popover containing a calendar. Selecting a date populates the button label and closes the popover. Supports three variants: single date, date range, and presets.

### Interaction Model

| Interaction | Trigger | Result |
|-------------|---------|--------|
| Open calendar | Click trigger Button | Popover opens with Calendar rendered inside |
| Select date (single) | Click a day in Calendar | Date state updates, button label shows formatted date, popover closes |
| Select date range | Click start day, then click end day | `DateRange` state updates with `from`/`to`, button shows range, popover stays open until both selected |
| Navigate month | Click left/right chevron in Calendar header | Calendar renders previous/next month |
| Navigate month/year | Use Select dropdowns in Calendar header | Calendar jumps to selected month/year |
| Select preset | Choose from Select component (presets variant) | Date state updates to preset value, calendar reflects selection |
| Clear selection | (Implementation-specific) | Date state resets, button shows placeholder text |

### State Management

Date state is managed via React `useState`. The pattern does not require a state management library.

```tsx
// Single date
const [date, setDate] = React.useState<Date>()

// Date range
const [date, setDate] = React.useState<DateRange | undefined>({
  from: new Date(2022, 0, 20),
  to: addDays(new Date(2022, 0, 20), 20),
})
```

## Anti-Patterns

| Anti-Pattern | Why It Fails | Correct Approach |
|-------------|-------------|-----------------|
| Using an Input field as the trigger instead of a Button | Input implies typing; date picker is a selection control | Use `Button variant="outline"` as the `PopoverTrigger` |
| Building a custom calendar instead of using the Calendar component | Duplicates date logic, breaks accessibility, loses keyboard nav | Use the Calendar component (wraps `react-day-picker`) |
| Keeping the popover open after single date selection | Confusing — user expects the picker to close after choosing | Close the popover on date selection (single mode) |
| Hardcoding date format strings | Breaks i18n, inconsistent across the app | Use `date-fns` `format()` with a consistent format pattern |
| Placing the calendar inline (not in a Popover) for space-constrained UIs | Takes permanent screen space, breaks the input-field mental model | Use Popover for on-demand display; use inline Calendar only for dedicated calendar views |
| Omitting the placeholder when no date is selected | Button appears empty or broken | Show placeholder text like "Pick a date" with `--muted-foreground` styling |

## Composed Components

| Component | Role | Required |
|-----------|------|----------|
| Popover | Container that positions the calendar dropdown relative to the trigger | Yes |
| PopoverTrigger | Wraps the Button that opens the popover | Yes |
| PopoverContent | Contains the Calendar (and optionally presets) | Yes |
| Button | Trigger element displaying the selected date or placeholder | Yes |
| Calendar | Date selection surface (wraps `react-day-picker`) | Yes |
| Select | Preset date ranges (presets variant only) | No |

## Composition Patterns

### Single Date Picker

Core pattern: Button trigger inside Popover, Calendar in PopoverContent.

```tsx
import { format } from "date-fns"
import { CalendarIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import {
  Popover, PopoverContent, PopoverTrigger,
} from "@/components/ui/popover"

function DatePickerDemo() {
  const [date, setDate] = React.useState<Date>()

  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button
          variant="outline"
          className={cn(
            "w-[280px] justify-start text-left font-normal",
            !date && "text-muted-foreground"
          )}
        >
          <CalendarIcon className="mr-2 h-4 w-4" />
          {date ? format(date, "PPP") : <span>Pick a date</span>}
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-auto p-0">
        <Calendar
          mode="single"
          selected={date}
          onSelect={setDate}
          initialFocus
        />
      </PopoverContent>
    </Popover>
  )
}
```

### Date Range Picker

Uses `mode="range"` on Calendar with `numberOfMonths={2}` to show two months side by side.

```tsx
import { addDays, format } from "date-fns"
import { DateRange } from "react-day-picker"
import { CalendarIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import {
  Popover, PopoverContent, PopoverTrigger,
} from "@/components/ui/popover"

function DatePickerWithRange({
  className,
}: React.HTMLAttributes<HTMLDivElement>) {
  const [date, setDate] = React.useState<DateRange | undefined>({
    from: new Date(2022, 0, 20),
    to: addDays(new Date(2022, 0, 20), 20),
  })

  return (
    <div className={cn("grid gap-2", className)}>
      <Popover>
        <PopoverTrigger asChild>
          <Button
            id="date"
            variant="outline"
            className={cn(
              "w-[300px] justify-start text-left font-normal",
              !date && "text-muted-foreground"
            )}
          >
            <CalendarIcon className="mr-2 h-4 w-4" />
            {date?.from ? (
              date.to ? (
                <>
                  {format(date.from, "LLL dd, y")} -{" "}
                  {format(date.to, "LLL dd, y")}
                </>
              ) : (
                format(date.from, "LLL dd, y")
              )
            ) : (
              <span>Pick a date</span>
            )}
          </Button>
        </PopoverTrigger>
        <PopoverContent className="w-auto p-0" align="start">
          <Calendar
            initialFocus
            mode="range"
            defaultMonth={date?.from}
            selected={date}
            onSelect={setDate}
            numberOfMonths={2}
          />
        </PopoverContent>
      </Popover>
    </div>
  )
}
```

### Date Picker with Presets

Adds a Select component alongside the Calendar for quick preset selections (Today, Tomorrow, In 3 days, In a week).

```tsx
import { addDays, format } from "date-fns"
import { CalendarIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import {
  Popover, PopoverContent, PopoverTrigger,
} from "@/components/ui/popover"
import {
  Select, SelectContent, SelectItem,
  SelectTrigger, SelectValue,
} from "@/components/ui/select"

function DatePickerWithPresets() {
  const [date, setDate] = React.useState<Date>()

  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button
          variant="outline"
          className={cn(
            "w-[280px] justify-start text-left font-normal",
            !date && "text-muted-foreground"
          )}
        >
          <CalendarIcon className="mr-2 h-4 w-4" />
          {date ? format(date, "PPP") : <span>Pick a date</span>}
        </Button>
      </PopoverTrigger>
      <PopoverContent className="flex w-auto flex-col space-y-2 p-2">
        <Select
          onValueChange={(value) =>
            setDate(addDays(new Date(), parseInt(value)))
          }
        >
          <SelectTrigger>
            <SelectValue placeholder="Select" />
          </SelectTrigger>
          <SelectContent position="popper">
            <SelectItem value="0">Today</SelectItem>
            <SelectItem value="1">Tomorrow</SelectItem>
            <SelectItem value="3">In 3 days</SelectItem>
            <SelectItem value="7">In a week</SelectItem>
          </SelectContent>
        </Select>
        <div className="rounded-md border">
          <Calendar
            mode="single"
            selected={date}
            onSelect={setDate}
          />
        </div>
      </PopoverContent>
    </Popover>
  )
}
```

## Accessibility

### WAI-ARIA Reference

`https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/` (popover is a dialog)
`https://www.w3.org/WAI/ARIA/apg/patterns/grid/` (calendar grid)

### ARIA Roles and Attributes

| Element | Role/Attribute | Purpose |
|---------|---------------|---------|
| Trigger Button | `aria-haspopup="dialog"`, `aria-expanded` | Indicates button opens a popover dialog |
| PopoverContent | `role="dialog"` | Identifies the popover as a dialog |
| Calendar grid | `role="grid"` (via `react-day-picker`) | Identifies the calendar as a navigable grid |
| Day buttons | `role="gridcell"` | Each day is a grid cell |
| Selected day | `aria-selected="true"` | Communicates selected state |
| Disabled day | `aria-disabled="true"` | Communicates non-selectable days |
| Month navigation | `aria-label="Go to previous month"` / `"Go to next month"` | Labels chevron navigation buttons |
| Placeholder text | `text-muted-foreground` class | Visual distinction; screen readers read button text content |

### Keyboard Navigation

| Key | Action |
|-----|--------|
| `Space` / `Enter` | Opens popover (on trigger), selects day (in calendar) |
| `Arrow Up/Down/Left/Right` | Navigates between days in the calendar grid |
| `Page Up` / `Page Down` | Navigates to previous/next month |
| `Shift + Page Up` / `Shift + Page Down` | Navigates to previous/next year |
| `Home` / `End` | Moves to first/last day of the week |
| `Escape` | Closes the popover, returns focus to trigger |
| `Tab` | Moves focus between popover elements (presets Select, calendar, navigation) |

## CSS Variable Dependencies

Inherits CSS variable dependencies from composed components (Calendar, Popover, Button, Select). No pattern-specific CSS variables.

Key inherited variables:

| Variable | Purpose | Source Component |
|----------|---------|-----------------|
| `--primary` | Selected day background | Calendar |
| `--primary-foreground` | Selected day text color | Calendar |
| `--accent` | Day hover background, today highlight | Calendar |
| `--accent-foreground` | Day hover text color | Calendar |
| `--muted-foreground` | Placeholder text, outside-month days, navigation icons | Button, Calendar |
| `--popover` | Popover background | Popover |
| `--popover-foreground` | Popover text color | Popover |
| `--border` | Popover border, calendar container border | Popover, Calendar |
| `--ring` | Focus ring on trigger button and day cells | Button, Calendar |
| `--background` | Trigger button background | Button |

## Theme Support

### Component Structure

The Date Picker is composed of a trigger input field and a Calendar dropdown.

#### Trigger States

| State | Filled | Unfilled |
|-------|--------|----------|
| Enabled | Shows formatted date | Shows placeholder text |
| Hover | Border highlight | Border highlight |
| Focus | Focus ring visible | Focus ring visible |
| Disabled | Reduced opacity, non-interactive | Reduced opacity, non-interactive |

#### Calendar Block Structure

The calendar dropdown uses the following structure:

| Section | Content |
|---------|---------|
| Header | Month Select dropdown + Year Select dropdown + left/right chevron navigation buttons |
| Day grid | 7-column grid (Su–Sa headers), day buttons in rows |
| Day states | Default, Selected (`--primary` background), Hover (`--accent` background), Today (outlined or `--accent` background), Outside-month (50% opacity) |

#### Navigation

- Left chevron: previous month
- Right chevron: next month
- Month dropdown: jump to specific month
- Year dropdown: jump to specific year

#### Date Picker Example Layout (node `7008:49170`)

Full assembled view: Date Picker input field positioned above, Calendar 13 Block dropdown below. The calendar shows a single month with the month/year select dropdowns in the header, 7-column day grid, and chevron navigation.

### Theme Behavior

Inherits theme behavior from composed components. No pattern-specific theme overrides.

| State | Behavior |
|-------|----------|
| Trigger (no date) | `--muted-foreground` text, `--background` fill |
| Trigger (date selected) | `--foreground` text, `--background` fill |
| Calendar day (default) | `--background` or transparent |
| Calendar day (hover) | `--accent` background |
| Calendar day (selected) | `--primary` background, `--primary-foreground` text |
| Calendar day (today) | `--accent` background (subtle highlight) |
| Calendar day (outside month) | 50% opacity, `--muted-foreground` text |
| Calendar day (disabled) | `--muted-foreground` text, reduced opacity |
| Light/Dark | All values resolve from theme CSS variables — no hardcoded colors |
