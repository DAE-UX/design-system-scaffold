# Component: Field

> **TL;DR:** A form field layout system for labels, descriptions, errors, and controls. 10 parts: Field, FieldSet, FieldLegend, FieldGroup, FieldContent, FieldLabel, FieldTitle, FieldDescription, FieldSeparator, FieldError. Field supports `vertical`, `horizontal`, and `responsive` orientations via CVA. FieldLegend supports `legend` and `label` variants. FieldError accepts an `errors` array for automatic deduplication and list rendering. No external primitive — uses internal Label and Separator. Container queries on FieldGroup for responsive layout.

## Metadata

- **Component Name:** Field
- **Source:** shadCN
- **Dependencies:** (none — no external primitive)

## Behavior

A layout system for composing form fields with labels, descriptions, validation errors, and controls.

- Field renders as a flex group with `role="group"` and orientation-aware layout
- Supports three orientations: `vertical` (default, stacked), `horizontal` (side-by-side), `responsive` (stacked on small, side-by-side on `@md` container query)
- FieldSet renders as a `fieldset` element with reduced gap when containing checkbox/radio groups
- FieldLegend supports `legend` (base size) and `label` (small size) variants
- FieldGroup uses `@container/field-group` for responsive orientation queries; nested FieldGroups get reduced gap
- FieldContent wraps the control area with flex-1 and gap for description/error stacking
- FieldLabel wraps Label component; supports nested Field inside label (bordered, with checked state highlighting)
- FieldTitle is a non-label heading alternative (same visual as label but no `for` association)
- FieldDescription renders helper text with muted foreground; links auto-styled with underline and primary hover
- FieldSeparator renders a horizontal separator with optional centered text content
- FieldError renders validation messages with `role="alert"`; accepts `errors` array prop for automatic deduplication and list rendering (single error = inline text, multiple = bulleted list)
- `data-invalid="true"` on Field triggers destructive text color
- `data-disabled="true"` on Field triggers 50% opacity on labels
- Supports RTL layout

### Anti-Patterns

#### Do
- Provide a label for every form control
- Indicate optional fields by adding "optional" to the label — mandatory fields don't need marking
- Use consistent validation icons

#### Don't
- Don't use error validation for non-blocking messages — use warning validation instead
- Don't omit labels
- Don't mark required fields with asterisks when most fields are required

Sources: shadCN, Radix UI

## API

### Parts

- `Field` — Root layout container. Flex group with orientation variants.
- `FieldSet` — HTML `fieldset` wrapper. Reduced gap for checkbox/radio groups.
- `FieldLegend` — HTML `legend` element. Supports `legend` and `label` size variants.
- `FieldGroup` — Container for multiple fields. Uses `@container` for responsive queries.
- `FieldContent` — Control wrapper. Flex column with gap for stacking description/error below control.
- `FieldLabel` — Label wrapper. Supports nested Field with border and checked-state highlighting.
- `FieldTitle` — Non-label heading. Same visual as FieldLabel but no `htmlFor` association.
- `FieldDescription` — Helper text. Muted foreground with auto-styled links.
- `FieldSeparator` — Horizontal divider with optional centered text.
- `FieldError` — Validation message with `role="alert"`. Supports `errors` array prop.

### Props

#### Field

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"vertical" \| "horizontal" \| "responsive"` | `"vertical"` | Layout direction |
| className | `string` | — | Additional CSS classes |

#### FieldSet

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldLegend

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `"legend" \| "label"` | `"legend"` | Text size variant |
| className | `string` | — | Additional CSS classes |

#### FieldGroup

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldTitle

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FieldSeparator

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| children | `React.ReactNode` | — | Optional centered text content |
| className | `string` | — | Additional CSS classes |

#### FieldError

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| errors | `Array<{ message?: string } \| undefined>` | — | Error objects for automatic rendering |
| children | `React.ReactNode` | — | Custom error content (overrides `errors`) |
| className | `string` | — | Additional CSS classes |

### Variants

#### Field Orientation Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `vertical` | Stacked layout, full-width children | Standard form fields |
| `horizontal` | Side-by-side layout, label flex-auto | Checkbox/radio with label beside |
| `responsive` | Stacked on small, horizontal on `@md` container | Adaptive form layouts |

#### FieldLegend Variants

| Variant | Description | Use Case |
|---------|-------------|----------|
| `legend` | Base text size (`text-base`) | Fieldset headings |
| `label` | Small text size (`text-sm`) | Label-like legends |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"field"`, `"field-set"`, `"field-legend"`, `"field-group"`, `"field-content"`, `"field-label"`, `"field-title"`, `"field-description"`, `"field-separator"`, `"field-error"`, `"field-separator-content"` | All parts |
| `[data-orientation]` | `"vertical"`, `"horizontal"`, `"responsive"` | Field |
| `[data-variant]` | `"legend"`, `"label"` | FieldLegend |
| `[data-invalid]` | `"true"` | Field (when invalid) |
| `[data-disabled]` | `"true"` | Field (when disabled) |
| `[data-content]` | `"true"`, `"false"` | FieldSeparator |

### CSS Variables (Radix)

Not applicable — Field does not use Radix primitives.

## Composition Patterns

### Vertical Field (Default)

```tsx
<Field>
  <FieldLabel>Email</FieldLabel>
  <FieldContent>
    <Input type="email" />
    <FieldDescription>We'll never share your email.</FieldDescription>
  </FieldContent>
</Field>
```

### Horizontal Field (Checkbox)

```tsx
<Field orientation="horizontal">
  <Checkbox id="terms" />
  <FieldContent>
    <FieldLabel htmlFor="terms">Accept terms</FieldLabel>
    <FieldDescription>You agree to our Terms of Service.</FieldDescription>
  </FieldContent>
</Field>
```

### With Validation Error

```tsx
<Field data-invalid="true">
  <FieldLabel>Username</FieldLabel>
  <FieldContent>
    <Input aria-invalid="true" />
    <FieldError errors={[{ message: "Username is required" }]} />
  </FieldContent>
</Field>
```

### FieldSet with Legend

```tsx
<FieldSet>
  <FieldLegend>Notification Preferences</FieldLegend>
  <FieldDescription>Choose how you want to be notified.</FieldDescription>
  <FieldGroup>
    <Field orientation="horizontal">
      <Checkbox id="email-notif" />
      <FieldLabel htmlFor="email-notif">Email</FieldLabel>
    </Field>
    <Field orientation="horizontal">
      <Checkbox id="sms-notif" />
      <FieldLabel htmlFor="sms-notif">SMS</FieldLabel>
    </Field>
  </FieldGroup>
</FieldSet>
```

### With Separator

```tsx
<FieldGroup>
  <Field>
    <FieldLabel>First Name</FieldLabel>
    <Input />
  </Field>
  <FieldSeparator>or</FieldSeparator>
  <Field>
    <FieldLabel>Display Name</FieldLabel>
    <Input />
  </Field>
</FieldGroup>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Field uses native HTML form semantics

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Field | `role="group"` | Groups label + control + description + error |
| FieldSet | `fieldset` (native) | Groups related fields with legend |
| FieldLegend | `legend` (native) | Accessible name for fieldset |
| FieldError | `role="alert"` | Announces validation errors to screen readers |

### Keyboard Behavior

No keyboard interaction — Field is a layout container. Interactive elements inside (inputs, checkboxes) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<div data-slot="field" data-orientation="vertical" role="group">
  <label data-slot="field-label">Email</label>
  <div data-slot="field-content">
    <input type="email" />
    <p data-slot="field-description">We'll never share your email.</p>
  </div>
</div>
```

## CSS

### Raw CSS

```css
/* Field — base */
.Field {
  display: flex;
  width: 100%;
  gap: 0.75rem;
}

.Field[data-invalid="true"] {
  color: var(--destructive);
}

/* Field — vertical */
.Field[data-orientation="vertical"] {
  flex-direction: column;
}

.Field[data-orientation="vertical"] > * {
  width: 100%;
}

/* Field — horizontal */
.Field[data-orientation="horizontal"] {
  flex-direction: row;
  align-items: center;
}

/* FieldSet */
.FieldSet {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.FieldSet:has(> [data-slot="checkbox-group"]),
.FieldSet:has(> [data-slot="radio-group"]) {
  gap: 0.75rem;
}

/* FieldLegend */
.FieldLegend {
  margin-bottom: 0.75rem;
  font-weight: 500;
}

.FieldLegend[data-variant="legend"] {
  font-size: 1rem;
}

.FieldLegend[data-variant="label"] {
  font-size: 0.875rem;
}

/* FieldGroup */
.FieldGroup {
  container-type: inline-size;
  container-name: field-group;
  display: flex;
  width: 100%;
  flex-direction: column;
  gap: 1.75rem;
}

/* FieldContent */
.FieldContent {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: 0.375rem;
  line-height: 1.375;
}

/* FieldLabel */
.FieldLabel {
  display: flex;
  width: fit-content;
  gap: 0.5rem;
  line-height: 1.375;
}

/* FieldTitle */
.FieldTitle {
  display: flex;
  width: fit-content;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  line-height: 1.375;
  font-weight: 500;
}

/* FieldDescription */
.FieldDescription {
  font-size: 0.875rem;
  line-height: 1.5;
  font-weight: 400;
  color: var(--muted-foreground);
}

.FieldDescription a {
  text-decoration: underline;
  text-underline-offset: 4px;
}

.FieldDescription a:hover {
  color: var(--primary);
}

/* FieldSeparator */
.FieldSeparator {
  position: relative;
  margin-top: -0.5rem;
  margin-bottom: -0.5rem;
  height: 1.25rem;
  font-size: 0.875rem;
}

/* FieldError */
.FieldError {
  font-size: 0.875rem;
  font-weight: 400;
  color: var(--destructive);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Field (base) | `group/field flex w-full gap-3 data-[invalid=true]:text-destructive` | Base layout |
| Field (vertical) | `flex-col [&>*]:w-full [&>.sr-only]:w-auto` | Stacked layout |
| Field (horizontal) | `flex-row items-center [&>[data-slot=field-label]]:flex-auto` | Side-by-side layout |
| Field (responsive) | `flex-col @md/field-group:flex-row @md/field-group:items-center [&>*]:w-full @md/field-group:[&>*]:w-auto` | Adaptive layout |
| FieldSet | `flex flex-col gap-6 has-[>[data-slot=checkbox-group]]:gap-3 has-[>[data-slot=radio-group]]:gap-3` | Fieldset container |
| FieldLegend | `mb-3 font-medium data-[variant=legend]:text-base data-[variant=label]:text-sm` | Legend text |
| FieldGroup | `@container/field-group flex w-full flex-col gap-7 [&>[data-slot=field-group]]:gap-4` | Field container |
| FieldContent | `group/field-content flex flex-1 flex-col gap-1.5 leading-snug` | Control wrapper |
| FieldLabel | `group/field-label peer/field-label flex w-fit gap-2 leading-snug group-data-[disabled=true]/field:opacity-50` | Label wrapper |
| FieldLabel (nested field) | `has-[>[data-slot=field]]:w-full has-[>[data-slot=field]]:flex-col has-[>[data-slot=field]]:rounded-md has-[>[data-slot=field]]:border` | Bordered nested field |
| FieldLabel (checked) | `has-data-[state=checked]:border-primary has-data-[state=checked]:bg-primary/5 dark:has-data-[state=checked]:bg-primary/10` | Checked highlight |
| FieldTitle | `flex w-fit items-center gap-2 text-sm leading-snug font-medium group-data-[disabled=true]/field:opacity-50` | Title text |
| FieldDescription | `text-sm leading-normal font-normal text-muted-foreground [&>a]:underline [&>a]:underline-offset-4 [&>a:hover]:text-primary` | Helper text |
| FieldSeparator | `relative -my-2 h-5 text-sm` | Divider |
| FieldSeparator content | `relative mx-auto block w-fit bg-background px-2 text-muted-foreground` | Centered text |
| FieldError | `text-sm font-normal text-destructive` | Error message |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--destructive` | Error text color, invalid field text | `shadcn` |
| `--muted-foreground` | Description text, separator content text | `shadcn` |
| `--primary` | Description link hover, checked label border/background | `shadcn` |
| `--background` | Separator content background | `shadcn` |
| `--border` | Separator line, nested field label border (implicit) | `shadcn` |

## Theme Support

### Component Structure

**Field** — 2 variant properties:

| Variant Property | Values |
|-----------------|--------|
| Data invalid | False, True |
| Display | Desc last, Desc second, Horizontal |

Field layout: flex column (gap 12px, width 270px) → Label (text-sm, font-medium) + Input (background, border, rounded, shadow-xs, px 12px, py 8px) + Description (text-sm, muted-foreground)

**Field legend** — Types: Legend, Label

**Fieldset**: Full fieldset with legend, description, and multiple field rows

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--foreground)` | `--foreground` | Label text color |
| `var(--muted-foreground)` | `--muted-foreground` | Description text, placeholder text |
| `var(--input)` | `--input` | Input border color |
| `--radius` (derived) | `--radius` | Input border radius |
| `px-3` (12px) | `px-3` | Input horizontal padding |
| `font-sans` | `--font-sans` | Font family |
| `font-medium` (500) | `font-medium` | Label font weight |
| `text-sm` (14px) | `text-sm` | All text size |
| `shadow-xs` | `shadow-xs` | Input box shadow |

### Theme Behavior

- Error states use `--destructive` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- Checked label highlight differs: `bg-primary/5` in light, `bg-primary/10` in dark
- Separator content uses `--background` for text overlay — adapts to light/dark via theme
- All spacing and typography tokens are mode-independent
