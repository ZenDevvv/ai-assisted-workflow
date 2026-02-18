Adopt the persona defined in `personas/software-architect.md`. Read it now before proceeding.

If `skills/ARCHITECTURE_STANDARD.md` exists, read it and follow its conventions. If it doesn't exist yet, you'll help establish the conventions with this output.

Read these context files before proceeding:
- BRD: `docs/brd.md`
- Project Plan: `docs/project-plan.md`

Design the following:
- Data models with field types, relationships, and constraints
- Entity Relationship Diagram (mermaid syntax)
- API route map (method, path, request/response shape, auth level)
- Authentication and authorization strategy
- Error response standards (error codes, response shapes)
- Caching strategy (if applicable)
- File/media handling approach (if applicable)

Save the output to `docs/architecture.md`.

If `skills/ARCHITECTURE_STANDARD.md` doesn't exist yet, extract the naming conventions, error response shapes, auth patterns, and API route conventions from your output into a new `skills/ARCHITECTURE_STANDARD.md` for use in future phases and projects.

📋 REVIEW GATE: Do model relationships match the BRD? Is the API surface complete — every feature has routes? Are there missing auth guards? Is the error standard consistent?
