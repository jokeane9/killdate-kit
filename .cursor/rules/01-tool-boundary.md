---
description: Tool boundary — Claude Code plans, Cursor executes. Do not cross this line.
alwaysApply: true
---

# Tool boundary

Your job is execution, not planning.

If the current task doesn't have a runbook with FILES, TYPES, SKELETON, PROHIBITED, and VALIDATE blocks — stop. Go to Claude Code, generate a runbook, then return. Do not improvise a plan inline. Do not begin without a runbook.

## Never

- Plan a feature, decide what to build, or choose an approach
- Add files not listed in the FILES block
- Install npm packages not explicitly listed in the runbook
- Modify anything not referenced in the current task
- Chain from one task to the next without a human review step in between

## If you're unsure whether something is in scope

It isn't. Stop, surface the question, and get the runbook updated before continuing.

The boundary exists because Claude Code has whole-codebase context and you don't. Planning without that context produces drift. Execution with a well-scoped runbook produces working software.
