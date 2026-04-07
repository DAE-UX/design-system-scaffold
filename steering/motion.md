# Motion (Animation Library) — Design System Skill

> **TL;DR:** Motion (prev Framer Motion) is the animation library for cases where CSS transitions are insufficient and shadCN components don't provide native animation. Priority chain: shadCN native animation → CSS transitions → Motion. Use Motion for spring physics, gesture-driven animations, layout transitions, scroll-linked effects, and exit animations. Always reference the motion heuristics in `guidelines/design/ui-guidelines.md` → Motion when selecting animations. Minimize dependencies — reach for Motion only when simpler approaches fall short.

## Purpose

Animation library reference and usage guidance for agents generating UI with the design system.

**Use for:** Adding animation to components, pages, and prototypes when CSS transitions or shadCN native approaches are insufficient.

**Do not use for:** Simple hover color changes or opacity fades (use CSS transitions), animations already handled by shadCN/Radix (use their native approach), component behavior specs (see `design-system/components/`).

---

## Animation Priority Chain

Before reaching for Motion, follow this decision tree:

```
Does the shadCN component handle this animation natively?
├─ YES → Use the shadCN/Radix native approach (Accordion expand, Dialog open/close, etc.)
│        The component spec is authoritative. Do NOT override unless the user explicitly requests it.
│
└─ NO → Can a CSS transition handle it reliably?
    ├─ YES → Use CSS transition (e.g., `transition: opacity 200ms ease`)
    │        Prefer Tailwind utilities: `transition-opacity duration-200 ease-in-out`
    │
    └─ NO → Is the animation complex, physics-based, gesture-driven, or layout-dependent?
        └─ YES → Use Motion
```

### When CSS Transitions Are Sufficient

- Simple property changes: opacity, color, background-color, border-color, transform
- Hover/focus state changes with predictable start/end values
- Single-property transitions with standard easing

### When Motion Is Needed

- Spring physics (natural-feeling bounces, snaps)
- Interruptible animations (user can change direction mid-animation)
- Gesture-driven: drag, pan, tap with animated feedback
- Layout animations (animating width/height changes, reordering lists)
- Shared element transitions between components (`layoutId`)
- Scroll-linked animations (parallax, progress bars, sticky storytelling)
- Exit animations (animating elements as they leave the DOM)
- Orchestrated sequences (staggered children, chained animations)
- Animating values CSS can't (e.g., `mask-image`, complex `filter` chains, `height: auto`)

---

## Installation

```bash
npm install motion
```

Import in React components:

```typescript
import { motion, AnimatePresence } from "motion/react"
```

Source: `https://motion.dev/docs/react-quick-start`

---

## Core API

### The `<motion />` Component

Prefix any HTML or SVG element with `motion.` to enable animation props:

```tsx
<motion.div animate={{ opacity: 1 }} />
```

### Key Animation Props

| Prop | Purpose | Example |
|------|---------|---------|
| `animate` | Target values to animate to | `animate={{ x: 100, opacity: 1 }}` |
| `initial` | Starting values (before enter animation) | `initial={{ opacity: 0, y: 20 }}` |
| `exit` | Values to animate to when removed from DOM | `exit={{ opacity: 0 }}` |
| `transition` | Animation configuration (duration, easing, spring) | `transition={{ type: "spring", stiffness: 300 }}` |
| `whileHover` | Animate while pointer hovers | `whileHover={{ scale: 1.05 }}` |
| `whileTap` | Animate while pressed | `whileTap={{ scale: 0.95 }}` |
| `whileFocus` | Animate while focused (`:focus-visible` rules) | `whileFocus={{ borderColor: "var(--ring)" }}` |
| `whileInView` | Animate when element enters viewport | `whileInView={{ opacity: 1 }}` |
| `whileDrag` | Animate while being dragged | `whileDrag={{ scale: 1.1 }}` |
| `layout` | Animate layout changes automatically | `<motion.div layout />` |
| `layoutId` | Shared element transitions between components | `<motion.div layoutId="modal" />` |
| `drag` | Enable drag gesture | `<motion.div drag="x" />` |

### Transition Types

| Type | Use Case | Example |
|------|----------|---------|
| `spring` (default for transforms) | Natural-feeling motion | `{ type: "spring", stiffness: 300, damping: 20 }` |
| `tween` (default for visual props) | Precise duration control | `{ type: "tween", duration: 0.3, ease: "easeInOut" }` |
| `inertia` | Deceleration from velocity | `{ type: "inertia", velocity: 200 }` |

### Value-Specific Transitions

```tsx
<motion.div
  animate={{ x: 0, opacity: 1 }}
  transition={{
    default: { type: "spring" },
    opacity: { ease: "linear", duration: 0.2 }
  }}
/>
```

---

## Common Patterns

### Enter/Exit with AnimatePresence

Required for exit animations — wraps components that may be removed from the DOM:

```tsx
import { motion, AnimatePresence } from "motion/react"

<AnimatePresence>
  {isVisible && (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -20 }}
    />
  )}
</AnimatePresence>
```

### Scroll-Triggered (Fade In on Scroll)

```tsx
<motion.section
  initial={{ opacity: 0, y: 40 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true }}
  transition={{ duration: 0.6 }}
/>
```

### Scroll-Linked (Progress Bar)

```tsx
import { motion, useScroll } from "motion/react"

function ProgressBar() {
  const { scrollYProgress } = useScroll()
  return <motion.div style={{ scaleX: scrollYProgress, originX: 0 }} />
}
```

### Layout Animation (List Reorder)

```tsx
<motion.ul layout>
  {items.map(item => (
    <motion.li key={item.id} layout />
  ))}
</motion.ul>
```

### Shared Element Transition

```tsx
// Thumbnail
<motion.img layoutId={`image-${id}`} src={thumb} />

// Expanded view
<AnimatePresence>
  {selected && <motion.img layoutId={`image-${selected}`} src={full} />}
</AnimatePresence>
```

### Staggered Children

```tsx
const container = {
  hidden: {},
  show: { transition: { staggerChildren: 0.1 } }
}
const item = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0 }
}

<motion.ul variants={container} initial="hidden" animate="show">
  {items.map(i => <motion.li key={i} variants={item} />)}
</motion.ul>
```

---

## Design System Integration

### Referencing Motion Heuristics

When selecting animations, always consult `guidelines/design/ui-guidelines.md` → Motion section:

- Ship at least 2–3 intentional motions for visually led work
- Use motion to create presence and hierarchy, not noise
- Recommended motions: one entrance sequence in the hero, one scroll-linked/sticky/depth effect, one hover/reveal/layout transition that sharpens affordance
- Motion must be noticeable in a quick recording

### shadCN Components with Native Animation

These components already handle their own animation via Radix UI or CSS. Do NOT add Motion unless the user explicitly requests it:

| Component | Native Animation | Mechanism |
|-----------|-----------------|-----------|
| Accordion | Expand/collapse content | Radix `data-state` + CSS transition on height |
| Collapsible | Open/close content | Radix `data-state` + CSS transition |
| Dialog | Open/close overlay + content | Radix `data-state` + CSS transition/keyframes |
| Sheet | Slide in/out from edge | Radix `data-state` + CSS transform |
| Drawer | Slide up from bottom | vaul library handles gesture + animation |
| Dropdown Menu | Open/close menu | Radix `data-state` + CSS transition |
| Popover | Show/hide popover | Radix `data-state` + CSS transition |
| Tooltip | Show/hide on hover | Radix `data-state` + CSS transition |
| Navigation Menu | Submenu reveal | Radix `data-state` + CSS transition |
| Context Menu | Open/close on right-click | Radix `data-state` + CSS transition |
| Alert Dialog | Open/close modal | Radix `data-state` + CSS transition |
| Hover Card | Show/hide on hover | Radix `data-state` + CSS transition |
| Sonner | Toast enter/exit | sonner library handles animation |

### Where Motion Adds Value Over Native

| Use Case | Why Motion | Example |
|----------|-----------|---------|
| Page transitions | No native support in shadCN | `AnimatePresence` + `initial`/`animate`/`exit` |
| List reordering | CSS can't animate layout shifts | `layout` prop on list items |
| Shared element transitions | No CSS equivalent | `layoutId` between thumbnail and modal |
| Scroll-linked parallax | CSS `scroll-timeline` has limited support | `useScroll` + `useTransform` |
| Staggered reveals | CSS requires hardcoded delays | `variants` with `staggerChildren` |
| Drag interactions | Beyond shadCN's scope | `drag` prop with constraints |
| Spring physics | CSS `linear()` is verbose and limited | `type: "spring"` with stiffness/damping |
| Exit animations | React removes DOM nodes immediately | `AnimatePresence` preserves until exit completes |

### Using CSS Variables with Motion

Motion can animate CSS variables from the design system:

```tsx
<motion.div
  animate={{ backgroundColor: "var(--accent)" }}
  whileHover={{ backgroundColor: "var(--accent-foreground)" }}
/>
```

Use `var()` references to stay theme-aware. Never hardcode color values in Motion animations.

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Adds ~30KB to bundle (tree-shakable) | Bundle size increase | Only import what you use; prefer CSS for simple cases |
| `AnimatePresence` required for exit animations | Extra wrapper component | Wrap conditionally rendered elements |
| `layout` animations can conflict with CSS transforms | Unexpected positioning | Don't mix `layout` with manual `transform` styles |
| SVG filter elements don't receive gesture events | Can't use `whileHover` etc. on filters | Add gesture handlers to parent element |
| `height: "auto"` animation requires `visibility` not `display: none` | Measurement fails with `display: none` | Use `visibility: "hidden"` instead |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| Motion Docs | `https://motion.dev/docs` | Full API reference, installation, guides |
| Motion React Quick Start | `https://motion.dev/docs/react-quick-start` | Installation, first animation |
| Motion Animation | `https://motion.dev/docs/react-animation` | Animate prop, keyframes, transitions |
| Motion Gestures | `https://motion.dev/docs/react-gestures` | Hover, tap, drag, pan, focus, inView |
| Motion Layout | `https://motion.dev/docs/react-layout-animations` | Layout prop, layoutId, shared transitions |
| Motion Scroll | `https://motion.dev/docs/react-scroll-animations` | useScroll, whileInView, parallax |
| Motion Transitions | `https://motion.dev/docs/react-transitions` | Spring, tween, inertia configuration |
| UI Guidelines → Motion | `guidelines/design/ui-guidelines.md` | Animation heuristics and selection rules |
