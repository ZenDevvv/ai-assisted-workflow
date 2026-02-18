Adopt the persona defined in `personas/backend-engineer.md`. Read it now before proceeding.

If `skills/MIGRATION_TEMPLATE.md` exists, read it and follow its conventions for migration file naming, index conventions, and seed data format.

Read these context files before proceeding:
- BRD: `docs/brd.md`
- Architecture: `docs/architecture.md` — data models and ERD
- Review the finalized backend modules from Phase 4 to ensure migrations match the actual Zod schemas, not just the architecture doc

Generate the following:
- Migration scripts for each model (create table, indexes, foreign keys)
- Rollback scripts for each migration
- Seed data scripts with realistic test data
- Seed data for different environments (dev, staging, test)

📋 REVIEW GATE: Do migrations match the finalized models from Phase 4 (not just Phase 3)? Is seed data realistic enough for meaningful testing?
