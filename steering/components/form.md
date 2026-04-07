# Component: Form

> **TL;DR:** A React Hook Form integration layer for shadCN form fields. 7 parts: Form (FormProvider), FormField (Controller), FormItem, FormLabel, FormControl, FormDescription, FormMessage. Provides automatic `id` generation, `aria-describedby` wiring, `aria-invalid` propagation, and error message display. Built on react-hook-form with Radix Label and Slot. `useFormField` hook exposes field state and generated IDs.

## Metadata

- **Component Name:** Form
- **Source:** shadCN (React Hook Form integration)
- **Dependencies:** radix-ui, react-hook-form, @hookform/resolvers, zod

## Behavior

An integration layer that connects React Hook Form to shadCN form UI components with automatic accessibility wiring.

- Form is a re-export of `FormProvider` from react-hook-form
- FormField wraps `Controller` and provides field name context via `FormFieldContext`
- FormItem generates a unique `id` via `useId()` and provides it via `FormItemContext`
- FormLabel renders a Radix Label with `htmlFor` pointing to the generated form item ID; shows destructive color on error
- FormControl renders a Radix Slot that injects `id`, `aria-describedby`, and `aria-invalid` onto its child
- `aria-describedby` includes description ID always; adds message ID when error exists
- FormDescription renders helper text with the generated description ID
- FormMessage renders error message from field state with the generated message ID; returns null when no error and no children
- `useFormField` hook returns field state, error, and all generated IDs (`formItemId`, `formDescriptionId`, `formMessageId`)
- All IDs follow the pattern: `{useId()}-form-item`, `{useId()}-form-item-description`, `{useId()}-form-item-message`

### Anti-Patterns

#### Do
- Provide a label for every form control
- Group related fields together
- Use inline validation where possible
- Provide clear error messages that explain what went wrong and how to fix it

#### Don't
- Don't disable submit buttons in multi-field forms — validate on submit
- Don't use error validation for non-blocking messages
- Don't require users to re-enter data after a validation error

Sources: shadCN, Radix UI

## API

### Parts

- `Form` — Root provider. Re-export of `FormProvider` from react-hook-form.
- `FormField` — Field controller. Wraps `Controller` with name context.
- `FormItem` — Field container. Generates unique ID for accessibility wiring.
- `FormLabel` — Label element. Auto-wired to form item via `htmlFor`.
- `FormControl` — Control slot. Injects `id`, `aria-describedby`, `aria-invalid` onto child.
- `FormDescription` — Helper text. Wired to control via `aria-describedby`.
- `FormMessage` — Error message. Displays field error or custom children.

### Props

#### Form

Accepts all `FormProvider` props from react-hook-form (pass the return value of `useForm()`).

#### FormField

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| name | `FieldPath<TFieldValues>` | — | Field name in the form schema |
| control | `Control<TFieldValues>` | — | Form control from `useForm()` |
| render | `({ field, fieldState, formState }) => React.ReactElement` | — | Render function for the field |

#### FormItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FormLabel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FormControl

Accepts all props of Radix `Slot.Root`. Renders as its child element.

#### FormDescription

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes |

#### FormMessage

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| children | `React.ReactNode` | — | Custom message (overrides field error) |
| className | `string` | — | Additional CSS classes |

### Variants

No variants — Form components have a single visual style.

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"form-item"`, `"form-label"`, `"form-control"`, `"form-description"`, `"form-message"` | All parts |
| `[data-error]` | `"true"`, `"false"` | FormLabel |

### CSS Variables (Radix)

Not applicable — Form does not use Radix visual primitives.

## Composition Patterns

### Basic Form Field

```tsx
<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)}>
    <FormField
      control={form.control}
      name="username"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Username</FormLabel>
          <FormControl>
            <Input placeholder="shadcn" {...field} />
          </FormControl>
          <FormDescription>
            This is your public display name.
          </FormDescription>
          <FormMessage />
        </FormItem>
      )}
    />
    <Button type="submit">Submit</Button>
  </form>
</Form>
```

### Multiple Fields

```tsx
<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
    <FormField
      control={form.control}
      name="email"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Email</FormLabel>
          <FormControl>
            <Input type="email" {...field} />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
    <FormField
      control={form.control}
      name="password"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Password</FormLabel>
          <FormControl>
            <Input type="password" {...field} />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
    <Button type="submit">Sign In</Button>
  </form>
</Form>
```

## Accessibility

- **WAI-ARIA Pattern:** None — Form uses native HTML form semantics with enhanced ARIA wiring

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| FormControl | `aria-invalid` | Set to `true` when field has error |
| FormControl | `aria-describedby` | Points to description ID; adds message ID on error |
| FormLabel | `label` with `htmlFor` | Auto-wired to form item ID |
| FormDescription | `id={formDescriptionId}` | Referenced by `aria-describedby` |
| FormMessage | `id={formMessageId}` | Referenced by `aria-describedby` on error |

### Keyboard Behavior

No keyboard interaction — Form is a layout/wiring layer. Interactive elements inside (inputs, selects) handle their own keyboard behavior.

## HTML

### Standalone HTML Structure

```html
<form>
  <div data-slot="form-item">
    <label data-slot="form-label" for="r1-form-item">Username</label>
    <input data-slot="form-control" id="r1-form-item"
           aria-describedby="r1-form-item-description"
           aria-invalid="false" />
    <p data-slot="form-description" id="r1-form-item-description">
      This is your public display name.
    </p>
    <p data-slot="form-message" id="r1-form-item-message">
      <!-- Error message appears here -->
    </p>
  </div>
</form>
```

## CSS

### Raw CSS

```css
/* FormItem */
.FormItem {
  display: grid;
  gap: 0.5rem;
}

/* FormLabel */
.FormLabel[data-error="true"] {
  color: var(--destructive);
}

/* FormDescription */
.FormDescription {
  font-size: 0.875rem;
  color: var(--muted-foreground);
}

/* FormMessage */
.FormMessage {
  font-size: 0.875rem;
  color: var(--destructive);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| FormItem | `grid gap-2` | Field container layout |
| FormLabel | `data-[error=true]:text-destructive` | Error state color |
| FormDescription | `text-sm text-muted-foreground` | Helper text |
| FormMessage | `text-sm text-destructive` | Error message |

## CSS Variable Dependencies

| Variable | Purpose | Source |
|----------|---------|--------|
| `--destructive` | Error label color, error message text | `shadcn` |
| `--muted-foreground` | Description text color | `shadcn` |

## Theme Support

### Component Structure

Form is a React Hook Form integration layer — it provides accessibility wiring and state management, not visual components. The visual appearance is determined by child components (Input, Select, Checkbox, etc.) which have their own component specs.

### CSS Variable Mapping

Form has no component-specific design tokens. It is a wiring layer that delegates all visual styling to child components.

The error Alert uses the same tokens documented in `alert.md`:

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `--card` | `--card` | Alert background |
| `--border` | `--border` | Alert border |
| `--foreground` | `--foreground` | Title text |
| `--muted-foreground` | `--muted-foreground` | Description text |

### Theme Behavior

- Error states use `--destructive` — adapts to light/dark via theme
- Description uses `--muted-foreground` — adapts to light/dark via theme
- All spacing tokens are mode-independent
- Form is primarily a wiring layer; visual theming is handled by the child components (Input, Select, etc.)
