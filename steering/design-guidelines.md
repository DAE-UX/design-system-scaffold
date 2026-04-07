# Design Guidelines — Design System Skill

> **TL;DR:** UI design heuristics for building interfaces with Design System components. 54 heuristics across 9 domains: cognitive foundations, visual perception, interaction, controls, content structure, typography, media, color science, and accessibility. Use the Task Router to find relevant sections. Use the Verification Checklist after every UI build or review. These guidelines govern UI construction — they do NOT override component specs or theme definitions.

**Dependency:** All product terminology must align with definitions in `guidelines/copywriting/glossary.md`.

**Standards references:**
- sRGB standards (IEC 61966-2-1)
- W3C WAI — Accessibility Principles (https://www.w3.org/WAI/fundamentals/accessibility-principles/)
- W3C WCAG 2.2 (https://www.w3.org/TR/WCAG22/)

## Scope

**Use for:**
- Building or reviewing UI layouts from existing components
- Composing complex patterns from multiple components
- Prototyping interactions and flows
- Generating production UI code using design system components
- Evaluating UI implementations for usability

**Do not use for:**
- Changing component API, design, variants, or styling (see component specs)
- Changing theme variable values or structure (see theme definitions)
- Tactical UI composition, layout production, imagery, copy, and motion rules (use `guidelines/design/ui-guidelines.md`)

**CRITICAL — Heuristic Precedence:**
- Component library specs and theme definitions always supersede these guidelines
- These heuristics MUST NOT justify changing an existing component's design, API, or styling
- These heuristics MUST NOT justify changing theme variable values or structure
- Heuristics apply to the **use** of components, not the **definition** of components
- For bespoke components: see Bespoke Component Violation Workflow below

**Stop conditions:**
- You have identified the relevant heuristics for your task
- You have enough detail to implement or evaluate
- You have run the Verification Checklist and all items pass

---

## Task Router

Determine what to read based on your current task.

| Task | Read | Stop when |
|------|------|-----------|
| Quick heuristic lookup | Quick Reference Table only | You found the heuristic ID and one-line rule |
| Layout or page design | Visual Perception, Content Structure, Typography | You have grouping, hierarchy, and layout rules |
| Form or input design | Cognitive Foundations, Interaction Design, Controls | You have cognitive load, error, and control rules |
| Color or theme work | Color Science | You have color science and sRGB rules |
| Accessibility review | Accessibility Standards | You have WCAG POUR requirements |
| Interaction or flow design | Interaction Design | You have feedback, control, and error rules |
| Full UI build or review | All sections + Verification Checklist | Checklist passes |
| Tactical layout/composition | `guidelines/design/ui-guidelines.md` | Composition and litmus checks pass |
| Prototyping | Cognitive Foundations, Visual Perception, Interaction Design, Content Structure | You have cognitive, visual, interaction, and IA rules |
| Code review for UI | Verification Checklist + relevant sections | Checklist passes |

---

## Quick Reference Table

Use for fast heuristic identification. Read the full section only when you need mechanism or application details.

| ID | Heuristic | One-Line Rule | Section |
|----|-----------|---------------|---------|
| 1.1 | Reduce Cognitive Load | Shift computation and memory from user to system | General Design Principles |
| 1.2 | Recognition Over Recall | Show options; don't require memory | General Design Principles |
| 1.3 | Working Memory Limits | Don't require retention across screens | General Design Principles |
| 1.4 | Mental Model Alignment | Match user expectations and real-world metaphors | General Design Principles |
| 1.5 | Performance Over Preference | Optimize task success before aesthetics | General Design Principles |
| 2.1 | Proximity | Nearby elements are perceived as related | General Design Principles |
| 2.2 | Similarity | Shared styling signals shared category | General Design Principles |
| 2.3 | Continuity | Alignment guides the eye | General Design Principles |
| 2.4 | Closure | Users infer complete shapes from partial info | General Design Principles |
| 2.5 | Figure-Ground | Contrast separates foreground from background | General Design Principles |
| 2.6 | Common Region | Containers group elements | General Design Principles |
| 2.7 | Connectedness | Visual connectors signal relationships | General Design Principles |
| 2.8 | Prägnanz | Favor simple, stable forms | General Design Principles |
| 2.9 | Common Fate | Shared motion signals grouping | General Design Principles |
| 2.10 | Visual Hierarchy | Size, weight, color, position guide attention | General Design Principles |
| 2.11 | Balance | Distribute visual weight across the layout | General Design Principles |
| 3.1 | System Status | Always communicate current state | UX Best Practices |
| 3.2 | User Control | Provide undo, cancel, back, close | UX Best Practices |
| 3.3 | Consistency | Follow platform and internal conventions | UX Best Practices |
| 3.4 | Error Prevention | Prevent before handling | UX Best Practices |
| 3.5 | Error Recovery | Plain language, specific, actionable | UX Best Practices |
| 3.6 | Flexibility | Support novice and expert paths | UX Best Practices |
| 3.7 | Help & Documentation | Searchable, task-focused, concise | UX Best Practices |
| 3.8 | Goal-Directed Design | Prioritize user goals over features | UX Best Practices |
| 3.9 | Flow Preservation | Minimize interruptions | UX Best Practices |
| 3.10 | Reduce Excise | Remove unnecessary steps | UX Best Practices |
| 3.11 | Microinteraction Design | Define trigger, rules, feedback, loops for recurring interactions | UX Best Practices |
| 3.12 | Clear Affordances | Make interactive elements and their triggers obvious | UX Best Practices |
| 3.13 | Trust and Motivation | Treat emotional tone, progress, and perceived control as design material | UX Best Practices |
| 4.1 | Labeling | Clear, descriptive, consistent labels | Interaction Patterns |
| 4.2 | Input Optimization | Minimize required input | Interaction Patterns |
| 4.3 | Control Selection | Match control type to data relationship | Interaction Patterns |
| 4.4 | Field Design | Labels near inputs, group related fields | Interaction Patterns |
| 4.5 | Familiar Patterns | Use standard controls | Interaction Patterns |
| 5.1 | Content Primacy | Content matters most | Interaction Patterns |
| 5.2 | Information Hierarchy | Important content first, top, prominent | Interaction Patterns |
| 5.3 | Scannability | Design for scanning, not reading | Interaction Patterns |
| 5.4 | List Design | Important items first, grouped, introduced | Interaction Patterns |
| 5.5 | Homepage Design | Access, purpose, first impression | Interaction Patterns |
| 5.6 | Navigation Systems | Multiple paths, clear orientation | Interaction Patterns |
| 5.7 | Layout Adaptability | Fluid, responsive, appropriate density | Interaction Patterns |
| 5.8 | Scrolling & Paging | Scroll for reading, page for tasks | Interaction Patterns |
| 5.9 | Links & Actions | Meaningful labels, predictable destinations | Interaction Patterns |
| 5.10 | Information Scent | Clear labels, categories, and local cues that help users predict where to go | Interaction Patterns |
| 5.11 | Landmarks and Orientation | Stable headings, place markers, and route cues for mental mapping | Interaction Patterns |
| 6.1 | Readability | High contrast, legible fonts, ≥12pt | Layout Guidance |
| 6.2 | Emphasis & Consistency | Sparse emphasis, uniform styling | Layout Guidance |
| 6.3 | Minimalist Design | Only relevant information on screen | Layout Guidance |
| 7.1 | Purpose-Driven Media | Visuals must support tasks | Layout Guidance |
| 7.2 | Media Performance | Optimize speed, minimize distraction | Layout Guidance |
| 8.1 | Color Relativity | Adjacent colors shift perception | Layout Guidance |
| 8.2 | Simultaneous Contrast | Adjacent colors push toward complements | Layout Guidance |
| 8.3 | Color Deception | Validate color in context, not isolation | Layout Guidance |
| 8.4 | sRGB Compliance | Stay within sRGB gamut | Layout Guidance |
| 8.5 | Gamma Awareness | Brightness is non-linear (~2.2) | Layout Guidance |
| 8.6 | Color Accessibility | Never rely on color alone | Layout Guidance |
| 9.1 | Perceivable | Text alternatives, captions, adaptable, contrast | Accessibility Standards |
| 9.2 | Operable | Keyboard, time, no seizures, navigation, input | Accessibility Standards |
| 9.3 | Understandable | Readable, predictable, input assistance | Accessibility Standards |
| 9.4 | Robust | Valid markup, name/role/value, AT compatible | Accessibility Standards |

---

## General Design Principles

> **TL;DR:** Users have limited working memory (~3–4 items), scan rather than read, and apply existing mental models. The visual system groups elements by proximity, similarity, continuity, closure, common region, connectedness, and common fate. Design to reduce cognitive load, favor recognition over recall, and establish clear visual hierarchy.

### Cognitive Foundations (1.1–1.5)

| ID | Heuristic | Rule | Apply When |
|----|-----------|------|------------|
| 1.1 | Reduce Cognitive Load | Shift computation, memory, repetition to system | Forms, multi-step flows, data-heavy UIs, cross-screen comparisons |
| 1.2 | Recognition Over Recall | Show options visibly; don't require memory retrieval | Menus, filters, search (autocomplete), history lists |
| 1.3 | Working Memory Limits | Don't require retention across screens | Product comparison, multi-page forms, config flows |
| 1.4 | Mental Model Alignment | Match user expectations, real-world metaphors, natural mapping | All UI: terminology, layout, icons, navigation |
| 1.5 | Performance Over Preference | Optimize task completion, speed, error reduction before aesthetics | Prioritizing design effort, evaluating tradeoffs |

**Mechanism details (read only if needed):**
- 1.1: Working memory holds ~3–4 items; retention decays in seconds. Forcing calculation, cross-screen memory, or repetition increases errors and task time.
- 1.2: Recognition uses pattern matching (low effort). Recall requires memory search and context reconstruction.
- 1.3: Cross-screen memory tasks force mental reconstruction → friction, errors, abandonment.
- 1.4: Users apply existing mental models. Misalignment causes confusion, errors, distrust.
- 1.5: Visual design has minimal performance impact vs content clarity, navigation, task flow. First impressions form in ~50ms.

### Visual Perception — Gestalt Principles (2.1–2.11)

| ID | Principle | Rule | Apply When |
|----|-----------|------|------------|
| 2.1 | Proximity | Nearby elements perceived as related | Form groups, nav clusters, tables, cards |
| 2.2 | Similarity | Shared styling signals shared category | Nav groups, interactive vs static, status |
| 2.3 | Continuity | Alignment guides the eye | Layout alignment, nav flow, timelines |
| 2.4 | Closure | Brain fills gaps to perceive whole forms | Icons, logos, progress indicators |
| 2.5 | Figure-Ground | Contrast separates foreground from background | CTAs, modals, alerts |
| 2.6 | Common Region | Containers group elements | Cards, form sections, dashboard widgets |
| 2.7 | Connectedness | Visual connectors signal relationships | Flowcharts, linked nav, data relationships |
| 2.8 | Prägnanz | Favor simple, stable, regular forms | All layout decisions |
| 2.9 | Common Fate | Shared motion signals grouping | Animation groups, drag-drop, carousels |
| 2.10 | Visual Hierarchy | Size, weight, color, position guide attention | All layouts, typography, component prominence |
| 2.11 | Balance | Distribute visual weight across axis | Page composition, dashboards, landing pages |

**Mechanism details (read only if needed):**
- 2.1: Visual system clusters nearby elements to reduce processing; treats them as single unit.
- 2.5: Primary content reads as "figure"; supporting context as "ground."
- 2.10: 2–3 type sizes establish clear hierarchy. Scale, contrast, and placement work together.
- 2.11: Symmetrical → stability. Asymmetrical → energy. Radial → center focus. Unbalanced → instability, reduced trust.

---

## UX Best Practices

> **TL;DR:** Always show system status. Provide undo/cancel/back/close. Follow platform conventions. Prevent errors before handling them. Support novice and expert paths. Prioritize user goals. Minimize interruptions and unnecessary steps.

| ID | Heuristic | Rule | Apply When |
|----|-----------|------|------------|
| 3.1 | System Status | Communicate current state; <1s subtle, 1–10s spinner, >10s progress, >60s notify. Warn before timeouts. No unsolicited pop-ups. | Loading, submissions, nav changes, background ops, state changes |
| 3.2 | User Control | Provide back, cancel, close, undo/redo | Multi-step flows, destructive actions, modals, forms |
| 3.3 | Consistency | Follow platform and internal conventions | Nav, terminology, interaction, visual styling |
| 3.4 | Error Prevention | Constraints, suggestions, defaults, forgiving formatting, input-time validation | Forms, destructive actions, financial, irreversible |
| 3.5 | Error Recovery | Plain language, specific problem, actionable fix, visual emphasis, no blame | All error states |
| 3.6 | Flexibility | Shortcuts for experts, hidden from novices; customizable frequent actions | All interfaces |
| 3.7 | Help & Docs | Searchable, task-focused, concise; contextual tips over tutorials | Complex workflows, first-time use, uncommon features |
| 3.8 | Goal-Directed | Prioritize user goals over system capabilities | Feature prioritization, nav, workflows, dashboards |
| 3.9 | Flow | Minimize interruptions; each has recovery cost | Task flows, forms, editing, reading, data entry |
| 3.10 | Reduce Excise | Remove unnecessary clicks, redundant inputs, nav overhead | Any repeated interaction, multi-step processes |
| 3.11 | Microinteraction Design | Define trigger, rules, feedback, and loops/modes for every recurring interaction. Include edge cases and what happens over time. | Notifications, refresh, autosave, likes, follows, reactions, device controls, settings, recurring status updates |
| 3.12 | Clear Affordances | Make it obvious what starts an interaction, what is interactive, and how the interaction can be used. Interactive elements must be visually distinct from static content. | Buttons, toggles, gestures, hover states, inline editing, navigation controls, embedded controls in complex layouts |
| 3.13 | Trust and Motivation | Treat look and feel, emotional tone, motivation, progress, and perceived control as part of the interface. Users decide and act with more than logic alone. | Onboarding, conversion flows, confirmations, progress tracking, empty states, rewards, signup, checkout, trust-sensitive surfaces |

**Mechanism details (read only if needed):**
- 3.1: Feedback closes the "gulf of evaluation": user acts → system responds → user interprets. Without feedback, users assume failure, repeat actions, lose trust.
- 3.2: Easy exits foster confidence, encourage exploration, facilitate learning.
- 3.3: Jakob's Law: users spend most time on other products; expectations set by those experiences.
- 3.4: Two error types: slips (unconscious, autopilot) and mistakes (wrong mental model). Prevention strategies: helpful constraints, contextual suggestions, good defaults, forgiving formatting, input-time validation.
- 3.8: Users care about outcomes, not features. Feature-driven design creates complexity.
- 3.9: Flow enables efficiency, satisfaction, reduced cognitive switching.
- 3.11: A microinteraction has four parts: trigger (what starts it), rules (what happens), feedback (what the user sees/hears/feels), loops and modes (what happens over time or on repeat). Designing all four prevents edge-case failures.
- 3.12: If a user cannot tell what is interactive without hovering or tapping, the affordance is too weak. Standard CSS `:hover` is unreliable on touch devices — use visual distinction at rest.
- 3.13: First impressions form in ~50ms. Progress indicators, completion rewards, and perceived control increase engagement and reduce abandonment. Trust cues (consistency, transparency, professional polish) reduce friction on sensitive surfaces.

---

## Accessibility Standards

> **TL;DR:** All UI must be Perceivable, Operable, Understandable, and Robust (WCAG POUR). Text alternatives for non-text. Keyboard accessible. No traps. Sufficient time. No seizures. Readable. Predictable. Valid markup.

**Reference:** W3C WCAG 2.2 (https://www.w3.org/TR/WCAG22/) · W3C WAI Accessibility Principles (https://www.w3.org/WAI/fundamentals/accessibility-principles/)

| ID | Principle | Requirements |
|----|-----------|-------------|
| 9.1 | Perceivable | Text alternatives for non-text. Captions for multimedia. Adaptable presentations (proper markup). Sufficient contrast. Text resizable to 400%. |
| 9.2 | Operable | Keyboard accessible, no traps. Sufficient time. No seizure content. Visible focus, meaningful order. Multiple input modalities. Large touch targets. |
| 9.3 | Understandable | Language identified. Definitions for unusual terms. Simplest language. Consistent nav/labels. No unsolicited changes. Descriptive errors. Context help. Review/correct opportunity. |
| 9.4 | Robust | Valid markup. Name/role/value for custom controls. AT compatible. |

### Color Accessibility (8.6)

Never rely on color alone to convey information. ~8% males and ~0.5% females are color blind. Supplement with text, icons, or patterns. Maintain sufficient contrast ratios. Validate with accessibility checkers.

---

## Interaction Patterns

> **TL;DR:** Labels must be clear, descriptive, consistent. Minimize input effort. Match control type to data relationship. Use standard controls. Content is the most critical usability factor. Establish clear information hierarchy. Design for scanning. Provide multiple navigation paths. Use fluid layouts.

### Controls and Inputs (4.1–4.5)

| ID | Heuristic | Rules |
|----|-----------|-------|
| 4.1 | Labeling | User terminology, not system. Consistent conventions. Button labels = action ("Submit Form" not "OK"). Required/optional indicated. Table headings descriptive. Units labeled. No unnecessary case sensitivity. |
| 4.2 | Input Optimization | Defaults, autofill, auto-formatting. Selection controls over free text. Partition long data. Cursor in first field. Auto-tab between segments. Accept flexible formats, reformat behind scenes. |
| 4.3 | Control Selection | Radio → mutually exclusive (min 2). Checkbox → multiple. Dropdown → large sets. Open list → show max options. Text → unbounded input. Primary action visually emphasized. |
| 4.4 | Field Design | Labels close to inputs (proximity). Group related fields. Show entered data. Prevent double-click issues. Single entry method per field. |
| 4.5 | Familiar Patterns | Standard controls leverage existing knowledge. Novel controls increase errors. Platform consistency reduces cognitive load. |

### Content Structure and Information Architecture (5.1–5.9)

| ID | Heuristic | Key Rules |
|----|-----------|-----------|
| 5.1 | Content Primacy | Relevant, task-oriented, audience-specific. Directly usable format. Quantitative content designed for understanding. |
| 5.2 | Information Hierarchy | Important content top/prominent. Center or left-aligned priority. Progressive disclosure. Display only necessary info. |
| 5.3 | Scannability | Descriptive headings (unique, specific). Bullet lists. Short paragraphs. Visual grouping. Sparse highlighting. |
| 5.4 | List Design | Important items top. Group related. Introduce with context. Numbered for sequences, bullets for non-sequential. |
| 5.5 | Homepage | Return from any page. Show major options. Communicate purpose. Limit prose. Above-the-fold priority. |
| 5.6 | Navigation | Multiple paths. Show location and actions. Left/top placement. Breadcrumbs. Active state. Progress. Consistent behavior. Descriptive tabs. Filters/categories/sorting. |
| 5.7 | Layout Adaptability | Fluid layouts. Persistent controls when needed. Appropriate density. Moderate white space. Task-appropriate page length. |
| 5.8 | Scrolling & Paging | No horizontal scroll. Scroll for reading. Page for tasks. Minimize scrolling. No scroll stoppers. |
| 5.9 | Links & Actions | Meaningful labels (never "click here"). Match text to destination. Hover/active/visited states. Clear affordances. Text over image links. No false clickability. |
| 5.10 | Information Scent | Help users predict where to go next with clear labels, meaningful categories, strong local cues, and navigation paths that preserve orientation. Users follow the strongest scent — weak labels cause backtracking. |
| 5.11 | Landmarks and Orientation | Give users stable landmarks, headings, place markers, and route cues so they can build a mental map of where they are and how to return. Critical in multi-step flows, large applications, and nested navigation. |

---

## Layout Guidance

> **TL;DR:** High contrast, legible fonts, ≥12pt. Emphasis sparingly. Only task-relevant information on screen. Visuals must support tasks. Optimize media for speed. Color perception is relative and context-dependent. Stay within sRGB. Account for ~2.2 gamma. Never rely on color alone.

### Typography and Visual Design (6.1–6.3)

| ID | Heuristic | Rules |
|----|-----------|-------|
| 6.1 | Readability | High contrast (black on plain preferred). Familiar fonts. ≥12pt. Mixed case for prose. Optimal line length. |
| 6.2 | Emphasis & Consistency | Bold/color/highlights sparingly. Uniform typography. Color never sole meaning carrier. |
| 6.3 | Minimalist Design | Every extra info unit competes with relevant ones. Include all necessary elements, only necessary elements. Aesthetics establish first impressions (~50ms) and reinforce brand. |

### Media and Visual Assets (7.1–7.2)

| ID | Heuristic | Rules |
|----|-----------|-------|
| 7.1 | Purpose-Driven Media | No decorative-only graphics. No ad-like graphics. Limit large images above fold. Thumbnails for large images. Data alongside data graphics. Real-world metaphors. |
| 7.2 | Media Performance | Optimize file sizes. Careful animation. Simple backgrounds. Label clickable images. Multimedia only when valuable. |

### Color Science (8.1–8.6)

| ID | Heuristic | Rule | Mechanism |
|----|-----------|------|-----------|
| 8.1 | Color Relativity | Adjacent colors shift perceived appearance | Visual system interprets color relationally. Same color appears lighter on dark, darker on light. |
| 8.2 | Simultaneous Contrast | Adjacent colors push toward complements | Visual system exaggerates differences. Neutrals shift toward opposite hue of neighbor. |
| 8.3 | Color Deception | Validate color in actual UI context | Identical colors can appear different; different colors can appear identical. Context, lighting, adjacency all influence. |
| 8.4 | sRGB Compliance | All UI colors within sRGB gamut | Most displays assume sRGB. Out-of-gamut colors clip or shift unpredictably. |
| 8.5 | Gamma Awareness | Brightness is non-linear (~2.2) | Midtones appear darker than linear values. Linear interpolation produces uneven gradients. Apply to gradients, shadows, elevation, opacity. |
| 8.6 | Color Accessibility | Never rely on color alone | ~8% males, ~0.5% females color blind. Supplement with text, icons, patterns. Sufficient contrast ratios. |

---

## Verification Checklist

**REQUIRED:** Run this checklist after constructing or reviewing any UI. Each item maps to a heuristic ID. A failing item means the corresponding section should be consulted for remediation.

<phase name="structure">

### Phase 1: Structure and Layout

1. [ ] Visual hierarchy established (size, weight, color, position) → 2.10
2. [ ] Related elements grouped (proximity, common region) → 2.1, 2.6
3. [ ] Layout balanced (symmetrical or intentionally asymmetrical) → 2.11
4. [ ] Alignment consistent (continuity guides scanning) → 2.3
5. [ ] Simple, stable forms preferred (Prägnanz) → 2.8
6. [ ] Information hierarchy clear (important content top/prominent) → 5.2
7. [ ] Designed for scanning (headings, lists, short paragraphs) → 5.3
8. [ ] Display density appropriate → 5.7
9. [ ] Fluid/responsive layout → 5.7
10. [ ] No horizontal scrolling → 5.8
11. [ ] Navigation labels provide strong information scent → 5.10
12. [ ] Stable landmarks and orientation cues present in multi-step/large UIs → 5.11

</phase>

<phase name="interaction">

### Phase 2: Interaction and Feedback

1. [ ] System status visible (loading, success, error states) → 3.1
2. [ ] Feedback timing appropriate (<1s subtle, 1–10s spinner, >10s progress) → 3.1
3. [ ] Undo, cancel, back, close available → 3.2
4. [ ] Consistent with platform conventions → 3.3
5. [ ] Errors prevented (constraints, defaults, validation) → 3.4
6. [ ] Error messages: plain language, specific, actionable, no blame → 3.5
7. [ ] Novice and expert paths supported → 3.6
8. [ ] User goals prioritized over features → 3.8
9. [ ] Flow preserved (minimal interruptions) → 3.9
10. [ ] No unnecessary steps (excise removed) → 3.10
11. [ ] Interactive elements visually distinct from static content → 3.12
12. [ ] Recurring interactions have defined trigger, rules, feedback, loops → 3.11
13. [ ] Trust cues present on sensitive surfaces (progress, transparency, polish) → 3.13

</phase>

<phase name="components">

### Phase 3: Components and Inputs

1. [ ] Labels clear, descriptive, consistent → 4.1
2. [ ] Input minimized (defaults, autofill, selection controls) → 4.2
3. [ ] Control type matches data relationship → 4.3
4. [ ] Labels close to inputs, related fields grouped → 4.4
5. [ ] Standard controls used (no unnecessary invention) → 4.5
6. [ ] Required vs optional fields indicated → 4.1
7. [ ] Links meaningful (no "click here"), states visible → 5.9

</phase>

<phase name="visual">

### Phase 4: Typography, Color, and Media

1. [ ] High contrast, legible fonts, ≥12pt → 6.1
2. [ ] Emphasis used sparingly, styling consistent → 6.2
3. [ ] Only task-relevant information displayed → 6.3
4. [ ] Media supports tasks (no decorative-only graphics) → 7.1
5. [ ] Images optimized, animation minimal → 7.2
6. [ ] Colors validated in context (not isolation) → 8.1, 8.3
7. [ ] Colors within sRGB gamut → 8.4
8. [ ] Gamma accounted for in gradients/shadows → 8.5
9. [ ] Color never sole information carrier → 8.6

</phase>

<phase name="accessibility">

### Phase 5: Accessibility (WCAG POUR)

1. [ ] Text alternatives for non-text content → 9.1
2. [ ] Captions/alternatives for multimedia → 9.1
3. [ ] Content adaptable to different presentations → 9.1
4. [ ] All functionality keyboard-accessible, no traps → 9.2
5. [ ] Sufficient time provided → 9.2
6. [ ] No seizure-inducing content → 9.2
7. [ ] Visible focus, meaningful focus order → 9.2
8. [ ] Text readable, language identified → 9.3
9. [ ] Behavior predictable → 9.3
10. [ ] Valid markup, name/role/value for custom controls → 9.4

</phase>

---

## Anti-Patterns

**NEVER:**
- ❌ Cluttered layouts with no visual hierarchy
- ❌ Hidden or inconsistent navigation
- ❌ Requiring users to remember data across screens
- ❌ Decorative-only visuals that don't support tasks
- ❌ Missing feedback for user actions
- ❌ Hardcoded colors without contextual validation
- ❌ Color as sole information carrier
- ❌ Novel controls without user testing
- ❌ Blame language in error messages
- ❌ Unsolicited pop-ups or windows

---

## Bespoke Component Violation Workflow

When constructing a new component not in the design system library, run the Verification Checklist. If any item fails:

1. **STOP** — Report the heuristic ID and specific violation to the component author
2. **ASK** — "This violates heuristic [ID]: [one-line rule]. Fix the violation, or provide a justification to proceed?"
3. **If user provides justification** — Record in the component page under `## Heuristic Violations`:

```markdown
## Heuristic Violations

| Heuristic ID | Violation | Justification |
|--------------|-----------|---------------|
| [ID] | [What was violated] | [User's justification] |
```

4. **If no justification provided** — Do not proceed with the violating design

**NEVER** apply this workflow to existing design system components or themes. Those specs are authoritative.

---

## References

- sRGB standards (IEC 61966-2-1)
- W3C WAI — Accessibility Principles (https://www.w3.org/WAI/fundamentals/accessibility-principles/)
- W3C WCAG 2.2 (https://www.w3.org/TR/WCAG22/)
