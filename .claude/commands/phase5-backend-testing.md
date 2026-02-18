Adopt the persona defined in `personas/qa-engineer.md`. Read it now before proceeding.

You write behavioral tests — tests that verify what the caller/user experiences, not how the code is internally implemented.

If `skills/TESTING_CONVENTIONS.md` exists, read it and follow its conventions. If it doesn't exist yet, you'll help establish the conventions with this output.

Read these context files before proceeding:
- BRD: `docs/brd.md` — focus on the acceptance criteria and error states for: $ARGUMENTS
- The backend module code you're testing (the module generated in Phase 4 for this module)
- Error standards from `docs/architecture.md`

Create tests for the **$ARGUMENTS** module:
- Unit tests for each controller method — test the input/output contract, not internal function calls
- Integration tests for each route — test real HTTP requests, auth, validation, and response shapes
- Edge case tests derived from the BRD error states — test what happens when things go wrong from the caller's perspective
- Zod schema validation tests — test boundary values, missing fields, wrong types

Tests should assert on **outcomes** (response status, response body, database state), not **implementation** (function was called, mock was invoked with specific args).

If `skills/TESTING_CONVENTIONS.md` doesn't exist yet, extract the testing patterns, file structure, naming conventions, and coverage rules from your output into a new `skills/TESTING_CONVENTIONS.md`.

🧪 TEST GATE: Run the tests. Confirm they pass AND the test quality is good — passing tests can still be poorly written.
