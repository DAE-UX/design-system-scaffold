# Dashboard Template

## Metadata

- **Name:** Dashboard Template
- **Source:** shadCN
- **Category:** Template (layout blueprint)
- **Uses:** Sidebar (component), Card, Chart, Table, Tabs, Select, Toggle Group, Badge, Separator

## Layout Structure

Full-page dashboard with sidebar navigation and a vertical content stack. The sidebar uses the inset variant with a custom width. The content area stacks vertically: site header, KPI summary cards, interactive chart, and data table.

```
+--------------------------------------------------+
| SidebarProvider                                  |
| +------------+-----------------------------------+
| |  Sidebar   |  SidebarInset                     |
| |  (inset)   |  +-------------------------------+|
| |            |  | SiteHeader (trigger + title)   ||
| |  Header    |  +-------------------------------+|
| |  -----     |  | SectionCards (4 KPI cards)     ||
| |  NavMain   |  +-------------------------------+|
| |  NavDocs   |  | ChartAreaInteractive           ||
| |  NavSecond |  | (AreaChart in Card)             ||
| |  -----     |  +-------------------------------+|
| |  Footer    |  | DataTable (Tabs + Table)       ||
| |  (user)    |  |                                ||
| +------------+-----------------------------------+
+--------------------------------------------------+
```

## Regions

| Region | Purpose | Components Used |
|--------|---------|----------------|
| Sidebar | App navigation, branding, user profile | Sidebar (variant="inset", collapsible="offcanvas"), SidebarHeader, SidebarContent, SidebarFooter |
| Site Header | Sidebar trigger, page title, actions | SidebarTrigger, Separator, heading, Button |
| KPI Cards Row | Summary metrics (4 cards in responsive grid) | Card, CardHeader, CardTitle, CardDescription, CardAction, CardFooter, Badge |
| Chart Area | Interactive data visualization | Card, ChartContainer (recharts AreaChart), Select or ToggleGroup for time range |
| Data Table | Tabular data with tabs, filtering, pagination | Tabs, TabsList, TabsTrigger, Table, TableHeader, TableBody, TableRow, TableCell, Select, Button, Badge, Dropdown Menu |

## Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| Desktop (lg+) | Sidebar visible. KPI cards in 4-column grid (`@5xl/main:grid-cols-4`). Chart and table at full width with `px-6` padding. Time range selector uses ToggleGroup. |
| Tablet (md-lg) | Sidebar collapsible via trigger. KPI cards in 2-column grid (`@xl/main:grid-cols-2`). Chart uses Select dropdown for time range instead of ToggleGroup. |
| Mobile (<md) | Sidebar renders as Sheet overlay. KPI cards stack to single column. Table columns may be hidden via column visibility. Reduced padding (`px-4`). |

The dashboard uses container queries (`@container/main`) for the content area, enabling responsive behavior independent of viewport width.

## Component Reference

### Layout Structure

1 dashboard layout block:

| Block | Description |
|-------|-------------|
| Dashboard 1 | Full dashboard with inset sidebar, site header, 4 KPI summary cards, interactive area chart with time range selector, tabbed data table with column customization and pagination |

### Theme Behavior

Inherits theme behavior from composed components. No template-specific theme overrides. KPI cards use a subtle gradient (`from-primary/5 to-card`) in light mode, falling back to flat `bg-card` in dark mode.

---

## Construct the Base Template

Build the dashboard shell using shadcn components. The layout composes the Sidebar Template (inset variant) with a vertical content stack.

### 1. Set Up the Page Shell

Use SidebarProvider with custom CSS properties for sidebar width and header height. The sidebar uses `variant="inset"` and `collapsible="offcanvas"`.

```tsx
import { SidebarInset, SidebarProvider } from "@/components/ui/sidebar"

export default function DashboardPage() {
  return (
    <SidebarProvider
      style={{
        "--sidebar-width": "calc(var(--spacing) * 72)",
        "--header-height": "calc(var(--spacing) * 12)",
      } as React.CSSProperties}
    >
      <AppSidebar variant="inset" />
      <SidebarInset>
        <SiteHeader />
        <div className="flex flex-1 flex-col">
          <div className="@container/main flex flex-1 flex-col gap-2">
            <div className="flex flex-col gap-4 py-4 md:gap-6 md:py-6">
              <SectionCards />
              <div className="px-4 lg:px-6">
                <ChartAreaInteractive />
              </div>
              <DataTable data={data} />
            </div>
          </div>
        </div>
      </SidebarInset>
    </SidebarProvider>
  )
}
```

### 2. Build the Site Header

The header sits inside SidebarInset. It uses a fixed height via `--header-height` CSS variable and contains the SidebarTrigger, a vertical Separator, and the page title.

```tsx
import { SidebarTrigger } from "@/components/ui/sidebar"
import { Separator } from "@/components/ui/separator"
import { Button } from "@/components/ui/button"

function SiteHeader() {
  return (
    <header className="flex h-(--header-height) shrink-0 items-center gap-2 border-b transition-[width,height] ease-linear group-has-data-[collapsible=icon]/sidebar-wrapper:h-(--header-height)">
      <div className="flex w-full items-center gap-1 px-4 lg:gap-2 lg:px-6">
        <SidebarTrigger className="-ml-1" />
        <Separator
          orientation="vertical"
          className="mx-2 data-[orientation=vertical]:h-4"
        />
        <h1 className="text-base font-medium">Dashboard</h1>
        <div className="ml-auto flex items-center gap-2">
          {/* Action buttons */}
        </div>
      </div>
    </header>
  )
}
```

### 3. Build the KPI Cards Row

Four summary cards in a responsive grid. Each card shows a metric title, value, trend badge, and footer description. Uses container queries for responsive columns.

```tsx
import { Badge } from "@/components/ui/badge"
import {
  Card, CardAction, CardDescription, CardFooter, CardHeader, CardTitle,
} from "@/components/ui/card"

function SectionCards() {
  return (
    <div className="grid grid-cols-1 gap-4 px-4 *:data-[slot=card]:shadow-xs lg:px-6 @xl/main:grid-cols-2 @5xl/main:grid-cols-4">
      <Card className="@container/card">
        <CardHeader>
          <CardDescription>Total Revenue</CardDescription>
          <CardTitle className="text-2xl font-semibold tabular-nums @[250px]/card:text-3xl">
            $1,250.00
          </CardTitle>
          <CardAction>
            <Badge variant="outline">+12.5%</Badge>
          </CardAction>
        </CardHeader>
        <CardFooter className="flex-col items-start gap-1.5 text-sm">
          <div className="line-clamp-1 flex gap-2 font-medium">
            Trending up this month
          </div>
          <div className="text-muted-foreground">
            Visitors for the last 6 months
          </div>
        </CardFooter>
      </Card>
      {/* Repeat for additional KPI cards */}
    </div>
  )
}
```

### 4. Build the Interactive Chart

An area chart inside a Card with a time range selector. Desktop uses ToggleGroup; mobile uses Select. The chart uses recharts AreaChart with gradient fills.

```tsx
import {
  Card, CardAction, CardContent, CardDescription, CardHeader, CardTitle,
} from "@/components/ui/card"
import { ChartContainer, ChartTooltip, ChartTooltipContent } from "@/components/ui/chart"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"
import { ToggleGroup, ToggleGroupItem } from "@/components/ui/toggle-group"
import { Area, AreaChart, CartesianGrid, XAxis } from "recharts"

function ChartAreaInteractive() {
  const [timeRange, setTimeRange] = React.useState("90d")

  return (
    <Card className="@container/card">
      <CardHeader>
        <CardTitle>Total Visitors</CardTitle>
        <CardDescription>
          <span className="hidden @[540px]/card:block">
            Total for the last 3 months
          </span>
          <span className="@[540px]/card:hidden">Last 3 months</span>
        </CardDescription>
        <CardAction>
          {/* Desktop: ToggleGroup */}
          <ToggleGroup
            type="single"
            value={timeRange}
            onValueChange={setTimeRange}
            variant="outline"
            className="hidden @[767px]/card:flex"
          >
            <ToggleGroupItem value="90d">Last 3 months</ToggleGroupItem>
            <ToggleGroupItem value="30d">Last 30 days</ToggleGroupItem>
            <ToggleGroupItem value="7d">Last 7 days</ToggleGroupItem>
          </ToggleGroup>
          {/* Mobile: Select */}
          <Select value={timeRange} onValueChange={setTimeRange}>
            <SelectTrigger className="w-40 @[767px]/card:hidden" size="sm">
              <SelectValue placeholder="Last 3 months" />
            </SelectTrigger>
            <SelectContent>
              <SelectItem value="90d">Last 3 months</SelectItem>
              <SelectItem value="30d">Last 30 days</SelectItem>
              <SelectItem value="7d">Last 7 days</SelectItem>
            </SelectContent>
          </Select>
        </CardAction>
      </CardHeader>
      <CardContent className="px-2 pt-4 sm:px-6 sm:pt-6">
        <ChartContainer config={chartConfig} className="aspect-auto h-[250px] w-full">
          <AreaChart data={filteredData}>
            <defs>
              <linearGradient id="fillDesktop" x1="0" y1="0" x2="0" y2="1">
                <stop offset="5%" stopColor="var(--color-desktop)" stopOpacity={1.0} />
                <stop offset="95%" stopColor="var(--color-desktop)" stopOpacity={0.1} />
              </linearGradient>
            </defs>
            <CartesianGrid vertical={false} />
            <XAxis dataKey="date" tickLine={false} axisLine={false} tickMargin={8} minTickGap={32} />
            <ChartTooltip cursor={false} content={<ChartTooltipContent indicator="dot" />} />
            <Area dataKey="desktop" type="natural" fill="url(#fillDesktop)" stroke="var(--color-desktop)" stackId="a" />
          </AreaChart>
        </ChartContainer>
      </CardContent>
    </Card>
  )
}
```

### 5. Build the Data Table Section

A tabbed data table with column customization and pagination. Uses Tabs for view switching, Table for data display, DropdownMenu for column visibility, and Select/Button for pagination.

```tsx
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import { DropdownMenu, DropdownMenuCheckboxItem, DropdownMenuContent, DropdownMenuTrigger } from "@/components/ui/dropdown-menu"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"

function DataTable({ data }) {
  return (
    <Tabs defaultValue="outline" className="w-full flex-col justify-start gap-6">
      <div className="flex items-center justify-between px-4 lg:px-6">
        {/* Mobile: Select for tab switching */}
        <Select defaultValue="outline">
          <SelectTrigger className="flex w-fit @4xl/main:hidden" size="sm">
            <SelectValue placeholder="Select a view" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem value="outline">Outline</SelectItem>
            <SelectItem value="past-performance">Past Performance</SelectItem>
          </SelectContent>
        </Select>
        {/* Desktop: TabsList */}
        <TabsList className="hidden @4xl/main:flex">
          <TabsTrigger value="outline">Outline</TabsTrigger>
          <TabsTrigger value="past-performance">
            Past Performance <Badge variant="secondary">3</Badge>
          </TabsTrigger>
        </TabsList>
        <div className="flex items-center gap-2">
          {/* Column visibility dropdown */}
          <DropdownMenu>
            <DropdownMenuTrigger asChild>
              <Button variant="outline" size="sm">Columns</Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent align="end">
              {/* DropdownMenuCheckboxItem per column */}
            </DropdownMenuContent>
          </DropdownMenu>
          <Button variant="outline" size="sm">Add Section</Button>
        </div>
      </div>
      <TabsContent value="outline" className="relative flex flex-col gap-4 overflow-auto px-4 lg:px-6">
        <div className="overflow-hidden rounded-lg border">
          <Table>
            <TableHeader className="sticky top-0 z-10 bg-muted">
              <TableRow>
                <TableHead>Header</TableHead>
                <TableHead>Type</TableHead>
                <TableHead>Status</TableHead>
                <TableHead className="text-right">Target</TableHead>
                <TableHead>Reviewer</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {/* Map data rows */}
            </TableBody>
          </Table>
        </div>
        {/* Pagination controls */}
        <div className="flex items-center justify-between px-4">
          <div className="text-sm text-muted-foreground">
            {/* Row selection count */}
          </div>
          <div className="flex items-center gap-8">
            {/* Rows per page Select + page navigation Buttons */}
          </div>
        </div>
      </TabsContent>
    </Tabs>
  )
}
```

---

## Construct the Sidebar

The dashboard sidebar follows the Sidebar Template structure with the `inset` variant. See `templates/sidebar-template.md` for full sidebar construction instructions.

Key differences from the base sidebar template:
- Uses `variant="inset"` for coordinated layout with SidebarInset
- Uses `collapsible="offcanvas"` (slides off-screen when collapsed)
- Custom width via `--sidebar-width` CSS property on SidebarProvider
- Navigation organized into primary nav, documents section, and secondary nav (settings/help)
- User profile in footer with Avatar and DropdownMenu