# AI-Assisted Fullstack Development Workflow

This project uses a 14-phase AI-assisted development workflow. The full playbook is in `AI-Assisted Fullstack Development Workflow.md`.

## How to Use

Each phase has a slash command: `/phase1-brd`, `/phase4-backend`, `/phase9-pages`, etc.
Run them in order. Pass the module or page name as an argument when needed.

**Examples:**
- `/phase1-brd` — generate the BRD from your app concept
- `/phase4-backend AUTH` — generate the backend module for AUTH
- `/phase9-pages DashboardPage` — generate the dashboard page

## Project Layout

```
personas/          — AI persona files (loaded automatically by phase commands)
skills/            — Skill docs: MODULE_TEMPLATE.md, API_STANDARD.md, BRD_FORMAT.md
docs/              — Project artifacts (BRD, architecture, designs, progress) — created as you go
```

## Workflow Dependency Map

```
Phase 1 — BRD ✅ VERIFY
  └── Phase 2 — Planning
        └── Phase 3 — Architecture
              ├── Phase 4 → 5 → 6  (Backend track)
              └── Phase 7           (Design track — UI design + style guide)
                    ↘
              Phase 8 → 9 → 10 → 11 → 12 → 13 → 14  (Frontend + Finalization)
```

After Phase 3, the backend track (4→5→6) and design track (7) can run in parallel.
The frontend track (8+) starts once both are complete.

## Skills (Reference Docs)

| Skill | File | Used In |
|-------|------|---------|
| BRD Format | `skills/BRD_FORMAT.md` | Phase 1 |
| Module Template | `skills/MODULE_TEMPLATE.md` | Phase 4 |
| API Standard | `skills/API_STANDARD.md` | Phase 8 |
| Architecture Standard | `skills/ARCHITECTURE_STANDARD.md` | Phase 3, 4, 12 |
| Testing Conventions | `skills/TESTING_CONVENTIONS.md` | Phase 5, 10 |
| Style Guide | embedded in `docs/ui-design.md` | Phase 9 (per-project, created in Phase 7) |

Skills marked as pending don't exist yet — they'll be created as you run through projects and refine your conventions.

## Progress Tracking

Each phase appends a row to `docs/progress.md` when it completes. The file is created automatically on first use.

Format: `| Phase | Name | Scope | Status | Date | Notes |`

- Single-run phases (1, 2, 3, 6, 7, 11, 13, 14): one row per run, scope `—`
- Per-module/page phases (4, 5, 8, 9, 10): one row per module/page
- Phase 12 (Review): one row per review checkpoint

## Core Principles

1. **BRD is the anchor** — every phase references it
2. **One phase at a time** — review each output before moving on
3. **Per-module iteration** — for phases 4, 5, 8, 9, 10, run one module/page at a time
4. **Independent modules first** — start with models that have no FK dependencies
5. **Rolling code reviews** — run Phase 12 after the first backend module and first frontend page, not just at the end
