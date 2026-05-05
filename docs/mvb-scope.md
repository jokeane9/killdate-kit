# MVB Scope

The Minimum Viable Build is the smallest build that proves your stack works end-to-end. Not a proof of concept — the real stack, one thin vertical slice, running in production.

Use this before your first build on a new project.

---

## MVB is not MVP

- **MVP** (Minimum Viable Product) is for users — the smallest thing worth shipping to real people.
- **MVB** (Minimum Viable Build) is for you — the smallest thing that proves the stack is wired together correctly.

MVB happens before anyone sees the product. Its output is confidence that the infrastructure works, the layers connect, and the deploy pipeline runs clean.

---

## Why MVB before everything else

Drift is invisible until it compounds. A wide first build touches three layers at once — if something breaks, you don't know which layer caused it. A narrow first build makes drift visible early, when it's cheap to fix.

The MVB also forces real infrastructure decisions before feature work starts. You can't stub your way through a staging database, a deploy pipeline, or an auth flow. Better to learn the hard constraints on build one.

---

## MVB rules

1. **One layer first.** If your stack has a frontend and a backend, Build 1 proves the frontend. Build 2 proves the backend. Build 3 connects them. Don't connect layers until each works independently.

2. **Infrastructure via CLI before Cursor opens a file.** Provision databases, queues, services, and deploy targets before writing application code. Surprises at deploy time cost more than surprises at setup time.

3. **Lock scope before mocks. Mocks before code.** The MVB scope lock (below) defines what's in scope. An approved skeleton defines what "done" looks like. Only then does Cursor execute.

4. **Signal that scope is too wide:** if your Build 1 touches 3+ layers (e.g. Python pipeline + database + React component), split it. Each additional layer compounds drift risk.

---

## MVB Scope Lock

Fill this in for your first build. This is a specialised pre-build lock — use the full `docs/pre-build-lock.md` for subsequent features.

**Stack layer being proven:**

> Example: "Remix frontend — authenticated route, database read, rendered output."

[your answer]

---

**One sentence: what does Build 1 prove?**

> Example: "A logged-in user can load /dashboard and see a list of their items fetched from the database."

[your answer]

---

**IN — what Build 1 includes:**

- [ ] [specific piece]
- [ ] [specific piece]

---

**OUT — everything else (be explicit, not lazy):**

- [ ] No write operations
- [ ] No payments or billing
- [ ] No email or notifications
- [ ] No background jobs or queues
- [ ] No admin interface
- [ ] [anything else that's tempting but out of scope]

---

**Done when:**

A user can [specific action] and [specific observable result happens].

> Example: "A user can log in, load /dashboard, and see a rendered list of items from the database — no errors, no blank states, no stubs."

[your done condition]

---

**Runbook tasks for Build 1:**

Copy `docs/runbook-template.md` for each task below.

1. [task name — e.g. "Provision database and run initial migration"]
2. [task name — e.g. "Add authenticated /dashboard route with loader"]
3. [task name — e.g. "Render item list from database query"]
