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

## Output: `docs/ui-design.md`

Save everything below into a single file. Downstream phases (pages, E2E, review) all reference this one file.

### 1. Style Guide
The code-ready style system. Every rule must include **exact values** — no vague descriptions. The project uses Tailwind CSS and shadcn/ui.

Extracted from screenshots (if provided) or derived from design rules and sensible defaults:

- **Color palette** — exact hex values for primary, secondary, accent, background, surface, text colors, mapped to Tailwind classes
- **Typography scale** — font families, exact sizes, weights, line heights — with Tailwind classes
- **Spacing system** — base unit, exact scale values — with Tailwind classes
- **Component styling** — buttons, inputs, cards, modals, tables — with exact Tailwind classes and shadcn variants
- **Border radius and shadows** — with exact Tailwind classes
- **Animation/transition standards** — exact durations and easing
- **Dark mode rules** — if applicable
- **Accessibility** — contrast ratios, focus states, ARIA patterns

Every rule must be specific enough to produce identical results across independently prompted pages. "Use a muted color" is too vague — "use `text-slate-500`" is correct.

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

📋 REVIEW GATE:
1. Does every page in the Page Manifest have a wireframe?
2. Do extracted styles match the reference screenshots?
3. Is every style guide rule specific enough to produce identical results across independently prompted pages?
4. Are the user flows complete? Are error/empty states accounted for?
