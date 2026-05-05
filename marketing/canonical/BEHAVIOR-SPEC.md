# [AppName] — Behavior Spec

> How the product works. Architecture, pipeline behavior, product principles, feature grammar, validation criteria.
> Keep this precise, exhaustive, and testable.
> Update this doc when product behavior changes — the feature grammar section determines whether new features belong.

---

## What it does

[2–3 sentences. What the product does at a functional level. Precise enough that someone could build a test against it.]

---

## What it does not do

The NOT list is often more valuable than the does list. Every "should we add X" conversation ends faster when X is explicitly out of scope in writing.

- Not [X]
- Not [Y]
- Not [Z]

---

## Principles

The decisions that constrain every downstream choice. These aren't preferences — they're written constraints that fail specific builds when violated.

1. **[Principle name]** — [one-sentence statement of the constraint and why it exists]
2. **[Principle name]** — [one-sentence statement]
3. **[Principle name]** — [one-sentence statement]

---

## Feature grammar

How new features get evaluated. A feature belongs in this product if and only if it satisfies all of these:

- [ ] [criterion 1 — e.g. "it reduces a recurring manual task for the primary user"]
- [ ] [criterion 2 — e.g. "it works with data already in the system, no new data sources"]
- [ ] [criterion 3 — e.g. "it doesn't require the user to learn a new concept"]

If a proposed feature can't check all boxes, it either belongs in a different product or it's scope creep.

---

## How it works (functional spec)

[The behavioral description — the stuff that would be inconsistently reinvented if it weren't written down. State machines, data flows, pipeline behavior, user flows. Be specific.]

---

## Validation criteria

What "correct" looks like. Observable, not aspirational. Used at the start of every feature build to check alignment.

- [observable criterion 1 — e.g. "a new user completes the primary flow in under 3 minutes on first try"]
- [observable criterion 2]
- [observable criterion 3]
