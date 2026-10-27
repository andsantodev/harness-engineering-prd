# Changelog

## v8 (current)

- **Generated layout moved to `docs/`.** PRD, `feature_list.json`, `progress.md`, `session-handoff.md`, verification notes and engineering rules now live under `docs/`; `AGENTS.md` stays at the root. `init.sh` is a script, not a document, so it stays at the root.
- **New entry mode: existing PRD.** Reads the user's PRD without rewriting it, compares it with the template and the fixed interview block, asks only what's missing for the Harness, records gaps as `[a definir]`, and warns when the requirements aren't verifiable. The 5-pillar interview is replaced; the PRD approval gate and the Harness stage remain. Trigger updated; still excluded when both PRD and Harness already exist.
- **Definition of Done** in the generated `AGENTS.md`: automatic checks (typecheck, lint, tests; production build at the end) plus judgment calls (no new dependency without approval, no dead code, simplest solution). Before marking a feature done: `git diff --stat` against the acceptance criteria, with out-of-scope files justified in `progress.md`.
- **Verification that can actually fail:** `tsc --noEmit` doesn't ban explicit `any`, React hooks rules are warnings by default (use `--max-warnings 0`), and `no-console` catches debug logs.
- **Progress log stays short:** one entry per session (~10 lines), no code, tables or full logs; detailed evidence lives in `verification_evidence` and commit messages, and older sessions move to `docs/progress-archive.md` once the file passes ~300 lines.
- **Folder structure heuristic** in the generated `AGENTS.md` (by responsibility up to ~15 screens, by feature above that). F001 now owns the structure, the 404 route and the Vitest + React Testing Library setup.
- **`feature_list.json` rules:** async features carry explicit loading/success/error/empty criteria; criteria must be specific and only cite paths that follow the folder convention; a final **release inspection** feature is always last and depends on all others (full and reduced versions).
- **Conditional stack rules** in `references/harness-pack/stack-rules/` (Next.js, React + Vite, Supabase Auth, n8n), generated into `docs/engineering-rules.md` only when the stack matches. Supabase guidance follows the current docs: `getClaims()` is preferred over `getUser()` on the server, and `getSession()` is treated as unverified data.
- **Interview:** recommended answers only for technical-choice questions; product and business questions stay open. New fixed-block questions for styling, Next.js Cache Components, authentication (only when the project has it) and `code-review-lite`.
- **Passo 0** documents manual project creation for Next.js and Vite, with `create-next-app` 16.4 flags (`--no-agents-md`, `--no-cache-components`, `--use-npm`, `--yes`), the `agentRules: false` adjustment for Next >= 16.3, and the `node_modules/next/dist/docs/` pointer. npm is the default; pnpm/yarn/bun remain supported when chosen consistently. It also covers the "harness already exists" case with a temporary-folder merge.
- **Bootstrap corrected:** `init.sh` is now a bootstrap health check, not the Definition of Done. It runs `cd "$(dirname "$0")"`, install without `--frozen-lockfile`, lint, typecheck and a smoke build; it skips tests with a message until F001 creates the runner. Next 16 type-check runs `next typegen && tsc --noEmit` so a clean project doesn't fail on `LayoutProps`/`PageProps`. The generated `AGENTS.md` marks test scripts as available only from F001, and F001 carries the "init.sh passes from root and docs/" acceptance criterion.
- **n8n evidence** goes to `verification_evidence` in `docs/feature_list.json`; `docs/progress.md` keeps only a one-line summary.
- **Test anti-patterns** added to `engineering-rules.md`: tests that can't fail and tests coupled to implementation.
- Removed the Gemini Gem reference; README no longer implies Gemini compatibility. README EN/PT updated for the new layout, entry modes and stack rules.

## v7

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
