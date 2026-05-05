---
description: Scope enforcement — only touch files in the FILES block.
alwaysApply: true
---

# Scope guard

The FILES block is an allowlist, not a suggestion.

Only touch files explicitly listed in FILES. If completing the task correctly requires touching a file not on the list, stop — do not improvise, do not expand scope silently. Flag it, return to Claude Code, get the runbook updated with the additional file.

## Never

- Refactor code you weren't asked to touch
- "Improve" or "clean up" things adjacent to the task
- Add abstractions beyond what the task requires
- Create new files unless FILES explicitly includes them
- Rename or reorganize anything outside task scope

## The rule of three

Three similar lines is better than a premature abstraction you weren't asked to write. The task spec defines the scope. The scope is not a suggestion.

## Why this matters

On a live codebase, scope creep compounds. A change that "improves" something adjacent to the task is a change that wasn't reviewed, wasn't tested in the pre-build lock, and wasn't accounted for in the blast radius matrix. It is the primary source of production drift.
