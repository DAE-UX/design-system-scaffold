# Component: Carousel

> **TL;DR:** A slideshow component built on embla-carousel-react. 5 parts: Carousel, CarouselContent, CarouselItem, CarouselPrevious, CarouselNext. Supports horizontal/vertical orientation, configurable slide sizes via `basis` utility, spacing via negative margin pattern, Embla options (loop, align), plugins (autoplay), and API access via `setApi`. Uses `role="region"` with `aria-roledescription="carousel"` and `role="group"` with `aria-roledescription="slide"` for accessibility. Arrow key navigation built in.

## Metadata

- **Component Name:** Carousel
- **Source:** shadCN (embla-carousel)
- **Dependencies:** embla-carousel-react

## Behavior

A slideshow component for cycling through content slides. Built on Embla Carousel with shadCN styling and accessibility.

- Wraps `useEmblaCarousel` hook with React context for child component communication
- Supports horizontal (default) and vertical orientation
- Horizontal maps to Embla `axis: "x"`, vertical maps to `axis: "y"`
- Navigation buttons (Previous/Next) auto-disable when at scroll boundaries (`canScrollPrev` / `canScrollNext`)
- Arrow key navigation: ArrowLeft scrolls previous, ArrowRight scrolls next (captured at root level)
- Slide sizes controlled via `basis` utility on CarouselItem (e.g., `basis-1/3` for 3 visible slides)
- Spacing controlled via negative margin on CarouselContent (`-ml-4`) and padding on CarouselItem (`pl-4`)
- Supports Embla options via `opts` prop (loop, align, dragFree, etc.)
- Supports Embla plugins via `plugins` prop (autoplay, auto-scroll, etc.)
- API instance accessible via `setApi` prop for programmatic control and event listening
- Navigation buttons render as absolute-positioned round outline buttons
- Supports RTL via `dir` prop and `opts.direction` (both must match)
- RTL navigation buttons should use `rtl:rotate-180` class

### Anti-Patterns

#### Do
- Use for browsing a set of related items (images, cards)
- Provide visible navigation controls
- Indicate the current position within the set
- Ensure carousels advance a narrative or serve a functional purpose

#### Don't
- Don't auto-advance without user control
- Don't use for critical content that users might miss
- Don't use for unrelated content items
- Don't hide navigation indicators
- Don't use carousels purely to fill space — every slide should have a reason

Sources: shadCN, Radix UI, UI Guidelines

## API

### Parts

- `Carousel` — Root container. Provides context (carouselRef, api, orientation, scroll methods). Renders as `div` with `role="region"` and `aria-roledescription="carousel"`.
- `CarouselContent` — Scrollable content wrapper. Applies Embla ref and flex layout.
- `CarouselItem` — Individual slide. Renders as `div` with `role="group"` and `aria-roledescription="slide"`.
- `CarouselPrevious` — Previous slide button. Renders as absolute-positioned round Button.
- `CarouselNext` — Next slide button. Renders as absolute-positioned round Button.

### Props

#### Carousel

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| orientation | `"horizontal" \| "vertical"` | `"horizontal"` | Slide direction |
| opts | `EmblaOptionsType` | — | Embla carousel options (loop, align, dragFree, etc.) |
| plugins | `EmblaPluginType[]` | — | Embla plugins (autoplay, auto-scroll, etc.) |
| setApi | `(api: CarouselApi) => void` | — | Callback to receive the Embla API instance |
| dir | `"ltr" \| "rtl"` | — | Text direction |
| className | `string` | — | Additional CSS classes |

#### CarouselContent

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes (use `-ml-[VALUE]` for spacing) |

#### CarouselItem

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| className | `string` | — | Additional CSS classes (use `basis-[VALUE]` for sizing, `pl-[VALUE]` for spacing) |

#### CarouselPrevious / CarouselNext

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | `ButtonVariant` | `"outline"` | Button style variant |
| size | `ButtonSize` | `"icon"` | Button size |
| className | `string` | — | Additional CSS classes |

### Variants

No CVA variants. Layout is controlled by `orientation` prop and Embla options.

| Configuration | Description | Use Case |
|---------------|-------------|----------|
| `orientation="horizontal"` | Slides scroll left/right | Default carousel |
| `orientation="vertical"` | Slides scroll up/down | Vertical content feeds |
| `opts={{ loop: true }}` | Infinite loop scrolling | Continuous slideshows |
| `opts={{ align: "start" }}` | Align slides to start | Multi-slide carousels |

### Data Attributes

| Attribute | Values | Applies To |
|-----------|--------|------------|
| `[data-slot]` | `"carousel"` | Carousel root |
| `[data-slot]` | `"carousel-content"` | CarouselContent overflow wrapper |
| `[data-slot]` | `"carousel-item"` | CarouselItem |
| `[data-slot]` | `"carousel-previous"` | CarouselPrevious button |
| `[data-slot]` | `"carousel-next"` | CarouselNext button |

### CSS Variables (Radix)

Not applicable — Carousel uses embla-carousel-react, not Radix primitives.

## Composition Patterns

### Basic Carousel

```tsx
<Carousel>
  <CarouselContent>
    <CarouselItem>Slide 1</CarouselItem>
    <CarouselItem>Slide 2</CarouselItem>
    <CarouselItem>Slide 3</CarouselItem>
  </CarouselContent>
  <CarouselPrevious />
  <CarouselNext />
</Carousel>
```

### Multiple Slides Visible (Sizes)

```tsx
<Carousel>
  <CarouselContent>
    <CarouselItem className="basis-1/3">Slide 1</CarouselItem>
    <CarouselItem className="basis-1/3">Slide 2</CarouselItem>
    <CarouselItem className="basis-1/3">Slide 3</CarouselItem>
  </CarouselContent>
</Carousel>
```

### Responsive Sizes

```tsx
<Carousel>
  <CarouselContent>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">Slide 1</CarouselItem>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">Slide 2</CarouselItem>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">Slide 3</CarouselItem>
  </CarouselContent>
</Carousel>
```

### Custom Spacing

```tsx
<Carousel>
  <CarouselContent className="-ml-2 md:-ml-4">
    <CarouselItem className="pl-2 md:pl-4">Slide 1</CarouselItem>
    <CarouselItem className="pl-2 md:pl-4">Slide 2</CarouselItem>
    <CarouselItem className="pl-2 md:pl-4">Slide 3</CarouselItem>
  </CarouselContent>
</Carousel>
```

### Vertical Orientation

```tsx
<Carousel orientation="vertical">
  <CarouselContent>
    <CarouselItem>Slide 1</CarouselItem>
    <CarouselItem>Slide 2</CarouselItem>
    <CarouselItem>Slide 3</CarouselItem>
  </CarouselContent>
  <CarouselPrevious />
  <CarouselNext />
</Carousel>
```

### With Loop and Autoplay

```tsx
import Autoplay from "embla-carousel-autoplay"

<Carousel
  opts={{ loop: true }}
  plugins={[Autoplay({ delay: 2000 })]}
>
  <CarouselContent>
    <CarouselItem>Slide 1</CarouselItem>
    <CarouselItem>Slide 2</CarouselItem>
    <CarouselItem>Slide 3</CarouselItem>
  </CarouselContent>
</Carousel>
```

### API Access (Slide Counter)

```tsx
const [api, setApi] = React.useState<CarouselApi>()
const [current, setCurrent] = React.useState(0)
const [count, setCount] = React.useState(0)

React.useEffect(() => {
  if (!api) return
  setCount(api.scrollSnapList().length)
  setCurrent(api.selectedScrollSnap() + 1)
  api.on("select", () => {
    setCurrent(api.selectedScrollSnap() + 1)
  })
}, [api])

<Carousel setApi={setApi}>
  <CarouselContent>
    <CarouselItem>Slide 1</CarouselItem>
    <CarouselItem>Slide 2</CarouselItem>
  </CarouselContent>
</Carousel>
<p>Slide {current} of {count}</p>
```

### RTL

```tsx
<Carousel dir="rtl" opts={{ direction: "rtl" }}>
  <CarouselContent>
    <CarouselItem>Slide 1</CarouselItem>
    <CarouselItem>Slide 2</CarouselItem>
  </CarouselContent>
  <CarouselPrevious className="rtl:rotate-180" />
  <CarouselNext className="rtl:rotate-180" />
</Carousel>
```

## Accessibility

- **WAI-ARIA Pattern:** Carousel pattern — uses `role="region"` with `aria-roledescription="carousel"` on root, `role="group"` with `aria-roledescription="slide"` on each item

### ARIA Roles

| Element | Role / Attribute | Notes |
|---------|-----------------|-------|
| Carousel | `role="region"`, `aria-roledescription="carousel"` | Identifies the carousel landmark |
| CarouselItem | `role="group"`, `aria-roledescription="slide"` | Identifies each slide |
| CarouselPrevious | `button` with `sr-only` text "Previous slide" | Screen reader label |
| CarouselNext | `button` with `sr-only` text "Next slide" | Screen reader label |

### Keyboard Behavior

| Key | Behavior |
|-----|----------|
| Arrow Left | Scroll to previous slide |
| Arrow Right | Scroll to next slide |
| Tab | Move focus to next focusable element (navigation buttons, slide content) |

## HTML

### Standalone HTML Structure

```html
<div role="region" aria-roledescription="carousel" data-slot="carousel">
  <div class="overflow-hidden" data-slot="carousel-content">
    <div class="flex -ml-4">
      <div role="group" aria-roledescription="slide" data-slot="carousel-item" class="pl-4 min-w-0 shrink-0 grow-0 basis-full">
        Slide 1
      </div>
      <div role="group" aria-roledescription="slide" data-slot="carousel-item" class="pl-4 min-w-0 shrink-0 grow-0 basis-full">
        Slide 2
      </div>
    </div>
  </div>
  <button data-slot="carousel-previous" class="absolute size-8 rounded-full top-1/2 -left-12 -translate-y-1/2" disabled>
    <svg><!-- ArrowLeft --></svg>
    <span class="sr-only">Previous slide</span>
  </button>
  <button data-slot="carousel-next" class="absolute size-8 rounded-full top-1/2 -right-12 -translate-y-1/2">
    <svg><!-- ArrowRight --></svg>
    <span class="sr-only">Next slide</span>
  </button>
</div>
```

## CSS

### Raw CSS

```css
/* Carousel root */
.Carousel {
  position: relative;
}

/* CarouselContent overflow wrapper */
.CarouselContent {
  overflow: hidden;
}

/* CarouselContent inner flex (horizontal) */
.CarouselContent > div {
  display: flex;
  margin-left: -1rem;
}

/* CarouselContent inner flex (vertical) */
.CarouselContent[data-orientation="vertical"] > div {
  display: flex;
  flex-direction: column;
  margin-top: -1rem;
}

/* CarouselItem */
.CarouselItem {
  min-width: 0;
  flex-shrink: 0;
  flex-grow: 0;
  flex-basis: 100%;
  padding-left: 1rem;
}

/* CarouselItem (vertical) */
.CarouselItem[data-orientation="vertical"] {
  padding-left: 0;
  padding-top: 1rem;
}

/* Navigation buttons */
.CarouselPrevious,
.CarouselNext {
  position: absolute;
  width: 2rem;
  height: 2rem;
  border-radius: 9999px;
}

/* Horizontal positioning */
.CarouselPrevious {
  top: 50%;
  left: -3rem;
  transform: translateY(-50%);
}

.CarouselNext {
  top: 50%;
  right: -3rem;
  transform: translateY(-50%);
}

/* Vertical positioning */
.Carousel[data-orientation="vertical"] .CarouselPrevious {
  top: -3rem;
  left: 50%;
  transform: translateX(-50%) rotate(90deg);
}

.Carousel[data-orientation="vertical"] .CarouselNext {
  bottom: -3rem;
  left: 50%;
  transform: translateX(-50%) rotate(90deg);
}
```

### Tailwind Mapping

| Element | Tailwind Classes | Purpose |
|---------|-----------------|---------|
| Carousel | `relative` | Positioning context |
| CarouselContent (wrapper) | `overflow-hidden` | Hide overflow slides |
| CarouselContent (inner, horizontal) | `flex -ml-4` | Flex row with negative margin for spacing |
| CarouselContent (inner, vertical) | `flex -mt-4 flex-col` | Flex column with negative margin |
| CarouselItem (horizontal) | `min-w-0 shrink-0 grow-0 basis-full pl-4` | Full-width slide with left padding |
| CarouselItem (vertical) | `min-w-0 shrink-0 grow-0 basis-full pt-4` | Full-height slide with top padding |
| CarouselPrevious (horizontal) | `absolute size-8 rounded-full top-1/2 -left-12 -translate-y-1/2` | Left-centered nav button |
| CarouselNext (horizontal) | `absolute size-8 rounded-full top-1/2 -right-12 -translate-y-1/2` | Right-centered nav button |
| CarouselPrevious (vertical) | `absolute size-8 rounded-full -top-12 left-1/2 -translate-x-1/2 rotate-90` | Top-centered nav button |
| CarouselNext (vertical) | `absolute size-8 rounded-full -bottom-12 left-1/2 -translate-x-1/2 rotate-90` | Bottom-centered nav button |

## CSS Variable Dependencies

Carousel itself uses no CSS variables directly. Navigation buttons inherit from the Button component:

| Variable | Purpose | Source |
|----------|---------|--------|
| `--border` | Navigation button border (outline variant) | `shadcn` |
| `--background` | Navigation button background (outline variant) | `shadcn` |
| `--foreground` | Navigation button icon color | `shadcn` |
| `--accent` | Navigation button hover background | `shadcn` |
| `--accent-foreground` | Navigation button hover text | `shadcn` |

## Theme Support

### Component Structure

**Carousel Arrows** — 4 variant properties:

| Variant Property | Values |
|-----------------|--------|
| Hover | False, True |
| Disabled | False, True |
| Focus | False, True |
| Size | Small (32px), Medium (44px), Large (64px) |

Arrow layout: flex center (32×32px default, background, border, rounded-full, shadow) → Arrow icon (16×16px)

**Carousel Card** — Types: Image, Component swap

**Carousel** (composed) — Horizontal/Vertical × Large/Small

### CSS Variable Mapping

CSS variable mappings for this component. Values are defined in the active theme file (e.g., `default.md`), not here.

| Token | CSS Variable | Purpose |
|-------|-------------|---------|
| `var(--background)` | `--background` | Arrow button background |
| `var(--border)` | `--border` | Arrow button border |
| `rounded-full` | `rounded-full` | Arrow button shape |
| `size-8` (32px) | `size-8` | Small arrow button size |
| `shadow-2xs` | `shadow-2xs` | Arrow button box shadow |

### Theme Behavior

- Carousel container is transparent — no background color
- Navigation buttons use Button component's outline variant — adapts to light/dark via theme
- Navigation buttons auto-disable at scroll boundaries (reduced opacity via `disabled` attribute)
- Slide content inherits parent theme — no carousel-specific theme tokens
- Spacing and sizing tokens (`-ml-4`, `pl-4`, `size-8`, `-left-12`) are mode-independent
- Vertical orientation rotates navigation buttons 90° — same theme behavior
