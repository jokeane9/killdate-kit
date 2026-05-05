# Runbook template

Copy this into `feature-builds/[feature]/runbook-[task-name].md` for every Cursor task. Fill every block. No exceptions for "simple" tasks — the PROHIBITED block is what makes agentic work safe on a live codebase, and a short VALIDATE is the difference between "I think it works" and "I know it works."

The five blocks are not negotiable. If you can't fill one of them, you don't understand the task well enough to execute it yet.

---

# Runbook: [TASK NAME]

**Feature:** [feature name]
**Task:** [N of M — e.g. 2 of 4]
**Branch:** [branch name]

## FILES

Explicit allowlist. Cursor may only touch these files. If completing this task requires touching a file not listed here, stop — update this runbook before proceeding.

```
[path/to/file.ts]
[path/to/another-file.ts]
```

## TYPES

Data contracts this task reads or writes. Paste the relevant interfaces, types, schema definitions, or API shapes. If there are none, write `N/A — no data contracts in this task`.

```typescript
// [paste types here]
```

## SKELETON

The expected output shape — what a correct result looks like before any code is written. This is the thing you approved before Cursor opened a file. Be concrete: describe the UI state, the function signature, the API response shape, or the database row — whatever "done" looks like for this task.

```
[describe the expected output]
```

## PROHIBITED

What not to do. Be explicit. This block prevents the most common failure modes. Add task-specific items below the defaults.

- Do not modify files outside the FILES block
- Do not install npm packages not listed in this runbook
- Do not refactor code adjacent to the task
- Do not add abstractions beyond what the task requires
- Do not add error handling for scenarios that can't happen in this context
- [task-specific prohibition]
- [task-specific prohibition]

## VALIDATE

How to confirm the task is done correctly. Steps a human can run. Be specific — "it works" is not a validation step.

- [ ] [specific thing to check]
- [ ] [specific thing to check]
- [ ] FILES check: only touched files listed above
- [ ] No new files created outside the allowlist
- [ ] Tests pass: `[test command]`

---

## Worked example

Below is a filled-in example for reference. Delete this section before handing to Cursor.

---

# Runbook: Add /settings route

**Feature:** user-settings
**Task:** 1 of 3
**Branch:** feature/user-settings

## FILES

```
app/routes/settings.tsx
app/components/SettingsForm.tsx
app/lib/settings.server.ts
```

## TYPES

```typescript
interface UserSettings {
  notificationsEnabled: boolean;
  timezone: string;
  displayName: string;
}

// Loader return type
type SettingsLoaderData = {
  settings: UserSettings;
};
```

## SKELETON

A settings page at `/settings` that renders a form with three fields matching UserSettings: a notifications toggle (checkbox), a timezone selector (select with common timezones), and a display name input (text). Form POSTs to the same route. On success, redirects back to `/settings`. No database writes yet — this task is UI and loader only. The loader returns hardcoded placeholder data.

## PROHIBITED

- Do not add database writes or mutations (that's task 2)
- Do not modify `app/routes/dashboard.tsx` or any other existing route
- Do not add form validation beyond HTML `required` attributes
- Do not install new packages
- Do not add loading states or optimistic UI

## VALIDATE

- [ ] `npm run dev` — `/settings` renders without errors in browser
- [ ] Form has exactly three fields matching UserSettings interface
- [ ] Form POST redirects back to `/settings`
- [ ] No console errors on page load
- [ ] No TypeScript errors: `npm run typecheck`
- [ ] FILES check: only the three files above were touched
