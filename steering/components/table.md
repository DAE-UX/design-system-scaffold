# Component: Table

> **TL;DR:** A styled HTML table with 8 parts: Table, TableHeader, TableBody, TableFooter, TableHead, TableRow, TableCell, TableCaption. No external primitive — pure shadCN wrappers around native HTML table elements. Table is wrapped in an overflow-x-auto container for horizontal scrolling. Supports row selection state via `data-[state=selected]`. Commonly used for data display, admin panels, and list views.

## Metadata

- **Component Name:** Table
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A styled wrapper around native HTML table elements for displaying tabular data.

- Table is wrapped in a container `div` with `overflow-x-auto` for horizontal scrolling on narrow viewports
- Table renders as `w-full` with `caption-bottom` and `text-sm` base styling
- TableHeader applies bottom border to all child rows
- TableBody removes bottom border from the last row
- TableFooter has top border, muted background at 50% opacity, and medium font weight
- TableRow has bottom border and hover state with muted background at 50% opacity
- TableRow supports `data-[state=selected]` for selected row highlighting with muted background
- TableHead and TableCell use `whitespace-nowrap` to prevent text wrapping
- TableHead has medium font weight and foreground text color
- TableHead and TableCell support checkbox alignment via `has-[role=checkbox]` and `[&:has([role=checkbox])]:pr-0`
- TableCaption renders below the table with muted foreground color

### Anti-Patterns

EMPTY: None recorded.

## API

### Parts

- `Table` — Root `table` element wrapped in an overflow container. Full width with caption-bottom.
- `TableHeader` — `thead` element. Applies bottom border to child rows.
- `TableBody` — `tbody` element. Removes bottom border from last row.
- `TableFooter` — `tfoot` element. Top border with muted background and medium font weight.
- `TableHead` — `th` element. Fixed height, medium font weight, foreground color, whitespace-nowrap.
- `TableRow` — `tr` element. Bottom border, hover state, and selected state support.
- `TableCell` — `td` element. Padding, whitespace-nowrap, checkbox alignment.
- `TableCaption` — `caption` element. Muted foreground text below the table.

### Props

All parts accept `className` and their respective native HTML element attributes.

#### Table

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### TableRow

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

All other parts (TableHeader, TableBody, TableFooter, TableHead, TableCell, TableCaption) accept `className` and standard HTML attributes for their respective elements.

### Variants

No CVA variants — Table uses native HTML table semantics with consistent styling.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"table-container"`, `"table"`, `"table-header"`, `"table-body"`, `"table-footer"`, `"table-head"`, `"table-row"`, `"table-cell"`, `"table-caption"` | All parts |
| `[data-state]` | `"selected"` | TableRow (when row is selected) |

### CSS Variables (Radix)

Not applicable — Table does not use Radix primitives.

## Composition Patterns

### Basic Table

```tsx
<Table>
  <TableCaption>A list of recent invoices.</TableCaption>
  <TableHeader>
    <TableRow>
      <TableHead>Invoice</TableHead>
      <TableHead>Status</TableHead>
      <TableHead>Method</TableHead>
      <TableHead className="text-right">Amount</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell className="font-medium">INV001</TableCell>
      <TableCell>Paid</TableCell>
      <TableCell>Credit Card</TableCell>
      <TableCell className="text-right">$250.00</TableCell>
    </TableRow>
    <TableRow>
      <TableCell className="font-medium">INV002</TableCell>
      <TableCell>Pending</TableCell>
      <TableCell>PayPal</TableCell>
      <TableCell className="text-right">$150.00</TableCell>
    </TableRow>
  </TableBody>
  <TableFooter>
    <TableRow>
      <TableCell colSpan={3}>Total</TableCell>
      <TableCell className="text-right">$400.00</TableCell>
    </TableRow>
  </TableFooter>
</Table>
```

### With Selection State

```tsx
<Table>
  <TableHeader>
    <TableRow>
      <TableHead role="checkbox" aria-checked="false">
        <Checkbox />
      </TableHead>
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow data-state="selected">
      <TableCell role="checkbox">
        <Checkbox checked />
      </TableCell>
      <TableCell>Jane Doe</TableCell>
      <TableCell>jane@example.com</TableCell>
    </TableRow>
  </TableBody>
</Table>
```

## Accessibility

- **WAI-ARIA Pattern:** [Table](https://www.w3.org/WAI/ARIA/apg/patterns/table/) (native HTML table semantics)

### ARIA Roles

- Uses native HTML table elements (`table`, `thead`, `tbody`, `tfoot`, `tr`, `th`, `td`, `caption`) which provide implicit ARIA roles
- `caption` element provides an accessible name for the table
- `th` elements in `thead` provide column headers
- No additional ARIA attributes required for basic usage

### Keyboard Behavior

No custom keyboard interaction — Table is a static data display. Interactive elements inside cells (checkboxes, buttons, links) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<div data-slot="table-container">
  <table data-slot="table">
    <caption data-slot="table-caption">A list of recent invoices.</caption>
    <thead data-slot="table-header">
      <tr data-slot="table-row">
        <th data-slot="table-head">Invoice</th>
        <th data-slot="table-head">Status</th>
        <th data-slot="table-head">Amount</th>
      </tr>
    </thead>
    <tbody data-slot="table-body">
      <tr data-slot="table-row">
        <td data-slot="table-cell">INV001</td>
        <td data-slot="table-cell">Paid</td>
        <td data-slot="table-cell">$250.00</td>
      </tr>
    </tbody>
    <tfoot data-slot="table-footer">
      <tr data-slot="table-row">
        <td data-slot="table-cell" colspan="2">Total</td>
        <td data-slot="table-cell">$250.00</td>
      </tr>
    </tfoot>
  </table>
</div>
```

## CSS

### Raw CSS

```css
/* Table container */
.TableContainer {
  position: relative;
  width: 100%;
  overflow-x: auto;
}

/* Table */
.Table {
  width: 100%;
  caption-side: bottom;
  font-size: 0.875rem;
  border-collapse: collapse;
}

/* TableHeader */
.TableHeader tr {
  border-bottom: 1px solid var(--border);
}

/* TableBody — remove border from last row */
.TableBody tr:last-child {
  border-bottom: 0;
}

/* TableFooter */
.TableFooter {
  border-top: 1px solid var(--border);
  background-color: color-mix(in srgb, var(--muted) 50%, transparent);
  font-weight: 500;
}

/* TableRow */
.TableRow {
  border-bottom: 1px solid var(--border);
  transition: background-color 150ms;
}

.TableRow:hover {
  background-color: color-mix(in srgb, var(--muted) 50%, transparent);
}

.TableRow[data-state="selected"] {
  background-color: var(--muted);
}

/* TableHead */
.TableHead {
  height: 2.5rem;
  padding-left: 0.5rem;
  padding-right: 0.5rem;
  text-align: left;
  font-weight: 500;
  white-space: nowrap;
  color: var(--foreground);
}

/* TableCell */
.TableCell {
  padding: 0.5rem;
  white-space: nowrap;
}

/* Checkbox alignment for TableHead and TableCell */
.TableHead:has([role="checkbox"]),
.TableCell:has([role="checkbox"]) {
  padding-right: 0;
}

/* TableCaption */
.TableCaption {
  margin-top: 1rem;
  font-size: 0.875rem;
  color: var(--muted-foreground);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Table container | `relative w-full overflow-auto` | Horizontal scroll wrapper |
| Table | `w-full caption-bottom text-sm` | Full-width table base |
| TableHeader | `[&_tr]:border-b` | Bottom border on header rows |
| TableBody | `[&_tr:last-child]:border-0` | Remove border from last body row |
| TableFooter | `border-t bg-muted/50 font-medium [&>tr]:last:border-b-0` | Footer with muted background |
| TableRow | `border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted` | Row with hover and selection |
| TableHead | `h-10 px-2 text-left align-middle font-medium whitespace-nowrap text-foreground [&:has([role=checkbox])]:pr-0` | Header cell |
| TableCell | `p-2 align-middle whitespace-nowrap [&:has([role=checkbox])]:pr-0` | Data cell |
| TableCaption | `mt-4 text-sm text-muted-foreground` | Caption text |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Row borders, header border, footer border (implicit from `border-b` class) | `shadcn` |
| `--muted` | Hover background, selected row background, footer background | `shadcn` |
| `--muted-foreground` | Caption text color | `shadcn` |
| `--foreground` | TableHead text color | `shadcn` |

## Theme Support

### Component Structure

Component organizes Table as an assembled composition:

| Variant Property | Values |
|-----------------|--------|
| (none) | Single assembled table variant |

- Header row: TableHead cells (`h-10`, `px-2`, `py-3`, `font-medium`, `border-b`)
- Body rows: TableCell cells (`h-56px`, `p-2`, `font-normal`, `border-b`)
- Footer row: Full-width cell with `bg-muted` background, `font-medium`
- Caption: Below table, `text-sm`, `px-3.5`, `py-4`
- Header and body rows use alternating content for demonstration
- Footer row spans full width for totals display


### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--foreground` | `--foreground` | Header and body text color |
| `--muted-foreground` | `--muted-foreground` | Caption text color |
| `--border` | `--border` | Row border color |
| `--muted` | `--muted` | Footer background, hover/selected row background |
| `--background` | `--background` | Table background |
| `h-10` (40px) | `height` | Header cell height |
| `p-2` (8px) | `padding` | Cell padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-weight` | Header and footer font weight |
| `text-sm` (14px) | `font-size` | Text size |

### Theme Behavior

- Row borders use `--border` (implicit) — adapts to light/dark via theme
- Hover and footer backgrounds use `--muted` at 50% opacity — adapts to light/dark via theme
- Selected row uses `--muted` — adapts to light/dark via theme
- Header text uses `--foreground` — adapts to light/dark via theme
- Caption uses `--muted-foreground` — adapts to light/dark via theme
- All spacing, typography, and layout tokens are mode-independent
