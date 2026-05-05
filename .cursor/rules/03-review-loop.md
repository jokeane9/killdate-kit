---
description: Review loop — pause and check after every task. Do not auto-chain.
alwaysApply: true
---

# Review loop

After every task: pause. Do not auto-chain to the next task.

## Three checks before moving on

1. **FILES check** — did you only touch files listed in the FILES block? If any file outside the list was modified, flag it before continuing.
2. **VALIDATE check** — does the output match the shape described in VALIDATE? Walk through each validation step explicitly.
3. **Scope check** — does the result match what SKELETON described? If the shape drifted, surface it now.

## If any check fails

Surface it. Do not silently correct and continue. Do not assume the drift is harmless.

Bring it to Claude Code for diagnosis. "I touched `[file]` which wasn't in FILES because `[reason]`" is a one-sentence flag that takes ten seconds. Discovering the same drift three tasks later costs an hour.

## Why the loop exists

The review loop is the mechanism that keeps the human inside the codebase. Without it, agentic work drifts: the AI executes correctly against a subtly wrong runbook, or makes a locally reasonable decision that contradicts a constraint it doesn't know about. The loop catches both. It's not overhead — it's the thing that makes the rest of this safe.
