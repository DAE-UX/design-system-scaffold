# Login Template

## Metadata

- **Name:** Login Template
- **Source:** shadCN
- **Category:** Template (layout blueprint)
- **Uses:** Card, Field, Input, Button, Label, Separator

## Layout Structure

Centered authentication page with a single Card containing the login form. The page fills the viewport height and centers the form both horizontally and vertically. The Card is constrained to a max width for readability.

```
+--------------------------------------------------+
|                                                  |
|              +------------------+                |
|              |  Card            |                |
|              |  CardHeader      |                |
|              |  Title           |                |
|              |  Description     |                |
|              |  +--------------+|                |
|              |  | CardContent  ||                |
|              |  | Email        ||                |
|              |  | Password     ||                |
|              |  | [Login]      ||                |
|              |  | [Social]     ||                |
|              |  | Sign up link ||                |
|              |  +--------------+|                |
|              +------------------+                |
|                                                  |
+--------------------------------------------------+
```

## Regions

| Region | Purpose | Components Used |
|--------|---------|----------------|
| Page Container | Full-viewport centering wrapper | `div` with flex centering |
| Form Container | Max-width constraint for the form | `div` with `max-w-sm` |
| Card Header | Title and description text | Card, CardHeader, CardTitle, CardDescription |
| Card Content | Form fields and actions | CardContent, Field, FieldGroup, FieldLabel, Input, Button |
| Supporting Links | Forgot password, sign up, social auth | Button (variant="outline"), FieldDescription, anchor tags |

## Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| Desktop (md+) | Card centered in viewport with `p-10` page padding. Form width constrained by `max-w-sm`. |
| Mobile (<md) | Card centered with `p-6` page padding. Form fills available width within max-width constraint. Card maintains its structure. |

## Component Reference

### Layout Variants

5 login layout variants:

| Variant | Description |
|---------|-------------|
| Login 1 | Centered Card with email + password fields, login button, social login button, sign-up link |
| Login 2 | Split layout: left panel with branding/image, right panel with login form |
| Login 3 | Split layout: left panel with login form, right panel with branding/image |
| Login 4 | Full-width form without Card wrapper, centered on page |
| Login 5 | Centered Card with additional "Remember me" checkbox and terms links |

### Theme Behavior

Inherits theme behavior from composed components. No template-specific theme overrides.

---

## Construct the Base Template (Login 1)

Build the centered Card login form using shadcn components. This is the simplest variant: a single centered Card with email/password fields.

### 1. Set Up the Page Container

The page wrapper centers the Card vertically and horizontally using flexbox. `min-h-svh` ensures full viewport height.

```tsx
export default function LoginPage() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6 md:p-10">
      <div className="w-full max-w-sm">
        <LoginForm />
      </div>
    </div>
  )
}
```

### 2. Build the Login Form

The form uses Card for structure, Field/FieldGroup for form layout, and Input for fields. Button handles submission and social auth.

```tsx
import { Button } from "@/components/ui/button"
import {
  Card, CardContent, CardDescription, CardHeader, CardTitle,
} from "@/components/ui/card"
import {
  Field, FieldDescription, FieldGroup, FieldLabel,
} from "@/components/ui/field"
import { Input } from "@/components/ui/input"

export function LoginForm({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div className={cn("flex flex-col gap-6", className)} {...props}>
      <Card>
        <CardHeader>
          <CardTitle>Login to your account</CardTitle>
          <CardDescription>
            Enter your email below to login to your account
          </CardDescription>
        </CardHeader>
        <CardContent>
          <form>
            <FieldGroup>
              <Field>
                <FieldLabel htmlFor="email">Email</FieldLabel>
                <Input
                  id="email"
                  type="email"
                  placeholder="m@example.com"
                  required
                />
              </Field>
              <Field>
                <div className="flex items-center">
                  <FieldLabel htmlFor="password">Password</FieldLabel>
                  <a
                    href="#"
                    className="ml-auto inline-block text-sm underline-offset-4 hover:underline"
                  >
                    Forgot your password?
                  </a>
                </div>
                <Input id="password" type="password" required />
              </Field>
              <Field>
                <Button type="submit">Login</Button>
                <Button variant="outline" type="button">
                  Login with Google
                </Button>
                <FieldDescription className="text-center">
                  Don&apos;t have an account? <a href="#">Sign up</a>
                </FieldDescription>
              </Field>
            </FieldGroup>
          </form>
        </CardContent>
      </Card>
    </div>
  )
}
```

---

## Construct Variant Layouts

### Split Layout (Login 2 / Login 3)

A two-column layout with the login form on one side and a branding/image panel on the other. Uses CSS Grid with `lg:grid-cols-2`. The image panel is hidden on mobile.

```tsx
export default function LoginPage() {
  return (
    <div className="grid min-h-svh lg:grid-cols-2">
      <div className="flex flex-col gap-4 p-6 md:p-10">
        <div className="flex justify-center gap-2 md:justify-start">
          {/* Logo / branding */}
        </div>
        <div className="flex flex-1 items-center justify-center">
          <div className="w-full max-w-xs">
            <LoginForm />
          </div>
        </div>
      </div>
      <div className="relative hidden bg-muted lg:block">
        <img
          src="/placeholder.svg"
          alt="Login illustration"
          className="absolute inset-0 h-full w-full object-cover"
        />
      </div>
    </div>
  )
}
```

To swap sides (Login 3), reverse the grid column order or place the image `div` first.

### Full-Width Form (Login 4)

Remove the Card wrapper. Center the form fields directly on the page.

```tsx
export default function LoginPage() {
  return (
    <div className="flex min-h-svh flex-col items-center justify-center gap-6 p-6 md:p-10">
      <div className="flex w-full max-w-sm flex-col gap-6">
        <div className="flex flex-col gap-2 text-center">
          <h1 className="text-2xl font-bold">Login to your account</h1>
          <p className="text-sm text-muted-foreground">
            Enter your email below to login
          </p>
        </div>
        <form>
          <FieldGroup>
            {/* Same field structure as base template */}
          </FieldGroup>
        </form>
      </div>
    </div>
  )
}
```

### With Remember Me and Terms (Login 5)

Extend the base Card form with a Checkbox for "Remember me" and footer links for Terms/Privacy.

```tsx
import { Checkbox } from "@/components/ui/checkbox"

{/* Inside FieldGroup, after password field */}
<Field>
  <div className="flex items-center gap-2">
    <Checkbox id="remember" />
    <FieldLabel htmlFor="remember" className="text-sm font-normal">
      Remember me
    </FieldLabel>
  </div>
</Field>

{/* After the Card */}
<div className="text-center text-xs text-muted-foreground">
  By clicking continue, you agree to our{" "}
  <a href="#" className="underline underline-offset-4 hover:text-primary">
    Terms of Service
  </a>{" "}
  and{" "}
  <a href="#" className="underline underline-offset-4 hover:text-primary">
    Privacy Policy
  </a>.
</div>
```
