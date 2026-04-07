# Skill Workflows — Design System Skill

> **TL;DR:** Agent-executable workflows that users of the skill can trigger by request. Each workflow provides step-by-step instructions for the agent to follow in the target project. Current workflows: Generate a New Theme (theme discovery, base theme selection, boilerplate with all tokens, guided or bulk population, New York defaults for unspecified values, dual-format oklch+hex, Storybook alignment), Generate a Storybook (theme discovery, dynamic toolbar population).

## Purpose

Predefined workflows that an agent can execute when a user requests a specific task. These are skill content — they ship with the build outputs and are available to any project using this skill.

**Use for:** Triggering structured, repeatable tasks in the target project (e.g., "generate a storybook", "create a new theme").

**Do not use for:** Project-level operational workflows (those are in the root `workflows.md`).

---

## Workflow: Generate a New Theme

**Trigger:** User says "generate a new theme", "create a theme", or similar.

**Prerequisites:**
- Target project has shadCN installed and configured (see `tech/shadcn.md`)
- CSS file with `@import "tailwindcss"` and `@theme inline` block exists

### Theme discovery

Before generating a theme, the agent discovers all available themes:

| Source | Location | Priority |
|--------|----------|----------|
| Skill built-in themes | Skill's `themes/` directory | Base |
| Project themes | Project's `themes/` folder | Override |

Discovery order:
1. Read built-in themes from the skill (Default)
2. Scan the project for a `themes/` directory
3. Merge — project themes take precedence if names collide with built-in themes

### Steps

1. Ask the user for the theme name (e.g., "finops", "platform")

2. Ask the user which existing theme to use as a base:
   - Default (New York) — always available regardless of skill variant
   - Any other built-in theme available in the current skill variant
   - Any project theme already generated
   - If the user doesn't specify, use Default (New York)

3. Generate a boilerplate theme file matching the structure of the selected base theme:
   - Standard variable groups: Core, Chart, Sidebar, Typography
   - If the base theme has custom variable groups (Status, Focus, Secondary Accent, Shadow, Gradient, Agentic Gradient), include those as well
   - All variable names identical to the base theme — variable names and tokens MUST NOT be changed
   - All cells empty (placeholder for user input) except structure and headers
   - Include the Tailwind Mapping, Global Styles, and Usage Notes sections as boilerplate

3. Ask the user how they want to populate the theme:
   - **Guided walkthrough:** Walk through each token one by one, explaining what it controls and which components use it. This is collaborative — the user provides values as you go.
   - **Bulk import:** User provides values in bulk (e.g., from Figma export via the Figma power, a CSS file, or a design token JSON)
   - **Manual:** User fills in the boilerplate themselves and comes back for validation

4. If guided walkthrough:
   - For each variable group present in the boilerplate:
     - Explain the group's purpose
     - For each variable, explain: what it controls, which components reference it, the background/foreground pairing convention
     - Accept the user's value in any format (hex, rgb, oklch, color name)
     - Convert the provided value to both oklch (primary) and hex (fallback) for the theme file
     - If the user doesn't specify a value, use the shadCN New York default and mark it as `<!-- INCOMPLETE: using New York default -->` in the theme file

5. If bulk import:
   - Accept the user's input (CSS variables, Figma export, JSON tokens)
   - Map provided values to the skill's variable names — do NOT rename or restructure variables
   - If a provided token doesn't match an existing variable name, work with the user to determine the best fit based on how the variable is applied in shadCN and Tailwind components
   - Convert all color values to dual-format (oklch + hex)
   - Any variables not covered by the import get New York defaults marked as `<!-- INCOMPLETE: using New York default -->`

6. Color format handling:
   - The user will likely provide single color values (hex, rgb, or color names)
   - Convert all values to the dual-format tracked in theme files: oklch (primary) + hex (fallback)
   - For light/dark pairs: if only one mode is provided, derive the other using appropriate contrast inversion

7. Validation (REQUIRED before finishing):
   - Verify all variable names match the base theme's structure exactly
   - Verify all color values are valid oklch and hex
   - Verify background/foreground pairs maintain sufficient contrast
   - Verify the file structure conforms to the theme file template (same sections, same table formats as the base theme)
   - Flag any variables still using New York defaults as incomplete

8. Create the theme CSS block for the project's stylesheet:
   ```css
   .theme-[name] {
     --background: /* light value */;
     --foreground: /* light value */;
     /* ... all light mode variables */
   }

   .theme-[name].dark {
     --background: /* dark value */;
     --foreground: /* dark value */;
     /* ... all dark mode variables */
   }
   ```

9. If Storybook is configured in the project:
   - Add the new theme to the toolbar options in `.storybook/preview.ts`
   - Generate or update the Storybook theme to track the new theme as it's being built
   - The Storybook theme should stay aligned with the theme file through iterations until it reaches its final state

10. Present the completed theme to the user for final review

### Variable Name Rules (HARD)

- Variable names MUST match the base theme's structure exactly — no renaming, no restructuring
- The standard token set is: Core, Chart, Sidebar, Typography. Custom groups (Status, Focus, Secondary Accent, Shadow, Gradient, Agentic Gradient) are included only if the base theme has them
- If the user provides a token that doesn't map to an existing variable, do NOT create a new variable — work with the user to find the best fit among existing variables
- Reference the selected base theme as the canonical variable list

### Incomplete Value Handling

- Any variable where the user hasn't provided a value uses the shadCN New York default
- Mark these with `<!-- INCOMPLETE: using New York default -->` in the theme file
- The theme file is still valid and importable with defaults in place
- The user can return later to fill in incomplete values

### Output

- A fully compliant and importable theme `.md` file, matching the structure of the selected base theme, with all variables and color values accounted for (user-provided or New York defaults marked incomplete)
- Theme file written to the project's `themes/` folder (e.g., `themes/finops.md`) — immediately discoverable by the agent for future prototype and Storybook generation
- CSS theme block added to the project's stylesheet
- If Storybook is generated: an aligned Storybook theme tracking the new theme through iterations until final

---

## Workflow: Generate a Storybook

**Trigger:** User says "generate a storybook", "set up storybook", "create stories", or similar.

**Prerequisites:**
- Target project has shadCN installed and configured (see `tech/shadcn.md`)
- Design system components are added to the project

### Steps

1. Check if Storybook is already installed in the project
   - If not: run `npm create storybook@latest` and select "Recommended" configuration
   - If yes: verify configuration is compatible (React framework, Vite or Webpack)
2. Install required addons (if not present):
   ```bash
   npx storybook@latest add @storybook/addon-a11y
   ```
3. Configure theme and mode switching:
   - Discover all available themes (built-in from skill + project themes from root `*.theme.md` or `themes/` folder)
   - Create/update `.storybook/preview.ts` with global toolbar decorators (see `tech/storybook.md` → preview.ts)
   - Set up `globalTypes` for theme (populated from discovered themes) and mode (light/dark)
   - Add decorator that applies theme class and dark mode to document root
4. Verify the project's CSS file has theme variable blocks for each theme
   - If missing: notify the user and offer to run the "Generate a New Theme" workflow
5. Generate story files for all design system components present in the project:
   - Check which shadCN components are installed (scan `components/ui/` directory)
   - For each installed component, generate a story file using the component spec from `design-system/components/[name].md`:
     - Default story
     - One story per variant
     - Key composition patterns
     - Interactive controls for all props
   - Place stories in `src/stories/components/[name].stories.tsx`
6. Generate story files for patterns (if the composed components are present):
   - Data Table → `src/stories/patterns/data-table.stories.tsx`
   - Date Picker → `src/stories/patterns/date-picker.stories.tsx`
7. Generate story files for templates (if the composed components are present):
   - Calendar, Dashboard, Login, Sidebar → `src/stories/templates/[name].stories.tsx`
   - Use `parameters: { layout: "fullscreen" }` for template stories
8. Optionally customize the Storybook UI theme (see `tech/storybook.md` → Storybook UI Theme)
9. Run Storybook to verify: `npm run storybook`
10. Present a summary of generated stories to the user

### MCP Connections (Optional, Recommended)

- **shadCN MCP:** Enables the agent to query component APIs and verify installed components. See `setup/mcp.md` for configuration.
- **GitHub MCP:** Enables the agent to commit generated story files directly. See `setup/mcp.md` for configuration.

If MCP connections are not available, the agent can still generate all story files using the design system skill content and the project's local file system.

### Output

- Storybook installed and configured with theme/mode switching
- Story files for all installed components, patterns, and templates
- Accessibility addon enabled
- Storybook running at `localhost:6006`
