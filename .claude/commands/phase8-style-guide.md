Adopt the persona defined in `personas/ui-designer.md`. Read it now before proceeding.

Read these context files before proceeding:
- BRD: `docs/brd.md`
- UI Design: `docs/ui-design.md` — wireframes and mockups from Phase 7
- If the user provides screenshots of mock screens, analyze those for concrete visual values

Create a concrete style guide that an AI can follow consistently during code generation. The project uses Tailwind CSS and shadcn/ui. Every rule must include **exact values** — no vague descriptions:

- Color palette (exact hex values for primary, secondary, semantic colors, mapped to Tailwind classes)
- Typography scale (font families, exact sizes, weights, line heights)
- Spacing system (base unit, exact scale values)
- Component styling rules (buttons, inputs, cards, modals, tables — with Tailwind classes and shadcn variants)
- Animation/transition standards (exact durations, easing)
- Dark mode rules (if applicable)
- Accessibility requirements (contrast ratios, focus states, ARIA patterns)

Save the output to `skills/STYLE_GUIDE.md` — this becomes a skill document consumed by Phase 10 (page generation).

📋 REVIEW GATE: Is every rule specific enough to produce identical results across independently prompted pages? If a rule says "use a muted color" instead of "use `text-slate-500`", it's not specific enough. This is the most important review gate for frontend consistency.
