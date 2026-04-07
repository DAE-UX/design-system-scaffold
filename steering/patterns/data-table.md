# Data Table

## Metadata

- **Name:** Data Table
- **Source:** shadcn (`https://ui.shadcn.com/docs/components/data-table`)
- **Category:** Pattern (functional composition)
- **Composes:** Table, Button, Checkbox, Input, Select, Dropdown Menu, Pagination
- **Dependencies:** `@tanstack/react-table`

## Behavior

Enhanced table with sorting, filtering, pagination, row selection, column visibility, and row actions. Builds on the atomic Table component by adding interactive data manipulation via `@tanstack/react-table`.

### Interaction Model

| Interaction | Trigger | Result |
|-------------|---------|--------|
| Sort column | Click header button (with arrow icon) | Toggles ascending → descending → none. `SortingState` updates. Rows reorder. |
| Filter rows | Type in filter Input | `ColumnFiltersState` updates. Rows filter in real-time. |
| Select row | Click row Checkbox | `RowSelectionState` updates. Row visually highlights. Header checkbox toggles all. |
| Toggle column visibility | Open Dropdown Menu → toggle column items | `VisibilityState` updates. Columns show/hide. |
| Navigate pages | Click Previous/Next buttons or page number buttons | `PaginationState` updates. Table renders next page of rows. |
| Change rows per page | Select value from rows-per-page Select | `pageSize` updates. Table re-paginates. |
| Row action | Click ellipsis (⋯) button on row → select action from Dropdown Menu | Action callback fires (copy, edit, delete, view details). |

### State Management

All state is managed via `@tanstack/react-table`'s `useReactTable` hook with React state:

```tsx
const [sorting, setSorting] = React.useState<SortingState>([])
const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>([])
const [columnVisibility, setColumnVisibility] = React.useState<VisibilityState>({})
const [rowSelection, setRowSelection] = React.useState({})
```

State is passed to `useReactTable` via `onSortingChange`, `onColumnFiltersChange`, `onColumnVisibilityChange`, `onRowSelectionChange`.

## Anti-Patterns

| Anti-Pattern | Why It Fails | Correct Approach |
|-------------|-------------|-----------------|
| Building custom sort/filter logic outside `@tanstack/react-table` | Duplicates state management, breaks consistency | Use `getSortedRowModel()`, `getFilteredRowModel()` from tanstack |
| Hardcoding column definitions inline | Unmaintainable, not reusable | Define `columns` array outside the component with `ColumnDef<TData>[]` |
| Using `<table>` directly instead of Table component parts | Loses design system styling and accessibility | Use `Table`, `TableHeader`, `TableBody`, `TableRow`, `TableHead`, `TableCell` |
| Placing filter Input inside the table header | Breaks layout, confuses header semantics | Place filter controls above the table in a toolbar area |
| Omitting `accessorKey` or `accessorFn` on columns | Tanstack cannot resolve cell values | Every data column needs `accessorKey` (string path) or `accessorFn` (function) |
| Using pagination without `getPaginationRowModel()` | Pagination controls render but don't paginate | Include `getPaginationRowModel()` in `useReactTable` config |

## Composed Components

| Component | Role | Required |
|-----------|------|----------|
| Table | Data display surface (`TableHeader`, `TableBody`, `TableRow`, `TableHead`, `TableCell`) | Yes |
| Button | Sort triggers in headers, pagination Previous/Next, row action triggers | Yes |
| Checkbox | Row selection (header = select all, row = select individual) | No (only if row selection enabled) |
| Input | Filter/search input above table | No (only if filtering enabled) |
| Dropdown Menu | Column visibility toggle, row actions (ellipsis menu) | No (only if column visibility or row actions enabled) |
| Select | Rows-per-page selector in pagination | No (only if rows-per-page is configurable) |

## Composition Patterns

### Basic Data Table with Sorting and Filtering

Core pattern: define columns, pass data to `useReactTable`, render with Table components and `flexRender`.

```tsx
import {
  ColumnDef,
  ColumnFiltersState,
  SortingState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table"

import {
  Table, TableBody, TableCell, TableHead,
  TableHeader, TableRow,
} from "@/components/ui/table"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"

// 1. Define columns outside the component
const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: "status",
    header: "Status",
  },
  {
    accessorKey: "email",
    header: ({ column }) => (
      <Button
        variant="ghost"
        onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
      >
        Email
        <ArrowUpDown className="ml-2 h-4 w-4" />
      </Button>
    ),
  },
  {
    accessorKey: "amount",
    header: () => <div className="text-right">Amount</div>,
    cell: ({ row }) => {
      const formatted = new Intl.NumberFormat("en-US", {
        style: "currency", currency: "USD",
      }).format(parseFloat(row.getValue("amount")))
      return <div className="text-right font-medium">{formatted}</div>
    },
  },
]

// 2. Create the DataTable component
function DataTable<TData, TValue>({
  columns, data,
}: { columns: ColumnDef<TData, TValue>[]; data: TData[] }) {
  const [sorting, setSorting] = React.useState<SortingState>([])
  const [columnFilters, setColumnFilters] =
    React.useState<ColumnFiltersState>([])

  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    state: { sorting, columnFilters },
  })

  return (
    <div>
      {/* Filter toolbar */}
      <div className="flex items-center py-4">
        <Input
          placeholder="Filter emails..."
          value={(table.getColumn("email")?.getFilterValue() as string) ?? ""}
          onChange={(e) =>
            table.getColumn("email")?.setFilterValue(e.target.value)
          }
          className="max-w-sm"
        />
      </div>

      {/* Table */}
      <div className="rounded-md border">
        <Table>
          <TableHeader>
            {table.getHeaderGroups().map((headerGroup) => (
              <TableRow key={headerGroup.id}>
                {headerGroup.headers.map((header) => (
                  <TableHead key={header.id}>
                    {header.isPlaceholder
                      ? null
                      : flexRender(
                          header.column.columnDef.header,
                          header.getContext()
                        )}
                  </TableHead>
                ))}
              </TableRow>
            ))}
          </TableHeader>
          <TableBody>
            {table.getRowModel().rows?.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow
                  key={row.id}
                  data-state={row.getIsSelected() && "selected"}
                >
                  {row.getVisibleCells().map((cell) => (
                    <TableCell key={cell.id}>
                      {flexRender(
                        cell.column.columnDef.cell,
                        cell.getContext()
                      )}
                    </TableCell>
                  ))}
                </TableRow>
              ))
            ) : (
              <TableRow>
                <TableCell
                  colSpan={columns.length}
                  className="h-24 text-center"
                >
                  No results.
                </TableCell>
              </TableRow>
            )}
          </TableBody>
        </Table>
      </div>

      {/* Pagination */}
      <div className="flex items-center justify-end space-x-2 py-4">
        <Button
          variant="outline" size="sm"
          onClick={() => table.previousPage()}
          disabled={!table.getCanPreviousPage()}
        >
          Previous
        </Button>
        <Button
          variant="outline" size="sm"
          onClick={() => table.nextPage()}
          disabled={!table.getCanNextPage()}
        >
          Next
        </Button>
      </div>
    </div>
  )
}
```

### Add Row Selection

Add a Checkbox column as the first column definition. Wire `onRowSelectionChange` to state.

```tsx
import { Checkbox } from "@/components/ui/checkbox"

// Prepend to columns array:
{
  id: "select",
  header: ({ table }) => (
    <Checkbox
      checked={
        table.getIsAllPageRowsSelected() ||
        (table.getIsSomePageRowsSelected() && "indeterminate")
      }
      onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
      aria-label="Select all"
    />
  ),
  cell: ({ row }) => (
    <Checkbox
      checked={row.getIsSelected()}
      onCheckedChange={(value) => row.toggleSelected(!!value)}
      aria-label="Select row"
    />
  ),
  enableSorting: false,
  enableHiding: false,
}

// Add to useReactTable config:
onRowSelectionChange: setRowSelection,
state: { sorting, columnFilters, columnVisibility, rowSelection },

// Display selected count:
<div className="text-sm text-muted-foreground">
  {table.getFilteredSelectedRowModel().rows.length} of{" "}
  {table.getFilteredRowModel().rows.length} row(s) selected.
</div>
```

### Add Column Visibility Toggle

Use Dropdown Menu to let users show/hide columns.

```tsx
import {
  DropdownMenu, DropdownMenuCheckboxItem,
  DropdownMenuContent, DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

// Place in toolbar alongside filter:
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="outline" className="ml-auto">
      Columns
    </Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent align="end">
    {table.getAllColumns()
      .filter((column) => column.getCanHide())
      .map((column) => (
        <DropdownMenuCheckboxItem
          key={column.id}
          className="capitalize"
          checked={column.getIsVisible()}
          onCheckedChange={(value) => column.toggleVisibility(!!value)}
        >
          {column.id}
        </DropdownMenuCheckboxItem>
      ))}
  </DropdownMenuContent>
</DropdownMenu>
```

### Add Row Actions (Ellipsis Menu)

Append an actions column with a Dropdown Menu triggered by an ellipsis button.

```tsx
import {
  DropdownMenu, DropdownMenuContent, DropdownMenuItem,
  DropdownMenuLabel, DropdownMenuSeparator, DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
import { MoreHorizontal } from "lucide-react"

// Append to columns array:
{
  id: "actions",
  enableHiding: false,
  cell: ({ row }) => {
    const item = row.original
    return (
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <Button variant="ghost" className="h-8 w-8 p-0">
            <span className="sr-only">Open menu</span>
            <MoreHorizontal className="h-4 w-4" />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="end">
          <DropdownMenuLabel>Actions</DropdownMenuLabel>
          <DropdownMenuItem
            onClick={() => navigator.clipboard.writeText(item.id)}
          >
            Copy ID
          </DropdownMenuItem>
          <DropdownMenuSeparator />
          <DropdownMenuItem>View details</DropdownMenuItem>
          <DropdownMenuItem>Edit</DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    )
  },
}
```

## Accessibility

### WAI-ARIA Reference

`https://www.w3.org/WAI/ARIA/apg/patterns/table/`

### ARIA Roles and Attributes

| Element | Role/Attribute | Purpose |
|---------|---------------|---------|
| `<table>` | `role="table"` (implicit) | Identifies the data table |
| `<thead>` | `role="rowgroup"` (implicit) | Groups header rows |
| `<tbody>` | `role="rowgroup"` (implicit) | Groups body rows |
| `<tr>` | `role="row"` (implicit) | Identifies a table row |
| `<th>` | `role="columnheader"` (implicit) | Identifies a column header |
| `<td>` | `role="cell"` (implicit) | Identifies a data cell |
| Sort button | `aria-sort="ascending"` / `"descending"` / `"none"` | Communicates current sort state |
| Select all checkbox | `aria-label="Select all"` | Labels the header checkbox |
| Row checkbox | `aria-label="Select row"` | Labels individual row checkboxes |
| Actions button | `<span className="sr-only">Open menu</span>` | Provides accessible name for icon-only button |
| Selected row | `data-state="selected"` | Visual indicator; pair with `aria-selected="true"` for screen readers |
| Filter input | `placeholder` + visible label or `aria-label` | Identifies the filter purpose |
| Pagination buttons | `disabled` attribute when at boundary | Prevents navigation past first/last page |

### Keyboard Navigation

| Key | Action |
|-----|--------|
| `Tab` | Moves focus between interactive elements (filter input → column visibility → table headers → checkboxes → action buttons → pagination) |
| `Space` / `Enter` | Activates focused button (sort, pagination, action menu trigger) or toggles checkbox |
| `Arrow Up` / `Arrow Down` | Navigates within open dropdown menus |
| `Escape` | Closes open dropdown menus |

## CSS Variable Dependencies

Inherits CSS variable dependencies from composed components (Table, Button, Input, Checkbox, Dropdown Menu, Select). No pattern-specific CSS variables.

Key inherited variables:

| Variable | Purpose | Source Component |
|----------|---------|-----------------|
| `--border` | Table borders, rounded container border | Table |
| `--muted-foreground` | "No results" text, selected row count, disabled pagination | Table, Button |
| `--accent` | Row hover background | Table |
| `--primary` | Checkbox checked state, sort icon active | Checkbox, Button |
| `--background` | Table background | Table |
| `--input` | Filter input border | Input |
| `--ring` | Focus ring on interactive elements | Button, Input, Checkbox |

## Theme Support

### Component Structure

The Data Table is composed of discrete cell, header, and pagination sub-components assembled into a full table layout.

#### Cell Types (13 variants)

Each cell type has 3 sizes (Default, Medium, Large) and hover states:

| Cell Type | Content | Usage |
|-----------|---------|-------|
| Start-Checkbox | Checkbox control | Row selection (first column) |
| Start-Grip | Drag handle icon | Row reordering |
| Text | Plain text | Standard data display |
| Text+Description | Primary text + secondary description | Two-line cell content |
| Avatar | Avatar image | User/entity identification |
| Avatar+Description | Avatar + text + description | User profile cells |
| Badge | Badge component | Status indicators |
| Image | Thumbnail image | Media content |
| Input | Inline Input field | Editable cells |
| Button | Action Button | Cell-level actions |
| Toggle | Toggle switch | Boolean cell values |
| Switch | Switch control | On/off cell values |
| End-Ellipsis | Ellipsis (⋯) icon button | Row actions menu trigger |
| Footer | Summary/aggregate text | Table footer row |

#### Header Types

| Header Type | Variants | Usage |
|-------------|----------|-------|
| Text | Default, Hover, Right-Align | Static column header |
| Button | Default, Hover, Right-Align | Sortable column header (click to sort) |
| Checkbox | Default, Hover | Select-all header |

#### Pagination Types

| Type | Description |
|------|-------------|
| Simple | Previous/Next buttons only |
| Detailed | Row count display + rows-per-page selector + page navigation (First, Previous, Next, Last) |

#### Header Controls

Filter Input + Column Visibility Dropdown positioned above the table.

### Theme Behavior

Inherits theme behavior from composed components. No pattern-specific theme overrides.

| State | Behavior |
|-------|----------|
| Default row | `--background` background |
| Hover row | `--accent` background via `TableRow` hover style |
| Selected row | `--accent` background via `data-state="selected"` |
| Sort active | Sort icon uses `--foreground`; inactive uses `--muted-foreground` |
| Disabled pagination | `--muted-foreground` text, reduced opacity |
| Light/Dark | All values resolve from theme CSS variables — no hardcoded colors |
