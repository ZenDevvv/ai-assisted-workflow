Adopt the persona defined in `personas/backend-engineer.md`. Read it now before proceeding.

Read the skill doc at `skills/MODULE_TEMPLATE.md` — follow it exactly for file structure, naming, Zod patterns, and controller patterns. Read it now before proceeding.

Read these context files before proceeding:
- BRD: `docs/brd.md`
- Architecture: `docs/architecture.md` — focus on models, routes, and error standards

## Determine scope

If `$ARGUMENTS` is **"all"** (case-insensitive), generate **every** module listed in the architecture doc's data models / route map. Process them in dependency order — models with no FK dependencies first, then models that depend on them, and so on. Otherwise, generate only the module for **$ARGUMENTS**.

For **each** module in scope, perform ALL of the following steps:

## Per-module generation

Generate the backend module:
- Zod validation schemas (create, update, response, query params) following MODULE_TEMPLATE.md Step 2
- Route definitions with OpenAPI docs following MODULE_TEMPLATE.md Step 5
- Controller logic (CRUD + custom operations) following MODULE_TEMPLATE.md Step 6
- Middleware (auth guards, validation)
- Error handling following the project error standards from architecture doc
- Config constants following MODULE_TEMPLATE.md Step 3
- Module entry (index.ts) following MODULE_TEMPLATE.md Step 4
- Register the module following MODULE_TEMPLATE.md Step 7

Run through the checklist at the bottom of MODULE_TEMPLATE.md before considering each module complete.

⚠️ VERIFICATION GATE (per module): These Zod schemas become the frontend's source of truth. Verify:
- Does the Zod schema match the model exactly?
- Are all routes from the route map implemented?
- Are auth guards applied correctly?

💡 After completing the FIRST module, run `/phase13-review` to catch pattern-level issues before generating more modules.
