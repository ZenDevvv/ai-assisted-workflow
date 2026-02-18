Adopt the persona defined in `personas/frontend-engineer.md`. Read it now before proceeding.

Read the style guide at `skills/STYLE_GUIDE.md` — follow it exactly for all styling decisions. Read it now before proceeding.

Read these context files before proceeding:
- BRD: `docs/brd.md` — focus on the relevant module requirements for: $ARGUMENTS
- UI Design: `docs/ui-design.md` — the wireframe/mockup for this specific page
- The frontend API module from Phase 9 — hook signatures, types, mock data factories

Build the page for **$ARGUMENTS**:
- Implement the layout matching the wireframe/design from Phase 7
- Use Tailwind CSS for styling, following the style guide exactly
- Use shadcn/ui components where applicable
- Import and use the generated hooks from Phase 9 for data fetching
- Use mock data for development/preview
- Implement all states: loading, empty, error, populated
- Include form validation using the frontend Zod schemas
- Implement responsive behavior per the style guide
- Follow Page Integration Rules from API_STANDARD.md Step 5 and Step 6:
  - Use Zod types directly — no intermediate row/item types
  - Put display helpers in reusable utility files, not inline

📋 REVIEW GATE: Does the page match the design? Are Tailwind classes and shadcn components consistent with the style guide? Do all states render correctly? Compare against previously generated pages for visual consistency.

💡 After completing the FIRST page, run `/phase13-review` to catch pattern-level issues before generating more pages.
