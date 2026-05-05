# Feature builds

One folder per feature. Each folder contains the build artifacts for that feature.

---

## Folder structure

```
feature-builds/
├── _completed/              # Finished features, kept for reference
│   └── [feature-name]/
│       ├── PRE-BUILD-LOCK.md
│       └── runbook-[task].md
├── [active-feature]/        # Current in-progress feature
│   ├── PRE-BUILD-LOCK.md
│   └── runbook-[task].md
└── README.md
```

---

## Per-feature contents

Every feature folder should contain:

- `PRE-BUILD-LOCK.md` — filled-in scope lock (copy from `docs/pre-build-lock.md`)
- `runbook-[task-name].md` — one runbook per Cursor task (copy from `docs/runbook-template.md`)

---

## When to create a feature folder

When the pre-build lock is filled and passes the pre-build checklist. Not before. Creating the folder is the signal that scope is locked and the build can start.

---

## What "completed" means

A feature moves to `_completed/` when:

- All Cursor tasks are done and reviewed
- The deploy checklist has been run
- Any kill dates are recorded in `project-management/_log.md`
- The feature is in production and verified

The `_completed/` folder is reference material — the locked docs show what was decided and why. Don't delete them.
