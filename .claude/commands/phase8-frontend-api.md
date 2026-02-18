Adopt the persona defined in `personas/frontend-engineer.md`. Read it now before proceeding.

Read the skill doc at `skills/API_STANDARD.md` — follow it exactly for Zod copy rules, hook patterns, service layer structure, and endpoint configuration. Read it now before proceeding.

Read these context files before proceeding:

- BRD: `docs/brd.md`
- The backend Zod schemas from Phase 4 (these are the source of truth)
- Architecture: `docs/architecture.md` — route map and data models

## Determine scope

If `$ARGUMENTS` is **"all"** (case-insensitive), generate **every** frontend API module listed in the architecture doc's data models / route map. Process them in dependency order — modules with no FK dependencies first, then modules that depend on them, and so on. Otherwise, generate only the frontend API module for **$ARGUMENTS**.

For **each** module in scope, perform ALL of the following steps:

## Per-module generation

Using the backend Zod schemas as the single source of truth, generate the frontend API module following API_STANDARD.md:

1. Copy Zod schemas from backend into frontend (Step 4 of API_STANDARD.md — replace mongoose ObjectId with ObjectIdSchema)
2. Generate TypeScript types inferred from the Zod schemas
3. Generate API endpoint configuration (Step 1)
4. Generate service layer functions (Step 2)
5. Generate custom React Query hooks (Step 3)
6. Generate mock data factories using the copied Zod schemas

Run through the Quick Reference Checklist at the bottom of API_STANDARD.md.

📋 REVIEW GATE: Do the copied Zod schemas exactly match the backend? Are mock data factories producing realistic data?

## Log Progress

After completing each module, update `docs/progress.md`:

1. If `docs/progress.md` does not exist, create it with this header:

   ```
   # Project Progress

   | Phase | Name | Scope | Status | Date | Notes |
   |-------|------|-------|--------|------|-------|
   ```

2. Append one row per completed module (fill in today's date and a one-line summary):
   `| 8 | Frontend API | {MODULE_NAME} | ✅ Complete | YYYY-MM-DD | {summary} |`
