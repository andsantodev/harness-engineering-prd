# Changelog

## v7 (current)

Changes since v5 (intermediate v6 notes aren't tracked):

- **Interview order reversed.** The adaptive 5-pillar sabatina now runs first (Part 1); the fixed checklist moved to a closing pass (Part 2) that only asks what the sabatina didn't already surface. Previously the fixed checklist ran first.
- The sabatina's rigorous, stress-testing posture is now explicit and holds for the whole session, not just the early rounds.
- `AGENTS.md` generation adds a **Row Level Security (RLS) guardrail** for Supabase (or other RLS-capable) backends.
- `feature_list.json` guidance adds two rules: `verification_command` must be a self-terminating pass/fail command (no long-running dev servers or watch mode), and cross-cutting "transversal" features (design system, shared layout, shared API contracts) should be positioned early in the array, right after the walking skeleton.
- PRD template section "Usuários e personas" renamed to "Usuários e jornada", now expected to include the primary user flow pulled from the sabatina.
- Terminology cleanup: "IDE" → "coding agent" throughout, for consistency.
- Removed the "planned SDD support" note that used to sit at the end of `SKILL.md`.

## v5

- Added the adaptive 5-pillar interrogation (Interview Part B) on top of the original fixed-checklist block, to stress-test the project idea before generating the PRD.

## Earlier iterations

Exact version-by-version notes for v1–v4 aren't tracked yet. What's confirmed from real-world testing so far:

- Introduced a **walking-skeleton first feature** rule, so the first feature generated always wires up a real end-to-end (even if empty) flow instead of leaving the agent to start from a blank screen.
- Discovered and documented a **conflict between the Harness flow and always-on global coding-agent plugins** — plugins that inject themselves before `AGENTS.md` is read can cause unexpected extra questions, stray files, or context bloat. This led to the standing warning now included at the end of every generated Harness package.
- Integrated with the official `frontend-design` skill and with Supabase via MCP.

If you want a fuller v1–v4 history reconstructed, it can be added later.
