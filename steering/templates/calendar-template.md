# Calendar Template

## Metadata

- **Name:** Calendar Template
- **Source:** shadCN
- **Category:** Template (layout blueprint)
- **Uses:** Calendar, Sidebar (component), Card, Button, Popover, Select, Separator

## Layout Structure

Full-page calendar application layout. Organizes a calendar view as the primary content area with a sidebar for navigation and a top header bar. The layout follows the same SidebarProvider shell as the Sidebar Template and Dashboard Template.

The primary content area displays a calendar in month, week, or day view. A secondary sidebar or panel may provide event details, mini-calendar navigation, or filters.

```
+--------------------------------------------------+
| SidebarProvider                                  |
| +------------+-----------------------------------+
| |  Sidebar   |  SidebarInset                     |
| |            |  +-------------------------------+|
| |  Header    |  | Header (trigger + nav + view) ||
| |  -----     |  +-------------------------------+|
| |  Mini-Cal  |  |                               ||
| |  -----     |  |  Calendar View                ||
| |  Filters   |  |  (month / week / day grid)    ||
| |  -----     |  |                               ||
| |  Upcoming  |  |                               ||
| |            |  +-------------------------------+|
| +------------+-----------------------------------+
+--------------------------------------------------+
```

## Regions

| Region | Purpose | Components Used |
|--------|---------|----------------|
| Sidebar Header | Branding, workspace context | SidebarHeader, SidebarMenu, SidebarMenuButton |
| Sidebar Mini-Calendar | Month-at-a-glance date picker for quick navigation | Calendar (component), SidebarGroup |
| Sidebar Filters | Event type filters, calendar toggles | SidebarGroup, Checkbox, SidebarGroupLabel |
| Sidebar Upcoming | Upcoming events list | SidebarGroup, Card or custom list items |
| Content Header | Sidebar trigger, current date/range display, view switcher (Month/Week/Day), navigation arrows (prev/next), "Today" button | SidebarTrigger, Separator, Button, Select or Tabs |
| Content Main | Full calendar grid (month, week, or day view) | Custom grid layout using CSS Grid, Card for event items |

## Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| Desktop (lg+) | Sidebar visible with mini-calendar, filters, and upcoming events. Full calendar grid with all columns visible. View switcher uses Tabs or ToggleGroup. |
| Tablet (md-lg) | Sidebar collapsible via trigger. Calendar grid may reduce visible days or switch to a more compact layout. View switcher uses Select dropdown. |
| Mobile (<md) | Sidebar renders as Sheet overlay. Calendar defaults to day or agenda view. Mini-calendar accessible via Popover trigger. Navigation simplified to prev/next with date display. |

## Component Reference

### Layout Variants

21 calendar layout variants are available:

| Variant | Description |
|---------|-------------|
| Calendar 01 | Full month grid with sidebar (mini-calendar + upcoming events) |
| Calendar 02 | Week view with time slots and sidebar |
| Calendar 03 | Day view with hourly time slots |
| Calendar 04 | Month grid without sidebar (standalone) |
| Calendar 05 | Compact month grid with event dots |
| Calendar 06 | Week view with event cards in time slots |
| Calendar 07 | Agenda/list view of upcoming events |
| Calendar 08-10 | Variations on month/week layouts with different sidebar configurations |
| Calendar 11-15 | Variations with different header styles, view switchers, and filter panels |
| Calendar 16-21 | Additional layout variations including split views, overlay panels, and compact mobile layouts |

### Theme Behavior

Inherits theme behavior from composed components. No template-specific theme overrides. Calendar grid uses `--border` for cell borders, `--primary` / `--primary-foreground` for selected dates, `--accent` for hover states.

---

## Construct the Base Template (Calendar 01)

Build the full-page calendar layout using shadcn components. This produces a sidebar + calendar grid layout. The Calendar component (documented in `components/calendar.md`) provides the date picker; the page-level calendar grid is a custom layout.

### 1. Set Up the Page Shell

Use the same SidebarProvider pattern as the Dashboard Template. The sidebar contains a mini-calendar for date navigation.

```tsx
import { SidebarInset, SidebarProvider } from "@/components/ui/sidebar"

export default function CalendarPage() {
  return (
    <SidebarProvider>
      <CalendarSidebar />
      <SidebarInset>
        <CalendarHeader />
        <main className="flex flex-1 flex-col">
          <CalendarGrid />
        </main>
      </SidebarInset>
    </SidebarProvider>
  )
}
```

### 2. Build the Calendar Sidebar

The sidebar contains a mini-calendar (using the Calendar component), event type filters, and an upcoming events list.

```tsx
import {
  Sidebar, SidebarContent, SidebarGroup, SidebarGroupLabel,
  SidebarHeader, SidebarMenu, SidebarMenuButton, SidebarMenuItem,
} from "@/components/ui/sidebar"
import { Calendar } from "@/components/ui/calendar"

function CalendarSidebar(props: React.ComponentProps<typeof Sidebar>) {
  const [date, setDate] = React.useState<Date | undefined>(new Date())

  return (
    <Sidebar {...props}>
      <SidebarHeader>
        <SidebarMenu>
          <SidebarMenuItem>
            <SidebarMenuButton size="lg">
              <AppIcon />
              <span>Calendar App</span>
            </SidebarMenuButton>
          </SidebarMenuItem>
        </SidebarMenu>
      </SidebarHeader>
      <SidebarContent>
        {/* Mini-calendar for date navigation */}
        <SidebarGroup>
          <SidebarGroupLabel>Navigate</SidebarGroupLabel>
          <div className="px-2">
            <Calendar
              mode="single"
              selected={date}
              onSelect={setDate}
              className="rounded-md border"
            />
          </div>
        </SidebarGroup>
        {/* Event type filters */}
        <SidebarGroup>
          <SidebarGroupLabel>Calendars</SidebarGroupLabel>
          {/* Checkbox list for calendar types */}
        </SidebarGroup>
        {/* Upcoming events */}
        <SidebarGroup>
          <SidebarGroupLabel>Upcoming</SidebarGroupLabel>
          {/* Event list items */}
        </SidebarGroup>
      </SidebarContent>
    </Sidebar>
  )
}
```

### 3. Build the Calendar Header

The content header contains the sidebar trigger, current date display, navigation arrows, a "Today" button, and a view switcher.

```tsx
import { SidebarTrigger } from "@/components/ui/sidebar"
import { Separator } from "@/components/ui/separator"
import { Button } from "@/components/ui/button"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"

function CalendarHeader() {
  return (
    <header className="flex h-16 shrink-0 items-center gap-2 border-b px-4 lg:px-6">
      <SidebarTrigger className="-ml-1" />
      <Separator
        orientation="vertical"
        className="mx-2 data-[orientation=vertical]:h-4"
      />
      {/* Date navigation */}
      <div className="flex items-center gap-2">
        <Button variant="outline" size="sm">Today</Button>
        <Button variant="ghost" size="icon">
          {/* ChevronLeft icon */}
        </Button>
        <Button variant="ghost" size="icon">
          {/* ChevronRight icon */}
        </Button>
        <h1 className="text-base font-medium">March 2026</h1>
      </div>
      {/* View switcher */}
      <div className="ml-auto">
        <Select defaultValue="month">
          <SelectTrigger size="sm" className="w-28">
            <SelectValue />
          </SelectTrigger>
          <SelectContent>
            <SelectItem value="month">Month</SelectItem>
            <SelectItem value="week">Week</SelectItem>
            <SelectItem value="day">Day</SelectItem>
            <SelectItem value="agenda">Agenda</SelectItem>
          </SelectContent>
        </Select>
      </div>
    </header>
  )
}
```

### 4. Build the Calendar Grid (Month View)

The month view is a 7-column CSS Grid. Each cell represents a day. Events render as small cards or badges within cells. The grid header shows day-of-week labels.

```tsx
function CalendarGrid() {
  const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]

  return (
    <div className="flex flex-1 flex-col">
      {/* Day-of-week header */}
      <div className="grid grid-cols-7 border-b">
        {daysOfWeek.map((day) => (
          <div
            key={day}
            className="px-2 py-2 text-center text-xs font-medium text-muted-foreground"
          >
            {day}
          </div>
        ))}
      </div>
      {/* Calendar grid (5-6 rows of 7 days) */}
      <div className="grid flex-1 grid-cols-7 grid-rows-5">
        {/* Map days of the month */}
        {days.map((day) => (
          <div
            key={day.date}
            className="min-h-24 border-b border-r p-1 text-sm"
          >
            <span className={cn(
              "inline-flex size-6 items-center justify-center rounded-full text-xs",
              day.isToday && "bg-primary text-primary-foreground",
              day.isOutsideMonth && "text-muted-foreground"
            )}>
              {day.number}
            </span>
            {/* Event items */}
            <div className="mt-1 flex flex-col gap-0.5">
              {day.events?.map((event) => (
                <div
                  key={event.id}
                  className="truncate rounded px-1 py-0.5 text-xs"
                  style={{ backgroundColor: `var(--chart-${event.color})` }}
                >
                  {event.title}
                </div>
              ))}
            </div>
          </div>
        ))}
      </div>
    </div>
  )
}
```

---

## Construct Variant Layouts

### Week View (Calendar 02)

Replace the month grid with a 7-column + time-slot layout. The left column shows hourly time labels. Each day column contains positioned event cards.

```tsx
function WeekGrid() {
  const hours = Array.from({ length: 24 }, (_, i) => i)

  return (
    <div className="flex flex-1 overflow-auto">
      {/* Time labels column */}
      <div className="w-16 shrink-0 border-r">
        {hours.map((hour) => (
          <div key={hour} className="h-12 border-b px-2 text-right text-xs text-muted-foreground">
            {hour === 0 ? "12 AM" : hour < 12 ? `${hour} AM` : hour === 12 ? "12 PM" : `${hour - 12} PM`}
          </div>
        ))}
      </div>
      {/* Day columns */}
      <div className="grid flex-1 grid-cols-7">
        {weekDays.map((day) => (
          <div key={day.date} className="relative border-r">
            {hours.map((hour) => (
              <div key={hour} className="h-12 border-b" />
            ))}
            {/* Positioned event cards */}
          </div>
        ))}
      </div>
    </div>
  )
}
```

### Day View (Calendar 03)

Single-column time-slot layout. Shows one day with hourly rows and event cards positioned by time.

```tsx
function DayGrid() {
  return (
    <div className="flex flex-1 overflow-auto">
      <div className="w-16 shrink-0 border-r">
        {/* Time labels */}
      </div>
      <div className="relative flex-1">
        {hours.map((hour) => (
          <div key={hour} className="h-12 border-b" />
        ))}
        {/* Positioned event cards */}
      </div>
    </div>
  )
}
```

### Agenda View (Calendar 07)

A flat list of upcoming events grouped by date. No grid layout. Uses Card or simple list items.

```tsx
function AgendaView() {
  return (
    <div className="flex flex-1 flex-col gap-4 overflow-auto p-4 lg:p-6">
      {groupedEvents.map((group) => (
        <div key={group.date}>
          <h3 className="mb-2 text-sm font-medium text-muted-foreground">
            {group.dateLabel}
          </h3>
          <div className="flex flex-col gap-2">
            {group.events.map((event) => (
              <Card key={event.id} className="p-3">
                <div className="flex items-center gap-3">
                  <div
                    className="size-2 rounded-full"
                    style={{ backgroundColor: `var(--chart-${event.color})` }}
                  />
                  <div className="flex-1">
                    <div className="text-sm font-medium">{event.title}</div>
                    <div className="text-xs text-muted-foreground">
                      {event.time}
                    </div>
                  </div>
                </div>
              </Card>
            ))}
          </div>
        </div>
      ))}
    </div>
  )
}
```

### Standalone Calendar (Calendar 04)

Month grid without sidebar. Remove SidebarProvider and render the calendar header + grid directly.

```tsx
export default function CalendarPage() {
  return (
    <div className="flex min-h-svh flex-col">
      <CalendarHeader />
      <CalendarGrid />
    </div>
  )
}
```