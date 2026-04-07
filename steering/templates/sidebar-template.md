# Sidebar Template

## Metadata

- **Name:** Sidebar Template
- **Source:** shadCN
- **Category:** Template (layout blueprint)
- **Uses:** Sidebar (component), Breadcrumb, Separator, Dropdown Menu, Avatar, Sheet

## Layout Structure

Full-page application shell with a collapsible sidebar and a main content area. The sidebar provides persistent navigation, branding, and user context. The main content area (SidebarInset) fills the remaining viewport and includes a top header bar with breadcrumb navigation.

The base layout is a horizontal two-panel split: sidebar (fixed, left) + content (flex, fills remaining width). SidebarProvider wraps both panels and manages open/closed state, mobile detection, and keyboard shortcut (Ctrl/Cmd+B).

```
┌──────────────────────────────────────────────────┐
│ SidebarProvider                                  │
│ ┌────────────┬───────────────────────────────┐   │
│ │  Sidebar   │  SidebarInset                 │   │
│ │            │  ┌─────────────────────────┐   │   │
│ │  Header    │  │ Header (Trigger + Nav)  │   │   │
│ │  ─────     │  ├─────────────────────────┤   │   │
│ │  Content   │  │                         │   │   │
│ │  (groups)  │  │  Main Content Area      │   │   │
│ │            │  │                         │   │   │
│ │  ─────     │  │                         │   │   │
│ │  Footer    │  └─────────────────────────┘   │   │
│ └────────────┴───────────────────────────────┘   │
└──────────────────────────────────────────────────┘
```

## Regions

| Region | Purpose | Components Used |
|--------|---------|----------------|
| Sidebar Header | Branding, workspace/version switcher, search | SidebarHeader, SidebarMenu, SidebarMenuButton (size="lg"), SidebarInput, Dropdown Menu |
| Sidebar Content | Primary and secondary navigation groups | SidebarContent, SidebarGroup, SidebarGroupLabel, SidebarGroupContent, SidebarMenu, SidebarMenuButton, SidebarMenuSub |
| Sidebar Footer | User profile, account menu | SidebarFooter, SidebarMenu, SidebarMenuButton (size="lg"), Avatar, Dropdown Menu |
| Sidebar Rail | Thin hover-target for toggling sidebar | SidebarRail |
| Content Header | Sidebar trigger, breadcrumb navigation, page actions | SidebarTrigger, Separator, Breadcrumb |
| Content Main | Page content (placeholder — template does not define) | SidebarInset (wraps header + main) |

## Responsive Behavior

| Breakpoint | Behavior |
|------------|----------|
| Desktop (md+) | Sidebar renders as a fixed panel. Width controlled by `--sidebar-width` (default `16rem`). Collapsible modes: `offcanvas` (slides off-screen), `icon` (shrinks to `3rem`), `none` (static). |
| Mobile (<md) | Sidebar renders inside a Sheet overlay. Width: `--sidebar-width-mobile` (`18rem`). Triggered by SidebarTrigger. Sheet slides in from the sidebar's `side` prop direction. |
| Icon-collapsed | Labels, sub-menus, group labels, and group actions are hidden. SidebarMenuButton with `tooltip` prop shows tooltip on hover. SidebarRail provides click/double-click toggle. |

## Component Reference

### Layout Examples

15 sidebar layout examples:

| Example | Description |
|---------|-------------|
| Sidebar 1 | Header (version switcher + search) + grouped navigation sections + rail |
| Sidebar 2 | Header (branding) + flat navigation + documents section + secondary nav + user footer |
| Sidebar 3 | Header (workspace switcher) + collapsible nav groups + projects with actions + user footer |
| Sidebar 4 | Header (team switcher) + platform nav + projects + secondary nav + user footer |
| Sidebar 5 | Header (team switcher) + favorites + workspace nav + user footer |
| Sidebar 6 | Header (branding) + nav with badges + user footer |
| Sidebar 7 | Floating variant with grouped navigation |
| Sidebar 8 | Right-side sidebar with grouped navigation |
| Sidebar 9 | Inset variant with grouped navigation |
| Sidebar 10 | Controlled sidebar (external toggle) |
| Sidebar 11 | Header (team switcher) + nav + projects + secondary + user footer |
| Sidebar 12 | Header (team switcher) + nav + projects + secondary + user footer (variant) |
| Sidebar 13 | Header (team switcher) + nav + projects + secondary + user footer (variant) |
| Sidebar 14 | Header (team switcher) + nav + projects + secondary + user footer (variant) |
| Sidebar 16 | Header (team switcher) + nav + projects + secondary + user footer (variant) |

Each example includes Desktop and Mobile breakpoint frames.

### Theme Behavior

Inherits theme behavior from composed components. No template-specific theme overrides. Sidebar uses `--sidebar` / `--sidebar-foreground` / `--sidebar-border` / `--sidebar-accent` / `--sidebar-accent-foreground` / `--sidebar-ring` CSS variables — all resolve from the active theme.

---

## Construct the Base Template

Build the sidebar template shell using shadcn Sidebar component parts. This produces the foundational two-panel layout that all sidebar examples extend.

### 1. Set Up the Provider

Wrap the entire layout in `SidebarProvider`. This manages open/closed state, mobile detection, cookie persistence, and the Ctrl/Cmd+B keyboard shortcut.

```tsx
import {
  SidebarProvider,
  SidebarInset,
} from "@/components/ui/sidebar"

export default function Layout({ children }) {
  return (
    <SidebarProvider>
      <AppSidebar />
      <SidebarInset>
        <SiteHeader />
        <main>{children}</main>
      </SidebarInset>
    </SidebarProvider>
  )
}
```

### 2. Build the Sidebar Shell

The sidebar has three vertical regions: Header, Content, Footer. Add a SidebarRail for toggle affordance.

```tsx
import {
  Sidebar,
  SidebarHeader,
  SidebarContent,
  SidebarFooter,
  SidebarRail,
} from "@/components/ui/sidebar"

export function AppSidebar(props: React.ComponentProps<typeof Sidebar>) {
  return (
    <Sidebar {...props}>
      <SidebarHeader>
        {/* Branding / version switcher / search */}
      </SidebarHeader>
      <SidebarContent>
        {/* Navigation groups */}
      </SidebarContent>
      <SidebarFooter>
        {/* User profile */}
      </SidebarFooter>
      <SidebarRail />
    </Sidebar>
  )
}
```

### 3. Build the Content Header

The content header sits inside SidebarInset. It contains the SidebarTrigger (toggle button), a vertical Separator, and Breadcrumb navigation.

```tsx
import { SidebarTrigger } from "@/components/ui/sidebar"
import { Separator } from "@/components/ui/separator"
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb"

function SiteHeader() {
  return (
    <header className="flex h-16 shrink-0 items-center gap-2 border-b px-4">
      <SidebarTrigger className="-ml-1" />
      <Separator
        orientation="vertical"
        className="mr-2 data-[orientation=vertical]:h-4"
      />
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem className="hidden md:block">
            <BreadcrumbLink href="#">Section</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator className="hidden md:block" />
          <BreadcrumbItem>
            <BreadcrumbPage>Current Page</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
    </header>
  )
}
```

---

## Construct Sidebar Regions

### Populate the Header

Use SidebarMenu with a `size="lg"` button for branding. Optionally add a version/workspace switcher via Dropdown Menu, and a search input via SidebarInput.

```tsx
<SidebarHeader>
  <SidebarMenu>
    <SidebarMenuItem>
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <SidebarMenuButton
            size="lg"
            className="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
          >
            <div className="flex aspect-square size-8 items-center justify-center rounded-lg bg-sidebar-primary text-sidebar-primary-foreground">
              <AppIcon className="size-4" />
            </div>
            <div className="flex flex-col gap-0.5 leading-none">
              <span className="font-medium">App Name</span>
              <span className="">v1.0.0</span>
            </div>
            <ChevronsUpDown className="ml-auto" />
          </SidebarMenuButton>
        </DropdownMenuTrigger>
        <DropdownMenuContent
          className="w-(--radix-dropdown-menu-trigger-width)"
          align="start"
        >
          {/* Version/workspace items */}
        </DropdownMenuContent>
      </DropdownMenu>
    </SidebarMenuItem>
  </SidebarMenu>
  {/* Optional: SearchForm using SidebarInput */}
</SidebarHeader>
```

### Populate Navigation Groups

Each navigation section is a SidebarGroup with a label and menu items. Items use `asChild` with anchor tags for navigation. Mark the active item with `isActive`.

```tsx
<SidebarContent>
  <SidebarGroup>
    <SidebarGroupLabel>Navigation</SidebarGroupLabel>
    <SidebarGroupContent>
      <SidebarMenu>
        {navItems.map((item) => (
          <SidebarMenuItem key={item.title}>
            <SidebarMenuButton asChild isActive={item.isActive}>
              <a href={item.url}>
                <item.icon />
                <span>{item.title}</span>
              </a>
            </SidebarMenuButton>
          </SidebarMenuItem>
        ))}
      </SidebarMenu>
    </SidebarGroupContent>
  </SidebarGroup>
</SidebarContent>
```

### Populate the Footer (User Profile)

Use a `size="lg"` SidebarMenuButton with Avatar and a Dropdown Menu for account actions.

```tsx
<SidebarFooter>
  <SidebarMenu>
    <SidebarMenuItem>
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <SidebarMenuButton
            size="lg"
            className="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
          >
            <Avatar className="h-8 w-8 rounded-lg">
              <AvatarImage src={user.avatar} alt={user.name} />
              <AvatarFallback className="rounded-lg">CN</AvatarFallback>
            </Avatar>
            <div className="grid flex-1 text-left text-sm leading-tight">
              <span className="truncate font-medium">{user.name}</span>
              <span className="truncate text-xs text-muted-foreground">
                {user.email}
              </span>
            </div>
          </SidebarMenuButton>
        </DropdownMenuTrigger>
        <DropdownMenuContent side="right" align="end" sideOffset={4}>
          {/* Account, Billing, Notifications, Log out */}
        </DropdownMenuContent>
      </DropdownMenu>
    </SidebarMenuItem>
  </SidebarMenu>
</SidebarFooter>
```

---

## Construct Variant Layouts

### Floating Variant

Adds padding, rounded corners, border, and shadow to the sidebar inner container. Pass `variant="floating"` to the Sidebar component.

```tsx
<SidebarProvider>
  <Sidebar variant="floating">
    {/* Same header/content/footer structure */}
  </Sidebar>
  <SidebarInset>{/* Content */}</SidebarInset>
</SidebarProvider>
```

### Inset Variant

Similar to floating but paired with SidebarInset for coordinated layout. The sidebar wrapper gets `bg-sidebar` background. Pass `variant="inset"`.

```tsx
<SidebarProvider>
  <Sidebar variant="inset">
    {/* Same header/content/footer structure */}
  </Sidebar>
  <SidebarInset>{/* Content */}</SidebarInset>
</SidebarProvider>
```

### Icon-Collapsed Variant

Sidebar shrinks to icon-only width (`3rem`). Add `tooltip` prop to SidebarMenuButton for hover labels. Pass `collapsible="icon"`.

```tsx
<SidebarProvider>
  <Sidebar collapsible="icon">
    <SidebarContent>
      <SidebarGroup>
        <SidebarGroupContent>
          <SidebarMenu>
            <SidebarMenuItem>
              <SidebarMenuButton tooltip="Dashboard">
                <DashboardIcon />
                <span>Dashboard</span>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarGroupContent>
      </SidebarGroup>
    </SidebarContent>
    <SidebarRail />
  </Sidebar>
  <SidebarInset>{/* Content */}</SidebarInset>
</SidebarProvider>
```

### Right-Side Sidebar

Place the sidebar on the right edge. Pass `side="right"`.

```tsx
<SidebarProvider>
  <SidebarInset>{/* Content (renders first) */}</SidebarInset>
  <Sidebar side="right">
    {/* Same structure */}
  </Sidebar>
</SidebarProvider>
```

### Custom Width

Override the default `16rem` width via CSS custom properties on SidebarProvider.

```tsx
<SidebarProvider
  style={{
    "--sidebar-width": "calc(var(--spacing) * 72)",
    "--header-height": "calc(var(--spacing) * 12)",
  } as React.CSSProperties}
>
  <Sidebar variant="inset">{/* ... */}</Sidebar>
  <SidebarInset>{/* ... */}</SidebarInset>
</SidebarProvider>
```
