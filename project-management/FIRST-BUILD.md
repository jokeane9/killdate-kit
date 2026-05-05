# MVB Scope

The Minimum Viable Build is the smallest build that proves your stack works end-to-end on real infrastructure. Not a proof of concept, not a prototype — the real stack, one thin vertical slice, running in production.

Use `docs/pre-build-lock.md` for your MVB, just like any other feature build. This doc explains how each gate applies to an MVB specifically, what's N/A, and the one pre-step that doesn't exist in regular builds.

---

## MVB is not MVP

- **MVP** (Minimum Viable Product) — for users. The smallest thing worth shipping to real people.
- **MVB** (Minimum Viable Build) — for you. The smallest thing that proves the stack is wired together correctly.

MVB happens before anyone sees the product. Its output is confidence: the layers connect, the deploy pipeline runs clean, the infrastructure behaves as expected under real conditions.

---

## Why narrow scope matters

Drift is invisible until it compounds. A wide first build touches three layers at once — if something breaks, you don't know which layer caused it. A narrow MVB makes the failure surface small and the feedback loop fast.

**Signal that scope is too wide:** if your MVB touches 3+ layers simultaneously (e.g. data pipeline + database + UI), split it. Build 1 proves one layer. Build 2 proves the next. Build 3 connects them.

---

## The pre-step: infrastructure before code

This is the one step that doesn't exist in regular feature builds, and the one most often skipped.

Before Cursor opens a file:

1. Provision the database (real instance, not local-only)
2. Set up the deploy target (container, serverless function, hosting — whatever your stack uses)
3. Wire CI/CD — push to main should trigger a deploy
4. Confirm secrets management (environment variables, not hardcoded credentials)
5. Verify the deploy pipeline runs clean on an empty app

Surprises at deploy time cost more than surprises at setup time. Every hour spent on infrastructure before code saves two hours of debugging after.

---

## How the pre-build lock gates apply to an MVB

Run the full pre-build lock. Here's how each gate maps:

**Gate 1 — Canonical alignment**
Still required. Your canonical should exist before MVB starts — it's the document that defines what you're building and for whom. If it doesn't exist yet, write it first. The MVB is the first build that proves the canonical is achievable, not the first build before the canonical exists.

**Gate 2 — Scope lock**
Critical. The MVB's scope statement has a specific form:

> "A [role] can [action] and [observable result happens] — on real infrastructure, no stubs."

Section-vs-replacement: there is no prior surface. Write "new surface — first build." Unique-job: "proves the stack is wired end-to-end."

OUT list is especially important here. The temptation to expand scope on the first build is high — everything feels foundational. Write the OUT list first.

**Gate 3 — Invariants inventory**
Lighter than a feature build. There are no prior surfaces to protect. Your invariants are infrastructure-level:

| Surface | Must stay unchanged | How to verify |
|---|---|---|
| Deploy pipeline | Runs clean after every push | CI passes |
| Database connection | Accessible from app at runtime | Health check endpoint |
| Auth flow | Blocks unauthenticated access | Manual test |

**Gate 4 — Blast radius audit**
Simpler on an MVB — there's less existing surface to affect. Focus on: does this build create any hard constraints that will limit future builds? Name them explicitly.

**Gate 5 — Four-state rendering**
Still required for any UI surface in the MVB. Empty, loading, error, and populated states all need to be designed before Cursor opens a file. "We'll handle errors later" is how you end up with blank screens in production.

**Gate 6 — Technical interface contract**
On an MVB, this is forward-facing rather than backward-compatible. Define the interfaces you're establishing — the data shapes, the API contracts, the loader patterns. These become the baseline everything else builds on. Getting them right now is cheaper than migrating them later.

**Gate 7 — Sunset criterion and kill date**
Always N/A. No prior version being phased out.

**Gate 8 — Mocks sign-off**
Same rules apply. Full-surround render is just the app shell with the new surface in place. Side-by-side diff: before (empty shell) vs after (one working route). Approve the shape before Cursor executes.

---

## The MVB done condition

A feature build's done condition is user-observable value. The MVB's done condition is narrower:

> A [role] can [specific action] and [specific observable result] — on real infrastructure, with real data, no stubs, no local-only shortcuts.

If any part of that sentence contains a stub, a mock server, or a local-only workaround, the MVB is not done.

---

## MVB scope lock (fill this, then transfer to pre-build-lock.md)

**Stack layer being proven:**
[e.g. "authenticated web route reading from a real database and rendering to the browser"]

**Done condition:**
A [role] can [action] and [result] — on real infrastructure, no stubs.

**OUT (write this first):**
- No write operations
- No payments or billing
- No email or notifications
- No background jobs or queues
- No admin interface
- No secondary routes
- [anything else that's tempting]

**Hard constraints this build establishes:**
[Data shapes, patterns, or architectural decisions that future builds will depend on. Name them now.]
