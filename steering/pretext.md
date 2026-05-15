# Pretext (Text Measurement Library) — Design System Skill

> **TL;DR:** Pretext (`@chenglou/pretext`) is a pure JS/TS library for multiline text measurement and layout without DOM reflow. Use it when the UI needs text dimensions before rendering — AI chat virtualization, streaming message height, mixed inline content, canvas/SVG rendering, shrink-wrap layouts. Priority chain: CSS text layout (default) → Pretext (when pre-render height knowledge is needed). Does not replace shadCN component styling — it measures and positions text while components handle visual presentation.

## Purpose

Text measurement library reference and usage guidance for agents generating UI with the design system.

**Use for:** Measuring text height without DOM, virtualizing variable-height text lists, laying out mixed inline content (text + chips/badges), rendering text to canvas/SVG, preventing layout shift on dynamic content.

**Do not use for:** Standard page text (use CSS + Tailwind), text inside shadCN components (use component defaults), server-rendered static content (no measurement needed), simple single-line measurement (use CSS `fit-content`).

---

## Priority Chain

```text
Is this a standard page with static or server-rendered text?
├─ YES → Use CSS text layout (standard approach)
│        Tailwind utilities, shadCN component defaults, normal CSS
│
└─ NO → Does the UI need to know text dimensions before rendering?
    ├─ NO → Use CSS text layout
    │
    └─ YES → Is the use case one of these?
        ├─ Virtualized list with variable-height text items → Use Pretext
        ├─ AI chat with streaming messages and scroll anchoring → Use Pretext
        ├─ Mixed inline content (text + chips/badges/artifacts) → Use Pretext inline-flow
        ├─ Canvas/SVG/WebGL text rendering → Use Pretext
        ├─ Layout-shift prevention on dynamic content load → Use Pretext
        ├─ Shrink-wrap or balanced text layout → Use Pretext
        ├─ Text flowing around floated elements → Use Pretext
        │
        └─ None of the above → Use CSS text layout
```

When building chat views, text-heavy artifact pages, or any UI that satisfies the priority chain, consider Pretext as an option to improve text flow and layout precision.

---

## Installation

```bash
npm install @chenglou/pretext
```

Import in TypeScript/JavaScript:

```typescript
import { prepare, layout } from '@chenglou/pretext'
```

---

## Core API

### Measure Height Without DOM (Use Case 1)

```typescript
import { prepare, layout } from '@chenglou/pretext'

const prepared = prepare(messageText, '16px Inter')
const { height, lineCount } = layout(prepared, containerWidth, 24)
```

- `prepare()` — one-time cost: normalize, segment, measure. Cache the result. ~19ms for 500 texts.
- `layout()` — hot path: pure arithmetic. Call on resize, container width change. ~0.09ms for 500 texts.
- For textarea-like text: pass `{ whiteSpace: 'pre-wrap' }` to `prepare()`

### Manual Line Layout (Use Case 2)

```typescript
import { prepareWithSegments, layoutWithLines } from '@chenglou/pretext'

const prepared = prepareWithSegments(text, '18px Inter')
const { lines } = layoutWithLines(prepared, 320, 26)
// lines[i].text, lines[i].width, lines[i].start, lines[i].end
```

Additional APIs:

| API | Purpose |
|-----|---------|
| `walkLineRanges()` | Line widths and cursors without building strings. For shrink-wrap and balanced text layout. |
| `layoutNextLine()` | One line at a time with variable width. For text flowing around floated elements. |
| `measureNaturalWidth()` | Intrinsic width when wrapping isn't the constraint. |

### Mixed Inline Content (Use Case 3 — Experimental)

```typescript
import { prepareInlineFlow, walkInlineFlowLines } from '@chenglou/pretext/inline-flow'

const prepared = prepareInlineFlow([
  { text: 'Deployed by ', font: '500 16px Inter' },
  { text: '@maya', font: '700 12px Inter', break: 'never', extraWidth: 22 },
  { text: ' to production', font: '500 16px Inter' },
])

walkInlineFlowLines(prepared, containerWidth, line => {
  // line.fragments[i].itemIndex, .text, .gapBefore, .occupiedWidth
})
```

The inline-flow sidecar is intentionally narrow: raw text in, caller-owned `extraWidth` for padding/border, `break: 'never'` for atomic items. No nested markup, no `pre-wrap`, no general CSS inline formatting.

Additional inline-flow APIs:

| API | Purpose |
|-----|---------|
| `layoutNextInlineFlowLine()` | Stream one line at a time through an inline item sequence. |
| `measureInlineFlow()` | Line counter for inline fragment streams. |

### Utility APIs

| API | Purpose |
|-----|---------|
| `clearCache()` | Clear internal caches. Use when cycling through many fonts or text variants. |
| `setLocale()` | Set locale for future `prepare()` calls. Also clears cache. |

---

## API Type Reference

```typescript
// Use Case 1
prepare(text: string, font: string, options?: { whiteSpace?: 'normal' | 'pre-wrap' }): PreparedText
layout(prepared: PreparedText, maxWidth: number, lineHeight: number): { height: number, lineCount: number }

// Use Case 2
prepareWithSegments(text: string, font: string, options?: { whiteSpace?: 'normal' | 'pre-wrap' }): PreparedTextWithSegments
layoutWithLines(prepared: PreparedTextWithSegments, maxWidth: number, lineHeight: number): { height: number, lineCount: number, lines: LayoutLine[] }
walkLineRanges(prepared: PreparedTextWithSegments, maxWidth: number, onLine: (line: LayoutLineRange) => void): number
measureNaturalWidth(prepared: PreparedTextWithSegments): number
layoutNextLine(prepared: PreparedTextWithSegments, start: LayoutCursor, maxWidth: number): LayoutLine | null

type LayoutLine = { text: string, width: number, start: LayoutCursor, end: LayoutCursor }
type LayoutLineRange = { width: number, start: LayoutCursor, end: LayoutCursor }
type LayoutCursor = { segmentIndex: number, graphemeIndex: number }

// Use Case 3 (Experimental)
prepareInlineFlow(items: InlineFlowItem[]): PreparedInlineFlow
layoutNextInlineFlowLine(prepared: PreparedInlineFlow, maxWidth: number, start?: InlineFlowCursor): InlineFlowLine | null
walkInlineFlowLines(prepared: PreparedInlineFlow, maxWidth: number, onLine: (line: InlineFlowLine) => void): number
measureInlineFlow(prepared: PreparedInlineFlow, maxWidth: number, lineHeight: number): { height: number, lineCount: number }

type InlineFlowItem = { text: string, font: string, break?: 'normal' | 'never', extraWidth?: number }
type InlineFlowCursor = { itemIndex: number, segmentIndex: number, graphemeIndex: number }
type InlineFlowFragment = { itemIndex: number, text: string, gapBefore: number, occupiedWidth: number, start: LayoutCursor, end: LayoutCursor }
type InlineFlowLine = { fragments: InlineFlowFragment[], width: number, end: InlineFlowCursor }

// Utilities
clearCache(): void
setLocale(locale?: string): void
```

---

## Integration with Design System Components

Pretext does not replace shadCN component styling. It operates at a different layer — it measures and positions text, while components handle visual presentation.

| Integration point | How it works |
|-------------------|-------------|
| Chat message components | Use Pretext to measure message height for virtualization, then render with shadCN components (Card, Avatar, Badge) |
| Dynamic dashboards | Use Pretext to pre-calculate text block heights for layout algorithms, then render with shadCN layout components |
| Inline mixed content | Use Pretext inline-flow to lay out text + atomic items, then render each item with shadCN components |

Pretext uses `currentColor` and CSS variable references naturally since it doesn't own rendering — the rendering layer applies theme variables as usual.

### Font Sync Requirement

Pretext's `prepare()` takes a font string in canvas shorthand format. This must match the CSS `font` declaration for the text being measured.

| CSS declaration | Pretext font string |
|----------------|---------------------|
| `font-family: Inter; font-size: 16px` | `'16px Inter'` |
| `font-family: Inter; font-size: 16px; font-weight: 700` | `'700 16px Inter'` |
| `font-family: "Geist Mono"; font-size: 14px` | `'14px "Geist Mono"'` |

The design system's font stack (defined in `technical-guidelines.md`) must be used consistently in both CSS and Pretext calls.

**Caveat:** `system-ui` is unsafe for `layout()` accuracy on macOS. Always use a named font.

---

## Known Constraints

| Constraint | Impact | Workaround |
|-----------|--------|------------|
| Targets `white-space: normal` and `overflow-wrap: break-word` by default | Won't match CSS layouts using other white-space modes | Pass `{ whiteSpace: 'pre-wrap' }` for textarea-like text |
| `system-ui` is unsafe on macOS | Measurements may be inaccurate | Always use a named font |
| Inline-flow is experimental alpha | API may change | Use for prototyping; expect to update on new releases |
| Adds ~30KB to bundle (tree-shakable) | Bundle size increase | Only import what you use |
| Version 0.0.4 — pre-1.0 | Breaking changes possible | Pin version, sync on updates |

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| Pretext README | `https://github.com/chenglou/pretext` | API reference, installation |
| Pretext CHANGELOG | `https://github.com/chenglou/pretext/blob/main/CHANGELOG.md` | Version tracking, breaking changes |
| Pretext Demos | `https://chenglou.me/pretext/` | Live examples |
