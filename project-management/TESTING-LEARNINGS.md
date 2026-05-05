# Testing learnings

Production failure lessons. Read this before designing any test suite or before a session where tests are the primary work.

This is a log, not a principles doc. The principles (90/10 rule, golden fixture rule) live in `CLAUDE.md`. This file captures what broke in production that changed how you think about testing — specific failures, specific fixes, specific lessons that only experience teaches.

---

## What belongs here

- A test passed but production broke — what was the gap?
- A test caught something that would have been a bad deploy — what was the pattern worth keeping?
- A test suite became a maintenance burden — what made it fragile?
- A class of bug that tests consistently miss on this codebase

Not: general testing advice (that's `feature-builds/_playbook/BUILD-LEARNINGS.md`). Not: open bugs (that's `KNOWN-ISSUES.md`). Specifically: lessons from production that changed how you test.

---

## Entry format

**[YYYY-MM-DD] Short description of what broke**
- **What happened:** [what failed in production]
- **Why tests didn't catch it:** [the gap — mocked too much, wrong layer, happy path only, etc.]
- **Fix:** [what changed in the test suite]
- **Rule going forward:** [one-line principle derived from this failure]

---

[No entries yet. First entry goes here when production teaches you something.]
