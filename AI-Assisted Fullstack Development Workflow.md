# AI-Assisted Fullstack Development Workflow

## Core Principles

1. **BRD as Persistent Anchor** — The Business Requirements Document is the main context used throughout the entire development. Every phase references it.
2. **Phase-by-Phase Prompting** — Each phase is prompted separately to the AI. No end-to-end chaining. Phases that operate on multiple items (modules, pages) are prompted one by one — not batched.
3. **Human Intervention at Every Phase** — You review, correct, and refine each phase's output before it becomes input for the next phase. This breaks error propagation and shapes better inputs downstream.
4. **Decision Log** — When you manually correct or override AI output, note what and why. Over time this improves your prompt templates and serves as a reference when revisiting decisions.
5. **Skills as Living Documents** — Some phases consume skills, some phases produce skills that later phases consume. Skills are accumulated and refined across projects — not created all at once.
6. **Rolling Code Reviews** — Code review is not a single gate at the end. Run reviews periodically after completing a few phases or modules to catch pattern-level issues early before they multiply.
7. **Independent-First Module Ordering** — When iterating modules through build phases, always start with the most independent model (no foreign key dependencies) and work toward dependent models. You choose the order explicitly each time.

---

## Prompt Structure

Every prompt is assembled from four components:

```
PERSONA  — Who the AI is (shapes thinking, priorities, judgment)
SKILL    — How to execute (rules, patterns, templates, conventions)
CONTEXT  — What to work with (BRD + previous phase outputs, human-reviewed)
TASK     — What to produce
```

**Persona** controls the AI's perspective and decision-making lens. A Business Analyst prioritizes requirements coverage. An Architect thinks about scalability and relationships.

**Skill** is a reference document of concrete rules, patterns, and templates the persona must follow when producing output. Your `MODULE_TEMPLATE_MD` and `API_STANDARD` are skills. Skills are reusable across projects and improve over time through Decision Log feedback.

**Context** is always anchored by the BRD. Most phases only need BRD + the immediate prior output. Some later phases require multi-source context — these are flagged below with ⚠️.

**BRD Slicing Convention:** For module-level phases (4, 5, 9, 10, 11), extract only the relevant module's requirements, acceptance criteria, and error states from the BRD rather than including the full document. This keeps context focused and avoids hitting context window limits on larger projects. The full BRD is used for cross-cutting phases (2, 3, 7, 12, 13, 14, 15).

**Task** is the specific deliverable for the phase.

---

## Skill Documents

Skill documents are maintained separately from the project. They encode your team's standards, conventions, and patterns. They survive across projects and get refined over time.

**How to build skills over time:**
1. Start with the skills you already have (MODULE_TEMPLATE_MD, API_STANDARD)
2. After each project, review your Decision Log for recurring corrections
3. If you corrected the same type of issue more than twice, encode the fix into a skill document
4. Version your skill documents — what works for a solo dev may differ from a team setup
5. Not all skills exist on day one — your first run of a phase without a skill is also the opportunity to draft one from the AI's output, review it, and save it for next time

**Skill Lifecycle — Some Phases Produce Skills:**

Most phases consume skills, but some phases *produce* skill documents that later phases depend on. This is an important pattern:

| Producer Phase | Skill Produced | Consumer Phases |
|---|---|---|
| Phase 3 — Architecture | `ARCHITECTURE_STANDARD.md` (if not already cross-project) | Phases 4, 5, 6, 13 |
| Phase 8 — Style Guide | `STYLE_GUIDE.md` (per-project) | Phase 10 |
| First run of Phase 5 | `TESTING_CONVENTIONS.md` (if not already cross-project) | Phases 5, 11, 12 |

Cross-project skills (`MODULE_TEMPLATE`, `API_STANDARD`, `TESTING_CONVENTIONS`, etc.) stabilize after 2–3 projects. Per-project skills (`STYLE_GUIDE`) are generated fresh each time.

**Skill Document Template:**
```markdown
# [Skill Name]
## Purpose
What this skill controls and when to use it.
## Rules
Hard rules the AI must follow (naming, structure, patterns).
## Templates
Code/file templates with placeholders.
## Examples
Good and bad examples showing correct application.
## Anti-patterns
Common mistakes to explicitly avoid.
```

**Skill Reference Table:**

| Phase | Skill Doc | Description | Reusable? |
|---|---|---|---|
| 1 — Business Requirements | `BRD_FORMAT.md` | BRD structure, module IDs, requirement IDs, Given/When/Then criteria, error states | ✅ Cross-project |
| 3 — Architecture | `ARCHITECTURE_STANDARD.md` | Naming conventions, error response shape, auth patterns, API route conventions | ✅ Cross-project |
| 4 — Backend Modules | `MODULE_TEMPLATE.md` | File structure, naming, Zod patterns, controller patterns | ✅ Cross-project |
| 5 — Backend Testing | `TESTING_CONVENTIONS.md` | Behavioral testing approach — test what the user/caller experiences, not implementation details. File structure, naming, coverage rules | ✅ Cross-project |
| 6 — Migrations | `MIGRATION_TEMPLATE.md` | Migration file naming, index conventions, seed data format | ✅ Cross-project |
| 9 — Frontend Modules | `API_STANDARD.md` | Zod copy rules, hook patterns, service layer structure, endpoint config | ✅ Cross-project |
| 10 — Pages | `STYLE_GUIDE.md` | Visual rules extracted from Phase 8 screenshots — colors, spacing, typography, component styling. Tailwind + shadcn/ui as the component system | 🔁 Per-project |
| 11 — Frontend Testing | `TESTING_CONVENTIONS.md` | Same as Phase 5 — behavioral focus. Test user interactions and outcomes, not component internals | ✅ Cross-project |
| 12 — E2E Testing | `E2E_PATTERNS.md` | Selector strategy, fixture structure, flow test patterns | ✅ Cross-project |
| 13 — Code Review | `REVIEW_CHECKLIST.md` | Security checks, performance checks, consistency rules | ✅ Cross-project |
| 14 — Documentation | `DOC_TEMPLATES.md` | README structure, API doc format, onboarding guide template | ✅ Cross-project |
| 15 — Deployment | `INFRA_STANDARD.md` | Dockerfile patterns, CI/CD template, env config conventions | ✅ Cross-project |

**Note:** Phases 2 (Project Planning) and 7 (UI Design) do not require skill documents. These phases benefit from persona flexibility — rigid templates would constrain the AI's ability to think broadly about planning and design. Phase 1 (Business Requirements) uses `BRD_FORMAT.md` to ensure consistent, testable requirement structure. Phase 8 (Style Guide) does not consume a skill but *produces* one.

**Note on Component Library:** This workflow uses Tailwind CSS + shadcn/ui as the design system. This replaces the need for a dedicated shared component library generation phase — the component system already exists. The style guide skill ensures the AI uses it consistently.

---

## Decision Log Template

Keep this running throughout the project. One file, append as you go.

```markdown
## [Phase Name] — [Date]

### What the AI generated:
Brief description of the output.

### What I changed:
- [Change 1]: reason
- [Change 2]: reason

### Pattern to watch for:
Note if this is a recurring issue to address in the prompt template.
```

---

## Dependency Map

```
PHASE 1 — Business Requirements ✅ VERIFY
  └── PHASE 2 — Project Planning & Estimation
        └── PHASE 3 — Architecture & Model Design
              ├── PHASE 4 — Backend Module Generation ✅ VERIFY
              │     ├── PHASE 5 — Backend Testing
              │     └── PHASE 6 — DB Migrations & Seed Data
              │
              └── PHASE 7 — UI/UX Design
                    └── PHASE 8 — Style Guide
                          └── PHASE 9 — Frontend API Modules (copies Zod from Phase 4)
                                └── PHASE 10 — Page Generation (mock data from Zod)
                                      └── PHASE 11 — Frontend Testing
                                            └── PHASE 12 — Integration & E2E Testing
                                                  └── PHASE 13 — Code Review
                                                        └── PHASE 14 — Documentation
                                                              └── PHASE 15 — Deployment Config
```

**Parallel tracks:** Once Phase 3 is verified, the backend track (Phases 4–6) and the design track (Phases 7–8) can proceed in parallel. The frontend track (Phases 9–11) begins once both Phase 4 and Phase 8 are complete.

---

## Change Propagation

When a requirement changes mid-build:

1. Update the BRD
2. Check the dependency map to identify affected phases
3. Re-run affected phases in order using the updated BRD
4. Log the change and reasoning in the Decision Log

---

## PHASE 1 — Business Requirements

| | |
|---|---|
| **Persona** | Business Analyst |
| **Skill** | `BRD_FORMAT.md` — BRD structure, module IDs, requirement IDs, Given/When/Then criteria, error states |
| **Context** | App idea, user stories, stakeholder notes |
| **Output** | Business Requirements Document (BRD) |
| **Gate** | ✅ MANUAL VERIFICATION — this is your anchor for everything |

**Prompt:**
```
PERSONA: You are a Senior Business Analyst.

SKILL: Follow the BRD format standard below for document structure, module IDs,
requirement IDs, acceptance criteria format, and error state documentation.
{BRD_FORMAT.md}

CONTEXT:
App Concept: {INPUT}
User Stories: {INPUT}

TASK: Generate a complete Business Requirements Document:
- Project overview and objectives
- User roles and personas
- User stories for every user-facing interaction (As a / I want to / So that),
  each mapped to a module and page(s)
- Page Manifest table derived from user stories (page, stories, route)
- Functional requirements grouped by module
- Non-functional requirements (performance, security, scalability)
- Error states and edge cases for each functional requirement
- Acceptance criteria for each requirement
- Assumptions and constraints
- Out of scope items
```

**Human Review Focus:** Are all features captured? Are the acceptance criteria testable? Are edge cases realistic? This document drives everything — invest the most review time here.

---

## PHASE 2 — Project Planning & Estimation

| | |
|---|---|
| **Persona** | Project Manager |
| **Skill** | None — persona flexibility preferred |
| **Context** | BRD |
| **Output** | Project plan, estimates, dependency map |
| **Gate** | Review estimates for reasonableness |

**Prompt:**
```
PERSONA: You are a Senior Project Manager.

CONTEXT:
BRD: {BRD}

TASK: Generate the following:
- Module breakdown with task-level estimates (hours)
- Sprint/milestone plan with prioritized build order
- Dependency map (requirement ID → model → routes → frontend page)
- Risk register with mitigation strategies
- Critical path identification
```

**Human Review Focus:** Are estimates realistic for your team/skill level? Is the build order logical? Does the dependency map make sense?

---

## PHASE 3 — Architecture & Model Design

| | |
|---|---|
| **Persona** | Software Architect |
| **Skill** | `ARCHITECTURE_STANDARD.md` — naming conventions, error response shape, auth patterns, API route conventions |
| **Context** | BRD + Phase 2 output (plan, build order) |
| **Output** | Data models, ERD, API route map, auth strategy, error standards |
| **Gate** | Review model relationships and API surface |

**Prompt:**
```
PERSONA: You are a Senior Software Architect.

SKILL: Follow the architecture standards below for all naming conventions,
error response shapes, auth patterns, and API route conventions.
{ARCHITECTURE_STANDARD.md}

CONTEXT:
BRD: {BRD}
Project Plan: {PHASE 2 OUTPUT}

TASK: Design the following:
- Data models with field types, relationships, and constraints
- Entity Relationship Diagram (mermaid syntax)
- API route map (method, path, request/response shape, auth level)
- Authentication and authorization strategy
- Error response standards (error codes, response shapes)
- Caching strategy (if applicable)
- File/media handling approach (if applicable)
```

**Human Review Focus:** Do model relationships match the BRD? Is the API surface complete — every feature has routes? Are there missing auth guards? Is the error standard consistent?

---

## PHASE 4 — Backend Module Generation

| | |
|---|---|
| **Persona** | Backend Engineer |
| **Skill** | `MODULE_TEMPLATE.md` — file structure, naming, Zod patterns, controller patterns |
| **Context** | BRD + Phase 3 output (models, route map, error standards) |
| **Output** | Zod schemas, routes, controllers, middleware — per module |
| **Gate** | ✅ MANUAL VERIFICATION |

**Prompt (run per module, or pass "all" to generate every module in dependency order):**
```
PERSONA: You are a Senior Backend Engineer.

SKILL: Follow the module template below for file structure, naming conventions,
Zod schema patterns, and controller patterns.
{MODULE_TEMPLATE.md}

CONTEXT:
BRD: {BRD}
Model Design: {PHASE 3 — relevant model}
Route Map: {PHASE 3 — relevant routes}
Error Standards: {PHASE 3 — error standards}

TASK: Generate the backend module for {MODULE_NAME | all}:
- Zod validation schemas (create, update, response, query params)
- Route definitions
- Controller logic (CRUD + custom operations)
- Middleware (auth guards, validation)
- Error handling following the project error standards

If "all", process modules in dependency order — no FK dependencies first.
```

**Human Review Focus:** Does the Zod schema match the model exactly? Are all routes from the route map implemented? Are auth guards applied correctly? These Zod schemas become the frontend's source of truth — they must be right.

---

## PHASE 5 — Backend Testing

| | |
|---|---|
| **Persona** | QA Engineer |
| **Skill** | `TESTING_CONVENTIONS.md` — behavioral testing approach, file structure, naming, coverage rules |
| **Context** | BRD (acceptance criteria) + Phase 4 output (modules) |
| **Output** | Unit tests, integration tests, validation tests |
| **Gate** | All tests pass |

**Prompt (run per module, or pass "all" to test every module):**
```
PERSONA: You are a Senior QA Engineer who writes behavioral tests — tests that
verify what the caller/user experiences, not how the code is internally implemented.

SKILL: Follow the testing conventions below for file structure, naming,
coverage requirements, and assertion patterns. Focus on testing behavior
and outcomes, not implementation details.
{TESTING_CONVENTIONS.md}

CONTEXT:
BRD: {BRD — relevant requirements and acceptance criteria}
Module Code: {PHASE 4 — relevant module}
Error Standards: {PHASE 3 — error standards}

TASK: Create the following tests for {MODULE_NAME | all}:
- Unit tests for each controller method — test the input/output contract,
  not internal function calls
- Integration tests for each route — test real HTTP requests, auth, validation,
  and response shapes
- Edge case tests derived from the BRD error states — test what happens when
  things go wrong from the caller's perspective
- Zod schema validation tests — test boundary values, missing fields, wrong types

If "all", process modules in the same dependency order used in Phase 4.
```

**Human Review Focus:** Are tests asserting on outcomes (response status, response body, database state) rather than implementation (function was called, mock was invoked with specific args)? Tests coupled to implementation break on every refactor but catch no real bugs.

---

## PHASE 6 — Database Migrations & Seed Data

| | |
|---|---|
| **Persona** | Backend Engineer / DBA |
| **Skill** | `MIGRATION_TEMPLATE.md` — migration file naming, index conventions, seed data format |
| **Context** | BRD + Phase 3 output (models, ERD) + Phase 4 finalized modules |
| **Output** | SQL: migration scripts, rollback scripts, seed data. MongoDB: Prisma seed scripts |
| **Gate** | SQL: migrations run up and down cleanly. MongoDB: seed data passes Zod validation |

This phase auto-detects database type from the architecture doc or Prisma `datasource` block and adapts accordingly.

**Prompt (SQL databases — PostgreSQL, MySQL, etc.):**
```
PERSONA: You are a Senior Backend Engineer handling database operations.

SKILL: Follow the migration template below for file naming, index conventions,
and seed data format.
{MIGRATION_TEMPLATE.md}

CONTEXT:
BRD: {BRD}
Data Models: {PHASE 3 — models}
ERD: {PHASE 3 — relationships}

TASK: Generate the following:
- Migration scripts for each model (create table, indexes, foreign keys)
- Rollback scripts for each migration
- Seed data scripts with realistic test data
- Seed data for different environments (dev, staging, test)
```

**Prompt (MongoDB — Prisma + MongoDB):**
```
PERSONA: You are a Senior Backend Engineer handling database operations.

CONTEXT:
BRD: {BRD}
Data Models: {PHASE 3 — models}
Phase 4 Modules: {Finalized Zod schemas and Prisma schemas}

TASK: MongoDB is schemaless — skip traditional migrations. Instead:
- Generate Prisma seed script (prisma/seed.ts) for all models in dependency order
- Seed data per environment: dev (~5-10 records), staging (~50-100), test (~2-3)
- Do NOT generate an index verification script — prisma db push handles indexes
- Seed data must pass Zod validation and respect FK relationships
- Make seed scripts idempotent
```

**Human Review Focus:** Do migrations/seeds match the finalized models from Phase 4 (not just Phase 3)? Is seed data realistic enough for meaningful testing? For MongoDB: do seeds pass Zod validation?

---

## PHASE 7 — UI/UX Design

| | |
|---|---|
| **Persona** | Professional UI Designer |
| **Skill** | None — persona flexibility preferred for creative design work |
| **Context** | BRD (Page Manifest from user stories) + Phase 3 output (route map) + optional reference screenshots + optional design rules |
| **Output** | Design system summary, wireframes/mockups, component inventory, user flows |
| **Gate** | Review against Page Manifest — every page must have a wireframe |

**Input:** Optionally pass reference screenshots and/or design rules as arguments. Screenshots are analyzed to extract colors, typography, spacing, and component patterns. Design rules (e.g., "mobile first", "dark mode default") are applied as hard constraints.

**Prompt:**
```
PERSONA: You are a Professional UI/UX Designer.

CONTEXT:
BRD: {BRD — use the Page Manifest from User Stories as the definitive page list}
API Surface: {PHASE 3 — route map}
Reference Screenshots: {OPTIONAL — images to extract visual style from}
Design Rules: {OPTIONAL — e.g., "mobile first", "dark mode default"}

TASK: Generate the following:
- Design system summary (color palette, typography, spacing — extracted from
  screenshots if provided, or sensible defaults)
- Page inventory pulled from the BRD Page Manifest — do not invent pages
- Wireframe descriptions or mockups for each page
- User flow diagrams (mermaid syntax)
- Component inventory (reusable UI components)
- Responsive behavior (breakpoints, layout adaptation per breakpoint)
- State designs for each page: loading, empty, error, populated
```

**Human Review Focus:** Does every page in the Page Manifest have a wireframe? If screenshots were provided, do the extracted styles match? Are the user flows complete? Are error/empty states accounted for?

---

## PHASE 8 — Style Guide

| | |
|---|---|
| **Persona** | UI Designer |
| **Skill** | None — this phase *produces* a skill document (`STYLE_GUIDE.md`) used by later phases |
| **Context** | BRD + Phase 7 output (screenshots of mock screens/wireframes) |
| **Output** | Style guide document (becomes a skill for Phase 10+) |
| **Gate** | Review for specificity — every rule must have exact values, not descriptions |

**Prompt:**
```
PERSONA: You are a Professional UI Designer creating a style guide.

CONTEXT:
BRD: {BRD}
Screenshots: {PHASE 7 — screenshots of generated mock screens/wireframes}

TASK: Analyze the provided screenshots and extract a concrete style guide
that an AI can follow consistently during code generation. The project
uses Tailwind CSS and shadcn/ui. Every rule must include exact values:
- Color palette (exact hex values for primary, secondary, semantic colors, mapped to Tailwind classes)
- Typography scale (font families, exact sizes, weights, line heights)
- Spacing system (base unit, exact scale values)
- Component styling rules (buttons, inputs, cards, modals, tables — with Tailwind classes and shadcn variants)
- Animation/transition standards (exact durations, easing)
- Dark mode rules (if applicable)
- Accessibility requirements (contrast ratios, focus states, ARIA patterns)
```

**Why screenshots matter:** AI extracts concrete values (exact colors, spacing proportions, typography choices) from actual visuals far more reliably than generating them from text descriptions. This eliminates the vagueness problem where style guides say "use consistent spacing" instead of "8px base unit, scale of 8/16/24/32/48."

**Human Review Focus:** Is every rule specific enough to produce identical results across independently prompted pages? If a rule says "use a muted color" instead of "use `text-slate-500`", it's not specific enough. This is the most important review gate for frontend consistency.

**Note:** The output of this phase becomes `STYLE_GUIDE.md` — a per-project skill document used in Phase 10 (Page Generation).

**Human Review Focus:** Is the style guide specific enough that the AI can follow it consistently during page generation? Vague guidelines produce inconsistent pages.

---

## PHASE 9 — Frontend API Modules

⚠️ **Multi-source context required**

| | |
|---|---|
| **Persona** | Frontend Architect / Engineer |
| **Skill** | `API_STANDARD.md` — Zod copy rules, hook patterns, service layer structure, endpoint config |
| **Context** | BRD + Phase 4 output (Zod schemas — copied from backend) |
| **Output** | Copied Zod schemas, TypeScript types, hooks, services, endpoint configs, mock data factories |
| **Gate** | Zod schemas match backend exactly |

**Prompt (run per module):**
```
PERSONA: You are a Senior Frontend Engineer.

SKILL: Follow the API_STANDARD below for Zod copy rules, hook patterns,
service layer structure, and endpoint configuration.
{API_STANDARD.md}

CONTEXT:
BRD: {BRD}
Backend Zod Schemas: {PHASE 4 — zod files}
API Route Map: {PHASE 3 — routes}

TASK: Using the backend Zod schemas as the single source of truth:
1. Copy the Zod schemas from the backend into the frontend project
2. Generate TypeScript types inferred from the Zod schemas
3. Generate API endpoint configuration for each route
4. Generate service layer functions for each endpoint
5. Generate custom hooks wrapping each service function
6. Generate mock data factories using the copied Zod schemas
```

**Human Review Focus:** Do the copied Zod schemas exactly match the backend? Are mock data factories producing realistic data that covers edge cases?

---

## PHASE 10 — Page Generation

⚠️ **Multi-source context required — prompted one page at a time**

| | |
|---|---|
| **Persona** | Frontend Engineer |
| **Skill** | `STYLE_GUIDE.md` (from Phase 8) — ensures visual consistency across independently prompted pages |
| **Context** | BRD (relevant module only) + Phase 7 output (relevant page design) + Phase 9 output (hooks, types, mock data) |
| **Output** | Page components, layout components |
| **Gate** | Page renders correctly with mock data, matches design |

Each page is prompted individually. The style guide skill is included in every page prompt to ensure consistency across pages that are generated in separate conversations.

**Prompt (run per page):**
```
PERSONA: You are a Senior Frontend Engineer.

SKILL: Follow the style guide below for all styling decisions. The project
uses Tailwind CSS and shadcn/ui for components.
{STYLE_GUIDE.md}

CONTEXT:
BRD: {BRD — relevant module requirements only}
Design: {PHASE 7 — relevant page screenshot/wireframe}
Available Hooks: {PHASE 9 — hook signatures}
Mock Data: {PHASE 9 — mock factories}
Types: {PHASE 9 — inferred types}

TASK: Build the page for {PAGE_NAME}:
- Implement the layout matching the wireframe/design
- Use Tailwind CSS for styling, following the style guide exactly
- Use shadcn/ui components where applicable
- Import and use the generated hooks for data fetching
- Use mock data for development/preview
- Implement all states: loading, empty, error, populated
- Include form validation using the frontend Zod schemas
- Implement responsive behavior per the style guide
```

**Human Review Focus:** Does the page match the design? Are Tailwind classes and shadcn components used consistently with the style guide? Do all states render correctly? Compare against previously generated pages for visual consistency.

**Human Review Focus:** Does the page match the design? Do all states (loading, empty, error) render correctly? Is the style guide being followed consistently?

---

## PHASE 11 — Frontend Testing

| | |
|---|---|
| **Persona** | QA Engineer |
| **Skill** | `TESTING_CONVENTIONS.md` — behavioral testing approach, component/hook test patterns |
| **Context** | BRD (acceptance criteria) + Phase 10 output (pages) + Phase 9 output (mock data) |
| **Output** | Component tests, hook tests, form validation tests |
| **Gate** | All tests pass |

**Prompt (run per page/module):**
```
PERSONA: You are a Senior QA Engineer focused on frontend testing. You write
behavioral tests — test what the user sees and experiences, not component internals.

SKILL: Follow the testing conventions below for file structure, naming,
and assertion patterns. Test user-visible behavior, not implementation.
{TESTING_CONVENTIONS.md}

CONTEXT:
BRD: {BRD — relevant acceptance criteria}
Page Component: {PHASE 10 — relevant page}
Mock Data: {PHASE 9 — mock factories}

TASK: Create the following tests:
- Component tests: does the user see the right content? Do interactions
  (clicks, form submissions, navigation) produce the right outcomes?
- Hook tests: does the hook return the right data shape? Does it handle
  loading and error states correctly?
- Form validation tests: does the user see error messages for invalid input?
  Can the user submit valid input successfully?
- Accessibility tests: can the user navigate by keyboard? Are ARIA labels present?
```

**Human Review Focus:** Are tests written from the user's perspective? A good test reads like "when the user clicks submit with an empty name field, they see a validation error." A bad test reads like "expect setError to have been called with {name: 'required'}."

---

## PHASE 12 — Integration & E2E Testing

⚠️ **Multi-source context required**

| | |
|---|---|
| **Persona** | QA Engineer |
| **Skill** | `E2E_PATTERNS.md` — selector strategy, fixture structure, flow test patterns |
| **Context** | BRD + Phase 3 output (route map) + Phase 7 output (user flows) |
| **Output** | E2E test suites |
| **Gate** | All E2E flows pass against running backend |

**Prompt:**
```
PERSONA: You are a Senior QA Engineer writing end-to-end tests.

SKILL: Follow the E2E patterns below for selector strategy, fixture structure,
and flow test organization.
{E2E_PATTERNS.md}

CONTEXT:
BRD: {BRD}
User Flows: {PHASE 7 — user flow diagrams}
API Routes: {PHASE 3 — route map}

TASK: Create the following E2E tests:
- E2E test suites covering each user flow
- Happy path tests for all critical journeys
- Edge case and error path tests from BRD error states
- Cross-module integration tests (create → list → edit → delete)
- Auth flow tests (login, logout, unauthorized access, role-based access)
```

**Human Review Focus:** Do E2E tests map to real user journeys? Is the auth flow fully covered?

---

## PHASE 13 — Code Review (Rolling)

⚠️ **Multi-source context required**

| | |
|---|---|
| **Persona** | Senior Architect / Tech Lead |
| **Skill** | `REVIEW_CHECKLIST.md` — security checks, performance checks, consistency rules |
| **Context** | BRD + Phase 3 output (architecture) + relevant code |
| **Output** | Review report, recommended fixes |
| **Gate** | All critical issues resolved before proceeding |

**This is not a single phase at the end.** Run code reviews at these checkpoints:

| Checkpoint | What to review | Why |
|---|---|---|
| After first backend module (Phase 4, first module) | Controller patterns, auth guards, error handling, Zod structure | Catches template-level issues before you generate more modules with the same patterns |
| After backend track complete (Phases 4–6) | Cross-module consistency, migration correctness, query patterns | Catches N+1 queries, missing indexes, inconsistent error handling across modules |
| After first frontend page (Phase 10, first page) | Component structure, style guide adherence, hook usage patterns | Same logic — catch pattern issues before generating more pages |
| After frontend track complete (Phases 9–11) | Cross-page consistency, API contract alignment, state management | Catches drift between frontend types and backend Zod schemas |
| Final sweep (after Phase 12) | Full security review, performance review, everything together | Final gate before documentation and deployment |

**Prompt:**
```
PERSONA: You are a Senior Architect conducting a code review.

SKILL: Follow the review checklist below for security, performance,
and consistency checks.
{REVIEW_CHECKLIST.md}

CONTEXT:
BRD: {BRD}
Architecture Decisions: {PHASE 3 OUTPUT}
Code to Review: {RELEVANT CODE}

TASK: Review the code for:
- Security: auth guards, input sanitization, no exposed secrets, injection prevention
- Performance: N+1 queries, missing indexes, unnecessary re-renders, missing pagination
- Consistency: naming conventions, error handling patterns, structure matches architecture
- Missing pieces: unhandled edge cases, missing error boundaries, missing loading states
- API contract: frontend types still match backend Zod schemas
```

**Human Review Focus:** Prioritize security and performance findings. Verify the AI isn't hallucinating issues that don't exist in the code. Pay special attention to the first-module reviews — if patterns are right in the first module, they'll be right in all of them.

---

## PHASE 14 — Documentation

| | |
|---|---|
| **Persona** | Technical Writer |
| **Skill** | `DOC_TEMPLATES.md` — README structure, API doc format, onboarding guide template |
| **Context** | BRD + Phase 3 output (architecture) + Phase 4 output (Zod/routes) |
| **Output** | README, API docs, deployment guide, onboarding guide |
| **Gate** | Docs match actual implementation |

**Prompt:**
```
PERSONA: You are a Senior Technical Writer.

SKILL: Follow the documentation templates below for README structure,
API doc format, and onboarding guide layout.
{DOC_TEMPLATES.md}

CONTEXT:
BRD: {BRD}
Architecture: {PHASE 3 OUTPUT}
Route Map: {PHASE 3 — routes}
Zod Schemas: {PHASE 4 — schemas}

TASK: Generate the following documentation:
- README.md (project overview, tech stack, setup instructions, project structure)
- API documentation (endpoints, request/response examples, auth requirements, error codes)
- Environment variables documentation
- Developer onboarding guide (run locally, run tests, add a new module)
- Architecture decision records (key decisions and rationale)
```

**Human Review Focus:** Does the README setup actually work? Do the API doc examples match reality?

---

## PHASE 15 — Deployment Configuration

| | |
|---|---|
| **Persona** | DevOps Engineer |
| **Skill** | `INFRA_STANDARD.md` — Dockerfile patterns, CI/CD template, env config conventions |
| **Context** | BRD + Phase 3 output (architecture) + Phase 14 output (env docs) |
| **Output** | Dockerfiles, CI/CD configs, environment templates |
| **Gate** | Build and deploy succeeds in staging |

**Prompt:**
```
PERSONA: You are a Senior DevOps Engineer.

SKILL: Follow the infrastructure standards below for Dockerfile patterns,
CI/CD pipeline structure, and environment configuration.
{INFRA_STANDARD.md}

CONTEXT:
BRD: {BRD}
Architecture: {PHASE 3 OUTPUT}
Environment Docs: {PHASE 14 OUTPUT}

TASK: Generate the following:
- Dockerfile(s) for backend and frontend
- Docker Compose for local development (app + database + services)
- CI/CD pipeline configuration (lint → test → build → deploy)
- Environment configuration templates (.env.example per environment)
- Health check endpoints and monitoring setup
- Production deployment checklist
```

**Human Review Focus:** Does Docker Compose work locally? Does the CI/CD pipeline match your actual infrastructure?

---

## Quick Reference

| # | Phase | Persona | Skill | Context | Gate |
|---|---|---|---|---|---|
| 1 | Business Requirements | Business Analyst | `BRD_FORMAT` | App idea + user stories | ✅ VERIFY (includes User Stories + Page Manifest) |
| 2 | Project Planning | Project Manager | — | BRD | Review |
| 3 | Architecture | Architect | `ARCHITECTURE_STANDARD` | BRD + Phase 2 | Review |
| 4 | Backend Modules | Backend Engineer | `MODULE_TEMPLATE` | BRD + Phase 3 | ✅ VERIFY |
| 5 | Backend Testing | QA Engineer | `TESTING_CONVENTIONS` | BRD + Phase 4 | Tests pass |
| 6 | DB Migrations/Seeds | Backend Engineer | `MIGRATION_TEMPLATE` | BRD + Phase 3 + Phase 4 | SQL: migrations run / MongoDB: seeds pass Zod (no index verification) |
| 7 | UI/UX Design | UI Designer | — | BRD (Page Manifest) + Phase 3 + optional screenshots/rules | Review |
| 8 | Style Guide | UI Designer | — (produces `STYLE_GUIDE`) | BRD + Phase 7 | Review |
| 9 | Frontend API Modules | Frontend Engineer | `API_STANDARD` | BRD + Phase 4 Zod | ⚠️ Zod match |
| 10 | Page Generation | Frontend Engineer | `STYLE_GUIDE` + `COMPONENT_PATTERNS` | BRD + Phase 7, 9 | ⚠️ Renders correctly |
| 11 | Frontend Testing | QA Engineer | `TESTING_CONVENTIONS` | BRD + Phase 9, 10 | Tests pass |
| 12 | E2E Testing | QA Engineer | `E2E_PATTERNS` | BRD + Phase 3, 7 | ⚠️ All flows pass |
| 13 | Code Review | Architect | `REVIEW_CHECKLIST` | BRD + Phase 3 + code | ⚠️ Issues resolved |
| 14 | Documentation | Technical Writer | `DOC_TEMPLATES` | BRD + Phase 3, 4 | Docs match code |
| 15 | Deployment Config | DevOps Engineer | `INFRA_STANDARD` | BRD + Phase 3, 14 | Deploy succeeds |

⚠️ = Multi-source context — assemble inputs from multiple phases before prompting.

---

## Parallel Execution

```
Phase 3 complete
  ├── Backend Track: Phase 4 → 5 → 6
  └── Design Track:  Phase 7 → 8
                                 ↘
         Both tracks complete → Phase 9 → 10 → 11 → 12 → 13 → 14 → 15
```

---

## Iteration Strategy

For large apps, don't generate all modules at once. Run Phases 4–11 per module:

1. Generate backend module → test → migrate
2. Generate frontend API module → pages → test
3. Move to next module
4. Run Phase 12 (E2E) once all modules are integrated
5. Run Phases 13–15 as a final sweep
