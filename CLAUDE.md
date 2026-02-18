# AI-Assisted Fullstack Development Workflow

This project uses a 15-phase AI-assisted development workflow. The full playbook is in `AI-Assisted Fullstack Development Workflow.md`.

## How to Use

Each phase has a slash command: `/phase1-brd`, `/phase4-backend`, `/phase10-pages`, etc.
Run them in order. Pass the module or page name as an argument when needed.

**Examples:**
- `/phase1-brd` — generate the BRD from your app concept
- `/phase4-backend AUTH` — generate the backend module for AUTH
- `/phase10-pages DashboardPage` — generate the dashboard page

## Project Layout

```
personas/          — AI persona files (loaded automatically by phase commands)
skills/            — Skill docs: MODULE_TEMPLATE.md, API_STANDARD.md, BRD_FORMAT.md
docs/              — Project artifacts (BRD, architecture, designs) — created as you go
```

## Workflow Dependency Map

```
Phase 1 — BRD ✅ VERIFY
  └── Phase 2 — Planning
        └── Phase 3 — Architecture
              ├── Phase 4 → 5 → 6  (Backend track)
              └── Phase 7 → 8      (Design track)
                    ↘
              Phase 9 → 10 → 11 → 12 → 13 → 14 → 15  (Frontend + Finalization)
```

After Phase 3, the backend track (4→5→6) and design track (7→8) can run in parallel.
The frontend track (9+) starts once both are complete.

## Skills (Reference Docs)

| Skill | File | Used In |
|-------|------|---------|
| BRD Format | `skills/BRD_FORMAT.md` | Phase 1 |
| Module Template | `skills/MODULE_TEMPLATE.md` | Phase 4 |
| API Standard | `skills/API_STANDARD.md` | Phase 9 |
| Architecture Standard | `skills/ARCHITECTURE_STANDARD.md` | Phase 3, 4, 13 |
| Testing Conventions | `skills/TESTING_CONVENTIONS.md` | Phase 5, 11 |
| Style Guide | `skills/STYLE_GUIDE.md` | Phase 10 (per-project, created in Phase 8) |

Skills marked as pending don't exist yet — they'll be created as you run through projects and refine your conventions.

## Core Principles

1. **BRD is the anchor** — every phase references it
2. **One phase at a time** — review each output before moving on
3. **Per-module iteration** — for phases 4, 5, 9, 10, 11, run one module/page at a time
4. **Independent modules first** — start with models that have no FK dependencies
5. **Rolling code reviews** — run Phase 13 after the first backend module and first frontend page, not just at the end
