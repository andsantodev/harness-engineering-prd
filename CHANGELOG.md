# Changelog

## v5 (current)

- Added the adaptive 5-pillar interrogation (Interview Part B) on top of the original fixed-checklist block, to stress-test the project idea before generating the PRD.
- ⚠️ Not yet validated across real sessions — see the Validation status table in the README.

## Earlier iterations

Exact version-by-version notes for v1–v4 aren't tracked yet. What's confirmed from real-world testing so far:

- Introduced a **walking-skeleton first feature** rule, so the first feature generated always wires up a real end-to-end (even if empty) flow instead of leaving the agent to start from a blank screen.
- Discovered and documented a **conflict between the Harness flow and always-on global coding-agent plugins** — plugins that inject themselves before `AGENTS.md` is read can cause unexpected extra questions, stray files, or context bloat. This led to the standing warning now included at the end of every generated Harness package.
- Integrated with the official `frontend-design` skill and with Supabase via MCP.

If you want a fuller v1–v4 history reconstructed, it can be added later.
