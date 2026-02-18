Adopt the persona defined in `personas/ui-designer.md`. Read it now before proceeding.

Read these context files before proceeding:
- BRD: `docs/brd.md` — use the **Page Manifest** (from User Stories) as the definitive page list
- Architecture: `docs/architecture.md` — route map for data shape awareness

## Input: $ARGUMENTS

The arguments may include:
- **Screenshot paths** — reference images to extract visual style from (colors, spacing, typography, component patterns, layout structure). Read each image file provided.
- **Design rules** — custom directives like "mobile first", "dark mode default", "minimal sidebar", etc.

If screenshots are provided, analyze them and extract:
- Color palette (primary, secondary, accent, background, surface, text colors)
- Typography (font families, sizes, weights, line heights)
- Spacing system (padding, margins, gaps)
- Border radius, shadows, and elevation patterns
- Component styles (buttons, inputs, cards, tables, navigation)
- Layout patterns (sidebar, top nav, grid system)

Apply these extracted styles consistently across all page designs. If screenshots conflict with each other, prefer the most common pattern.

**IMPORTANT:** Only extract **visual style** from screenshots (colors, typography, spacing, layout patterns, component styling). Do NOT extract or imitate features, functionality, or content from the screenshots. The BRD is the sole source of truth for what the app does — screenshots are only a style reference.

If design rules are provided, treat them as hard constraints that override defaults.

---

## Generate the following:

### 1. Design System Summary
Extracted from screenshots (if provided) or sensible defaults:
- Color palette with hex values
- Typography scale
- Spacing scale
- Component style tokens (border radius, shadows, etc.)

### 2. Page Inventory
Pull directly from the BRD's **Page Manifest** table. Do not invent pages — every page must trace back to a user story.

### 3. Wireframe Descriptions
For each page in the manifest:
- Layout structure (header, sidebar, content area, footer)
- Component placement and hierarchy
- Content zones and data display patterns
- Interactive elements (buttons, forms, dropdowns)

### 4. User Flow Diagrams
Mermaid syntax showing how users navigate between pages. One diagram per major flow (auth, main workflow, admin, etc.).

### 5. Component Inventory
Reusable UI components identified across pages:
- Component name, description, variants
- Which pages use each component

### 6. Responsive Behavior
- Breakpoint strategy (mobile, tablet, desktop)
- How each page layout adapts per breakpoint
- Navigation changes (hamburger menu, collapsible sidebar, etc.)

### 7. State Designs
For each page: loading, empty, error, and populated states.

---

Save the output to `docs/ui-design.md`.

📋 REVIEW GATE: Does every page in the Page Manifest have a wireframe? Do extracted styles match the reference screenshots? Are the user flows complete? Are error/empty states accounted for?
