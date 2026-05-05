# Pre-Build Lock

Copy this into `feature-builds/[feature]/PRE-BUILD-LOCK.md` before any code is written. Fill every field. A blank or TBD field blocks the build — if you can't answer it, you don't understand the scope well enough to build safely.

The OUT list is where the discipline lives. The blast radius matrix is where drift hides. Fill both seriously.

---

# Pre-Build Lock: [FEATURE NAME]

**Date locked:** [YYYY-MM-DD]
**Branch:** [branch name]

---

## Scope statement

One sentence: what is *new* about this build. Not what it does — what is new.

> Example: "Add a timezone field to user settings and persist it on save."

[your scope statement]

---

## IN list

What this build adds or changes.

- [ ] [item]
- [ ] [item]
- [ ] [item]

---

## OUT list

What this build explicitly does NOT touch. **The discipline lives here.** If you're not sure whether something is out of scope, put it on this list until explicitly confirmed otherwise. An empty OUT list is a red flag.

- [ ] [surface that won't change]
- [ ] [surface that won't change]
- [ ] [surface that won't change]

---

## Single surgical change test

Complete this sentence: `V(N+1) = V(N) + [exactly one surgical change]`

If you can't complete it in one line, split the build.

> Example: `V(N+1) = V(N) + timezone field stored in user_settings and displayed in /settings`

[your answer]

---

## Invariants inventory

Surfaces that must stay unchanged. Verify each one before shipping.

| Surface | Must stay unchanged | How to verify |
|---|---|---|
| [route or component] | [specific behavior] | [manual step or test name] |
| [route or component] | [specific behavior] | [manual step or test name] |

---

## Blast radius matrix

Every touchpoint this change could affect. Empty cells = "investigate, don't assume clean." Fill in every row.

| Touchpoint | Affected? | Notes |
|---|---|---|
| [file or system] | yes / no / investigate | [notes] |
| [file or system] | yes / no / investigate | [notes] |
| [file or system] | yes / no / investigate | [notes] |

---

## Sunset criterion

What condition allows any temporary code (feature flags, shims, compatibility fallbacks) to be deleted?

If this build introduces no temporary code, write: `N/A — no temporary code introduced.`

> Example: "All users have migrated to v2 settings schema. Confirmed via: `SELECT COUNT(*) FROM users WHERE settings_version = 1` returns 0."

[your answer]

---

## Kill date

Hard date for temporary code deletion: `YYYY-MM-DD`

If N/A above, write: `N/A`

[your date]

---

## Mocks sign-off

- [ ] Output shape described in SKELETON blocks of all runbooks
- [ ] HTML mock or sketch approved before Cursor opens a file

[link to mock or note that it was reviewed]

---

## Pre-build checklist

- [ ] Canonical alignment: this feature is consistent with `project-management/CANONICAL.md`
- [ ] Architecture alignment: this feature is consistent with `project-management/ARCHITECTURE.md`
- [ ] Scope statement written and passes the single surgical change test
- [ ] IN list complete
- [ ] OUT list non-empty and specific
- [ ] Invariants inventory filled — no blank rows
- [ ] Blast radius matrix filled — no blank rows
- [ ] Sunset criterion and kill date set (or explicitly N/A)
- [ ] Mocks approved

**All boxes checked before any code is written.**
