# UI Guidelines — Design System Skill

> **TL;DR:** Tactical rules for constructing UIs with design system components. Covers composition, landing pages, app surfaces, imagery, copy, motion, and litmus checks. Use when building screens, layouts, prototypes, or templates. These rules guide component usage and arrangement — they do NOT modify component specs, theme definitions, or design heuristics.

**Dependency:** `guidelines/design/design-guidelines.md` (overarching heuristics), `guidelines/copywriting/glossary.md` (terminology), component specs (`design-system/components/*.md`), theme definitions (`themes/*.md`).

---

## Scope

**Use for:**
- Constructing landing pages, app UIs, dashboards, prototypes, and demos
- Composing layouts from existing design system components
- Creating patterns and templates that combine multiple components
- Guiding visual composition, imagery, copy, and motion decisions during UI production

**Do not use for:**
- Editing component specs — those are authoritative
- Editing theme definitions — those are authoritative
- Overriding design heuristics in `guidelines/design/design-guidelines.md` — those are the general principles
- Build or deployment procedures

**Precedence:**
1. Component library specs — highest authority
2. Theme definitions — highest authority
3. Design heuristics (`guidelines/design/design-guidelines.md`) — general principles
4. UI guidelines (this file) — tactical production rules

**Exception:** When working within an existing website or design system, preserve the established patterns, structure, and visual language.

**Stop conditions:**
- You have identified the relevant composition rules for your task
- You have enough detail to build or evaluate the layout
- You have run the Litmus Checks and all items pass

---

## Task Router

| Task | Read | Stop when |
|------|------|-----------|
| Landing page | Composition, Landing Pages, Imagery, Copy, Motion | Layout sequence and hero rules clear |
| App or dashboard | Composition, App Surfaces, Utility Copy | Workspace hierarchy and density rules clear |
| Quick layout check | Composition Hard Rules, Litmus Checks | All checks pass |
| Imagery selection | Imagery | Image criteria and placement rules clear |
| Copy review | Copy, Utility Copy | Copy type identified and rules applied |
| Motion planning | Motion | 2–3 intentional motions identified |
| Full UI build | All sections + Litmus Checks | Litmus checks pass |

---

## Composition

Start with composition, not components. The first viewport is a poster, not a document.

### Defaults

- Prefer a full-bleed hero or full-canvas visual anchor
- Make the brand or product name the loudest text
- Keep copy short enough to scan in seconds
- Use whitespace, alignment, scale, cropping, and contrast before adding chrome
- Limit the system: two typefaces max, one accent color by default
- Default to cardless layouts — use sections, columns, dividers, lists, and media blocks instead

### Hard Rules

| Rule | Detail |
|------|--------|
| One composition | The first viewport must read as one composition, not a dashboard (unless it is a dashboard) |
| Brand first | On branded pages, the brand or product name must be a hero-level signal, not just nav text |
| Brand test | If the first viewport could belong to another brand after removing the nav, branding is too weak |
| One job per section | Each section has one purpose, one headline, and usually one short supporting sentence |
| No cards by default | Cards are allowed only when they are the container for a user interaction (see `components/card.md` → Anti-Patterns for the card test) |
| Card test | See `components/card.md` → Anti-Patterns |
| No hero cards | See `components/card.md` → Anti-Patterns |
| Two typefaces max | No more than two typefaces without a clear reason |
| No split-screen hero | Unless text sits on a calm, unified side |
| No clutter | Avoid pill clusters, stat strips, icon rows, boxed promos, schedule snippets, and multiple competing text blocks |
| No purple bias | Choose a clear visual direction; define CSS variables; avoid purple-on-white defaults. No dark mode bias. |
| No overlapping UI | Keep fixed or floating elements from overlapping text, buttons, or key content across screen sizes |

---

## Landing Pages

Default sequence:

1. Hero — brand or product, promise, CTA, and one dominant visual
2. Support — one concrete feature, offer, or proof point
3. Detail — atmosphere, workflow, product depth, or story
4. Final CTA — convert, start, visit, or contact

### Hero Rules

| Rule | Detail |
|------|--------|
| Full-bleed | Hero image runs edge-to-edge with no inherited page gutters, framed container, or shared max-width; constrain only the inner text/action column |
| Hero budget | First viewport: brand, one headline, one short supporting sentence, one CTA group, one dominant image. No stats, schedules, event listings, address blocks, promos, or secondary marketing content. |
| No hero overlays | No detached labels, floating badges, promo stickers, info chips, or callout boxes on top of hero media |
| Text placement | Keep the text column narrow and anchored to a calm area of the image |
| Contrast | All text over imagery must maintain strong contrast and clear tap targets |
| Headline length | Roughly 2–3 lines on desktop, readable in one glance on mobile |
| Image test | If the first viewport still works after removing the image, the image is too weak |
| Brand test | If the brand disappears after hiding the nav, the hierarchy is too weak |

### Viewport Budget

- If the first screen includes a sticky/fixed header, that header counts against the hero
- Combined header + hero content must fit within the initial viewport at all responsive sizes
- When using `100vh`/`100svh` heroes, subtract persistent UI chrome (`calc(100svh - header-height)`) or overlay the header instead of stacking it in normal flow

---

## App Surfaces

Default to Linear-style restraint: calm surface hierarchy, strong typography and spacing, few colors, dense but readable information, minimal chrome.

### Organize Around

- Primary workspace
- Navigation
- Secondary context or inspector
- One clear accent for action or state

### Avoid

- Dashboard-card mosaics
- Thick borders on every region
- Decorative gradients behind routine product UI
- Multiple competing accent colors
- Ornamental icons that do not improve scanning

If a panel can become plain layout without losing meaning, remove the card treatment.

---

## Imagery

Imagery must do narrative work.

| Rule | Detail |
|------|--------|
| Real visual anchor | Use at least one strong, real-looking image for brands, venues, editorial pages, and lifestyle products |
| In-situ preferred | Prefer in-situ photography over abstract gradients or fake 3D objects |
| Text-safe crops | Choose or crop images with a stable tonal area for text |
| No embedded clutter | Do not use images with embedded signage, logos, or typographic clutter fighting the UI |
| No generated UI frames | Do not generate images with built-in UI frames, splits, cards, or panels |
| Multiple moments | If multiple moments are needed, use multiple images, not one collage |
| First viewport anchor | The first viewport needs a real visual anchor — decorative texture is not enough |

---

## Copy

### Marketing and Brand Copy

- Write in product language, not design commentary
- Let the headline carry the meaning
- Supporting copy: usually one short sentence
- Cut repetition between sections
- Never include prompt language or design commentary in the UI
- Give every section one responsibility: explain, prove, deepen, or convert
- Deletion test: if deleting 30% of the copy improves the page, keep deleting

### Utility Copy (Product UI)

When the work is a dashboard, app surface, admin tool, or operational workspace, default to utility copy over marketing copy.

| Rule | Detail |
|------|--------|
| Orientation over promise | Prioritize status, action, and context over mood or brand voice |
| Start with the surface | KPIs, charts, filters, tables, status, or task context first. No hero section unless explicitly requested. |
| Descriptive headings | Section headings say what the area is or what the user can do there |
| Good examples | "Selected KPIs", "Plan status", "Search metrics", "Top segments", "Last sync" |
| Bad examples | Aspirational hero lines, metaphors, campaign-style language, executive-summary banners |
| One-sentence support | Supporting text explains scope, behavior, freshness, or decision value |
| Homepage test | If a sentence could appear in a homepage hero or ad, rewrite it until it sounds like product UI |
| Operator test | If an operator scans only headings, labels, and numbers, can they understand the page immediately? |

---

## Motion

Use motion to create presence and hierarchy, not noise. Ship at least 2–3 intentional motions for visually led work.

### Recommended Motions

1. One entrance sequence in the hero
2. One scroll-linked, sticky, or depth effect
3. One hover, reveal, or layout transition that sharpens affordance

### Prefer Component Library Motion For

- Section reveals
- Shared layout transitions
- Scroll-linked opacity, translate, or scale shifts
- Sticky storytelling
- Carousels that advance narrative, not just fill space (see `components/carousel.md` → Anti-Patterns)
- Menus, drawers, and modal presence effects

### Motion Rules

- Noticeable in a quick recording
- Smooth on mobile
- Fast and restrained
- Consistent across the page
- Removed if ornamental only

---

## React Patterns

When generating React code for UI production:
- Prefer modern patterns including `useEffectEvent`, `startTransition`, and `useDeferredValue` when appropriate and used by the team
- Do not add `useMemo`/`useCallback` by default unless already used in the codebase
- Follow the repo's React Compiler guidance
- Ensure the page loads properly on both desktop and mobile

---

## Litmus Checks

Run after every UI build or review.

1. [ ] Is the brand or product unmistakable in the first screen?
2. [ ] Is there one strong visual anchor?
3. [ ] Can the page be understood by scanning headlines only?
4. [ ] Does each section have one job?
5. [ ] Are cards actually necessary?
6. [ ] Does motion improve hierarchy or atmosphere?
7. [ ] Would the design still feel premium if all decorative shadows were removed?

### Reject These Failures

- ❌ Generic SaaS card grid as the first impression
- ❌ Beautiful image with weak brand presence
- ❌ Strong headline with no clear action
- ❌ Busy imagery behind text
- ❌ Sections that repeat the same mood statement
- ❌ Carousel with no narrative purpose (see `components/carousel.md` → Anti-Patterns)
- ❌ App UI made of stacked cards instead of layout (see `components/card.md` → Anti-Patterns)
