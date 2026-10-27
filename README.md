# Harness Engineering PRD

A Claude Skill that turns a software project description into a full **PRD** (Product Requirements Document) and a complete **"Harness"** document pack — a root `AGENTS.md` plus `docs/` (`PRD.md`, `feature_list.json`, `progress.md`, `session-handoff.md`, verification notes, and stack-specific engineering rules) — the instruction and state scaffolding an AI coding agent (Claude Code, OpenCode, Cursor, and others) needs to work reliably across multiple sessions without losing context. The primary focus is React/Next.js, but the skill stays stack-agnostic.

Based on the patterns from the *Learn Harness Engineering* course ([walkinglabs/learn-harness-engineering](https://github.com/walkinglabs/learn-harness-engineering)). The alignment interview leads with an adaptive 5-pillar interrogation that stress-tests the idea, then closes with a fixed block of direct questions (stack, tooling, design) covering whatever wasn't already surfaced. Technical-choice questions come with a recommended answer; product and business questions don't.

🇧🇷 Prefer Portuguese? See [README.pt-BR.md](README.pt-BR.md).

---

## How it works

Three steps, always in this order. No document is generated before the previous gate is passed.

1. **Interview** — an adaptive 5-pillar interrogation that stress-tests the idea first, then a fixed checklist that closes out anything not already covered.
2. **PRD** — generated, presented, and held for your explicit approval. Nothing else is generated until you approve it.
3. **Harness package** — only after approval: root `AGENTS.md`, `docs/PRD.md`, `docs/feature_list.json`, `docs/progress.md`, `docs/session-handoff.md`, `docs/verification-notes.md`, root `init.sh`, optional `docs/engineering-rules.md` for the detected stack, plus a native adapter file for whichever coding agent(s) you're using (`CLAUDE.md` for Claude Code, `.cursor/rules/*.mdc` for Cursor, native support for OpenCode).

**Already have a PRD?** The skill has a second entry mode: it reads your PRD without rewriting it, compares it with the template and the fixed interview block, asks only what's missing for the Harness, records gaps as `[a definir]`, and warns when the requirements aren't verifiable. The PRD approval gate and the Harness stage stay the same; the 5-pillar interview is what gets replaced. If you already have both a PRD and a Harness, the skill is not the right tool.

**What the Harness includes:**
- A short **Definition of Done** with automatic checks (typecheck, lint, tests; production build at the end) and judgment calls (no new dependency without approval, no dead code, simplest solution).
- A **folder structure** heuristic: by responsibility up to ~15 screens, by feature above that.
- A final **release inspection** feature, always last in `docs/feature_list.json`: production build, no secrets in code or public env vars, `.env.example`, 404 route, basic accessibility, dependency audit, and a security review.
- A short **progress log**: one entry per session (~10 lines), with detailed evidence in `feature_list.json` and commit messages; older sessions move to `docs/progress-archive.md` after ~300 lines.
- **Stack rules** loaded only when they match: Next.js, React + Vite, Supabase Auth and n8n. Details go to `docs/engineering-rules.md`; the generated `AGENTS.md` only points to it.
- A **manual setup step** (Passo 0) documented for Next.js and Vite, including the current `create-next-app` flags (npm pinned with `--use-npm`) and the Vitest + React Testing Library setup that F001 owns. The generated `init.sh` is a bootstrap health check (install, lint, typecheck, smoke build) and skips tests until F001 creates the runner.

This skill covers **discovery and documentation only** — it doesn't write application code or run scripts. It prepares the ground for a coding agent (or a harness-creator flow) to take over.

## Generated layout

Everything that is a document lives under `docs/`; only the canonical instruction file (and agent-native files) stay at the root.

```text
project/
├── AGENTS.md                  # canonical, short
├── init.sh                    # generated from docs/verification-notes.md
├── docs/
│   ├── PRD.md
│   ├── feature_list.json
│   ├── progress.md
│   ├── session-handoff.md
│   ├── verification-notes.md
│   └── engineering-rules.md   # only when a stack rule matches
├── CLAUDE.md                  # Claude Code only (imports AGENTS.md)
└── .cursor/rules/harness.mdc  # Cursor only
```

## Repository layout

```text
SKILL.md
references/
├── interview-checklist.md
├── prd-template.md
├── existing-prd.md
├── ide-adapters/              # claude-code, cursor, opencode
└── harness-pack/
    ├── agents-md.md
    ├── feature-list-json.md
    ├── progress-md.md
    ├── session-handoff-md.md
    ├── verification-notes.md
    └── stack-rules/           # nextjs, react-vite, supabase-auth, n8n
```

## Installation

Two ways to install: the `skills` CLI (fastest — works for Claude Code, Cursor, OpenCode, Codex and more), or a manual copy of the raw folder. Both read straight from this GitHub repo — no separate hosting needed.

### Option A — CLI

```bash
npx skills add andsantodev/harness-engineering-prd
```

This pulls `SKILL.md` + `references/` from this repo and installs them into your agent's skills folder automatically. To target a single agent:

```bash
npx skills add andsantodev/harness-engineering-prd -a claude-code
```

Add `-g` to install globally instead of just the current project. Full docs: [skills.sh/docs/cli](https://skills.sh/docs/cli).

### Option B — Manual

This repo holds the **raw skill folder** (`SKILL.md` + `references/`) — not a zipped `.skill` file. Copy it into the right place for your tool.

**claude.ai**
Settings → Capabilities → Skills → upload the `harness-engineering-prd` folder (or a `.skill` zip of it).

**Claude Code**
Skills follow the open "Agent Skills" standard. Copy the folder into:
- `.claude/skills/harness-engineering-prd/` (project-scoped), or
- `~/.claude/skills/harness-engineering-prd/` (available in every project)

**OpenCode / Cursor**
Both read the same `SKILL.md` format. Copy the folder into your tool's skills directory — check your tool's current docs for the exact path, since this is evolving fast across agents.

```bash
git clone https://github.com/andsantodev/harness-engineering-prd.git
cp -r harness-engineering-prd/SKILL.md harness-engineering-prd/references \
      <your-agent-skills-folder>/harness-engineering-prd/
```

## Before the first command

The project must exist before you paste any prompt below:

1. Create the project with the command registered in the verification notes for your stack (Passo 0) — the skill documents it, you run it.
2. Copy `AGENTS.md`, `init.sh` and `docs/` into the new project root (`create-next-app` refuses a folder with pre-existing files, so copy after creation).
3. Run `init.sh`.
4. Open your coding agent in the project folder.

See the Passo 0 item under **What the Harness includes** above. The exact stack commands live in the Passo 0 section of `docs/verification-notes.md` (generated by the skill; source: `references/harness-pack/verification-notes.md`) and are not duplicated here. If the folder already contains the harness, that section also covers the temporary-folder merge.

## Quick-start prompts

Use these once the skill is installed and you're working inside a project.

<details>
<summary><strong>1. Start a new project</strong></summary>

```markdown
Read AGENTS.md, docs/PRD.md, docs/session-handoff.md and docs/feature_list.json before doing anything else.

Start with feature **[ID of the first eligible feature — usually the one with no depends_on]** — it has no dependencies and the others (or most of them) depend on it:

1. Implement the necessary code.
2. **PER PROJECT: if the project uses its own design skill (e.g. frontend-design), add here "use the [name] skill for layout/visual decisions"**
3. Run the feature's `verification_command` (field in docs/feature_list.json) and show me the full output.
4. Run `git diff --stat`, compare it with the feature's acceptance criteria, and justify in docs/progress.md any file outside the scope.
5. If verification passes, update:
   - docs/feature_list.json (set status to "done" and insert the evidence)
   - docs/progress.md (log what was done)
   - docs/session-handoff.md (update current state and next task)

Do not move to any other feature without my approval.
If the feature is the release inspection, it requires my explicit approval before you start.
```
</details>

<details>
<summary><strong>2. Next feature</strong></summary>

```markdown
Check AGENTS.md and docs/session-handoff.md.

Select the NEXT feature with status "todo" in docs/feature_list.json whose dependencies are complete (respecting the depends_on field).

Work EXCLUSIVELY on that feature:
1. Build the code following the acceptance criteria defined in the JSON.
2. Run the feature's `verification_command` (field in docs/feature_list.json) and show me the full output.
3. Run `git diff --stat`, compare it with the feature's acceptance criteria, and justify in docs/progress.md any file outside the scope.
4. If everything passes, update docs/feature_list.json, docs/progress.md and docs/session-handoff.md.
If the feature is the release inspection, it requires my explicit approval before you start.
```
</details>

<details>
<summary><strong>3. Autonomous loop mode</strong></summary>

```markdown
Work through the next eligible features in docs/feature_list.json in sequence, following AGENTS.md (one feature at a time, verification before marking complete).

For EACH feature:
1. Identify the next "todo" feature with completed dependencies.
2. Build the necessary code.
3. Strictly run the feature's `verification_command` (field in docs/feature_list.json) and show the full output.
4. Run `git diff --stat`, compare it with the feature's acceptance criteria, and justify in docs/progress.md any file outside the scope.

STOP CONDITIONS AND SAFETY RULES:
- 🔴 IF TESTS FAIL OR A BUILD/LINT ERROR OCCURS: STOP THE LOOP IMMEDIATELY. Do not mark the feature "done". Explain the error in chat and wait for my input.
- 🟢 IF TESTS PASS: Update docs/feature_list.json (status "done" + evidence), docs/progress.md and docs/session-handoff.md — ALWAYS all three together, after every completed feature (don't leave docs/session-handoff.md for a final pause only — if the loop stops mid-way, it must reflect the last feature actually completed).
- 🟠 IF THE NEXT ELIGIBLE FEATURE IS THE RELEASE INSPECTION: STOP THE LOOP BEFORE RUNNING IT. Tell me in chat that the next step is the final inspection and wait for my explicit approval.
- 🟡 IF THERE IS NO "todo" FEATURE WITH SATISFIED DEPENDENCIES (everything "done" or "blocked"): STOP THE LOOP. This is also a stop condition — not a failure, just no safe work left. Explain the state in chat.
- Otherwise (outside the two cases above), move AUTOMATICALLY to the next pending feature without asking me.

Safety limit: process at most 4 features in this round, then pause for review. You can start!
```
</details>

<details>
<summary><strong>4. Fixing bugs or failing tests</strong></summary>

```markdown
Feature **[EVERY TIME: feature ID]** failed verification with this result:

**[EVERY TIME: paste the verification command output / error / failing test here]**

Do not mark the feature as complete. Investigate the root cause, fix it, re-run the feature's `verification_command` (field in docs/feature_list.json) from scratch (don't assume it now passes), and only then update docs/feature_list.json with the real status and evidence.
Log what broke and how it was fixed in docs/progress.md.
```
</details>

<details>
<summary><strong>5. Ending / handing off a session</strong></summary>

```markdown
Let's wrap up this development session. Please:
1. Run the general verification commands listed in the Definition of Done of AGENTS.md (typically typecheck, lint and build).
2. Make sure the repo is in a clean state with no build errors.
3. Update docs/session-handoff.md describing exactly what was done this session, which files were changed, and the exact instruction to resume at F00X.
4. Update docs/progress.md with the latest progress.
5. Only commit if the project is in a safe state to resume later — if not, tell me before committing.
```
</details>

<details>
<summary><strong>6. Resuming a new session</strong></summary>

```markdown
Carefully read AGENTS.md, docs/progress.md, and especially docs/session-handoff.md to understand the current state of the project and where the previous session left off.

Before writing or changing any code:
1. Run the initial verification commands to confirm the repo is healthy and compiles.
2. Identify in docs/session-handoff.md and docs/feature_list.json which pending feature ("todo") has its dependencies already completed.
3. Briefly summarize which feature you'll work on this session and wait for my confirmation before starting.

Remember: work on ONLY ONE feature at a time (WIP = 1), and only update the documentation and status to "done" after running and passing verification tests.
```
</details>

## Example: what gets generated

Illustrative excerpt only (constructed for this README, not pulled from a real test run) — a small slice of `docs/feature_list.json` and `AGENTS.md` to show the shape of the output:

```json
// docs/feature_list.json (excerpt)
{
  "id": "F001",
  "title": "Walking skeleton: empty state end-to-end",
  "status": "todo",
  "depends_on": [],
  "acceptance_criteria": [
    "Home route renders with real data source wired up (even if empty)",
    "npm run test:unit passes"
  ]
}
```

```markdown
<!-- AGENTS.md (excerpt) -->
## Invariants
- One active feature at a time (WIP = 1).
- Never mark a feature "done" without verification-command evidence.
- Project-specific facts (stack, conventions) live in project docs, not in this skill.
```

## License

[MIT](LICENSE).

