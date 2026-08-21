# Harness Engineering PRD

A Claude Skill that turns a software project description into a full **PRD** (Product Requirements Document) and a complete **"Harness"** document pack — `AGENTS.md`, `feature_list.json`, `progress.md`, `session-handoff.md` — the instruction and state scaffolding an AI coding agent (Claude Code, OpenCode, Cursor, and others) needs to work reliably across multiple sessions without losing context.

Based on the patterns from the *Learn Harness Engineering* course ([walkinglabs/learn-harness-engineering](https://github.com/walkinglabs/learn-harness-engineering)). The alignment interview leads with an adaptive 5-pillar interrogation that stress-tests the idea, then closes with a fixed block of direct questions (stack, tooling, design) covering whatever wasn't already surfaced.

🇧🇷 Prefer Portuguese? See [README.pt-BR.md](README.pt-BR.md).

---

## How it works

Three steps, always in this order. No document is generated before the previous gate is passed.

1. **Interview** — an adaptive 5-pillar interrogation that stress-tests the idea first, then a fixed checklist that closes out anything not already covered.
2. **PRD** — generated, presented, and held for your explicit approval. Nothing else is generated until you approve it.
3. **Harness package** — only after approval: `docs/PRD.md`, `AGENTS.md`, `feature_list.json`, `progress.md`, `session-handoff.md`, verification/bootstrap notes, plus a native adapter file for whichever coding agent(s) you're using (`CLAUDE.md` for Claude Code, `.cursor/rules/*.mdc` for Cursor, native support for OpenCode).

This skill covers **discovery and documentation only** — it doesn't write application code or run scripts. It prepares the ground for a coding agent (or a harness-creator flow) to take over.

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

## Quick-start prompts

Use these once the skill is installed and you're working inside a project.

<details>
<summary><strong>1. Start a new project</strong></summary>

```markdown
Read AGENTS.md, docs/PRD.md, session-handoff.md and feature_list.json before doing anything else.

Start with feature **[ID of the first eligible feature — usually the one with no depends_on]** — it has no dependencies and the others (or most of them) depend on it:

1. Implement the necessary code.
2. **PER PROJECT: if the project uses its own design skill (e.g. frontend-design), add here "use the [name] skill for layout/visual decisions"**
3. Run the verification command: npm run test:unit
4. If tests pass, update:
   - feature_list.json (set status to "done" and insert the evidence)
   - progress.md (log what was done)
   - session-handoff.md (update current state and next task)

Do not move to any other feature without my approval.
```
</details>

<details>
<summary><strong>2. Next feature</strong></summary>

```markdown
Check AGENTS.md and session-handoff.md.

Select the NEXT feature with status "todo" in feature_list.json whose dependencies are complete (respecting the depends_on field).

Work EXCLUSIVELY on that feature:
1. Build the code following the acceptance criteria defined in the JSON.
2. Run the verification command configured for this feature (e.g. npm run test:unit or npm run test:e2e).
3. Show me the test results here in the chat.
4. If everything passes, update feature_list.json, progress.md and session-handoff.md.
```
</details>

<details>
<summary><strong>3. Autonomous loop mode</strong></summary>

```markdown
Work through the next eligible features in feature_list.json in sequence, following AGENTS.md (one feature at a time, verification before marking complete).

For EACH feature:
1. Identify the next "todo" feature with completed dependencies.
2. Build the necessary code.
3. Strictly run the corresponding verification command (e.g. npm run test:unit or npm run test:e2e).

STOP CONDITIONS AND SAFETY RULES:
- 🔴 IF TESTS FAIL OR A BUILD/LINT ERROR OCCURS: STOP THE LOOP IMMEDIATELY. Do not mark the feature "done". Explain the error in chat and wait for my input.
- 🟢 IF TESTS PASS: Update feature_list.json (status "done" + evidence), progress.md and session-handoff.md — ALWAYS all three together, after every completed feature (don't leave session-handoff.md for a final pause only — if the loop stops mid-way, it must reflect the last feature actually completed).
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

Do not mark the feature as complete. Investigate the root cause, fix it, re-run verification from scratch (don't assume it now passes), and only then update feature_list.json with the real status and evidence.
Log what broke and how it was fixed in progress.md.
```
</details>

<details>
<summary><strong>5. Ending / handing off a session</strong></summary>

```markdown
Let's wrap up this development session. Please:
1. Run the general verification commands: npm run lint and npm run build.
2. Make sure the repo is in a clean state with no build errors.
3. Update session-handoff.md describing exactly what was done this session, which files were changed, and the exact instruction to resume at F00X.
4. Update progress.md with the latest progress.
5. Only commit if the project is in a safe state to resume later — if not, tell me before committing.
```
</details>

<details>
<summary><strong>6. Resuming a new session</strong></summary>

```markdown
Carefully read AGENTS.md, progress.md, and especially session-handoff.md to understand the current state of the project and where the previous session left off.

Before writing or changing any code:
1. Run the initial verification commands to confirm the repo is healthy and compiles.
2. Identify in session-handoff.md and feature_list.json which pending feature ("todo") has its dependencies already completed.
3. Briefly summarize which feature you'll work on this session and wait for my confirmation before starting.

Remember: work on ONLY ONE feature at a time (WIP = 1), and only update the documentation and status to "done" after running and passing verification tests.
```
</details>

## Example: what gets generated

Illustrative excerpt only (constructed for this README, not pulled from a real test run) — a small slice of `feature_list.json` and `AGENTS.md` to show the shape of the output:

```json
// feature_list.json (excerpt)
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

