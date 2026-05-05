# [AppName] — Claude Context

> **Before your first session:** Fill in everything marked `[BRACKETS]`. The three things you must do first:
> 1. Replace `[AppName]` throughout this file
> 2. Fill in `project-management/CANONICAL.md` — your product truth
> 3. Fill in `project-management/ARCHITECTURE.md` — your stack

---

## Product Truth

**These two files are the foundation. Nothing else here works without them.**

- **What [AppName] IS** (positioning, language, who it's for, what changes for them): `project-management/CANONICAL.md`
- **How [AppName] works** (stack, architecture, layers, data model): `project-management/ARCHITECTURE.md`

Every user-facing string, feature decision, and build derives from these docs. When a feature conflicts with the canonical, update the canonical first — then build. Never silent drift.

---

## Build Discipline

Every change lands inside a working product. Two templates make this safe:

| Doc | What it is | When to use |
|---|---|---|
| `docs/pre-build-lock.md` | Scope lock per feature. IN list, OUT list, blast radius, kill date. | Before any code is written |
| `docs/runbook-template.md` | Task spec for Cursor. FILES · TYPES · SKELETON · PROHIBITED · VALIDATE. | One per Cursor task |

**The single invariant every build tests against:** `V(N+1) = V(N) + exactly one surgical change`. If the build can't be expressed that way, split it into multiple builds.

**The three foundational patterns** — reason from these, don't just follow them:

- **Parallel Change** — add new alongside old, migrate traffic, delete old. Never flip everyone at once. ([reference](https://martinfowler.com/bliki/ParallelChange.html))
- **Strangler Fig** — wrap the old thing, route one piece at a time to new, let old hollow out. Never rewrite everything. ([reference](https://martinfowler.com/bliki/StranglerFigApplication.html))
- **Canary Release** — deploy to one before deploying to all. Small controlled exposure catches problems before full rollout.

**N-1 rule:** Production runs version N. Version N-1 must stay bootable. N-2 and older get deleted. Every feature that introduces temporary code (flags, shims, fallbacks) ships with a sunset criterion and a hard kill date — written in the pre-build lock before code is written.

---

## Claude Code vs Cursor

**Claude Code** — whole-codebase reasoning, planning, runbook authorship. Reads all the context. Writes the task. Reviews the output. Diagnoses drift.

**Cursor** — execution against runbooks. One task at a time. Never plans. Only touches files in the FILES block of the current runbook.

**The handoff loop:**
1. Claude Code reads the pre-build lock and writes a runbook for task N
2. Cursor executes task N
3. You review the output (FILES check + VALIDATE check)
4. Claude Code diagnoses any drift before task N+1 starts

Never ask Cursor to plan. Never ask Claude Code to execute without a runbook. Drift caught at task N is a one-line fix. Drift caught at task N+4 is a rewrite.

---

## Feature Build Gate

Before any feature touches production code, in this order:

1. **Lock** — copy `docs/pre-build-lock.md` into `feature-builds/[feature]/PRE-BUILD-LOCK.md` and fill every field. A blank or TBD field blocks the build.
2. **Mocks** — approve the output shape before Cursor opens a file. HTML mock or sketch is fine. The point is: you've seen what "done" looks like before execution starts.
3. **Runbook** — one runbook per Cursor task, five blocks, no exceptions.
4. **Review** — after every task, before the next one.

---

## Testing Philosophy (90/10)

Test shared code. Skip cosmetics.

**Always test:**
- Shared utilities used by multiple routes or services
- Data contracts across language or service boundaries
- Integration points between external systems

**Never test:**
- Single-route UI tweaks
- CSS or copy changes
- Isolated one-off logic with no downstream consumers

**Golden fixture rule:** Use real output from a real run as your test fixture. Never invent fixtures — invented fixtures test your assumptions, not your code. When a golden fixture goes stale, update it from a real run.

Run `[YOUR TEST COMMAND]` after any change to shared code. Log new failures in `project-management/KNOWN-ISSUES.md` immediately — don't let them accumulate silently.

---

## Model Usage

Default to Sonnet. Ask to switch to Opus when:

- Filling in pre-build lock gates or canonical alignment decisions
- Architecture calls — anything that changes how layers connect
- Prompt engineering — AI system prompt edits, voice/posture decisions
- The wrong answer has real blast radius (schema changes, multi-layer features, rollout decisions)

Say explicitly: "This needs Opus — run `/model opus` and I'll continue." Don't silently proceed on a complex decision.

---

## Session Start — Every Session

1. Check git: `git branch --show-current`, `git log --oneline -10`, `git status`
2. Flag unmerged remote branches: `git branch -r --no-merged main`
3. Read `project-management/_log.md` (last 80 lines only)
4. Read `project-management/ROADMAP.md`
5. Read `project-management/KNOWN-ISSUES.md`

**Report:** current branch, last few commits, anything in-flight, any unmerged branches. If any unmerged branches exist, flag them — don't auto-clean.

Do not start work until this is done. The log is the primary context-transfer mechanism between sessions. Treat it seriously.

---

## Session End — Every Session

1. Update `project-management/_log.md` — what happened, decisions made
2. If mid-flight, add a **RESUME HERE** block with numbered next steps
3. Update `project-management/ROADMAP.md` — check off completed, reorder, add new
4. Update `project-management/KNOWN-ISSUES.md` if bugs were found or fixed
5. Record any permanent decisions in `project-management/STACK.md`
6. Git status report: branch, uncommitted work, stashes, unmerged branches

John does not use a PM tool. These files ARE the project management system. If state isn't written to disk, it's lost.

> **Rename "John" above to your name, or remove the line.**

---

## Deployment

When deploying, follow `docs/deploy-checklist.md` step by step. No skipped steps.

Key rules:
- Migrations run **before** new code deploys, never after
- Feature flags are the rollback mechanism — no redeploy needed to roll back a flagged feature
- Canary first: enable for one user or one test account, walk the flow manually, then expand

---

## Reference — Read on Trigger

Don't load these at session start. Read them when the task requires it.

| Doc | Read when... |
|---|---|
| `project-management/CANONICAL.md` | Before any feature touching user-facing language, positioning, or product behavior |
| `project-management/ARCHITECTURE.md` | Before any deploy, or any change to how layers connect |
| `project-management/STACK.md` | Before any permanent or architecture decision |
| `docs/pre-build-lock.md` | Before writing any code for a feature |
| `docs/runbook-template.md` | When writing a Cursor task |
| `docs/deploy-checklist.md` | When deploying |
| `docs/mvb-scope.md` | When scoping the first build on this project |
