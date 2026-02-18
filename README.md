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

```
/phase1-brd "My app concept..."     → generates BRD
/phase3-architecture                → designs models, routes, auth
/phase4-backend AUTH                → generates backend module for AUTH
/phase5-backend-testing AUTH        → generates tests for AUTH
/phase9-frontend-api AUTH           → generates frontend hooks/services
/phase10-pages DashboardPage        → generates the dashboard page
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

| # | Phase | Command | Persona | Skill | Per-module? |
|---|-------|---------|---------|-------|-------------|
| 1 | Business Requirements | `/phase1-brd` | Business Analyst | `BRD_FORMAT` | No |
| 2 | Project Planning | `/phase2-planning` | Project Manager | — | No |
| 3 | Architecture | `/phase3-architecture` | Software Architect | `ARCHITECTURE_STANDARD` | No |
| 4 | Backend Modules | `/phase4-backend` | Backend Engineer | `MODULE_TEMPLATE` | Yes |
| 5 | Backend Testing | `/phase5-backend-testing` | QA Engineer | `TESTING_CONVENTIONS` | Yes |
| 6 | DB Migrations | `/phase6-migrations` | Backend Engineer | `MIGRATION_TEMPLATE` | No |
| 7 | UI/UX Design | `/phase7-ui-design` | UI Designer | — | No |
| 8 | Style Guide | `/phase8-style-guide` | UI Designer | — (produces `STYLE_GUIDE`) | No |
| 9 | Frontend API | `/phase9-frontend-api` | Frontend Engineer | `API_STANDARD` | Yes |
| 10 | Page Generation | `/phase10-pages` | Frontend Engineer | `STYLE_GUIDE` | Yes |
| 11 | Frontend Testing | `/phase11-frontend-testing` | QA Engineer | `TESTING_CONVENTIONS` | Yes |
| 12 | E2E Testing | `/phase12-e2e` | QA Engineer | `E2E_PATTERNS` | No |
| 13 | Code Review | `/phase13-review` | Software Architect | `REVIEW_CHECKLIST` | No |
| 14 | Documentation | `/phase14-docs` | Technical Writer | `DOC_TEMPLATES` | No |
| 15 | Deployment | `/phase15-deployment` | DevOps Engineer | `INFRA_STANDARD` | No |

**Per-module phases** take a module or page name as argument: `/phase4-backend AUTH`

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

| When | Why |
|------|-----|
| After first backend module | Catch pattern-level issues before generating more modules |
| After backend track (Phases 4-6) | Cross-module consistency, migration correctness |
| After first frontend page | Catch UI pattern issues before generating more pages |
| After frontend track (Phases 9-11) | Cross-page consistency, API contract alignment |
| After E2E (Phase 12) | Final security + performance sweep |

---

## Skills

Skills are reusable reference documents encoding your conventions. They survive across projects and improve over time.

| Skill | Status | Used In |
|-------|--------|---------|
| `BRD_FORMAT.md` | ✅ Ready | Phase 1 |
| `MODULE_TEMPLATE.md` | ✅ Ready | Phase 4 |
| `API_STANDARD.md` | ✅ Ready | Phase 9 |
| `ARCHITECTURE_STANDARD.md` | Created by Phase 3 | Phase 3, 4, 13 |
| `TESTING_CONVENTIONS.md` | Created by Phase 5 | Phase 5, 11 |
| `STYLE_GUIDE.md` | Created per-project by Phase 8 | Phase 10 |
| `MIGRATION_TEMPLATE.md` | Add when ready | Phase 6 |
| `E2E_PATTERNS.md` | Add when ready | Phase 12 |
| `REVIEW_CHECKLIST.md` | Add when ready | Phase 13 |
| `DOC_TEMPLATES.md` | Add when ready | Phase 14 |
| `INFRA_STANDARD.md` | Add when ready | Phase 15 |

Skills you don't have yet won't block you — the phase commands handle missing skills gracefully. After your first project, extract patterns from what worked into new skill docs.

---

## Extending

**Add a skill:** Create a `.md` file in `skills/` (Purpose → Rules → Templates → Examples → Anti-patterns). The relevant phase command will pick it up automatically.

**Add a persona:** Create a `.md` file in `personas/` (Identity → Perspective → Priorities → What You Produce → What You Do Not Do).

**Refine over time:** When you correct the AI's output, note what you changed. If the same correction happens twice, add a rule to the relevant skill doc.
