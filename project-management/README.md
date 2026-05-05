# project-management/

This folder is the PM system. If a decision isn't written here, it's lost.

Every file has a trigger condition. Don't read them all at session start — read the one that's relevant to what you're doing right now.

---

## The files and when to reach for each

### Core — read every session

| File | What it is | When to read |
|---|---|---|
| `_log.md` | One entry per session. What happened, decisions made, RESUME HERE blocks for mid-flight work. | Session start — last 80 lines only |
| `ROADMAP.md` | What's next, ordered, with blockers and dependencies. | Session start — full read |
| `KNOWN-ISSUES.md` | Open bugs only. Fixed bugs live in git history. | Session start — quick scan |

### Process — read on trigger

| File | What it is | Read when... |
|---|---|---|
| `FEATURE-LOCK.md` | The scope lock template. Copy this into `feature-builds/[feature]/PRE-BUILD-LOCK.md` and fill every gate before writing code. Gates cover: canonical alignment, scope IN/OUT list, unique-job statement, blast radius, mocks, kill date. A blank gate blocks the build. | Before writing any code for a feature |
| `SHIP-TO-PROD.md` | Pre-deploy, staging, canary, post-deploy verification sequence. Step by step. No skipped steps. | When deploying |
| `SHIP-RULES.md` | The patterns every build plays by: Parallel Change, Strangler Fig, Canary Release. Versioning axes (MAJOR/MINOR/PATCH/NO BUMP). The N-1 rule — N-1 stays bootable, N-2 gets deleted. Every temporary code block ships with a kill date written here before any code is written. | Before any feature build, versioning decision, or anything involving how to ship |
| `FIRST-BUILD.md` | How to apply the feature lock to your first build. Infrastructure pre-steps, gate-by-gate notes for the MVB context. Read this once at project start, then reference `FEATURE-LOCK.md` directly for subsequent builds. | When scoping and running the first build |
| `DEBUGGING-TAXONOMY.md` | A classification system for bugs: which layer broke (Data / Model / API / UI / Integration / Config / Infrastructure), bug ID prefixes (B-, UI-, INT-, CFG-, INF-, SCH-, P-, SEC-), and a diagnostic sequence. The point is to classify before you fix — most debugging time is wasted fixing the wrong layer. | When something breaks and you're starting blind |
| `DEV-HEURISTICS.md` | Domain-indexed working knowledge: auth, database, API, deploy, testing. "If you're touching X, remember Y." These aren't architecture decisions (those are in STACK.md) — they're lessons learned the hard way that save 30 minutes of re-debugging. Fill in entries as you hit them. | By domain — before touching any area with known gotchas |

### Stable reference

| File | What it is | Read when... |
|---|---|---|
| `ARCHITECTURE.md` | How the system fits together: layers, data flow, stack choices and why. | Before any deploy, or any change to how layers connect |
| `STACK.md` | Permanent decisions that survive any single session. Library choices, infrastructure decisions, patterns that are now fixed. Read the first 20 lines every session; read in full before any architecture call. | Before any permanent or architecture decision |
| `TESTING-LEARNINGS.md` | Lessons from production failures, not test coverage goals. What broke in production that tests didn't catch, and why. | Before designing a test suite or before any session where tests are the primary work |

---

## The invariant

Every build tests against one statement:

> `V(N+1) = V(N) + exactly one surgical change.`

If the build can't be expressed that way, split it into two builds. The `FEATURE-LOCK.md` gates are where this becomes concrete for each feature.
