# [AppName] — Claude Context

> **Before your first session:** Fill in everything marked `[BRACKETS]`. The three things you must do first:
> 1. Replace `[AppName]` throughout this file
> 2. Fill in `marketing/canonical/MARKETING-TRUTH.md` and `marketing/canonical/BEHAVIOR-SPEC.md` — your product truth
> 3. Fill in `project-management/ARCHITECTURE.md` — your stack

---

## Product Truth

**These two files are the foundation. Nothing else here works without them.**

- **What [AppName] IS** (positioning, language, who it's for, what changes for them): `marketing/canonical/MARKETING-TRUTH.md` + `marketing/canonical/BEHAVIOR-SPEC.md`
- **How [AppName] works** (stack, architecture, layers, data model): `project-management/ARCHITECTURE.md`

Every user-facing string, feature decision, and build derives from these docs. When a feature conflicts with the canonical, update the canonical first — then build. Never silent drift.

---

## Build Discipline

Every change lands inside a working product. Two templates make this safe:

| Doc | What it is | When to use |
|---|---|---|
| `project-management/FEATURE-LOCK.md` | Scope lock per feature. IN list, OUT list, blast radius, kill date. | Before any code is written |
| `feature-builds/_playbook/BUILD-RULES.md` | Task spec for Cursor. FILES · TYPES · SKELETON · PROHIBITED · VALIDATE. | One per Cursor task |

**The single invariant every build tests against:** `V(N+1) = V(N) + exactly one surgical change`. If the build can't be expressed that way, split it.

**The three foundational patterns** — reason from these, don't just follow them:

- **Parallel Change** — add new alongside old, migrate traffic, delete old. Never flip everyone at once. ([reference](https://martinfowler.com/bliki/ParallelChange.html))
- **Strangler Fig** — wrap the old thing, route one piece at a time to new, let old hollow out. Never rewrite everything. ([reference](https://martinfowler.com/bliki/StranglerFigApplication.html))
- **Canary Release** — deploy to one before deploying to all. Small controlled exposure catches problems before full rollout.

**N-1 rule:** Production runs version N. Version N-1 must stay bootable. N-2 and older get deleted. Every feature that introduces temporary code ships with a sunset criterion and a hard kill date — written in the pre-build lock before code is written.

---

## How the disciplines compose

Different task types don't need the same process weight. Apply this table before starting any session:

| Task type | Pre-build lock | Runbook | 90/10 testing | Deploy checklist |
|---|---|---|---|---|
| Feature (new surface, new data, new flow) | Required | Required | Required | Required |
| Bug fix, null guard, error handling | Skip | Skip | Judgment | Required |
| Copy change, CSS tweak, config update | Skip | Skip | Skip | Required |
| Schema migration | Required | Required | Required | Required |
| Infrastructure / DevOps | Skip | Skip | Judgment | Required |

When in doubt: a 10-second question is cheaper than unwinding a week of drift.

---

## Claude Code vs Cursor — Tool Boundary

**Claude Code** — whole-codebase reasoning, planning, runbook authorship. Reads all the context. Writes the task. Reviews the output. Diagnoses drift.

**Cursor** — execution against runbooks. One task at a time. Never plans. Only touches files in the FILES block of the current runbook.

**The handoff loop:**
1. Claude Code reads the pre-build lock and writes a runbook for task N
2. Cursor executes task N
3. You review the output (FILES check + VALIDATE check)
4. Claude Code diagnoses any drift before task N+1 starts

Never ask Cursor to plan. Never ask Claude Code to execute without a runbook. Drift caught at task N is a one-line fix. Drift caught at task N+4 is a rewrite.

**Stop and write a runbook first if any of these are true:**

1. **New data entering the system** — a value that isn't stored, computed, or displayed today needs to be for the first time
2. **New UI surface** — a component, page, or section that doesn't exist yet (not restyling something that already renders)
3. **Schema migration required** — any ALTER TABLE or equivalent
4. **Full vertical slice** — the change touches 3+ layers simultaneously (e.g. pipeline + database + UI)
5. **New user-facing flow** — onboarding, settings, billing, or any multi-step interaction

**Claude Code can handle directly:**
- Bug fixes, null guards, error handling
- Copy changes, CSS tweaks, config updates
- Test fixes, doc updates, deploy operations
- Refactoring within a single layer when scope is clear

---

## Feature Build Gate

Before any feature touches production code, in this order:

1. **Lock** — copy `project-management/FEATURE-LOCK.md` into `feature-builds/[feature]/PRE-BUILD-LOCK.md` and fill every gate. A blank or TBD gate blocks the build.
2. **Mocks** — approve the output shape before Cursor opens a file. Full-surround render + side-by-side diff. The point is: you've seen what "done" looks like before execution starts.
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

Run `[YOUR TEST COMMAND]` after any change to shared code. Log new failures in `project-management/KNOWN-ISSUES.md` immediately.

---

## Model Usage

Default to Sonnet. Ask to switch to Opus when:

- Filling in pre-build lock gates or canonical alignment decisions
- Architecture calls — anything that changes how layers connect
- The wrong answer has real blast radius (schema changes, multi-layer features, rollout decisions)
- You're genuinely uncertain whether a plan is correct and the cost of being wrong is high

Say explicitly: "This needs Opus — run `/model opus` and I'll continue." Don't silently proceed on a complex decision.

---

## Session Start — Core read (every session)

Before anything else:

1. `git branch --show-current`, `git log --oneline -10`, `git status`
2. `git stash list` — any stashes sitting around?
3. `git branch -r --no-merged main` — flag any unmerged remote branches
4. Read `project-management/_log.md` (last 80 lines only)
5. Read `project-management/ROADMAP.md`
6. Read `project-management/KNOWN-ISSUES.md`

**Report:** current branch, last few commits, anything in-flight, any unmerged branches, any stashes. Flag unmerged branches — don't auto-clean. This is especially important when switching between Claude Code and Cursor — work may have been committed by either tool.

Do not start work until this is done. The log is the primary context-transfer mechanism between sessions.

**If the session opens with a status check** — anything like "where are we", "catch me up", "what's next", "status":

1. Do the core read above
2. Give a brief spoken summary: what was done last session, what's in progress, what the next concrete step is
3. Call out any **RESUME HERE** markers in `_log.md` — these are explicit pickup points left by the previous session
4. Wait — do not start work until asked

This is a distinct mode. Don't conflate it with a working session.

---

## Session End — Every Session

1. Update `project-management/_log.md` — what happened, decisions made
2. If mid-flight, add a **RESUME HERE** block with numbered next steps
3. Update `project-management/ROADMAP.md` — check off completed, reorder, add new
4. Update `project-management/KNOWN-ISSUES.md` if bugs were found or fixed
5. Record any permanent decisions in `project-management/STACK.md`
6. Git status report: `git branch --show-current`, `git status`, `git stash list`, `git branch -r --no-merged main`, `git log --oneline main..HEAD`

Present as a short summary: "You're on `[branch]`, X commits ahead of main, nothing uncommitted, no stashes, Y unmerged branches." Call out anything that needs attention explicitly.

**[YOUR NAME] does not use a PM tool. These files ARE the project management system. If state isn't written to disk, it's lost.**

> **Fill in your name above, or remove the line.**

---

## Deployment

When deploying, follow `project-management/SHIP-TO-PROD.md` step by step. No skipped steps.

Key rules:
- Migrations run **before** new code deploys, never after
- Feature flags are the rollback mechanism — no redeploy needed to roll back a flagged feature
- Canary first: enable for one user or one account, walk the flow manually, then expand

---

## Reference — Read on Trigger

Don't load these at session start. Read them when the task requires it.

| Doc | Read when... |
|---|---|
| `marketing/canonical/MARKETING-TRUTH.md` | Before any feature touching user-facing language, positioning, or product behavior |
| `marketing/canonical/BEHAVIOR-SPEC.md` | Before any feature touching product behavior, validating a new feature belongs |
| `project-management/ARCHITECTURE.md` | Before any deploy, or any change to how layers connect |
| `project-management/STACK.md` | Before any permanent or architecture decision |
| `project-management/TESTING-LEARNINGS.md` | Before any session where tests are the primary work |
| `project-management/FEATURE-LOCK.md` | Before writing any code for a feature |
| `project-management/SHIP-TO-PROD.md` | When deploying |
| `project-management/SHIP-RULES.md` | Before any feature build, versioning call, or shipping decision |
| `project-management/FIRST-BUILD.md` | When scoping the first build on this project |
| `project-management/DEBUGGING-TAXONOMY.md` | When starting a debug session — classify the layer before writing any fix |
| `project-management/DEV-HEURISTICS.md` | By domain — before touching auth, database, API, deploy, or testing |
| `feature-builds/_playbook/BUILD-RULES.md` | When writing a Cursor task |
| `feature-builds/_playbook/BUILD-LEARNINGS.md` | Before writing any runbook — especially UI-heavy or multi-step builds |
