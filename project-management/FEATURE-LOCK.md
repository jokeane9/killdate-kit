# Pre-Build Lock

Copy this into `feature-builds/[feature]/PRE-BUILD-LOCK.md` before any code is written. Fill every field. A blank or TBD field blocks the build — if you can't answer it, you don't understand the scope well enough to build safely.

Every gate produces a written artifact. "Confirmed in my head" doesn't count. If it isn't written down, it isn't locked.

---

## The pipeline

Every feature build walks this sequence. No step proceeds past a closed checkpoint.

```
canonical → [CP-0] → lock → [CP-1] → mocks → [CP-2] → runbook → [CP-3] → code → prod
```

- **CP-0** — canonical cite check. Confirm the canonical reference before filling the lock.
- **CP-1** — lock sign-off. Every gate passed, every artifact written.
- **CP-2** — mocks sign-off. Full-surround render and side-by-side diff approved before Cursor opens a file.
- **CP-3** — runbook sign-off. Every task maps to a gate, a mock element, or a copy-deck string. Nothing unmapped.

---

# Pre-Build Lock: [FEATURE NAME]

**Date locked:** [YYYY-MM-DD]
**Branch:** [branch name]

---

## Gate 1 — Canonical alignment

What lines or sections of `project-management/CANONICAL.md` does this build derive from? Quote them or cite them specifically. "The canonical" is not a citation.

**Canonical reference:**
[specific quote or section]

**Conflicts identified:**
[any conflict between this change and the canonical, or "none"]

**Resolution:**
[change adjusts to canonical / canonical updated first — state which]

**Status:** ☐ PASSED  ☐ BLOCKED

*Artifact: this written statement.*

---

## Gate 2 — Scope lock

**Scope statement** — one sentence: what is *new* about this build. Not what it does — what is new.

> Example: "Add a timezone field to user settings and persist it on save."

[your scope statement]

**IN:**
- [item]
- [item]

**OUT** — explicit, not implied. An empty OUT list is a red flag.
- [surface that won't change]
- [surface that won't change]

**Section-vs-Replacement** — one sentence: is this a new surface, replacing an existing surface in place, or modifying an existing surface? Name the exact slot, route, or component.

> Example: "Replaces the `<ActivityFeed>` component inside `/dashboard` with `<InsightsFeed>`. One slot. One component swap. No route change."

[your answer]

**Unique-Job** — one sentence: what does this feature do that no existing surface already does? If the answer is "shows the same data reshaped," descope.

> Example: "Surfaces time-of-day engagement patterns — no existing component shows this signal."

[your answer]

**Single surgical change test:** `V(N+1) = V(N) + [exactly one surgical change]`

If you can't complete it in one line, split the build.

[your answer]

**Status:** ☐ PASSED  ☐ BLOCKED

*Artifact: the IN/OUT lists, section-vs-replacement statement, unique-job statement, and single surgical change test above.*

---

## Gate 3 — Invariants inventory

Every surface that must behave byte-for-byte identically after this change. Empty cell = investigate, not assume clean.

| Surface | Must stay unchanged | How to verify |
|---|---|---|
| [route or component] | [specific behavior] | [manual step or test name] |
| [route or component] | [specific behavior] | [manual step or test name] |
| [add all surfaces — be exhaustive] | | |

**Status:** ☐ PASSED  ☐ BLOCKED

*Artifact: filled table above.*

---

## Gate 4 — Blast radius audit

Every touchpoint this change could affect. Scan all categories. Empty cell = investigate.

| Touchpoint | Behavior Δ? | Data Δ? | State Δ? | Tests Δ? | Coordinated update needed? |
|---|---|---|---|---|---|
| Primary route / surface | | | | | |
| Related routes / surfaces | | | | | |
| Shared components | | | | | |
| API routes / loaders / actions | | | | | |
| Database tables | | | | | |
| Background jobs / queues | | | | | |
| Auth / session handling | | | | | |
| Webhooks / integrations | | | | | |
| Test fixtures / snapshots | | | | | |
| [add others] | | | | | |

**Status:** ☐ PASSED  ☐ BLOCKED

*Artifact: filled matrix above.*

---

## Gate 5 — Four-state rendering

For every UI surface in this build, all four states must be designed before code starts. Not discovered during.

| Surface | Empty state | Loading state | Error state | Populated state |
|---|---|---|---|---|
| [surface] | ☐ designed | ☐ designed | ☐ designed | ☐ designed |

**Status:** ☐ PASSED  ☐ BLOCKED  ☐ N/A (no UI in this build)

*Artifact: mocks or sketches for all four states, committed to feature folder.*

---

## Gate 6 — Technical interface contract

For every API route, loader, data shape, or shared type that changes:

| Interface | Change type | Rollback path | Tests updated |
|---|---|---|---|
| [interface] | Breaking / Non-breaking / Same-shape-different-semantics | [how to roll back] | ☐ |

**Note on same-shape-different-semantics:** the most dangerous class. The shape doesn't change but the meaning does — downstream consumers silently misinterpret it. Flag these explicitly.

**Status:** ☐ PASSED  ☐ BLOCKED  ☐ N/A (no interface changes)

*Artifact: filled table above.*

---

## Gate 7 — Sunset criterion and kill date

**What prior version or temporary code does this build phase out?**
[e.g. "feature flag `new_dashboard`", "v1 settings schema", or "none"]

**Sunset criterion** — the measurable condition under which temporary code gets deleted:

> Example: "Delete the `new_dashboard` flag once 100% of users are on the new route for ≥14 days with zero errors logged."

[your criterion, or "N/A — no temporary code introduced"]

**Kill date:** [YYYY-MM-DD, or N/A]

**Status:** ☐ PASSED  ☐ BLOCKED  ☐ N/A

*Artifact: sunset criterion and kill date above.*

---

## Gate 8 — Mocks sign-off (CP-2)

Three rules before Cursor opens a file:

- [ ] **Full-surround render** — mock shows the complete application with the new surface substituted in. Untouched sections visible.
- [ ] **Side-by-side diff** — one file shows V(N-1) and V(N) side by side. Every pixel outside the swap is identical.
- [ ] **Runbook-ready** — SKELETON blocks in all runbook tasks derive from this mock, not from interpretation.

[link to mock or note that it was reviewed and by whom]

**Status:** ☐ PASSED  ☐ BLOCKED

---

## Pre-build checklist (CP-1)

All gates must be PASSED before any code is written. BLOCKED = build stops.

- [ ] Gate 1 PASSED — canonical alignment, conflict resolution written
- [ ] Gate 2 PASSED — IN/OUT, section-vs-replacement, unique-job, single surgical change test
- [ ] Gate 3 PASSED — invariants inventory filled, no blank rows
- [ ] Gate 4 PASSED — blast radius matrix filled, no blank rows
- [ ] Gate 5 PASSED (or N/A) — four-state mocks for all UI surfaces
- [ ] Gate 6 PASSED (or N/A) — interface contract for all changed interfaces
- [ ] Gate 7 PASSED (or N/A) — sunset criterion and kill date set
- [ ] Gate 8 PASSED — mocks approved, side-by-side diff signed off

**Signed off:** [name, date]

---

*After build completes: retain this doc in the feature folder. If any gate was bypassed or new drift surfaced during build, add a postmortem note below.*
