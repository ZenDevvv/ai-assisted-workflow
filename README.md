# AI-Assisted Fullstack Development Workflow

A 15-phase AI-assisted development workflow powered by Claude Code slash commands. Each phase automatically loads the right persona, skill documents, and context — no manual copy-pasting.

---

## How It Works

Every phase is a slash command. Type `/phase4-backend AUTH` and Claude Code automatically:

1. Adopts the Backend Engineer persona
2. Reads the MODULE_TEMPLATE.md skill doc
3. Reads your BRD and architecture doc for the AUTH module
4. Generates the module code

You review the output, make corrections, and move to the next phase.

## Sample Usage

```
/phase1-brd <your app concept or user stories>
/phase2-planning
/phase3-architecture
/phase4-backend <MODULE_NAME> | all
/phase5-backend-testing <MODULE_NAME> | all
/phase6-migrations
/phase7-ui-design <optional: screenshot paths and/or design rules>
/phase8-style-guide
/phase9-frontend-api <MODULE_NAME>
/phase10-pages <PAGE_NAME>
/phase11-frontend-testing <MODULE_OR_PAGE_NAME>
/phase12-e2e
/phase13-review <optional: what to review>
/phase14-docs
/phase15-deployment

```

---

## Repository Structure

```
├── CLAUDE.md                       # Auto-loaded by Claude Code — project context
├── .claude/commands/               # Slash commands for all 15 phases
│   ├── phase1-brd.md
│   ├── phase2-planning.md
│   ├── phase3-architecture.md
│   ├── phase4-backend.md
│   ├── phase5-backend-testing.md
│   ├── phase6-migrations.md
│   ├── phase7-ui-design.md
│   ├── phase8-style-guide.md
│   ├── phase9-frontend-api.md
│   ├── phase10-pages.md
│   ├── phase11-frontend-testing.md
│   ├── phase12-e2e.md
│   ├── phase13-review.md
│   ├── phase14-docs.md
│   └── phase15-deployment.md
│
├── personas/                       # AI persona files (9 roles)
│   ├── business-analyst.md
│   ├── project-manager.md
│   ├── software-architect.md
│   ├── backend-engineer.md
│   ├── qa-engineer.md
│   ├── ui-designer.md
│   ├── frontend-engineer.md
│   ├── technical-writer.md
│   └── devops-engineer.md
│
├── skills/                         # Reusable skill documents
│   ├── BRD_FORMAT.md               # BRD structure, module IDs, GWT criteria
│   ├── MODULE_TEMPLATE.md          # Backend file structure, Zod, controller patterns
│   ├── API_STANDARD.md             # Frontend hooks, service layer, Zod copy rules
│   ├── STYLE_GUIDE.md              # (created per-project in Phase 8)
│   └── ...                         # More skills added as you refine conventions
│
├── docs/                           # Project artifacts (created as you go)
│   ├── brd.md                      # Phase 1 output
│   ├── project-plan.md             # Phase 2 output
│   ├── architecture.md             # Phase 3 output
│   └── ui-design.md                # Phase 7 output
│
└── AI-Assisted Fullstack Development Workflow.md  # Full playbook reference
```

---

## Prerequisites

- **VSCode** with [Claude Code](https://marketplace.visualstudio.com/items?itemName=Anthropic.claude-code)
- A project idea to build

---

## Getting Started

1. **Clone this repo** into your workspace
2. **Open in VSCode** with Claude Code installed
3. **Run Phase 1**: type `/phase1-brd` followed by your app concept
4. **Review the BRD** carefully — it drives everything downstream
5. **Continue through phases** in order, using the slash commands

---

## The 15 Phases

### Phase 1 — Business Requirements

```
/phase1-brd <your app concept or user stories>
```

Persona: Business Analyst | Skill: `BRD_FORMAT` | Output: `docs/brd.md`

Generates a complete BRD with module IDs, requirement IDs, Given/When/Then acceptance criteria, error states, **user stories**, and a **Page Manifest**. User stories map every user-facing interaction to a page — this becomes the source of truth for what Phase 7 designs and Phase 10 builds.

**Gate:** ⚠️ VERIFY — this document drives everything. Invest the most review time here.

---

### Phase 2 — Project Planning & Estimation

```
/phase2-planning
```

Persona: Project Manager | Skill: — | Reads: `docs/brd.md` | Output: `docs/project-plan.md`

Generates module breakdown, task estimates, sprint plan, dependency map, and risk register.

**Gate:** Review estimates and build order.

---

### Phase 3 — Architecture & Model Design

```
/phase3-architecture
```

Persona: Software Architect | Skill: `ARCHITECTURE_STANDARD` | Reads: `docs/brd.md`, `docs/project-plan.md` | Output: `docs/architecture.md`

Designs data models, ERD, API route map, auth strategy, and error standards. If `skills/ARCHITECTURE_STANDARD.md` doesn't exist yet, this phase creates it.

**Gate:** Review model relationships, API surface completeness, auth guards.

---

### Phase 4 — Backend Module Generation

```
/phase4-backend <MODULE_NAME> | all
```

**Example:** `/phase4-backend AUTH` or `/phase4-backend all`

Persona: Backend Engineer | Skill: `MODULE_TEMPLATE` | Reads: `docs/brd.md` (module section), `docs/architecture.md` | Output: module code

Generates Zod schemas, routes, controllers, middleware. Pass a module name for one module, or `all` to generate every module in dependency order.

**Gate:** ⚠️ VERIFY — Zod schemas become the frontend's source of truth. After the **first** module, run `/phase13-review` before generating more.

---

### Phase 5 — Backend Testing

```
/phase5-backend-testing <MODULE_NAME> | all
```

**Example:** `/phase5-backend-testing AUTH` or `/phase5-backend-testing all`

Persona: QA Engineer | Skill: `TESTING_CONVENTIONS` | Reads: `docs/brd.md` (acceptance criteria), Phase 4 module code, `docs/architecture.md`

Generates behavioral unit tests, integration tests, and Zod validation tests. Pass a module name for one module, or `all` to test every module. If `skills/TESTING_CONVENTIONS.md` doesn't exist yet, this phase creates it.

**Gate:** Run tests. Confirm they pass AND the test quality is good.

---

### Phase 6 — Database Migrations & Seed Data

```
/phase6-migrations
```

Persona: Backend Engineer | Skill: `MIGRATION_TEMPLATE` | Reads: `docs/brd.md`, `docs/architecture.md`, Phase 4 modules

Auto-detects database type. **SQL:** generates migration scripts, rollback scripts, and seed data. **MongoDB:** skips migrations (Prisma handles schema), generates Prisma seed scripts for all models with per-environment data (dev/staging/test). Does not generate index verification scripts — `prisma db push` handles indexes.

**Gate:** SQL: migrations run up and down cleanly. MongoDB: seed data passes Zod validation, FK relationships are consistent.

---

### Phase 7 — UI/UX Design

```
/phase7-ui-design <optional: screenshot paths and/or design rules>
```

**Examples:**
- `/phase7-ui-design` — generate designs using defaults
- `/phase7-ui-design docs/screenshots/reference.png mobile first, dark mode default`
- `/phase7-ui-design minimal sidebar, dark mode default`

Persona: UI Designer | Skill: — | Reads: `docs/brd.md` (Page Manifest from user stories), `docs/architecture.md` | Output: `docs/ui-design.md`

Generates design system summary, wireframes, user flows, component inventory, responsive behavior, and state designs. If reference screenshots are provided, extracts colors, typography, spacing, and component patterns from them. Custom design rules (e.g., "mobile first", "dark mode default") are applied as hard constraints.

**Gate:** Does every page in the Page Manifest have a wireframe? Do extracted styles match the reference screenshots?

---

### Phase 8 — Style Guide

```
/phase8-style-guide
```

Persona: UI Designer | Skill: — | Reads: `docs/brd.md`, `docs/ui-design.md` | Output: `skills/STYLE_GUIDE.md`

Extracts a concrete style guide with exact values (hex colors, Tailwind classes, spacing units) from the wireframes. The output becomes a skill document used by Phase 10.

**Gate:** Is every rule specific enough to produce identical results across independently prompted pages? "Use a muted color" is too vague — "use `text-slate-500`" is correct.

---

### Phase 9 — Frontend API Modules

```
/phase9-frontend-api <MODULE_NAME>
```

**Example:** `/phase9-frontend-api AUTH`

Persona: Frontend Engineer | Skill: `API_STANDARD` | Reads: `docs/brd.md` (module section), Phase 4 Zod schemas, `docs/architecture.md`

Copies Zod schemas from backend, generates TypeScript types, endpoint configs, service layer, React Query hooks, and mock data factories for one module.

**Gate:** Do copied Zod schemas exactly match the backend?

---

### Phase 10 — Page Generation

```
/phase10-pages <PAGE_NAME>
```

**Example:** `/phase10-pages DashboardPage` or `/phase10-pages LoginPage`

Persona: Frontend Engineer | Skill: `STYLE_GUIDE` | Reads: `docs/brd.md` (module section), `docs/ui-design.md` (page wireframe), Phase 9 hooks/types

Builds one page at a time using Tailwind + shadcn/ui, following the style guide exactly. Implements all states: loading, empty, error, populated.

**Gate:** Does the page match the design? After the **first** page, run `/phase13-review` before generating more.

---

### Phase 11 — Frontend Testing

```
/phase11-frontend-testing <MODULE_OR_PAGE_NAME>
```

**Example:** `/phase11-frontend-testing DashboardPage`

Persona: QA Engineer | Skill: `TESTING_CONVENTIONS` | Reads: `docs/brd.md` (acceptance criteria), Phase 10 page, Phase 9 mock data

Generates behavioral component tests, hook tests, form validation tests, and accessibility tests.

**Gate:** Run tests. Confirm they pass AND the test quality is good.

---

### Phase 12 — Integration & E2E Testing

```
/phase12-e2e
```

Persona: QA Engineer | Skill: `E2E_PATTERNS` | Reads: `docs/brd.md`, `docs/architecture.md`, `docs/ui-design.md`

Generates E2E test suites covering user flows, happy paths, error paths, cross-module integration, and auth flows.

**Gate:** All E2E flows pass against a running backend.

---

### Phase 13 — Code Review (Rolling)

```
/phase13-review <optional: what to review>
```

**Examples:**

- `/phase13-review first backend module AUTH`
- `/phase13-review full backend track`
- `/phase13-review first frontend page DashboardPage`
- `/phase13-review` (auto-detects checkpoint based on progress)

Persona: Software Architect | Skill: `REVIEW_CHECKLIST` | Reads: `docs/brd.md`, `docs/architecture.md`, relevant code

Reviews code for security, performance, consistency, missing pieces, and API contract alignment. Run this at checkpoints — not just at the end.

**Gate:** All critical issues resolved before proceeding.

---

### Phase 14 — Documentation

```
/phase14-docs
```

Persona: Technical Writer | Skill: `DOC_TEMPLATES` | Reads: `docs/brd.md`, `docs/architecture.md`, Phase 4 Zod schemas

Generates README, API docs, environment variable docs, onboarding guide, and architecture decision records.

**Gate:** Does the README setup actually work? Do API doc examples match reality?

---

### Phase 15 — Deployment Configuration

```
/phase15-deployment
```

Persona: DevOps Engineer | Skill: `INFRA_STANDARD` | Reads: `docs/brd.md`, `docs/architecture.md`, Phase 14 docs

Generates Dockerfiles, Docker Compose, CI/CD pipeline, env templates, health checks, and production deployment checklist.

**Gate:** Does Docker Compose work locally? Does CI/CD match your infrastructure?

---

## Workflow Order

```
Phase 1 — BRD (VERIFY)
  └── Phase 2 — Planning
        └── Phase 3 — Architecture
              ├── Backend Track:  Phase 4 → 5 → 6    (can run in parallel)
              └── Design Track:   Phase 7 → 8         (can run in parallel)
                                        ↘
              Both tracks complete → Phase 9 → 10 → 11 → 12 → 13 → 14 → 15
```

After Phase 3, the backend track and design track can run in parallel (two separate Claude Code sessions).

---

## Code Review Checkpoints

Run `/phase13-review` at these points — don't wait until the end:

| When                               | Why                                                       |
| ---------------------------------- | --------------------------------------------------------- |
| After first backend module         | Catch pattern-level issues before generating more modules |
| After backend track (Phases 4-6)   | Cross-module consistency, migration correctness           |
| After first frontend page          | Catch UI pattern issues before generating more pages      |
| After frontend track (Phases 9-11) | Cross-page consistency, API contract alignment            |
| After E2E (Phase 12)               | Final security + performance sweep                        |

---

## Skills

Skills are reusable reference documents encoding your conventions. They survive across projects and improve over time.

| Skill                      | Status                         | Used In        |
| -------------------------- | ------------------------------ | -------------- |
| `BRD_FORMAT.md`            | ✅ Ready                       | Phase 1        |
| `MODULE_TEMPLATE.md`       | ✅ Ready                       | Phase 4        |
| `API_STANDARD.md`          | ✅ Ready                       | Phase 9        |
| `ARCHITECTURE_STANDARD.md` | Created by Phase 3             | Phase 3, 4, 13 |
| `TESTING_CONVENTIONS.md`   | Created by Phase 5             | Phase 5, 11    |
| `STYLE_GUIDE.md`           | Created per-project by Phase 8 | Phase 10       |
| `MIGRATION_TEMPLATE.md`    | Add when ready                 | Phase 6        |
| `E2E_PATTERNS.md`          | Add when ready                 | Phase 12       |
| `REVIEW_CHECKLIST.md`      | Add when ready                 | Phase 13       |
| `DOC_TEMPLATES.md`         | Add when ready                 | Phase 14       |
| `INFRA_STANDARD.md`        | Add when ready                 | Phase 15       |

Skills you don't have yet won't block you — the phase commands handle missing skills gracefully. After your first project, extract patterns from what worked into new skill docs.

---

## Extending

**Add a skill:** Create a `.md` file in `skills/` (Purpose → Rules → Templates → Examples → Anti-patterns). The relevant phase command will pick it up automatically.

**Add a persona:** Create a `.md` file in `personas/` (Identity → Perspective → Priorities → What You Produce → What You Do Not Do).

**Refine over time:** When you correct the AI's output, note what you changed. If the same correction happens twice, add a rule to the relevant skill doc.
