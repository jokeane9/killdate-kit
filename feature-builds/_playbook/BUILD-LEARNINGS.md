# Build learnings

Hard-won principles for writing runbooks that produce consistent Cursor output. These apply across stacks and projects — they're about how Cursor behaves, not how any specific codebase is structured.

Add your own project-specific lessons at the bottom as you earn them.

---

## How Cursor consumes instructions

### Context window priority

Cursor treats content differently based on where it appears:

1. **Pasted prompt text** — primary instruction. Cursor follows this most faithfully.
2. **`@`-referenced files** — supplementary context. Cursor loads them but deprioritises them, especially when they conflict with or repeat the prompt.
3. **Open editor tabs** — ambient context. Cursor may reference them but doesn't treat them as instructions.

**The implication:** Everything structural — field names, PROHIBITED items, the skeleton — must be in the runbook task directly. If it matters, it goes in the prompt. If it's in an `@file`, treat it as background, not instruction.

### Code > pseudocode > prose

Cursor reproduces code almost verbatim. It interprets pseudocode with some drift. It treats prose as suggestions.

| Instruction format | Cursor behaviour | Drift level |
|---|---|---|
| Copy-pasteable code skeleton (JSX, function sig, schema) | Reproduces structure exactly, fills in logic around it | Very low |
| Tree diagram or indented outline | Translates with some structural interpretation | Low–medium |
| Prose ("use a Card with a list inside") | Interprets freely, adds what seems right | High |
| "Match the visual layout from the mock" | Builds something inspired by it | Very high |

**The implication:** The SKELETON block should be code, not description. A copy-pasteable component tree or function signature is a contract. A paragraph describing the output is a suggestion.

---

## The primary failure mode: additive drift

Cursor rarely uses the wrong component or deletes things you asked for. Its most consistent failure mode is **adding things you didn't ask for** — an extra status banner, a wrapper element, helper text under an input, a secondary button "for consistency."

This is why the PROHIBITED block exists. It is not a restatement of scope rules (those live in the cursor rules files). It is a targeted countermeasure for additive drift.

**PROHIBITED works when it names specific things, not general principles.**

Too vague:
```
- Don't add anything not in the skeleton
```

Effective:
```
- No status banner on this screen
- No wrapper section around the form
- No helper text under input fields
- No secondary cancel button
- No Autocomplete component — use a flat clickable list
```

The specific items come from knowing what Cursor tends to invent in the context of your task. If you're building a form, think: what would Cursor add that you didn't ask for? Put those on the list.

---

## The global rules pattern

Some rules apply to every task in a build, not just one. If you repeat them in every task, the runbook becomes noisy. If you put them in one task and not others, Cursor forgets them by task 4. If you put them in an `@`-referenced file, they get deprioritised.

The fix: a **global rules section at the top of the runbook**, defined once, short. Each task's VALIDATE block references it by name.

Things that belong in global rules:
- Execution guardrail ("do not modify files outside the FILES block, do not rename variables in files you import from")
- Any invariant that must hold after every task (navigation guards, state checks, auth patterns)
- A component or pattern that every file in this build must export or implement
- Any constraint that, if violated in task 1, cascades into tasks 2–5

Global rules stay in the pasted prompt — they're at the top of the same document Cursor is executing from, so they stay in context.

---

## VALIDATE: state checks, not just UI checks

"Confirm the page renders" doesn't catch state bugs. A page can render correctly while writing the wrong value to the database, redirecting to the wrong route, or skipping a required step.

VALIDATE blocks that catch real bugs:
- Query the database and confirm the expected value is there
- Confirm the redirect goes to the right place with the right status
- Run the type checker, not just the dev server
- Check that a background job was queued, not just that the UI says it was

**The principle:** validate the state change, not the visual output. Visual output is a consequence of state. If state is correct, the UI follows. If you only check the UI, you can miss a silent state bug that breaks the next task.

---

## Schema migrations: always a separate task

Never embed a schema change inside a task that also writes application code. Cursor may apply them in the wrong order, skip the migration if the code task feels complete, or produce a task so large its fidelity drops.

The pattern that works:
1. Task N: migration only — exact DDL, VALIDATE confirms the column/table exists
2. Task N+1: application code that uses the new schema

This also makes rollback clean. If task N+1 goes wrong, you know the schema is in the right state and only the application code needs reverting.

---

## Test alignment: dedicated task, not an afterthought

When a runbook changes a pattern that existing tests depend on — a loader that switches from returning JSON to throwing a redirect, a shared type that gains a new required field, a constant that gets a new value — the tests that depended on the old pattern will break.

The failure mode: you batch all test fixes at the end, after all code tasks are done. By then you have multiple broken patterns across multiple files and it's hard to isolate which change broke which test.

The fix: schedule a dedicated test-alignment task immediately after any task that changes a shared pattern. One pattern change, one alignment task, right after it. Keep the blast radius small.

---

## Interaction paradigm drift

When a task involves a user interaction — clicking, selecting, dragging, matching — prose descriptions of the interaction are high-risk. "User matches products to competitors" can be implemented as a dropdown, an autocomplete, a drag-and-drop, a modal, or a flat clickable list. All are valid interpretations of the brief.

Cursor will pick one based on pattern matching. It may pick wrong.

For any interactive screen, three layers of spec prevent paradigm drift:
1. **A prototype** — clickable HTML or equivalent that demonstrates the exact interaction model
2. **Per-element interaction spec** — what happens on click, on hover, on submit, for each element
3. **Code skeleton** — the exact component structure implementing that interaction

The PROHIBITED block must also explicitly block the wrong paradigm by name. "No Autocomplete component" is unambiguous. "No dropdowns" is not — Cursor may not classify Autocomplete as a dropdown.

---

## Your project-specific learnings

Add entries here as you earn them. Format: the failure mode, what caused it, what prevents it.

---

*This document captures how Cursor behaves, not how any specific stack works. Stack-specific patterns belong in your `.cursorrules` file and your project's session log.*
